# Liquidation API

Liquidations on Derive are permissionless, meaning that anyone can flag an underwater subaccount for liquidations and bid on them. However, maintaining infrastructure for monitoring margin of the subaccounts, query their balances and do bidding on-chain is cumbersome to implement, which is why Derive also provides a liquidations API which works similar to orders and RFQs.

For those interested in manual on-chain liquidations, or those interested in the details on how the auction is implemented, please refer to the full [liquidations guide here](https://docs.lyra.finance/docs/liquidations-1). This guide will focus on how to use the API to monitor live auctions and liquidate flagged users.

The main features of the liquidation auctions are as follows:

* Subaccounts get flagged for liquidation when their maintenance margin falls below zero
* A subacount gets put up for an auction
* Liquidators bid to take over a percentage of the user's subaccount, where the percentage is capped to being "the minimum needed to bring the user's account into a good state"
* The bids take over an equal percentage of every position and collateral the liquidated user is holding, for example a 50% bid on a subaccount holding 2 long ETH perps, 1 short option, 0.5 ETH spot and $2000 USDC would result in the liquidator acquiring 1 long ETH perp, 0.5 short options, 0.25 ETH spot and $1000 USDC in exchange for paying the bid price to the liquidated user
* The auction price starts off at a 5% discount relative to an oracle mark-to-market value of the whole subaccount, then decays quickly to 30% over 15 minutes, followed by a slow decay towards a 100% discount
* If the liquidated user is insolvent, the auction instead starts off at a price of zero (i.e. liquidator can take over the account without paying anything) and then decays down to a negative price (i.e. liquidator being paid by security module to take over the bad account) over 60 minutes

The liquidations flow consists of the following steps:

1. Authentication.
2. Setting up a subscription to `auctions.watch` channel which publishes the state of ongoing auctions.
3. Upon receiving the notification from `auctions.watch`, call a `private/liquidate` RPC endpoint, assuming the current bid price is acceptable for the liquidator. Note that the liquidator's subaccount gets locked until the transaction settles, which can take up to 10 seconds, no orders or other liquidations can be submitted in this locked state.
4. Transaction state can be polled using `public/get_transaction` endpoint (the `private/liquidate` response will contain a `transaction_id` to track), note that the transaction may either end up being `settled` or `reverted`, e.g. if another bid took place shortly before this bid.
5. After the transaction settles, the liquidator will receive an update to their balances over the `{subaccount_id}.balances` channel (if subscribed), and they can check the details of the liquidation using the `private/get_liquidator_history` endpoint.

# 0. Constants & Setup

This examples use the following protocol constants, subaccount IDs, etc.

```typescript
import { ethers } from 'ethers';
import axios from 'axios';
import dotenv from 'dotenv';
import { WebSocket } from 'ws';

dotenv.config();

const PRIVATE_KEY = process.env.SESSION_PRIVATE_KEY as string;
const WS_ADDRESS = 'wss://api-demo.lyra.finance/ws';
const PROVIDER_URL = 'https://testnet-rpc.derive.xyz/';
const HTTP_ADDRESS = 'https://api-demo.lyra.finance';
const ACTION_TYPEHASH = '0x4d7a9f27c403ff9c0f19bce61d76d82f9aa29f8d6d4b0c5474607d9770d1af17';
const DOMAIN_SEPARATOR = '0x9bcf4dc06df5d8bf23af818d5716491b995020f377d3b7b64c29ed14e3dd1105';
const LIQUIDATE_ADDRESS = '0x3e2a570B915fEDAFf6176A261d105A4A68a0EA8D';

const PROVIDER = new ethers.JsonRpcProvider(PROVIDER_URL);
const SIGNER = new ethers.Wallet(PRIVATE_KEY, PROVIDER);

/// if using UI: "Funding Wallet Address" under https://testnet.lyra.finance/settings
const ACCOUNT = '0x2225F2B33AA18a48EAbb675f918E950878C53BE6'

const ENCODER = ethers.AbiCoder.defaultAbiCoder();

const subaccountIdLiquidator = 36919
const AUCTIONS_CHANNEL = "auctions.watch"
```

# 1. Authentication

The first step is to set up a websocket connection and log into it - see the [Authentication](https://docs.derive.xyz/reference/authentication) section for more. Note that the subscription to `auctions.watch` is public and does not require authentication. The private RPC calls can be executed over either websockets or HTTP, and this example will be utilizing websockets.

```typescript
async function connectWs(): Promise<WebSocket> {
  return new Promise((resolve, reject) => {
    const ws = new WebSocket(WS_ADDRESS);

    ws.on('open', () => {
      setTimeout(() => resolve(ws), 50);
    });

    ws.on('error', reject);

    ws.on('close', (code: number, reason: Buffer) => {
      if (code && reason.toString()) {
        console.log(`WebSocket closed with code: ${code}`, `Reason: ${reason}`);
      }
    });
  });
};


async function signAuthenticationHeader(): Promise<{[key: string]: string}> {
  const timestamp = Date.now().toString();
  const signature = await SIGNER.signMessage(timestamp);
    return {
      wallet: ACCOUNT,
      timestamp: timestamp,
      signature: signature,
    };
}

async function loginClient(wsc: WebSocket) {
  const rpcId = Math.floor(Math.random() * 10000);
  wsc.on('message', (data: string) => {
    const response = JSON.parse(data);
    if (response.id === rpcId) {
      console.log(`Got login response with id ${rpcId}:`);
      console.log(response);
    }
  });
  const login_request = JSON.stringify({
      method: 'public/login',
      params: await signAuthenticationHeader(),
      id: rpcId
  });
  wsc.send(login_request);
  await new Promise(resolve => setTimeout(resolve, 2000));
}
```

# 2. Subscribe to `auctions.watch`

The `auctions.watch` channel publishes the state ongoing auctions, as well notifies the subscribers when the auction ends.

Sample data from this channel may look like:

```json
[{
  "subaccount_id": 78202,
  "state": "ongoing",
  "timestamp": 1724715992679,
  "details": {  
     currency: null,  
     margin_type: 'SM',  
     min_cash_transfer: '1277.025016',  
     min_price_limit: '1053.257525',  
     last_seen_trade_id: 419716,  
     estimated_percent_bid: '0.175226',  
     estimated_bid_price: '6010.865122',  
     estimated_mtm: '9082.715036',  
     estimated_discount_pnl: '538.266783',  
     subaccount_balances: {  
       "USDC": '9643.945424640872731732',  
       "ETH-20241228-2500-C": '-1',  
       "ETH-PERP": '65'  
     }  
   }
}]
```

The full schema is available in the [API reference](https://docs.lyra.finance/reference/auctions-watch), and is also shown in the example below.

When a subaccount gets flagged for liquidation, a message in the above form will be emitted to subscribers. For as long as the auction is ongoing, messages will keep being re-sent with the updated data (e.g. changes in the subaccount MtM, changes in balances in case bids were made, etc.). Note that there is no guaranteed frequency for these data updates since during time of a high number of auctions, the publisher might slow down.

When the auction ends, a special message will be sent in the below format:

```json
[{
  "subaccount_id": 78202,
  "state": "ended",
  "timestamp": 1724718992679,
  "details": null
}]
```

Please refer to the [API reference](https://docs.lyra.finance/reference/auctions-watch) for the explanation of each field. An informal description is provided below:

* `currency` and `margin_type` are useful as it is preferred to match the liquidated account type when bidding (due to the limits on the # of assets in the subaccounts)
* `min_cash_transfer` is how much USDC the liquidator needs to have to make a bid **and** supply enough additional funds to meet margin requirements of the acquired portfolio
* `min_price_limit` is how much USDC the liquidator needs to pay for the bid (note that this value is typically smaller than `estimated_bid_price` since it is scaled down by the maximum % that can be liquidated)
* `last_seen_trade_id` is how we ensure the liquidated subaccount's state doesn't suddenly change (due to e.g. another bid) - say a liquidator sends a bid with this field being `419716` thinking they are about to get 17.5% of the above portfolio (11.3 perps, 0.175 short options, 1687 USDC), but another bid comes through causing the portfolio to change the last millisecond, then the RPC call will error out *or* the on-chain transaction will return with status `reverted`
* `estimated_percent_bid` is approximately how much of the account can currently be liquidated
* `estimated_bid_price` is the current bid price referencing the whole portfolio (i.e. the discounted mark-to-market value)
* `estimated_mtm` is the current mark-to-market value of the liquidated portfolio
* `estimated_discount_pnl` is roughly how much in $ will the liquidator make upon successful liquidation, on mark-to-market basis, note in the example above this equals `(estimated_mtm - estimated_discount_pnl) * estimated_percent_bid`
* `subaccount_balances` are the current balances of the liquidated subaccount - the liquidator should expect to acquire each of these balances scaled by `estimated_percent_bid` in exchange for paying `estimated_percent_bid * estimated_bid_price`

Below is a simple code snippet that listens to the `auctions.watch` and writes the results to a mapping:

```typescript

export interface AuctionDetailsSchema {
  currency: string | null;
  estimated_bid_price: string;
  estimated_discount_pnl: string;
  estimated_mtm: string;
  estimated_percent_bid: string;
  last_seen_trade_id: number;
  margin_type: "PM" | "SM";
  min_cash_transfer: string;
  min_price_limit: string;
  subaccount_balances: {[k: string]: string};  // asset name (as in get_subaccount) -> decimal balance
}

export type State = "ongoing" | "ended";

export interface AuctionResultSchema {
  details: AuctionDetailsSchema | null;
  state: State;
  subaccount_id: number;
  timestamp: number;
}

const AUCTIONS_STATE: {[subaccount_id: number]: AuctionDetailsSchema} = {}

async function subscribeAuctions(): Promise<WebSocket> {
    const wsc = await connectWs();

    wsc.on('message', (data: string) => {
      const response = JSON.parse(data);
      if (response.params?.channel == AUCTIONS_CHANNEL) {
        const data = response.params.data as AuctionResultSchema[];
        for (const auction of data) {
          if (auction.state === 'ongoing') {
            AUCTIONS_STATE[auction.subaccount_id] = auction.details!;
          } else {
            delete AUCTIONS_STATE[auction.subaccount_id];
          }
        }
      }
    })

    const subscribeRequest = JSON.stringify({
        method: 'subscribe',
        params: {
            channels: [AUCTIONS_CHANNEL],
        },
        id: Math.floor(Math.random() * 10000)
    });
    wsc.send(subscribeRequest);
    return wsc;
}
```

# 3. Sign and call `private/liquidate`

The `private/liquidate` endpoint is similar to deposits, withdrawals, orders and RFQs in that it requires a "signed action". The signature will be verified on-chain by the [liquidation module](https://github.com/lyra-finance/v2-matching/blob/master/src/modules/LiquidateModule.sol).

Under the hood, the module will create a temporary subaccount where some cash would be transferred to from the caller's subaccount. The temporary subaccount will then call the onchain `bid` function and acquire the percentage of the liquidated portfolio. Finally, the temporary subaccount will be "merged" back into the caller's subaccount (i.e. all positions and collaterals will be transferred back to the caller). Note that the module technically makes the merge feature optional, but the API only supports `mergeAccount=true`.

Below is a snippet that performs signing and a call to the API:

```typescript
export async function sendSignedLiquidate(
  wsc: WebSocket,
  subaccountId: number,
  liquidatedSubaccountId: number,
  priceLimit: string,
  percentBid: string,
  cashTransfer: string,
  lastSeenTradeId: number,
): Promise<any> {

  const nonce = Number(`${Date.now()}${Math.round(Math.random() * 999)}`)
  const signatureExpirySec = Math.floor(Date.now() / 1000 + 600)

  // struct being signed:
  // struct LiquidationData {
  //   uint liquidatedAccountId;  // subaccount id to liquidate
  //   uint cashTransfer;  // $ transferred into a temporary subaccount from caller's subaccount
  //   uint percentOfAcc;  // % of the liquidatedAccountId to liquidate
  //   int priceLimit;  // max $ to pay for the liquidated portion
  //   uint lastSeenTradeId;  // validation in case the liquidated account changes (e.g. via someone else's bid)
  //   bool mergeAccount;  // whether to merge the temporary subaccount into the caller's subaccount, must be true
  // }

  const liquidateDataABI = ['uint256', 'uint256', 'uint256', 'int256', 'uint256', 'bool'];
  const liquidateData = [
    liquidatedSubaccountId,
    ethers.parseUnits(cashTransfer, 18),
    ethers.parseUnits(percentBid, 18),
    ethers.parseUnits(priceLimit, 18),
    lastSeenTradeId,
    true, // API only supports merging the liquidated portion into caller's subaccount
  ];
  const liquidationData = ENCODER.encode(liquidateDataABI, liquidateData);
  const hashedLiquidationData = ethers.keccak256(liquidationData);

  const actionHash = ethers.keccak256(
    ENCODER.encode(
      ['bytes32', 'uint256', 'uint256', 'address', 'bytes32', 'uint256', 'address', 'address'],
      [
        ACTION_TYPEHASH,
        subaccountId,
        nonce,
        LIQUIDATE_ADDRESS,
        hashedLiquidationData,
        signatureExpirySec,
        ACCOUNT,
        SIGNER.address
      ]
    )
    );

  const signature = SIGNER.signingKey.sign(
    ethers.keccak256(Buffer.concat([
      Buffer.from("1901", "hex"),
      Buffer.from(DOMAIN_SEPARATOR.slice(2), "hex"),
      Buffer.from(actionHash.slice(2), "hex")
    ]))
  ).serialized;

  const rpcId = Math.floor(Math.random() * 10000);
  let liquidateResp = undefined;
  wsc.on('message', (data: string) => {
    const response = JSON.parse(data);
    if (response.id === rpcId) {
      console.log(`Got liquidate response with id ${rpcId}:`);
      console.log(response);
      liquidateResp = response;
    }
  });

  const params = {
    subaccount_id: subaccountId,
    liquidated_subaccount_id: liquidatedSubaccountId,
    cash_transfer: cashTransfer,
    percent_bid: percentBid,
    price_limit: priceLimit,
    last_seen_trade_id: lastSeenTradeId,
    signature: signature,
    signature_expiry_sec: signatureExpirySec,
    nonce: nonce,
    signer: SIGNER.address,
  }

  console.log(`Sending liquidate request with id ${rpcId}:`);
  console.log(params);

  wsc.send(JSON.stringify({
    method: 'private/liquidate',
    params: params,
    id: rpcId
  }));

  for (let i = 0; i < 10; i++) {
    if (liquidateResp) {
      break;
    }
    await new Promise(resolve => setTimeout(resolve, 1000));
  }

  return liquidateResp;
}
```

Note that the `auctions.watch` channel can help with figuring out values for some of the call parameters such as `cash_transfer`and `last_seen_trade_id`. For example, here's how one could send a liquidation call using the auctions channel output:

```typescript
async function liquidateTest() {
  const wscSub = await subscribeAuctions();
  await new Promise(resolve => setTimeout(resolve, 5000));
  console.log(AUCTIONS_STATE);
  const wsc = await connectWs();
  await loginClient(wsc);
  let liquidateResp: any = undefined;

  for (const subaccount_id in AUCTIONS_STATE) {
    const auction = AUCTIONS_STATE[subaccount_id];
    if (auction.margin_type !== 'SM') {
      continue; // only liquidate same margin type / currency combination as your subaccount
    }

    console.log(`Liquidating subaccount ${subaccount_id} with auction details:`);
    console.log(auction);

    // add small buffer to price limit and cash transfer in case market moves (here 5% of mtm or $10)
    const buffer = Math.max(Math.abs((+auction.estimated_mtm * 0.05)), 10)

    // cashTransfer has to be strictly greater than 0
    const cashTransfer = (+auction.min_cash_transfer == 0 ? 0.1 : +auction.min_cash_transfer + buffer).toFixed(2);

    liquidateResp = await sendSignedLiquidate(
      wsc,
      subaccountIdLiquidator,
      Number(subaccount_id),
      (+auction.min_price_limit + buffer).toFixed(2),
      '1', // liquidate "up to" 100% of the subaccount (actual % may be less, see auction.estimated_percent_bid)
      cashTransfer,
      auction.last_seen_trade_id,
    );
    break; // for the sake of example liquidate just the first auction
  }
}
```

# 4. Get transaction status

The RPC response to `private/liquidate` will contain `transaction_id` that can be polled via `public/get_transaction`.

```typescript
// Also available over websockets
async function getTxStatus(txId: string) {
  const resp = await axios.post(`${HTTP_ADDRESS}/public/get_transaction`, {transaction_id: txId})
  return resp.data.result;
}
```

A sample response looks like:

```json
{
  "result": {
    "status": "settled",
    "transaction_hash": "0x5ee256e742f6d88366f0bcdd76ce991b8785ddb5533a04994f2fd53c8b6e699e",
    "data": {
      "data": {
        "percent_bid": "1",
        "price_limit": "1144.08",
        "cash_transfer": "1367.85",
        "merge_account": true,
        "last_seen_trade_id": 419716,
        "liquidated_subaccount_id": 78202
      },
      "nonce": 1724715980786660,
      "owner": "0x2225F2B33AA18a48EAbb675f918E950878C53BE6",
      "expiry": 1724716280,
      "module": "0x3e2a570B915fEDAFf6176A261d105A4A68a0EA8D",
      "signer": "0xb94dCcaDf0c72E4A472f6ccf07595Ba27B49e033",
      "signature": "0xd328dfd529b975f74821e518007090015fa7c409fe803377ef496b0f3c305f010538d64e8264b16bc631d143df7f36b28016e58faf00240dac27d04adf75c5c71b",
      "subaccount_id": 36919
    },
    "error_log": {}
  },
  "id": "682f6ea4-a801-4306-bbd5-07fadf983fd6"
}
```

If the `status` field is either `settled` or `reverted`, then it is safe to assume that the transaction has been finalized (succeed or failed, respectively), and the liquidator can react accordingly.

Others statuses can be find in the [reference](https://docs.lyra.finance/reference/post_public-get-transaction), most commonly a transaction will be `pending` for a few seconds before finalizing.

# 5. Get balances and history

Only in the event of a successful `settled` transaction will the `{subaccount_id}.balances` [channel](https://docs.lyra.finance/reference/subaccount_id-balances) publish a balance update with the `update_type` of `liquidator`. This channel is currently the best way to keep balances in sync.

Additionally, a `settled` liquidation transaction will be recorded and will be viewable in the `private/get_liquidator_history` endpoint:

```typescript
// get and log liquidator history
async function getLiquidatorHist(wsc: WebSocket) {
  // avaiable as a regular HTTP call as well
  const rpcId = Math.floor(Math.random() * 10000);
  wsc.on('message', (data: string) => {
    const response = JSON.parse(data);
    if (response.id === rpcId) {
      console.log(`Got liquidator history response with id ${rpcId}:`);
      console.log(JSON.stringify(response, null, 2));
    }
  });

  const params = {
    subaccount_id: subaccountIdLiquidator,
  }

  console.log(`Sending liquidator history request with id ${rpcId}:`);
  console.log(params);

  wsc.send(JSON.stringify({
    method: 'private/get_liquidator_history',
    params: params,
    id: rpcId
  }));
}
```

Sample output :

```json
{
  "id": 4922,
  "result": {
    "bids": [
      {
        "timestamp": 1724715992679793,
        "tx_hash": "0x5ee256e742f6d88366f0bcdd76ce991b8785ddb5533a04994f2fd53c8b6e699e",
        "realized_pnl": "180.03751237036511212",
        "realized_pnl_excl_fees": "180.03751237036511212",
        "discount_pnl": "524.1153982323594391345977783203125",
        "percent_liquidated": "0.173325903015437602",
        "cash_received": "-1043.19768569751330486",
        "amounts_liquidated": {
          "ETH-PERP": "11.26618369600344413",
          "ETH-20241228-2500-C": "-0.173325903015437602",
          "USDC": "1674.313180473687227546"
        },
        "positions_realized_pnl": {
          "ETH-PERP": "180.03751237036511212",
        },
        "positions_realized_pnl_excl_fees": {
          "ETH-PERP": "180.03751237036511212",
        }
      }
 ]}}
```

## Putting it all together

```typescript
import { ethers } from 'ethers';
import axios from 'axios';
import dotenv from 'dotenv';
import { WebSocket } from 'ws';

dotenv.config();

const PRIVATE_KEY = process.env.SESSION_PRIVATE_KEY as string;
const WS_ADDRESS = 'wss://api-demo.lyra.finance/ws';
const PROVIDER_URL = 'https://rpc-prod-testnet-0eakp60405.t.conduit.xyz/';
const HTTP_ADDRESS = 'https://api-demo.lyra.finance';
const ACTION_TYPEHASH = '0x4d7a9f27c403ff9c0f19bce61d76d82f9aa29f8d6d4b0c5474607d9770d1af17';
const DOMAIN_SEPARATOR = '0x9bcf4dc06df5d8bf23af818d5716491b995020f377d3b7b64c29ed14e3dd1105';
const LIQUIDATE_ADDRESS = '0x3e2a570B915fEDAFf6176A261d105A4A68a0EA8D';

const PROVIDER = new ethers.JsonRpcProvider(PROVIDER_URL);
const SIGNER = new ethers.Wallet(PRIVATE_KEY, PROVIDER);

/// if using UI: "Funding Wallet Address" under https://testnet.lyra.finance/settings
const ACCOUNT = '0x2225F2B33AA18a48EAbb675f918E950878C53BE6'

const ENCODER = ethers.AbiCoder.defaultAbiCoder();

const subaccountIdLiquidator = 36919
const AUCTIONS_CHANNEL = "auctions.watch"


async function connectWs(): Promise<WebSocket> {
  return new Promise((resolve, reject) => {
    const ws = new WebSocket(WS_ADDRESS);

    ws.on('open', () => {
      setTimeout(() => resolve(ws), 50);
    });

    ws.on('error', reject);

    ws.on('close', (code: number, reason: Buffer) => {
      if (code && reason.toString()) {
        console.log(`WebSocket closed with code: ${code}`, `Reason: ${reason}`);
      }
    });
  });
};


async function signAuthenticationHeader(): Promise<{[key: string]: string}> {
  const timestamp = Date.now().toString();
  const signature = await SIGNER.signMessage(timestamp);
    return {
      wallet: ACCOUNT,
      timestamp: timestamp,
      signature: signature,
    };
}

async function loginClient(wsc: WebSocket) {
  const rpcId = Math.floor(Math.random() * 10000);
  wsc.on('message', (data: string) => {
    const response = JSON.parse(data);
    if (response.id === rpcId) {
      console.log(`Got login response with id ${rpcId}:`);
      console.log(response);
    }
  });
  const login_request = JSON.stringify({
      method: 'public/login',
      params: await signAuthenticationHeader(),
      id: rpcId
  });
  wsc.send(login_request);
  await new Promise(resolve => setTimeout(resolve, 2000));
}

export interface AuctionDetailsSchema {
  currency: string | null;
  estimated_bid_price: string;
  estimated_discount_pnl: string;
  estimated_mtm: string;
  estimated_percent_bid: string;
  last_seen_trade_id: number;
  margin_type: "PM" | "SM";
  min_cash_transfer: string;
  min_price_limit: string;
  subaccount_balances: {[k: string]: string};  // asset name (as in get_subaccount) -> decimal balance
}

export type State = "ongoing" | "ended";

export interface AuctionResultSchema {
  details: AuctionDetailsSchema | null;
  state: State;
  subaccount_id: number;
  timestamp: number;
}

const AUCTIONS_STATE: {[subaccount_id: number]: AuctionDetailsSchema} = {}

async function subscribeAuctions(): Promise<WebSocket> {
    const wsc = await connectWs();

    wsc.on('message', (data: string) => {
      const response = JSON.parse(data);
      if (response.params?.channel == AUCTIONS_CHANNEL) {
        const data = response.params.data as AuctionResultSchema[];
        for (const auction of data) {
          if (auction.state === 'ongoing') {
            AUCTIONS_STATE[auction.subaccount_id] = auction.details!;
          } else {
            delete AUCTIONS_STATE[auction.subaccount_id];
          }
        }
      }
    })

    const subscribeRequest = JSON.stringify({
        method: 'subscribe',
        params: {
            channels: [AUCTIONS_CHANNEL],
        },
        id: Math.floor(Math.random() * 10000)
    });
    wsc.send(subscribeRequest);
    return wsc;
}

export async function sendSignedLiquidate(
  wsc: WebSocket,
  subaccountId: number,
  liquidatedSubaccountId: number,
  priceLimit: string,
  percentBid: string,
  cashTransfer: string,
  lastSeenTradeId: number,
): Promise<any> {

  const nonce = Number(`${Date.now()}${Math.round(Math.random() * 999)}`)
  const signatureExpirySec = Math.floor(Date.now() / 1000 + 600)

  // struct being signed:
  // struct LiquidationData {
  //   uint liquidatedAccountId;  // subaccount id to liquidate
  //   uint cashTransfer;  // $ transferred into a temporary subaccount from caller's subaccount
  //   uint percentOfAcc;  // % of the liquidatedAccountId to liquidate
  //   int priceLimit;  // max $ to pay for the liquidated portion
  //   uint lastSeenTradeId;  // validation in case the liquidated account changes (e.g. via someone else's bid)
  //   bool mergeAccount;  // whether to merge the temporary subaccount into the caller's subaccount, must be true
  // }

  const liquidateDataABI = ['uint256', 'uint256', 'uint256', 'int256', 'uint256', 'bool'];
  const liquidateData = [
    liquidatedSubaccountId,
    ethers.parseUnits(cashTransfer, 18),
    ethers.parseUnits(percentBid, 18),
    ethers.parseUnits(priceLimit, 18),
    lastSeenTradeId,
    true, // API only supports merging the liquidated portion into caller's subaccount
  ];
  const liquidationData = ENCODER.encode(liquidateDataABI, liquidateData);
  const hashedLiquidationData = ethers.keccak256(liquidationData);

  const actionHash = ethers.keccak256(
    ENCODER.encode(
      ['bytes32', 'uint256', 'uint256', 'address', 'bytes32', 'uint256', 'address', 'address'],
      [
        ACTION_TYPEHASH,
        subaccountId,
        nonce,
        LIQUIDATE_ADDRESS,
        hashedLiquidationData,
        signatureExpirySec,
        ACCOUNT,
        SIGNER.address
      ]
    )
    );

  const signature = SIGNER.signingKey.sign(
    ethers.keccak256(Buffer.concat([
      Buffer.from("1901", "hex"),
      Buffer.from(DOMAIN_SEPARATOR.slice(2), "hex"),
      Buffer.from(actionHash.slice(2), "hex")
    ]))
  ).serialized;

  const rpcId = Math.floor(Math.random() * 10000);
  let liquidateResp = undefined;
  wsc.on('message', (data: string) => {
    const response = JSON.parse(data);
    if (response.id === rpcId) {
      console.log(`Got liquidate response with id ${rpcId}:`);
      console.log(response);
      liquidateResp = response;
    }
  });

  const params = {
    subaccount_id: subaccountId,
    liquidated_subaccount_id: liquidatedSubaccountId,
    cash_transfer: cashTransfer,
    percent_bid: percentBid,
    price_limit: priceLimit,
    last_seen_trade_id: lastSeenTradeId,
    signature: signature,
    signature_expiry_sec: signatureExpirySec,
    nonce: nonce,
    signer: SIGNER.address,
  }

  console.log(`Sending liquidate request with id ${rpcId}:`);
  console.log(params);

  wsc.send(JSON.stringify({
    method: 'private/liquidate',
    params: params,
    id: rpcId
  }));

  for (let i = 0; i < 10; i++) {
    if (liquidateResp) {
      break;
    }
    await new Promise(resolve => setTimeout(resolve, 1000));
  }

  return liquidateResp;
}


async function getLiquidatorHist(wsc: WebSocket) {
  // avaiable as a regular HTTP call as well
  const rpcId = Math.floor(Math.random() * 10000);
  wsc.on('message', (data: string) => {
    const response = JSON.parse(data);
    if (response.id === rpcId) {
      console.log(`Got liquidator history response with id ${rpcId}:`);
      console.log(JSON.stringify(response, null, 2));
    }
  });

  const params = {
    subaccount_id: subaccountIdLiquidator,
  }

  console.log(`Sending liquidator history request with id ${rpcId}:`);
  console.log(params);

  wsc.send(JSON.stringify({
    method: 'private/get_liquidator_history',
    params: params,
    id: rpcId
  }));

  return rpcId;
}

async function getTxStatus(txId: string) {
  const resp = await axios.post(`${HTTP_ADDRESS}/public/get_transaction`, {transaction_id: txId})
  return resp.data.result;
}

async function liquidateTest() {
  const wscSub = await subscribeAuctions();
  await new Promise(resolve => setTimeout(resolve, 5000));
  console.log(AUCTIONS_STATE);
  const wsc = await connectWs();
  await loginClient(wsc);
  let liquidateResp: any = undefined;

  for (const subaccount_id in AUCTIONS_STATE) {
    const auction = AUCTIONS_STATE[subaccount_id];
    if (auction.margin_type !== 'SM') {
      continue; // only liquidate same margin type / currency combination as your subaccount
    }

    console.log(`Liquidating subaccount ${subaccount_id} with auction details:`);
    console.log(auction);

    // add small buffer to price limit and cash transfer in case market moves (here 5% of mtm or $10)
    const buffer = Math.max(Math.abs((+auction.estimated_mtm * 0.05)), 10)

    // cashTransfer has to be strictly greater than 0
    const cashTransfer = (+auction.min_cash_transfer == 0 ? 0.1 : +auction.min_cash_transfer + buffer).toFixed(2);

    liquidateResp = await sendSignedLiquidate(
      wsc,
      subaccountIdLiquidator,
      Number(subaccount_id),
      (+auction.min_price_limit + buffer).toFixed(2),
      '1', // liquidate "up to" 100% of the subaccount (actual % may be less, see auction.estimated_percent_bid)
      cashTransfer,
      auction.last_seen_trade_id,
    );
    break; // for the sake of example liquidate just the first auction
  }

  if (liquidateResp === undefined) {
    console.log('Could not get a liquidation result');
    return;
  }

  for (let i = 0; i < 10; i++) {
    let tx = await getTxStatus(liquidateResp.result.transaction_id);
    if (tx.status === 'settled') {
      console.log('Liquidation successful');
      break;
    }
    else {
      console.log(`Liquidation status: ${tx.status}`);
    }
    await new Promise(resolve => setTimeout(resolve, 1000));
  }

  await getLiquidatorHist(wsc);
}

liquidateTest();
```
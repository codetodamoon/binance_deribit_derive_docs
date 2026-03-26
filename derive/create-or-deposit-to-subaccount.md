# Create or Deposit to Subaccount

**NOTE** If you haven't done so yet, use the `public/create_account` to create an account using an ETH EOA or Smart Contract wallet.

Creating subaccount and depositing cash is a "self-custodial" request, a request that is guaranteed to not be alter-able by anyone except you, which means that some extra signing is required.

Creating/Depositing is almost the same process, and you can create a new account and deposit to it within as a single request.

To create or deposit to a Sub Account:

1. Approve USDC for spending on the deposit contract
2. Create a DepositModuleData object and encode it as bytes
3. Create, hash and sign a SignedAction object using the encoded DepositData
4. Send the request to either `/private/deposit` or `/private/create_subaccount`

For debugging, we have provided routes that return all intermediate values using the `public/create_subaccount_debug` and `public/deposit_debug` routes.

***

> 👍 If you are struggling to encode data correctly, you can use the public/create\_subaccount\_debug endpoint.  The route takes in all raw inputs and returns intermediary outputs shown in the below steps.

## Code Example:

### 1. Approve USDC for spend on the deposit contract

This will allow the Deposit Module in`Matching.sol` to debit the USDC in your wallet.

```typescript
async function approveUSDCForDeposit(wallet: ethers.Wallet, amount: string) {
    const USDCcontract = new ethers.Contract(
      USDC_ADDRESS,
      ["function approve(address _spender, uint256 _value) public returns (bool success)"],
      wallet
    );
    const nonce = await wallet.provider?.getTransactionCount(wallet.address, "pending");
    await USDCcontract.approve(DEPOSIT_MODULE_ADDRESS, ethers.parseUnits(amount, 6), {  
        gasLimit: 1000000,
        nonce: nonce
    });
}
```

**NOTE**: to create an empty SubAccount you can skip this step and use 0 as the deposit amount in the next step

### 2. Encode `DepositModuleData`

In order to ensure that only you can deposit cash, the below creates a `DepositModuleData` object and encodes it as `Bytes` to be used in the `SignedAction` object. `depositAmount` can be zero if you are creating an account.

```typescript
function encodeDepositData(amount: string): Buffer {
    let encoded_data = encoder.encode( // same as "encoded_data" in public/create_subaccount_debug
      ['uint256', 'address', 'address'],
      [
        ethers.parseUnits(amount, 6),
        CASH_ADDRESS,
        STANDARD_RISK_MANAGER_ADDRESS
      ]
    );
    return ethers.keccak256(Buffer.from(encoded_data.slice(2), 'hex')) // same as "encoded_data_hashed" in public/create_subaccount_debug
}
```

### 3. Encode and Sign `SignedAction`

Finally, a `SignedAction` object is created and signed using your private key (or session\_key) which will include the `DepositModuleData` and additional needed info.

> 👍 The `subaccoutId` should be 0 if you are creating a new account.

```typescript
function generateSignature(subaccountId: number, encoded_data_hashed: Buffer, expiry: number, nonce: number): string {
    const action_hash = ethers.keccak256(  // same as "action_hash" in public/create_subaccount_debug
        encoder.encode(
            ['bytes32', 'uint256', 'uint256', 'address', 'bytes32', 'uint256', 'address', 'address'],
            [
                ACTION_TYPEHASH,
                subaccountId,
                nonce,
                DEPOSIT_MODULE_ADDRESS,
                encoded_data_hashed,
                expiry, // must be >5 min from now
                wallet.address,
                wallet.address
            ]
        )
    );

    const typed_data_hash = ethers.keccak256( // same as "typed_data_hash" in public/create_subaccount_debug
        Buffer.concat([
            Buffer.from("1901", "hex"),
            Buffer.from(DOMAIN_SEPARATOR.slice(2), "hex"),
            Buffer.from(action_hash.slice(2), "hex"),
        ])
    );

    return wallet.signingKey.sign(typed_data_hash).serialized  
}
```

### Fully working example with raw data

```typescript
import { ethers, Contract } from "ethers";
import axios from 'axios';
import dotenv from 'dotenv';
import { getUTCEpochSec } from "../utils/timer";

dotenv.config();

// Environment variables, double check these in the docs constants section
const PRIVATE_KEY = process.env.OWNER_PRIVATE_KEY as string;
const PROVIDER_URL = 'https://l2-prod-testnet-0eakp60405.t.conduit.xyz'
const USDC_ADDRESS = '0xe80F2a02398BBf1ab2C9cc52caD1978159c215BD'
const DEPOSIT_MODULE_ADDRESS = '0x43223Db33AdA0575D2E100829543f8B04A37a1ec'
const CASH_ADDRESS = '0x6caf294DaC985ff653d5aE75b4FF8E0A66025928'
const ACTION_TYPEHASH = '0x4d7a9f27c403ff9c0f19bce61d76d82f9aa29f8d6d4b0c5474607d9770d1af17'
const STANDARD_RISK_MANAGER_ADDRESS = '0x28bE681F7bEa6f465cbcA1D25A2125fe7533391C' // Use the "PortfolioManager" address if using PM
const DOMAIN_SEPARATOR = '0x9bcf4dc06df5d8bf23af818d5716491b995020f377d3b7b64c29ed14e3dd1105'

// Ethers setup
const PROVIDER = new ethers.JsonRpcProvider(PROVIDER_URL);
const wallet = new ethers.Wallet(PRIVATE_KEY, PROVIDER);
const encoder = ethers.AbiCoder.defaultAbiCoder();  

const depositAmount = "10000";
const subaccountId = 0; // 0 For a new account

async function approveUSDCForDeposit(wallet: ethers.Wallet, amount: string) {
    const USDCcontract = new ethers.Contract(
      USDC_ADDRESS,
      ["function approve(address _spender, uint256 _value) public returns (bool success)"],
      wallet
    );
    const nonce = await wallet.provider?.getTransactionCount(wallet.address, "pending");
    await USDCcontract.approve(DEPOSIT_MODULE_ADDRESS, ethers.parseUnits(amount, 6), {  
        gasLimit: 1000000,
        nonce: nonce
    });
}

function encodeDepositData(amount: string): Buffer {
    let encoded_data = encoder.encode( // same as "encoded_data" in public/create_subaccount_debug
      ['uint256', 'address', 'address'],
      [
        ethers.parseUnits(amount, 6),
        CASH_ADDRESS,
        STANDARD_RISK_MANAGER_ADDRESS
      ]
    );
    return ethers.keccak256(Buffer.from(encoded_data.slice(2), 'hex')) // same as "encoded_data_hashed" in public/create_subaccount_debug
}

function generateSignature(subaccountId: number, encodedData: Buffer, expiry: number, nonce: number): string {
    const action_hash = ethers.keccak256(  // same as "action_hash" in public/create_subaccount_debug
        encoder.encode(
            ['bytes32', 'uint256', 'uint256', 'address', 'bytes32', 'uint256', 'address', 'address'],
            [
                ACTION_TYPEHASH,
                subaccountId,
                nonce,
                DEPOSIT_MODULE_ADDRESS,
                encodedData,
                expiry,
                wallet.address,
                wallet.address
            ]
        )
    );

    const typed_data_hash = ethers.keccak256( // same as "typed_data_hash" in public/create_subaccount_debug
        Buffer.concat([
            Buffer.from("1901", "hex"),
            Buffer.from(DOMAIN_SEPARATOR.slice(2), "hex"),
            Buffer.from(action_hash.slice(2), "hex"),
        ])
    );

    return wallet.signingKey.sign(typed_data_hash).serialized 
}

async function signAuthenticationHeader() {
    const timestamp = Date.now().toString();
    const signature = await wallet.signMessage(timestamp);
    return {
        "X-LyraWallet": wallet.address,
        "X-LyraTimestamp": timestamp,
        "X-LyraSignature": signature
    };
}

async function createSubaccount() {
    // An action nonce is used to prevent replay attacks
		// LYRA nonce format: ${CURRENT UTC MS +/- 1 day}${RANDOM 3 DIGIT NUMBER}
    const nonce = Number(`${Date.now()}${Math.round(Math.random() * 999)}`);
    const expiry = getUTCEpochSec() + 600; // must be >5 min from now

    const encoded_data_hashed = encodeDepositData(depositAmount); // same as "encoded_data_hashed" in public/create_subaccount_debug
    const depositSignature = generateSignature(subaccountId, encoded_data_hashed, expiry, nonce);
    const authHeader = await signAuthenticationHeader();

    await approveUSDCForDeposit(wallet, depositAmount);

    try {
        const response = await axios.request({
            method: "POST",
            url: "https://api-demo.lyra.finance/private/create_subaccount",
            data: {
                margin_type: "SM",
                wallet: wallet.address,
                signer: wallet.address,
                nonce: nonce,
                amount: depositAmount,
                signature: depositSignature,
                signature_expiry_sec: expiry,
                asset_name: 'USDC',
            },
            headers: authHeader,
        });
    
        console.log(JSON.stringify(response.data, null, '\t'));
    } catch (error) {
        console.error("Error depositing to subaccount:", error);
    }
}

createSubaccount();

```

***

## \[Optional] Get subaccount id

If you created a new subaccount, you can retrieve the new subaccount id once the transaction has settled

```typescript
async get_subaccounts(){
  let timestamp = Date.now() // ensure UTC
  let signature = await wallet.signMessage(timestamp).toString()

  const response = await axios.request<R>({
    "POST",
    "https://api-demo.lyra.finance/private/get_subaccounts",
    {wallet: wallet.address},
    {
      "X-LyraWallet": wallet.address,
      "X-LyraTimestamp": timestamp,
      "X-LyraSignature": signature
    }
  });
}

```

***

# Solidity Objects

### SignedAction Schema

| Param           | Type      | Description                                                                                                                                 |
| --------------- | --------- | :------------------------------------------------------------------------------------------------------------------------------------------ |
| `subaccount_id` | `uint`    | User subaccount id for the action (0 for a new subaccounts when depositing)                                                                 |
| `nonce`         | `uint`    | Unique nonce defined as \<UTC\_timestamp in ms>\<random\_number\_up\_to\_6\_digits> (e.g. 1695836058725001, where 001 is the random number) |
| `module`        | `address` | Deposit module address (see [Protocol Constants](https://docs.derive.xyz/reference/protocol-constants))                                                                   |
| `data`          | `bytes`   | Encoded module data ("DepositData" for deposits/creates)                                                                                    |
| `expiry`        | `uint`    | Signature expiry timestamp in sec                                                                                                           |
| `owner`         | `address` | Wallet address of the account owner                                                                                                         |
| `signer`        | `address` | Either owner wallet or session key                                                                                                          |

### DepositModuleData Schema

| Param                  | Type      | Description                                                                                                                                       |
| ---------------------- | --------- | :------------------------------------------------------------------------------------------------------------------------------------------------ |
| `amount`               | `uint`    | Amount to deposit (Can be 0 to create an empty account)                                                                                           |
| `asset`                | `address` | Address of the asset being deposited. See [Protocol Constants](https://docs.derive.xyz/reference/protocol-constants)                                                            |
| `managerForNewAccount` | `address` | Use the "StandardManager.sol" address if using SM or "PortfolioManager.sol" address if using PM. See [Protocol Constants](https://docs.derive.xyz/reference/protocol-constants) |
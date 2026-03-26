# Authentication

There are two authentication types, both sign-able via the owner or registered session key wallets.

Please refer to our clients for working code:

* Python: [https://github.com/8ball030/derive\_client](https://github.com/8ball030/derive_client)
* Rust: [https://github.com/derivexyz/cockpit](https://github.com/derivexyz/cockpit/tree/master/lyra-client)

# Private Endpoints

All private endpoints and messages starting with `private/` in both REST and Websocket require authentication.

### Scheme

| Param             | Description                                                                                                                                                                                                  |
| ----------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `X-LyraWallet`    | The Derive wallet address (not "owner") of account. This is NOT your original EOA, but the smart contract wallet on the Derive Chain. To find it in the website go to Home -> Developers -> "Derive Wallet". |
| `X-LyraTimestamp` | Current UTC timestamp in ms                                                                                                                                                                                  |
| `X-LyraSignature` | Keccak-256 signature (standard ETH signing) of `X-LyraTimestamp` using`X-LyraWallet` or registered `session_key` private key                                                                                 |

The authentication scheme uses Ethereum signatures to validate that the sender of the request is either the owner of the account or a registered session key.

### REST

Add the authentication scheme as headers into any `private/` REST request:

```typescript

let wallet = new ethers.Wallet(process.env.OWNER_PRIVATE_KEY as string, provider);
let timestamp = Date.now() // ensure UTC
let signature = (await wallet.signMessage(timestamp)).toString()

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
```

### Websocket

Authenticate your websocket session by sending the below `public/login` message:

```typescript
let wallet = new ethers.Wallet(process.env.OWNER_PRIVATE_KEY as string, provider);
let timestamp = Date.now() // ensure UTC
let signature = await wallet.signMessage(timestamp)).toString()

wsc.send(JSON.stringify({
  method: 'public/login',
  params: {
    "wallet": wallet.address,
    "timestamp": timestamp,
    "signature": signature
  },
  id: 1,
}));
```

Private channels require the above login method to be called before they can be subscribed to. Channels are considered private when they reference `{subaccount_id}` or `{wallet}` parameter.

# Self-custodial Requests

Due to the self-custodial nature of the API, the orderbook cannot force the below user actions without an explicit signature by the user:

1. Post Orders
2. Deposit / Withdrawal
3. Transfer

As part of the request, the **owner or a registered session key** must explicitly sign a payload using the private key of the wallet - which is verified via the respective "module" in `Matching.sol`.

Thus, each self-custodial request requires two auth steps:

* pass endpoint authentication (via REST headers or WS login)
* include a sign of the specific payload as one of the API params

**Note** refer to the docs for each request to check the exact param scheme.

> 👍 See [Onboard via Interface](https://docs.derive.xyz/docs/onboard-via-interface) or [Submit Order](https://docs.derive.xyz/docs/submit-order) guides for example "self-custodial" requests.

# Session Keys

Session keys can be used to sign private requests instead of the owner wallet. See the [Session Keys ](https://v2-docs.lyra.finance/reference/session-keys)section
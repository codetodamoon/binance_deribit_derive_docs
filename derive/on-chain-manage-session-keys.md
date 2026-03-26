# Manage Session Keys

Refer to the "Session Keys" section in the "API Reference" for more information on the nature of session keys.

# Register via API

To avoid the overhead of managing a connection to the Derive Rollup via RPC, you can submit session key registration and deregistration directly through the orderbook. Transaction cost is still paid by the signer.

***NOTE*** Need to wait up to a minute after tx submission for the API to allow session key usage.

```typescript
let wallet = new ethers.Wallet(process.env.OWNER_PRIVATE_KEY as string, provider);
let newSessionKey = ethers.Wallet.createRandom();
let expirySec = Date.now() / 1000 + 3600 // valid for 1hr

// get API to build unsigned tx
const buildTxResponse = await axios.request<R>({
  "POST"
  "https://api-demo.lyra.finance/public/build_register_session_key_tx,
  {
    wallet: wallet.address,
    public_session_key: newSessionKey.address,
    expiry_sec: expirySec, 
    nonce: await wallet.getNonce(),
    gas: ethers.toBigInt(5000000),
	}
});

// API submits tx on-chain
const registerResponse = await axios.request<R>({
  "POST",
  "https://api-demo.lyra.finance/public/register_session_key,
  {
    wallet: wallet.address,
    label: 'my_label',
    public_session_key: newSessionKey.address,
    expiry_sec: expirySec, // 1 hour
    signed_raw_tx: await wallet.signTransaction(
      buildSessionKeyTxResponse.data.result.tx_params
    );
	};
});
```

The same flow can be used with the `public/deregister_session_key` endpoint to delete a session key.

# Register Directly On-chain

```typescript
let wallet = new ethers.Wallet(process.env.OWNER_PRIVATE_KEY as string, provider);
let newSessionKey = ethers.Wallet.createRandom();
let expirySec = Date.now() / 1000 + 3600 // valid for 1hr
let matchingABI = ["function registerSessionKey(address toAllow, uint256 expiry)"] 

const matchingContract = new ethers.Contract(
  process.env.MATCHING_ADDRESS,
  matchingABI,
  provider
)

let tx = await matching.connect(wallet).registerSessionKey.(
  newSessionKey.address, expirySec
)
```

# Deregister Directly On-chain

Once the transaction is submitted, the session key becomes unusable after a **10 minute cooldown period**

```typescript
let wallet = new ethers.Wallet(process.env.OWNER_PRIVATE_KEY as string, provider);
let newSessionKey = ethers.Wallet.createRandom();
let expirySec = Date.now() / 1000 + 3600 // valid for 1hr
let matchingABI = ["function deregisterSessionKey(address sessionKey)"] 

const matchingContract = new ethers.Contract(
  process.env.MATCHING_ADDRESS,
  matchingABI,
  provider
)

let tx = await matching.connect(wallet).deregisterSessionKey.(
  newSessionKey.address
)
```
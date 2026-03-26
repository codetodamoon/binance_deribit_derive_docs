# Deposit to Derive Chain

The easiest way to deposit to Derive Chain is by setting up an account via the Interface (see the guides under "Onboard via Interface) as this fully handles bridging / deposits / account setups / withdrawals. **Only use this guide if you'd like to setup your account fully on-chain.**

# Testnet

When onboarding fully on-chain in testnet you will need to reach out via our Discord `v2-support` channel . Note, onboarding via Interface will not require this step as you can mint USDC directly to testnet accounts via the interface.

# Mainnet

Derive Chain does not have a generic bridging interface yet. To deposit ETH and USDC to Derive Chain, you'll need to use etherscan.

> ❗️ **Do not deposit to Derive Chain unless you know what you are doing.** Derive uses multiple different bridges for different assets. If you are not sure, ask via the Discord `v2-support` channel. Funds can be lost if done incorrectly.

> 🚧 The [exchange frontend](https://www.lyra.finance/) uses smart contract wallets, and isn't suitable for a programmatic account setup that uses a regular EOA and private key.

## Depositing collateral assets (USDC, WETH, WBTC, etc.)

**To deposit collateral assets to Derive Chain, use the socket bridges.** Socket bridges enable fast withdrawals from the Derive chain within certain daily limits. Depending on the asset, deposits and withdrawals are available between Ethereum L1, Optimism and Arbitrum.

Derive uses a custom bridge that uses [Socket](https://socket.tech/) smart contracts and L1-L2 messaging infrastructure. This enables a fast bridge has the option for fast withdrawals that aren't subject to the 7 day challenge period.

For a full list of tokens, bridges and connectors, [see the socket github.](https://github.com/SocketDotTech/socket-plugs/blob/main/deployments/superbridge/prod_lyra_addresses.json) This has been tabulated below:

| Currency | Source chain | Token Address                              | Bridge address                             |
| :------- | :----------- | :----------------------------------------- | :----------------------------------------- |
| USDC     | Eth Mainnet  | 0xA0b86991c6218b36c1d19D4a2e9Eb0cE3606eB48 | 0x6D303CEE7959f814042D31E0624fB88Ec6fbcC1d |
| USDC.e   | Optimism     | 0x7F5c764cBc14f9669B88837ca1490cCa17c31607 | 0xBb9CF28Bc1B41c5c7c76Ee1B2722C33eBB8fbD8C |
| USDC.e   | Arbitrum     | 0xFF970A61A04b1cA14834A43f5dE4533eBDDB5CC8 | 0xFB7B06538d837e4212D72E2A38e6c074F9076E0B |
| USDC     | Optimism     | 0x0b2C639c533813f4Aa9D7837CAf62653d097Ff85 | 0xDEf0bfBdf7530C75AB3C73f8d2F64d9eaA7aA98e |
| USDC     | Arbitrum     | 0xaf88d065e77c8cC2239327C5EDb3A432268e5831 | 0x5e027ad442e031424b5a2C0ad6f656662Be32882 |
| WETH     | Eth Mainnet  | 0xc02aaa39b223fe8d0a0e5c4f27ead9083c756cc2 | 0xD4efe33C66B8CdE33B8896a2126E41e5dB571b7e |
| WETH     | Optimism     | 0x4200000000000000000000000000000000000006 | 0xdD4c717a69763176d8B7A687728e228597eAB86d |
| WETH     | Arbitrum     | 0x82af49447d8a07e3bd95bd0d56f35241523fbab1 | 0x8e9f58E6c206CB9C98aBb9F235E0f02D65dFc922 |
| WBTC     | Eth Mainnet  | 0x3Eec7c855aF33280F1eD38b93059F5aa5862E3ab | 0x3Eec7c855aF33280F1eD38b93059F5aa5862E3ab |
| WBTC     | Optimism     | 0x68f180fcce6836688e9084f035309e29bf0a2095 | 0xE5967877065f111a556850d8f05b8DaD88edCEc9 |
| WBTC     | Arbitrum     | 0x2f2a2543b76a4166549f7aab2e75bef0aefc5b0f | 0x3D20c6A2b719129af175E0ff7B1875DEb360896f |

Each of the bridges have their own limits for deposits and withdrawals. For example, the USDC mainnet fast connector is subject to the following global daily limits:

* $10m USDC in deposits
* $1m USDC in withdrawals

Here is the contract interface you can use to deposit USDC via the Socket bridge: [https://etherscan.io/address/0x6d303cee7959f814042d31e0624fb88ec6fbcc1d#writeContract](https://etherscan.io/address/0x6d303cee7959f814042d31e0624fb88ec6fbcc1d#writeContract)

Here is a sample USDC deposit via the Socket bridge: [https://etherscan.io/tx/0x69272bbed41fd09f4b50bba6e0e451cc57a19fe81db41ac7819e003cb3088a00](https://etherscan.io/tx/0x69272bbed41fd09f4b50bba6e0e451cc57a19fe81db41ac7819e003cb3088a00)

## Depositing ETH

ETH on the Derive chain is needed for certain transactions that require direct smart contract interaction. If you are manually setting up your account, you will need some small amount of eth. **Do not use this method for wETH to be used as collateral in the protocol.**

You may be able to bridge using the interface provided by the superbridge team here: [https://app.rollbridge.app/lyra](https://app.rollbridge.app/lyra)

Alternatively, to manually deposit ETH to Derive Chain, use the standard bridge. The native bridge is[OP Stack's](https://stack.optimism.io/) native bridge for deposits and withdrawals.

Deposits and withdrawals are subject to the following delays:

* Deposits confirmed in 5-10 minutes
* Withdrawals confirmed in 7 days, after the [challenge period](https://docs.optimism.io/builders/dapp-developers/bridging/messaging#understanding-the-challenge-period)

Here is the contract interface you can use to deposit via the native bridge: [https://etherscan.io/address/0x61e44dc0dae6888b5a301887732217d5725b0bff#writeProxyContract](https://etherscan.io/address/0x61e44dc0dae6888b5a301887732217d5725b0bff#writeProxyContract)

Here is a sample ETH deposit via the native bridge: [https://etherscan.io/tx/0x1c6b7bb4e060d2e335dfc1b3501d9e778cec1adac80652645f645a6d79daf159](https://etherscan.io/tx/0x1c6b7bb4e060d2e335dfc1b3501d9e778cec1adac80652645f645a6d79daf159)
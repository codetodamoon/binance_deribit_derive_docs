# Introduction

Derive is a self-custodial, high performance crypto trading platform for options, perpetuals and spot trading.

The trading platform is made up of three components:

1. **Derive Chain:** A settlement layer for transactions. This is an Optimistic rollup built on the [OP Stack](https://stack.optimism.io/), secured by Ethereum mainnet. Governed by the [Derive DAO](https://gov.lyra.finance/).
2. **Derive Protocol:** A settlement protocol that enables permissionless, self-custodial margin trading of perpetuals, options and spot, deployed to the Derive Chain (formerly Lyra Chain). Governed by the [Derive DAO](https://gov.lyra.finance/).
3. **Derive Exchange:** An orderbook that efficiently matches orders and settles them to the Derive Protocol. Operated by Lyra Trading Co.

The following docs describe Derive technical concepts relating to the Protocol and Exchange. For a deep dive into the Derive Chain, consult the [OP Stack Docs](https://stack.optimism.io/). For a deep dive into the Derive DAO and Derive's governance framework, consult the [Governance Docs](https://gov.lyra.finance). Note that Derive operates independently of the Lyra V1 AMM.

> 📘 The Derive Exchange uses a centralized limit order book, but remains self-custodial, and settles trades and liquidations in a trustless manner.

# Clients

* Python (community client): [https://github.com/8ball030/derive\_client](https://github.com/8ball030/derive_client)
* Python on-chain signing SDK (used above): [https://github.com/derivexyz/v2-action-signing-python](https://github.com/derivexyz/v2-action-signing-python)
* Rust (market making algos and example client): [https://github.com/derivexyz/cockpit](https://github.com/derivexyz/cockpit/tree/master/lyra-client)

<br />

# Useful integrations

* hummingbot integration: [https://hummingbot.org/exchanges/derive/](https://hummingbot.org/exchanges/derive/)
* ccxt integration: [https://github.com/ccxt/ccxt/blob/master/python/ccxt/derive.py](https://github.com/ccxt/ccxt/blob/master/python/ccxt/derive.py)

<br />

# Documentation

Visit [Documentation](https://docs.derive.xyz/docs/introduction) for Onboarding Guides and a deep dive into the Derive Protocol's standard margin and portfolio margin rules,  as well as an overview of supported products, liquidations and oracles:

* [Interface vs Manual Onboarding](https://docs.derive.xyz/docs/introduction-1)
* [Supported Products](https://docs.derive.xyz/docs/supported-products)
* [Standard Margin](https://docs.derive.xyz/docs/standard-margin)
* [Portfolio Margin](https://docs.derive.xyz/docs/portfolio-margin)
* [Liquidations](https://docs.derive.xyz/docs/liquidations)
* [Oracles](https://docs.derive.xyz/docs/oracles)

# Derive API

The Derive API provides access to the Derive Exchange orderbook which matches orders and settles trades. Derive provides two interfaces to access the API:

### Mainnet

* [JSON-RPC over HTTP](https://docs.derive.xyz/reference/public) at [https://api.lyra.finance](https://api.lyra.finance)
* [JSON-RPC over Websocket](https://docs.derive.xyz/reference/subscribe) at [wss://api.lyra.finance/ws](wss://api.lyra.finance/ws)

### Testnet

* [JSON-RPC over HTTP](https://docs.derive.xyz/reference/public) at [https://api-demo.lyra.finance](https://api-demo.lyra.finance)
* [JSON-RPC over Websocket](https://docs.derive.xyz/reference/subscribe) at [wss://api-demo.lyra.finance/ws](wss://api-demo.lyra.finance/ws)

The API v2.0-alpha is available in our testing environment which settles to Derive Chain / Protocol testnet. All examples in this documentation refer to the test environment.
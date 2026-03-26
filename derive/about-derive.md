# About Derive

Derive is a self-custodial, high performance cryptocurrency trading platform for options, perpetuals and spot trading. The platform is made up of three components:

1. **Derive Chain:** A settlement layer for transactions. This is an Optimistic rollup built on the [OP Stack](https://stack.optimism.io/), secured by Ethereum mainnet. Governed by the [Derive DAO](https://docs.lyra.finance/docs/dao-governance-overview).
2. **Derive Protocol:** A settlement protocol that enables permissionless, self-custodial margin trading of perpetuals, options and spot, deployed to the Derive Chain. Governed by the [Derive DAO](https://docs.lyra.finance/docs/dao-governance-overview).
3. **Derive Exchange:** An orderbook that efficiently matches orders and settles them to the Derive Protocol. Operated by Derive Trading Co.

For a deep dive into the Derive Chain, consult the [OP Stack Docs](https://stack.optimism.io/).

All components are governed by the [Derive DAO](https://docs.lyra.finance/docs/dao-governance-overview) via a fully on-chain and autonomous system. This documentation describes the chain, protocol and exchange. You can read it in its entirety or skip to whichever section is relevant to you.

> 📘 The Derive Exchange uses a centralized limit order book, but remains self-custodial, and settles trades and liquidations in a trustless manner.
# Overview

The Derive Protocol is a collection of smart contracts that collectively create a decentralised and self-custodial derivatives protocol.

The protocol is comprised of three main components:

* **Accounts**: user held ERC-721s that house their assets (including cash, derivatives and base assets). All accounts must subscribe to a manager.
* **Risk Managers**: These govern the margin requirements of subscribed accounts. If an account falls beneath its mandated margin requirements, the manager is responsible for liquidating said account.
* **Assets**: These are contracts that specify the properties of various assets and derivatives (say, options and perpetuals).

There also exists a Security Module that houses reserved funds which are used to pay off insolvent debt in the event of trader bankruptcy. In return for backstopping the system, the protocol, via the managers, charges fees to traders which are used to grow the Security Module.

All margin calculations are performed on chain in a trustless manner. The parameters used in these calculations are set via governance. For more detail, see the [Parameters](https://docs.derive.xyz/docs/parameters) section.

The smart contracts have been audited by Tier 1 firm [Sigma Prime](https://sigmaprime.io/). View the audit reports [here](https://github.com/sigp/public-audits/tree/master/reports/derive).
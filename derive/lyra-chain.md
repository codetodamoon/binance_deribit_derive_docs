# Derive Chain

Derive Chain is an Ethereum rollup built using the OP stack and is the home of the Derive Protocol. It is a permissionless smart contract platform that boosts the performance of Ethereum whilst inheriting its security.

#### Accessing Derive Chain

| Name         | Value                 | Notes                                                     |
| :----------- | :-------------------- | :-------------------------------------------------------- |
| RPC URL      | rpc.lyra.finance      | Remote Procedure Call for interacting with Derive Chain   |
| Explorer URL | explorer.lyra.finance | Graphical interface for transactions, blocks and accounts |
| Chain ID     | 957                   | Unique identifier for Derive Chain                        |

#### Deployer Whitelist

To safeguard the security and integrity of the Derive Chain, a deployer whitelist system is enforced through a vetting process for contract deployer addresses. This whitelist, integrated within the chain sequencer, ensures that only pre-approved addresses can deploy contracts. This effectively blocks any attempts to deploy contracts by unauthorized or unknown addresses.

#### Getting Added to the Deployer Whitelist

Entities seeking to deploy contracts on the Derive Chain must undergo an approval process by the Derive DAO. Applications submitted for vetting are reviewed for their security measures, the purpose of the contract, and overall impact on the network.

To submit an application, the following steps must be completed:

1. Submit a forum post to [https://forums.lyra.finance/](https://forums.lyra.finance/) stating your case and listing all addresses required to be whitelisted
2. After a few days of comments, initiate a snapshot vote

Once an application is approved, the deploying entity's address is added to the sequencer by Conduit, granting them permission to deploy contracts.
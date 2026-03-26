# Governance

Derive governance empowers DRV token holders to actively participate in the decision-making process of the Derive DAO.

# Overview

**Token rights**

By staking DRV tokens to receive stDRV, holders obtain governance powers proportional to their balance. There are initially two powers associated with the stDRV token:

1. Proposal right: Allows the creation of a new proposal.
2. Voting right: Enables voting on existing proposals.

**Delegation**\
Holders can choose to delegate one or both of their governance powers. Delegation helps Derive Governance retain the benefits of a council-based model, where less informed participants can delegate their voting power to more knowledgeable members without introducing the implementation risks associated with off-chain models.

**LEAPs**\
A LEAP is any proposal that impacts the protocol, treasury or governance framework. It has been adapted from industry standard frameworks, such as EIP and BIP. Examples may include protocol changes (listing new markets and changing parameters), treasury actions (allocating DRV tokens and remunerating service providers) as well as meta-governance updates (changing the governance process itself).

Each LEAP must include the following elements:

* *Simple Summary*: A clear and accessible explanation of the LEAP for a layperson.
* *Abstract*: A brief description of the LEAP.
* *Motivation*: A justification for the LEAP, explaining why the current system is inadequate to address the problem.
* *Specification*: A detailed outline of the proposal's process and structure.
* *Rationale*: An explanation of design decisions made, alternate designs considered, and related work.
* *Test Cases*: Test cases are required for code-related proposals (excluding treasury actions).
* *Copyright Waiver*: All LEAPs must be in the public domain.

# Architecture

<Image align="center" src="https://files.readme.io/3e68cd97ba1e0c461a165ae82cbae559230d66f89d51fec801fd1141e3c61152-TGE.png" />

# Governance Contract

The governance contract is to facilitate decentralized decision-making and enable the Derive community to have a say in the development and management of the protocol.Some key features and purposes of the governance contract include:

1. Proposal Creation: It allows users to create proposals for changes to the protocol.
2. Voting: Token holders with sufficient voting power can vote on the proposals submitted. The governance contract takes into account the voting power of each participant, ensuring that the decision-making process is fair and transparent.
3. Proposal Execution: If a proposal receives enough support (based on the quorum and majority thresholds), the governance contract executes the proposed changes, updating the protocol as needed.
4. Delegation: Token holders can delegate their voting power to other users or entities, allowing them to participate in the governance process on their behalf.

# Governance Strategy

* Contains logic to measure users' relative power to propose and vote.
* A user's power is calculated as their balance of stDRV including any token power delegated to them and excluding any token power delegated to another address.

# Timelocks

* The short time lock executor can execute proposals that changes part of the Derive protocol or any treasury proposals.
* The long time lock executor can execute proposals that handles execution of stDRV and meta governance proposals.
* The Optimism Executor is able to receive cross-chain transactions originating from the short time lock executor and queue proposals to be executed on Optimism.
* The Arbitrum Executor is able to receive cross-chain transactions originating from the short time lock executor and queue proposals to be executed on Arbitrum.

# Actors

* Proposers: Proposers are individuals or entities who create and submit proposals for changes or improvements to the protocol. These proposals can include updates to smart contracts, adjustments to key parameters, or new features that can enhance the protocol's functionality.
* Voters: Voters are stDRV token holders who participate in the governance process by voting on submitted proposals. They have the power to approve or reject changes to the protocol based on their assessment of the proposal's merits and potential impact on the ecosystem. Voters can either vote directly, using their own tokens, or delegate their voting power to other users who can vote on their behalf.

# Process

<Image align="center" src="https://files.readme.io/67b9c9a936a1bab628ee872926b30f260d6b7353a5e42e98d70db580c3e8aaa3-TGE_1.png" />

The process follows these main steps:

1. **Creation**\
   Anyone can write a Derive Request For Comment (LRFC) using the template available on the governance forums.
2. **Community**\
   The LRFC is posted for community review and discussion. The authors should incorporate feedback and seek rough consensus.
3. **Snapshot vote**\
   A snapshot shot vote is recommended to assess the sentiment before progressing to the slower, more expensive on-chain voting process. Any community member with more than 10,000 stDRV can create the snapshot vote. This requirement increases the cost of attacking the snapshot voting system. For proposals that don’t require an on-chain smart contract call, notably changes to the trading and liquidity rewards formulas, the snapshot vote is considered the binding and final vote.
4. **On-chain vote**\
   The LEAP is submitted to the Derive Governance system for stDRV holders to vote on.
5. **Execution**\
   If approved, the LEAP's payload is sent to the relevant timelock for automatic execution. If rejected, no further action is taken.

# Technical

**Deployed Contracts**

| Contract                 | Mainnet                                    | Goerli                                     |
| :----------------------- | :----------------------------------------- | :----------------------------------------- |
| Derive                   | 0x01BA67AAC7f75f647D94220Cc98FB30FCc5105Bf | 0x81dC2c079057Ec6Ce15F0f83F8209A529f4d1D0c |
| stDRV                    | 0xCb9f85730f57732fc899fb158164b9Ed60c77D49 | 0xd7947f2651304DA4cC360b9E81b453f26FAaD4B1 |
| DeriveGovernanceStrategy | 0x110597Da9Aad8EC9Fff32341F986bE70dC9A6811 | 0x803FFD0f5Dbb11B3582523f3Fec69619E05eBD3e |
| DeriveGovernanceV2       | 0xe8642cc1249F08756e70Bb8eb4BE0e6c09254fed | 0xD5BB4Cd3dbD5164eE5575FBB23542b120a52BdB8 |
| Executor (Short)         | 0xEE86E99b42981623236824D33b4235833Afd8044 | 0xb6f416a47cACb1583903ae6861D023FcBF3Be7b6 |
| Executor (Long)          | 0x394169592B06cAe64ed48316a79f9Bcf1330AF99 | N/A                                        |

**Audits**\
All contracts besides Derive and the DeriveGovernanceStrategy are unchanged versions of the AAVE governance V2 contracts, which can be found here. DRV is a deployment of the openzeppelin standard ERC20 contract. DeriveGovernanceStrategy was checked by IOSIRO in the following report (along with other future governance contracts):

[IOSIRO Audit](https://files.gitbook.com/v0/b/gitbook-x-prod.appspot.com/o/spaces%2Fw7xm8Ah9WBwUN9adk3qE%2Fuploads%2F2AdEhZXRQCoi4XcN1xFi%2Fiosiro_lyra-gov_2023-03-10.pdf?alt=media\&token=fd4d8592-6773-497a-b2d7-f93163493bc8)

**Voting Process**

1. **Creation**\
   A proposal can be created by calling the create() function on the DeriveGovernanceV2 contract. The caller must have a proposer power greater than the threshold and they must maintain it until the proposal is executed. After the proposal is created it assumes the PENDING state until the vote begins.
2. **Voting**\
   Voting begins after the votingDelay has elapsed, a value which can be read from getVotingDelay(). At this point, a snapshot of voting powers is taken and can no longer be delegated/transferred for the proposal being voted on. The proposal state is ACTIVE and users can submit a vote for or against the proposal, weighted by the user's total voting power (tokens + delegated voting power), within the allotted ING\_DURATION].
   3..
3. **End of Voting**\
   For a proposal to pass the voting power of for-votes needs to reach the quorum set by the IMUM\_QUORUM] par parameter. In addition, the difference between for-votes and against-votes (in % of total voting power) needs to exceed the vote differential threshold set by the E\_DIFFERENTIAL] par parameter. If the proposal has passed, then the proposal state becomes SUCCEEDED, otherwise, it is FAILED.
4. **Queuing and execution**\
   A SUCCEEDED proposal can be queued and will be executed after the execution delay and before the grace period expiration. The delay can be fetched from getDelay() and the GRACE\_PERIOD can be fetched from GRACE\_PERIOD().\
   The validation and execution of the proposal are performed by the time lock executor. A queued proposal state is QUEUED. A successfully executed proposal state is EXECUTED. If a queued proposal has not been executed before expiration, then the proposal state is EXPIRED.
5. **Proposal Canceling**\
   If the proposal creator's proposal power decrease and no longer meet the PROPOSITION\_THRESHOLD, any user can cancel the proposal. In addition as an initial safeguard to the protocol, the guardian account, controlled by a community multisig, is able to cancel a proposal before a proposal is executed. A cancelled proposal state is CANCELED.
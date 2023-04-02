# Compound Governance Audit

## 1.1 Overview 

The Compound Finance is a decentralized finance (DeFi) platform that allows users to lend and borrow cryptocurrencies in a trustless and transparent manner using smart contracts on the Ethereum blockchain. The platform's native token is called COMP, which allows holders to participate in the governance of the platform and earn rewards for using the protocol.

The core functionality of the platform revolves around the creation of markets for various cryptocurrencies, which users can then supply as collateral to borrow other cryptocurrencies or earn interest on their deposits. Interest rates are algorithmically determined based on supply and demand for each asset, and borrowers are subject to liquidation if the value of their collateral falls below a certain threshold.

In addition to lending and borrowing, Compound also offers users the ability to trade tokens using a built-in decentralized exchange (DEX) called Compound Swap. The platform has also expanded to support other blockchain networks, such as Polygon and Binance Smart Chain, to reduce the high gas fees and slow transaction speeds associated with using Ethereum.

### 1.1.1 Objectives

1. Study the pattern used in writing their smart contracts
2. Find potential vulnerability in the smart contract

### 1.1.2 Scope 

The following files were studied:
- [contracts/Governance/GovernorBravoDelegateG2.sol](https://github.com/compound-finance/compound-protocol/blob/master/contracts/Governance/GovernorBravoDelegateG2.sol)

## 2.1 Compound Governance

Compound Governance is a system within the Compound Finance platform that allows COMP token holders to participate in the decision-making process of the protocol. The governance system is designed to be decentralized, transparent, and community-driven, meaning that any COMP holder can propose and vote on changes to the platform.

The following is a detailed explanation of how the Compound Governance system operates:

1. Proposals: Any COMP holder can submit a proposal to change any aspect of the Compound protocol. Proposals can range from simple parameter adjustments, such as interest rates or collateral ratios, to more complex changes, such as adding support for new assets or upgrading the protocol's infrastructure. Proposals are submitted on-chain using the Compound Governance Interface, a user interface that allows users to interact with the governance system.

2. Discussion: Once a proposal is submitted, it becomes open for discussion among the community. Users can discuss the proposal on social media channels or on forums like the Compound Discourse. The community can also ask questions to the proposer and express their opinions on the proposal.

3. Voting: After a discussion period, the proposal enters the voting phase. All COMP token holders can vote on the proposal using their tokens. Each COMP token represents one vote, and users can vote either for or against the proposal. Users can also delegate their voting power to another user or a third-party entity, such as a Compound delegate, who can vote on their behalf.

4. Quorum and Thresholds: For a proposal to pass, it must meet two requirements: a quorum and a threshold. The quorum is the minimum number of votes required for a proposal to be considered valid. The quorum is set at 4% of the total supply of COMP tokens. The threshold is the minimum number of votes required for a proposal to pass. The threshold is set at 40% of the total supply of COMP tokens.

5. Execution: If a proposal meets the quorum and threshold requirements, it is considered successful, and the changes proposed in the proposal are executed on the protocol. The execution of the proposal is done automatically using smart contracts on the Ethereum blockchain.

6. Implementation: After a successful proposal, the changes are implemented, and the protocol is updated accordingly. Users can track the changes using the Compound Governance Interface or other blockchain explorers.

# Design Patterns

### 3.1 Abstraction of Structs and Events to Interface

The definitions of structs and events are declared in the [GovernorBravoInterfaces](https://github.com/compound-finance/compound-protocol/blob/master/contracts/Governance/GovernorBravoInterfaces.sol) and are simply used in the smart contract

### 3.2 
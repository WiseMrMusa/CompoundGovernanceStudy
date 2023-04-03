# Compound Governance Audit

## 1 Overview 

The Compound Finance is a decentralized finance (DeFi) platform that allows users to lend and borrow cryptocurrencies in a trustless and transparent manner using smart contracts on the Ethereum blockchain. The platform's native token is called COMP, which allows holders to participate in the governance of the platform and earn rewards for using the protocol.

The core functionality of the platform revolves around the creation of markets for various cryptocurrencies, which users can then supply as collateral to borrow other cryptocurrencies or earn interest on their deposits. Interest rates are algorithmically determined based on supply and demand for each asset, and borrowers are subject to liquidation if the value of their collateral falls below a certain threshold.

In addition to lending and borrowing, Compound also offers users the ability to trade tokens using a built-in decentralized exchange (DEX) called Compound Swap. The platform has also expanded to support other blockchain networks, such as Polygon and Binance Smart Chain, to reduce the high gas fees and slow transaction speeds associated with using Ethereum.

### 1.1.1 Objectives

1. Study the pattern used in writing their smart contracts
2. Find potential vulnerability in the smart contract

### 1.1.2 Scope 

The following files were studied:
- [contracts/Governance/Comp.sol](https://github.com/compound-finance/compound-protocol/blob/master/contracts/Governance/Comp.sol)
- [contracts/Governance/GovernorAlpha.sol](https://github.com/compound-finance/compound-protocol/blob/master/contracts/Governance/GovernorAlpha.sol)
- [contracts/Governance/GovernorBravoDelegate.sol](https://github.com/compound-finance/compound-protocol/blob/master/contracts/Governance/GovernorBravoDelegate.sol)
- [contracts/Governance/GovernorBravoDelegateG1.sol](https://github.com/compound-finance/compound-protocol/blob/master/contracts/Governance/GovernorBravoDelegateG1.sol)
- [contracts/Governance/GovernorBravoDelegateG2.sol](https://github.com/compound-finance/compound-protocol/blob/master/contracts/Governance/GovernorBravoDelegateG2.sol)
- [contracts/Governance/GovernorBravoInterfaces.sol](https://github.com/compound-finance/compound-protocol/blob/master/contracts/Governance/GovernorBravoInterfaces.sol)

## 2 Compound Governance

Compound Governance is a system within the Compound Finance platform that allows COMP token holders to participate in the decision-making process of the protocol. The governance system is designed to be decentralized, transparent, and community-driven, meaning that any COMP holder can propose and vote on changes to the platform.

![Gov Diagram](https://github.com/WiseMrMusa/CompoundGovernanceStudy/blob/main/gov_diagram.png)

The following is a detailed explanation of how the Compound Governance system operates:

1. COMP: This is an ERC20 Token that determines a user's voting right. 

2. Delegation: Before someone can vote, Their voting right has to be delegated to either their own address or another address.

3. Proposals: Any COMP holder can submit a proposal to change any aspect of the Compound protocol. Proposals can range from simple parameter adjustments, such as interest rates or collateral ratios, to more complex changes, such as adding support for new assets or upgrading the protocol's infrastructure. Proposals are submitted on-chain using the Compound Governance Interface, a user interface that allows users to interact with the governance system. A proposal is executable code that modifies the protocol and how it works. In order to create a proposal, a user must have at least 60,000 COMP delegated to their address. Proposals are stored in the “proposals” mapping of the Governance smart contract. All proposals are subject to a 3-day voting period. If the proposer does not maintain their vote weight balance throughout the voting period, the proposal may be canceled by anyone.

4. Discussion: Once a proposal is submitted, it becomes open for discussion among the community. Users can discuss the proposal on social media channels or on forums like the Compound Discourse. The community can also ask questions to the proposer and express their opinions on the proposal.

5. Voting: After a discussion period, the proposal enters the voting phase. All COMP token holders can vote on the proposal using their tokens. Each COMP token represents one vote, and users can vote either for or against the proposal. Users can also delegate their voting power to another user or a third-party entity, such as a Compound delegate, who can vote on their behalf.

6. Quorum and Thresholds: For a proposal to pass, it must meet two requirements: a quorum and a threshold. The quorum is the minimum number of votes required for a proposal to be considered valid. The quorum is set at 4% of the total supply of COMP tokens. The threshold is the minimum number of votes required for a proposal to pass. The threshold is set at 40% of the total supply of COMP tokens.

7. Execution: If a proposal meets the quorum and threshold requirements, it is considered successful, and the changes proposed in the proposal are executed on the protocol. The execution of the proposal is done automatically using smart contracts on the Ethereum blockchain.

8. Implementation: After a successful proposal, the changes are implemented, and the protocol is updated accordingly. Users can track the changes using the Compound Governance Interface or other blockchain explorers.

## 3 Contracts

### 3.5 Governance/GovernorBravoDelegateG2

#### 3.5.1 Constants

1. *Proposal Threshold:* 
    Proposal Threshold is between 50,000 Comp and 100,000 Comp

        uint public constant MIN_PROPOSAL_THRESHOLD = 50000e18; // 50,000 Comp
        uint public constant MAX_PROPOSAL_THRESHOLD = 100000e18; //100,000 Comp
2. *Quorum:*
    The minimum no of votes for a proposal

        uint public constant quorumVotes = 400000e18; // 400,000 = 4% of Comp

3. *Voting Period:*
    Voting period is between 1 Day and 2 Weeks

        uint public constant MIN_VOTING_PERIOD = 5760; // About 24 hours
        uint public constant MAX_VOTING_PERIOD = 80640; // About 2 weeks

4. *Voting Delay:*
    Voting delay is between the next block and about a week after the proposal

        uint public constant MIN_VOTING_DELAY = 1;
        uint public constant MAX_VOTING_DELAY = 40320; // About 1 week

5. *Type Hashes:*
    The domain and Ballot Hashes

        bytes32 public constant DOMAIN_TYPEHASH = keccak256("EIP712Domain(string name,uint256 chainId,address verifyingContract)");
        bytes32 public constant BALLOT_TYPEHASH = keccak256("Ballot(uint256 proposalId,uint8 support)");

#### 3.5.2 External Functions

1. *Initialize:*
    This is used to instantiate the state variables of the GovernorBravoDelegate which includes: 
    1. The timelock
    2. The comp token
    3. The voting delay,
    3. The voting period, and
    4. The proposal threshold

    It can only be instantiated once by the admin and the timelock address cannot be `address(0)` 

    function initialize(address timelock_, address comp_, uint votingPeriod_, uint votingDelay_, uint proposalThreshold_) public {
        require(address(timelock) == address(0), "GovernorBravo::initialize: can only initialize once");
        require(msg.sender == admin, "GovernorBravo::initialize: admin only");
        require(timelock_ != address(0), "GovernorBravo::initialize: invalid timelock address");
        require(comp_ != address(0), "GovernorBravo::initialize: invalid comp address");
        require(votingPeriod_ >= MIN_VOTING_PERIOD && votingPeriod_ <= MAX_VOTING_PERIOD, "GovernorBravo::initialize: invalid voting period");
        require(votingDelay_ >= MIN_VOTING_DELAY && votingDelay_ <= MAX_VOTING_DELAY, "GovernorBravo::initialize: invalid voting delay");
        require(proposalThreshold_ >= MIN_PROPOSAL_THRESHOLD && proposalThreshold_ <= MAX_PROPOSAL_THRESHOLD, "GovernorBravo::initialize: invalid proposal threshold");

        timelock = TimelockInterface(timelock_);
        comp = CompInterface(comp_);
        votingPeriod = votingPeriod_;
        votingDelay = votingDelay_;
        proposalThreshold = proposalThreshold_;
    }


## 4 Design Patterns

### 3.1 Abstraction of Structs and Events to Interface

The definitions of structs and events are declared in the [GovernorBravoInterfaces](https://github.com/compound-finance/compound-protocol/blob/master/contracts/Governance/GovernorBravoInterfaces.sol) and are simply used in the smart contract

### 3.2 
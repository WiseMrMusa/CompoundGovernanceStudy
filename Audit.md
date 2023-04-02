# Comound Governance Audit

## 1.1 Overview 

The Compound Finance is a decentralized finance (DeFi) platform that allows users to lend and borrow cryptocurrencies in a trustless and transparent manner using smart contracts on the Ethereum blockchain. The platform's native token is called COMP, which allows holders to participate in the governance of the platform and earn rewards for using the protocol.

The core functionality of the platform revolves around the creation of markets for various cryptocurrencies, which users can then supply as collateral to borrow other cryptocurrencies or earn interest on their deposits. Interest rates are algorithmically determined based on supply and demand for each asset, and borrowers are subject to liquidation if the value of their collateral falls below a certain threshold.

In addition to lending and borrowing, Compound also offers users the ability to trade tokens using a built-in decentralized exchange (DEX) called Compound Swap. The platform has also expanded to support other blockchain networks, such as Polygon and Binance Smart Chain, to reduce the high gas fees and slow transaction speeds associated with using Ethereum.

### 1.1.1 Objectives

1. Study the pattern used in writing their smart contracts
2. Find potential vulnerability in the smart contract

### 1.1.2 Scope 

The following files were studied:
- contracts/Governance/GovernorBravoDelegateG2.sol

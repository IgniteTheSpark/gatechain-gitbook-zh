# GateChain Gas Mechanism Overview

## What is Gas?

In blockchain networks, **Gas** refers to the unit that measures the computational effort required to execute operations such as transactions or smart contracts. Users pay Gas fees to compensate network validators for the resources consumed during these operations.

## GateChain's Gas Mechanism

GateChain, as a next-generation high-performance public blockchain, has implemented a unique Gas mechanism to enhance network efficiency, reduce transaction costs for users, and increase the scarcity of its native token, GT.

### Introduction of EIP-1559 Mechanism

In the GateChain v1.1.6 mainnet upgrade, the network adopted a fee structure inspired by Ethereum's **EIP-1559** proposal. This mechanism divides transaction fees into two components:

1. **Base Fee**: A dynamically adjusting fee that reflects the current demand for block space. This fee is burned, effectively reducing the total supply of GT tokens and increasing their scarcity.
2. **Tip**: An optional additional fee that users can include to incentivize validators to prioritize their transactions.

The advantages of this mechanism include:

- **Enhanced Network Efficiency**: By dynamically adjusting the base fee, the network can better handle varying levels of demand, leading to more predictable transaction costs.
- **Reduction of GT Supply**: The burning of the base fee decreases the total GT in circulation, potentially increasing its value over time.

### Gas Price and Limit Adjustments

Alongside the new fee mechanism, GateChain has made adjustments to Gas prices and block Gas limits:

- **Minimum Gas Price**: Set to 10 NanoGT.
- **Maximum Block Gas Limit**: Established at 150,000,000.

These adjustments aim to optimize network performance and ensure efficient transaction processing.

## Conclusion

GateChain's Gas mechanism, through the adoption of a dynamic fee structure and token burning strategy, not only improves network efficiency and reduces user costs but also enhances the scarcity of GT tokens, providing users with a superior experience.

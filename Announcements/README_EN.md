# Status Updates

Get the latest updates from the GateChain team.

---
## GateChain Mainnet Upgrade v1.1.9 & v1.20 (Cancun & EIP-4844) (2025-09-13)

### Overview

The GateChain consensus version will be upgraded to v1.20, with the core feature being the addition of support for Blob transactions (EIP-4844). This will provide low-cost data availability for L2 solutions like Gate Layer. All nodes participating in consensus must upgrade their node binaries.

### Changelog

#### 1. Core Upgrades
- **EVM Version Upgrade**: The EVM is upgraded to the Cancun version, introducing several of Ethereum's latest improvements.
- **EIP-4844 Compatibility**: Support for Shard Blob Transactions (Proto-Danksharding), adding a new Blob transaction type and a related gas fee mechanism to lay the foundation for L2 data storage and transaction optimization.

#### 2. EIP Compatibility Updates
- **EIP-1153**: Introduces transient storage opcodes to optimize temporary data handling in transactions.
- **EIP-2565**: Reduces the gas cost of the ModExp operation, improving the efficiency of large number arithmetic.
- **EIP-2929**: Increases the gas cost of state-access opcodes (like SLOAD) to enhance on-chain security.
- **EIP-2930**: Adds optional access lists to optimize transaction gas calculations.
- **EIP-3198**: Returns the block `BASEFEE`, facilitating transaction fee calculation and smart contract optimization.
- **EIP-3529**: Reduces gas refunds for EVM operations to improve the rationality of the fee model.
- **EIP-3541**: Prohibits deploying contract bytecode starting with `0xEF` to enhance contract security.
- **EIP-3651**: Warms up the `COINBASE` address to optimize block reward payments and gas consumption.
- **EIP-3855**: Adds the `PUSH0` instruction to save bytecode space and gas.
- **EIP-3860**: Limits and meters the size of contract `initcode` to prevent attacks from oversized contracts.
- **EIP-5656**: Adds the `MCOPY` memory copying instruction to improve EVM memory operation efficiency.
- **EIP-6780**: Restricts `SELFDESTRUCT` to be called only within the same transaction, enhancing the predictability of contract execution.

#### 3. RPC Interface Improvements
- Added new RPC interfaces related to Blob transactions.
- Adjusted and optimized the `eth_hash` return value.

### Important Notice

Please download and upgrade to the latest version promptly to prevent consensus accounts from being disconnected.

---
## GateChain Mainnet Upgrade v1.1.8 (2025-08-20)

### Overview

GateChain completed the mainnet upgrade to v1.1.8 on August 20, 2025, at 10:00 (UTC+8).

### Changelog

- **Bug Fix**: Fixed an issue where some successful transactions were incorrectly reported as 'out of gas', causing their status to show as 'failed'.

### Important Notice

All relevant external nodes are advised to be aware of and upgrade to version v1.1.8.

---
## GateChain Mainnet Upgrade v1.1.7 (2025-08-11)

### Overview

1. The GateChain consensus version will be upgraded to `v1.1.7`. All nodes participating in consensus need to upgrade their node binaries.
2. Planned upgrade date: 2025-08-18
3. For details, please refer to the [GateChain Documentation](https://www.gatechain.io/docs/en/developers/gatechain-build/).

### Upgrade Mechanism

The GateChain consensus upgrade process is as follows:
- When consensus nodes use the new version binary to assemble a block, a network-wide upgrade vote will be triggered.
- After a round of block generation, once the vote count exceeds the threshold, the new protocol consensus will take effect across the network.
- At this point, any consensus node that has not upgraded to the latest version will be unable to participate in consensus.

### Changelog

1. **GateChain Version Upgrade**:
   - The block version number is upgraded to v17.
2. **Transaction Fee Check**:
   - Standard transaction fees must be greater than the base fee.
   - The `GasRemaining` method is added to GasMeter for querying remaining Gas.
3. **EVM Transaction Adjustments**:
   - The initial nonce for EVM accounts is corrected to 0.
   - A fixed 21000 gas fee is now deducted by default for EVM transfers.
4. **Block Production Mechanism Adjustment**:
   - The `gasFeeThreshold` mechanism is introduced to prevent low-value, invalid transactions from farming staking rewards.
5. **RPC Interface Improvements**:
   - An LRU cache mechanism is introduced in the API layer to optimize access efficiency.
   - New RPC interfaces have been added.
6. **Various Fixes and Optimizations**:
   - Fixed an issue where a node could wait indefinitely if a transaction ran out of gas.
   - Fixed a concurrent map read panic.
   - Fixed precision issues in RPC interface returns.

### Important Notice

Please download and upgrade to the latest version promptly to prevent consensus accounts from being disconnected.

---
## GateChain Mainnet Upgrade v1.1.6 (2024-08-19)

### Overview

1. The GateChain consensus version will be upgraded to `v1.1.6`. All nodes participating in consensus need to upgrade their node binaries.
2. Planned upgrade date: 2024-08-26
3. For details, please refer to the [GateChain Documentation](https://www.gatechain.io/docs/en/developers/gatechain-build/).

### Upgrade Mechanism

The GateChain consensus upgrade process is as follows:
- When consensus nodes use the new version binary to assemble a block, a network-wide upgrade vote will be triggered.
- After a round of block generation, once the vote count exceeds the threshold, the new protocol consensus will take effect across the network.
- At this point, any consensus node that has not upgraded to the latest version will be unable to participate in consensus.

### Changelog

1. **EVM Version Upgrade**
2. **Minimum GasPrice Adjustment**:
   - Increased from `5NANOGT` to `10NANOGT`.
   - Block Gas Limit set to `150M`.
3. **EIP-2718 Compatibility**:
   - Added support for multiple EVM transaction types.
4. **EIP-1559 Compatibility**:
   - Implemented dynamic network fees.
   - Introduced a base fee burning mechanism.
5. **RPC Interface Improvements**:
   - Process optimization.
   - Bug fixes and stability enhancements.
6. **Compilation and User Experience Enhancements**:
   - Added support for creating local accounts with a default password.
7. **General Improvements**:
   - Various bug fixes and optimizations.

### Important Notice

Please download and upgrade to the latest version promptly to prevent consensus accounts from being disconnected.

---
## GateChain Mainnet Upgrade v1.1.5 (2022-12-14)

### Overview

1. The GateChain consensus version will be upgraded to `v1.1.5`. All nodes participating in consensus need to upgrade their node binaries.
2. Planned upgrade date: 2022-12-20
3. For details, please refer to the [GateChain Documentation](https://www.gatechain.io/docs/en/developers/gatechain-build/).

### Upgrade Mechanism

The GateChain consensus upgrade process is as follows:
- When consensus nodes use the new version binary to assemble a block, a network-wide upgrade vote will be triggered.
- After a round of block generation, once the vote count exceeds the threshold, the new protocol consensus will take effect across the network.
- At this point, any consensus node that has not upgraded to the latest version will be unable to participate in consensus.

### Changelog

1. **EVM Module Enhancements**:
   - Added functionality for reinvesting staking rewards.
   - Added a feature for all consensus nodes to withdraw rewards with a single click.
2. **Protocol Compatibility**:
   - Compatible with the `EIP-1989` proposal.
3. **Consensus Improvements**:
   - Optimized the loyalty factor growth scheme for committee-elected consensus nodes.
4. **General Improvements**:
   - Various bug fixes and optimizations.

### Important Notice

Please download and upgrade to the latest version promptly to prevent consensus accounts from being disconnected.

---
## GateChain Mainnet Upgrade v1.1.4 (2022-11-02)

### Overview

1. The GateChain consensus version will be upgraded to `v1.1.4`. All nodes participating in consensus need to upgrade their node binaries.
2. Planned upgrade date: 2022-11-03
3. For details, please refer to the [GateChain Documentation](https://www.gatechain.io/docs/en/developers/gatechain-build/).

### Upgrade Mechanism

The GateChain consensus upgrade process is as follows:
- When consensus nodes use the new version binary to assemble a block, a network-wide upgrade vote will be triggered.
- After a round of block generation, once the vote count exceeds the threshold, the new protocol consensus will take effect across the network.
- At this point, any consensus node that has not upgraded to the latest version will be unable to participate in consensus.

### Changelog

1. **Node Improvements**:
   - Added the ability for users to edit the fee rates of consensus nodes.
   - Implemented a "debug trace" feature for EVM transactions.
2. **System Enhancements**:
   - Added batch digital rollback capability to the Gatemint engine.
3. **General Improvements**:
   - Various bug fixes and optimizations.

### Important Notice

Please download and upgrade to the latest version promptly to prevent consensus accounts from being disconnected.

---
## GateChain Mainnet Upgrade v1.1.3 (2022-05-13)

### Overview

1. The GateChain consensus version will be upgraded to `v1.1.3`. All nodes participating in consensus need to upgrade their node binaries.
2. Planned upgrade date: 2022-05-17
3. For details, please refer to the [GateChain Documentation](https://www.gatechain.io/docs/en/developers/gatechain-build/).

### Upgrade Mechanism

The GateChain consensus upgrade process is as follows:
- When consensus nodes use the new version binary to assemble a block, a network-wide upgrade vote will be triggered.
- After a round of block generation, once the vote count exceeds the threshold, the new protocol consensus will take effect across the network.
- At this point, any consensus node that has not upgraded to the latest version will be unable to participate in consensus.

### Changelog

1. **System Upgrades**:
   - Upgraded consensus version to `v1.1.3`.
   - Enhanced the ledger snapshot feature.
2. **Performance Improvements**:
   - Implemented gas fee suggestions.
   - Added sorting of transactions by gas price.
   - Added caching for EVM transaction signatures to improve efficiency.
3. **Module Enhancements**:
   - Optimized the subscription push module.
   - Fixed the timing of computing power updates in EVM staking.

### Important Notice

Please download and upgrade to the latest version promptly to prevent consensus accounts from being disconnected.

---
## GateChain Mainnet Upgrade v1.1.0 (2021-09-14)

### Overview

1. The GateChain consensus version will be upgraded to `v1.1.0`. All nodes participating in consensus need to upgrade their node binaries.
2. Planned upgrade date: 2021-09-15
3. For details, please refer to the [GateChain Documentation](https://www.gatechain.io/docs/en/developers/gatechain-build/).

### Upgrade Mechanism

The GateChain consensus upgrade process is as follows:
- When consensus nodes use the new version binary to assemble a block, a network-wide upgrade vote will be triggered.
- After a round of block generation, once the vote count exceeds the threshold, the new protocol consensus will take effect across the network.
- At this point, any consensus node that has not upgraded to the latest version will be unable to participate in consensus.

### Changelog

1. **Version Upgrades**:
   - Upgraded consensus version to `v1.1.0`.
   - Upgraded EVM version to `v1.10.8`.
2. **System Improvements**:
   - Implemented caching for transactions with large nonces.
   - Enhanced `API debug_traceTransaction` to support debugging of failed transactions.

### Important Notice

Please download and upgrade to the latest version promptly to prevent consensus accounts from being disconnected.

---
## GateChain Launches Today (2020-06-29)

### Overview

GateChain is a next-generation public chain focused on on-chain asset security and decentralized trading. With its uniquely designed vault account, GateChain provides a remarkable clearing mechanism for handling abnormal transactions, addressing the challenges of asset theft and private key loss. It will also support core functions such as decentralized trading and cross-chain transfers.


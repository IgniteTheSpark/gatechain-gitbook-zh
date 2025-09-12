# Status Updates

Get the latest updates from the GateChain team.

---
## GateChain Mainnet Upgrade v1.1.8 (2025-08-20)

### Overview

GateChain completed the mainnet upgrade to v1.1.8 on August 20, 2025, at 10:00 (UTC+8).

### Changelog

- **GateChain Version Upgrade**: The block version number is upgraded to v18.
- **Bug Fix**: Fixed an issue where some successful transactions were incorrectly reported as 'out of gas', causing their status to show as 'failed'.

### Important Notice

All relevant external nodes are advised to be aware of and upgrade to version v1.1.8.

---
## GateChain Mainnet Upgrade v1.1.7 (2025-08-11)

### Overview

1. GateChain consensus version will be upgraded to `v1.1.7`. All nodes participating in the consensus need to upgrade their node binary.

2. Scheduled Upgrade Date: 2025-08-18

3. For detailed information, please refer to the [GateChain Documentation](https://www.gatechain.io/docs/en/developers/gatechain-build/)

### Upgrade Mechanism

The GateChain consensus upgrade process works as follows:

- When a consensus node uses the new binary version for block assembly, it triggers a network-wide upgrade vote
- Once the vote count exceeds the threshold after a round of block creation, the new protocol consensus takes effect network-wide
- At this point, any consensus nodes that haven't upgraded to the latest version will be unable to participate in consensus

### Changelog

1. GateChain consensus version upgrade
   - Update blockchain version number to V17

2.Adds transaction gas fee checkpoints：
   - STD transaction gas fee must be greater than basefee
   - Adds GasRemaining to GasMeter for querying remaining gas 

3.For EVM transactions：
   - Set  initial nounce  of the EVM account to zero
   - Apply a flat 21000 gas for EVM transfer by default

4.For block production mechanism：
   - Introduces gasFeeThreshold mechanism to avoid staking rewards cheating using insignificant staking transactions

5.RPC interface improvements：
   - Introduces LRU cache mechanism at the API layer to provide improved access efficiency
   - Adds new RPC interfaces

6.Other fixes and improvements：
   - Fixes indefinite node waiting issue caused by gas exhaustion
   - Fixes concurrent map read panic 
   - Fixes  precision issues for values returned by RPC interfaces 

### Important Notice

Please download and upgrade to the latest version promptly to prevent consensus account disconnection.

---
## GateChain Mainnet Upgrade v1.1.6 (2024-08-19)

### Overview

1. GateChain consensus version will be upgraded to `v1.1.6`. All nodes participating in the consensus need to upgrade their node binary.

2. Scheduled Upgrade Date: 2024-08-26

3. For detailed information, please refer to the [GateChain Documentation](https://www.gatechain.io/docs/en/developers/gatechain-build/)

### Upgrade Mechanism

The GateChain consensus upgrade process works as follows:

- When a consensus node uses the new binary version for block assembly, it triggers a network-wide upgrade vote
- Once the vote count exceeds the threshold after a round of block creation, the new protocol consensus takes effect network-wide
- At this point, any consensus nodes that haven't upgraded to the latest version will be unable to participate in consensus

### Changelog

1. EVM Version Upgrade

2. Minimum GasPrice Adjustments:
   - Increased from `5NANOGT` to `10NANOGT`
   - Block Gas Limit set to `150M`

3. EIP-2718 Compatibility:
   - Added support for multiple EVM transaction types

4. EIP-1559 Compatibility:
   - Implemented dynamic network fees
   - Introduced base fee burning mechanism

5. RPC Interface Improvements:
   - Process optimization
   - Bug fixes and stability improvements

6. Compilation and UX Enhancements:
   - Added support for local account creation with default password

7. General Improvements:
   - Various bug fixes and optimizations

### Important Notice

Please download and upgrade to the latest version promptly to prevent consensus account disconnection.

---

## GateChain Mainnet Upgrade v1.1.5 (2022-12-14)

### Overview

1. GateChain consensus version will be upgraded to `v1.1.5`. All nodes participating in the consensus need to upgrade their node binary.

2. Scheduled Upgrade Date: 2022-12-20

3. For detailed information, please refer to the [GateChain Documentation](https://www.gatechain.io/docs/en/developers/gatechain-build/)

### Upgrade Mechanism

The GateChain consensus upgrade process works as follows:

- When a consensus node uses the new binary version for block assembly, it triggers a network-wide upgrade vote
- Once the vote count exceeds the threshold after a round of block creation, the new protocol consensus takes effect network-wide
- At this point, any consensus nodes that haven't upgraded to the latest version will be unable to participate in consensus

### Changelog

1. EVM Module Enhancements:
   - Added pledge reward reinvestment function
   - Added one-click reward extraction for all consensus nodes

2. Protocol Compatibility:
   - Compatible with the `EIP-1989` proposal

3. Consensus Improvements:
   - Optimized loyalty coefficient growth plan for committee-selected consensus nodes

4. General Improvements:
   - Various bug fixes and optimizations

### Important Notice

Please download and upgrade to the latest version promptly to prevent consensus account disconnection.

---

## GateChain Mainnet Upgrade v1.1.4 (2022-11-02)

### Overview

1. GateChain consensus version will be upgraded to `v1.1.4`. All nodes participating in the consensus need to upgrade their node binary.

2. Scheduled Upgrade Date: 2022-11-03

3. For detailed information, please refer to the [GateChain Documentation](https://www.gatechain.io/docs/en/developers/gatechain-build/)

### Upgrade Mechanism

The GateChain consensus upgrade process works as follows:

- When a consensus node uses the new binary version for block assembly, it triggers a network-wide upgrade vote
- Once the vote count exceeds the threshold after a round of block creation, the new protocol consensus takes effect network-wide
- At this point, any consensus nodes that haven't upgraded to the latest version will be unable to participate in consensus

### Changelog

1. Node Improvements:
   - Added user-editable fee-rate for consensus nodes
   - Implemented "debug trace" for EVM transactions

2. System Enhancements:
   - Added batch number rollback capability with Gatemint engine

3. General Improvements:
   - Various bug fixes and optimizations

### Important Notice

Please download and upgrade to the latest version promptly to prevent consensus account disconnection.

---

## GateChain Mainnet Upgrade v1.1.3 (2022-05-13)

### Overview

1. GateChain consensus version will be upgraded to `v1.1.3`. All nodes participating in the consensus need to upgrade their node binary.

2. Scheduled Upgrade Date: 2022-05-17

3. For detailed information, please refer to the [GateChain Documentation](https://www.gatechain.io/docs/en/developers/gatechain-build/)

### Upgrade Mechanism

The GateChain consensus upgrade process works as follows:

- When a consensus node uses the new binary version for block assembly, it triggers a network-wide upgrade vote
- Once the vote count exceeds the threshold after a round of block creation, the new protocol consensus takes effect network-wide
- At this point, any consensus nodes that haven't upgraded to the latest version will be unable to participate in consensus

### Changelog

1. System Upgrades:
   - Upgraded consensus version to `v1.1.3`
   - Enhanced Ledger snapshot functionality

2. Performance Improvements:
   - Implemented gas fee recommendations
   - Added transaction sorting by gas price
   - Added signature cache for improved EVM transaction efficiency

3. Module Enhancements:
   - Optimized subscription push module
   - Fixed power update timing in EVM staking

### Important Notice

Please download and upgrade to the latest version promptly to prevent consensus account disconnection.

---

## GateChain Mainnet Upgrade v1.1.0 (2021-09-14)

### Overview

1. GateChain consensus version will be upgraded to `v1.1.0`. All nodes participating in the consensus need to upgrade their node binary.

2. Scheduled Upgrade Date: 2021-09-15

3. For detailed information, please refer to the [GateChain Documentation](https://www.gatechain.io/docs/en/developers/gatechain-build/)

### Upgrade Mechanism

The GateChain consensus upgrade process works as follows:

- When a consensus node uses the new binary version for block assembly, it triggers a network-wide upgrade vote
- Once the vote count exceeds the threshold after a round of block creation, the new protocol consensus takes effect network-wide
- At this point, any consensus nodes that haven't upgraded to the latest version will be unable to participate in consensus

### Changelog

1. Version Upgrades:
   - Upgraded consensus version to `v1.1.0`
   - Upgraded EVM version to `v1.10.8`

2. System Improvements:
   - Implemented caching for transactions with large nonce values
   - Enhanced `API debug_traceTransaction` with failed transaction debugging support

### Important Notice

Please download and upgrade to the latest version promptly to prevent consensus account disconnection.

---

## GateChain Launches Today (2020-06-29)

### Overview

GateChain is a next-generation public blockchain, focused on onchain asset safety and decentralized trading. With a uniquely designed Vault Account, primed for handling abnormal transactions, GateChain presents an extraordinary clearing mechanism, tackling the challenges of asset theft and private key loss. Decentralized trading and cross-chain transfers will also be supported, alongside other core features.
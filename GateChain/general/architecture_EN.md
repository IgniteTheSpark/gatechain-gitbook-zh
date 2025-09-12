# GateChain Blockchain

Initially, GateChain's mainnet was built on a UTXO-based architecture, implementing its own secure account model and conducting prototype verification. This work has been recognized in the industry and resulted in a research paper titled "Research on Secure Accounts Based on UTXO Model" (pending publication). Subsequently, to better integrate with cross-chain ecosystems, GateChain's architecture migrated to a blockchain framework supported by Tendermint infrastructure. 

At the consensus level, it draws inspiration from Algorand's consensus mechanism, utilizing VRF (Verifiable Random Function) algorithm to enhance security and efficiency. The new architecture directly employs an account model, focusing on upper-layer protocol innovation, with particular emphasis on implementing unique secure accounts featuring revocable transactions and clearing capabilities. 

At the application layer, the new architecture provides standard interfaces to meet the requirements of various upper-layer applications, and offers SDK-based functions for offline account generation and transaction signing, providing convenient development tools for third-party ecosystem applications.

![](../.gitbook/assets/images/chain_01.png)

## Layer Details

### Application Layer
The topmost layer containing user-facing applications:
- **Dex**: Decentralized exchange for token swapping and trading
- **Dapps**: Ecosystem of decentralized applications
- **Explorer**: Blockchain explorer for querying transactions and blocks
- **Wallet**: Digital asset wallet for managing user assets

### Framework Layer
Provides core blockchain services and functional frameworks:
- **Asset Service**: Handles token and asset management
- **Vault Account**: Secure storage system for digital assets
- **Goverment**: On-chain governance system for decision making
- **Module Manage**: Coordinates various functional modules

### Execution Layer
Responsible for smart contract and transaction execution environments:
- **CosmosSDK**: Development toolkit based on Cosmos, providing blockchain infrastructure
- **EVM**: Ethereum Virtual Machine for executing Ethereum-compatible smart contracts

### Core Layer
Forms the fundamental infrastructure and core functionalities:
- **Consensus**: Ensures network nodes reach agreement
- **Staking**: Mechanism for network security and validator selection
- **VRF**: Verifiable Random Function for secure randomness
- **ABCI**: Application Blockchain Interface connecting blockchain applications with consensus engine


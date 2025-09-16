# Gate Layer × GateChain: Technical Architecture

## High-Level Architecture

The diagram below illustrates the high-level technical architecture of Gate Layer, covering the complete flow from user interaction and L2 execution to L1 settlement and data availability. It clearly depicts how core components like the Sequencer, Batcher, and Proposer work in concert with the Rollup contracts and Blob storage on GateChain to ensure the system's security, efficiency, and scalability.

![Gate Layer High-Level Architecture](./gatelayer-architecture.png)

**Key Points of the Diagram**

*   **L2 Execution**: The Sequencer executes transactions on the EVM to form L2 blocks; EIP-1559 manages L2 fee adjustments.
*   **Batch Submission**: The Batcher writes transaction batches and **state roots** to **GateChain's Blobs** (Data Availability layer).
*   **Settlement & Finality**: The Proposer submits outputs (state root, output root, etc.) to the Rollup contracts on GateChain; finality is achieved after a finality window.
*   **Security & Governance**: Settlement security is provided by **GT staking + the validator network**; governance and the treasury reside on the GateChain side.
*   **Interoperability**: **LayerZero** and ecosystem bridges are integrated on the L2 side to enable cross-chain asset and message communication.
*   **Availability & Observability**: Nodes, indexers, and explorers on L2 read Blob/contract information to ensure verifiability and traceability.
*   **EVM Compatibility & Developer Experience**: The Gate Layer core utilizes the EVM execution engine, ensuring full compatibility with Ethereum. Developers can seamlessly migrate DApps and use standard toolchains like Hardhat and Remix.
*   **Modular Components**: The architecture separates core components: the **Sequencer** handles transaction ordering, the **Batcher** manages data bundling, and the **Proposer** submits state root proposals, enhancing the system's maintainability.
*   **Unified Interaction Entrypoint**: All on-chain interactions enter through a standard **RPC endpoint**, providing users and developers with an interaction experience consistent with Ethereum.

---

## Transaction and Settlement Flow

This sequence diagram details the entire lifecycle of an L2 transaction, from submission to final confirmation. It reveals the interaction sequence between the user, Sequencer, Batcher, Proposer, and GateChain (L1), helping to clarify how a transaction's status evolves from `unsafe` to `finalized`.

![Rollup Lifecycle](./rollup-lifecycle.png)

---

## Component Interaction Diagram

This diagram focuses on the core interactions between different roles in the Gate Layer ecosystem (Users, Nodes, Sequencers, Challengers) and the L1 and L2 layers. It simplifies internal complexities to highlight data flow and the division of responsibilities, such as how users submit transactions, how the Sequencer posts data to L1, and how nodes sync information from L1.

![Gate Layer Component Interaction](./gatelayer-component-interaction.png)

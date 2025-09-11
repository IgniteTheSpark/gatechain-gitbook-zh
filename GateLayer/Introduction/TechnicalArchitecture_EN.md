# GateLayer × GateChain: Technical Architecture

## High-Level Architecture

*Note: The diagram below is in Mermaid format. We recommend using the SVG version you provided for the best display quality.*
![GateLayer Architecture](gatelayer-architecture.svg)

```mermaid
flowchart TD
    %% Clients
    A["Users / Wallets / DApps"] --> B["RPC / JSON-RPC Endpoint"]
    subgraph L2["GateLayer (OP Stack-based L2)"]
      B --> C["Sequencer / Executor (EVM)"]
      C --> D["State DB & Mempool"]
      C --> E["EIP-1559 Fee Market\n(baseFee + priorityFee)"]
      C --> F["Logs / Events"]
      C --> G["Node / Full Node / Archive"]
      C --> H["Indexer & Explorer"]

      C --> I[Batcher]
      C --> J[Output Proposer]
      C --> K["Bridges on L2\n(LayerZero & Ecosystem Bridges)"]

      style L2 fill:#0b1,stroke:#0b1,color:#fff,fill-opacity:0.08
    end

    subgraph L1["GateChain (Settlement + DA)"]
      L["Rollup System Contracts\n(canonical bridge / portal)"]
      M["Blob DA (stores batch data & state roots)"]
      N["GT Staking Validators / Consensus"]
      O["Treasury / Governance"]
      style L1 fill:#08c,stroke:#08c,color:#fff,fill-opacity:0.08
    end

    %% Posting & Settlement
    I --> M
    J --> L
    L -. finality window / proofs .- J
    M --> L

    %% Economic/Security
    N --> L
    O --> L

    %% Interop
    P["External Chains\n(Ethereum / BNB / others)"]
    K <---> P

    %% Read paths
    H --> A
    G --> H
    F --> H
```

**Key Points of the Diagram**

*   **L2 Execution**: The Sequencer executes transactions on the EVM to form L2 blocks; EIP-1559 manages L2 fee adjustments.
*   **Batch Submission**: The Batcher writes transaction batches and **state roots** to **GateChain's Blobs** (Data Availability layer).
*   **Settlement & Finality**: The Proposer submits outputs (state root, output root, etc.) to the Rollup contracts on GateChain; finality is achieved after a finality window.
*   **Security & Governance**: Settlement security is provided by **GT staking + the validator network**; governance and the treasury reside on the GateChain side.
*   **Interoperability**: **LayerZero** and ecosystem bridges are integrated on the L2 side to enable cross-chain asset and message communication.
*   **Availability & Observability**: Nodes, indexers, and explorers on L2 read Blob/contract information to ensure verifiability and traceability.

---

## Transaction and Settlement Flow

*Note: The diagram below is in Mermaid format. We recommend using the SVG version you provided for the best display quality.*
![Rollup Lifecycle](rollup-lifecycle.svg)

```mermaid
sequenceDiagram
    autonumber
    participant U as User / DApp
    participant S as GateLayer Sequencer(EVM)
    participant B as Batcher
    participant P as Output Proposer
    participant GC as GateChain (Rollup Contracts + Blob)

    U->>S: Submit L2 tx (gas = baseFee + priorityFee)
    S->>S: Execute txs, build L2 block, update state root
    S->>U: Return receipt / near-instant confirmation

    S->>B: Send batch data (txs, traces, roots)
    B->>GC: Post batch data to Blob (DA)

    P->>GC: Post output root / state root to rollup contracts
    Note over GC: Finality window / challenge period (per config)

    GC-->>U: Finalized status (withdrawals / messages can be proved)
```

---

## Component Interaction Diagram

```mermaid
graph TD
    subgraph "Layer 1"
        L1_Chain["GateChain (Data Availability Layer)"]
    end

    subgraph "Layer 2"
        Challengers
        Nodes
        Users
        Sequencers
    end

    %% L2 Internal Flows
    Users -- "Submit transactions<br/>Query data (e.g. block explorers)" --> Nodes
    Nodes -- "P2P Realtime Updates" --> Sequencers
    
    %% L2 <--> L1 Flows
    Users -- "Submit deposits" --> L1_Chain
    Sequencers -- "Submit batches and assertions" --> L1_Chain
    Nodes -- "Get safe transactions<br/>and blocks" --> L1_Chain
    Challengers -- "Verify block hash assertions<br/>Submit fault proofs" --> L1_Chain
```

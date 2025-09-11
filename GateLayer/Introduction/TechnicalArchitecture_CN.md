# GateLayer × GateChain：技术架构

## 高层架构

*请注意：下图是 Mermaid 格式的源码，我们推荐使用您提供的 SVG 矢量图版本以获得最佳显示效果。*
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
      C --> K["Bridges on L2\n(LayerZero & 生态桥)"]

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

**图中要点**

*   **L2 执行**：Sequencer 在 EVM 上执行交易，形成 L2 区块；EIP-1559 负责 L2 费率调节。
*   **批量提交**：Batcher 将交易批次与**状态根**写入 **GateChain 的 Blob**（数据可用性层）。
*   **结算与最终性**：Proposer 把输出（state root / output root 等）提交到 GateChain 上的 Rollup 合约；在最终性窗口后确定。
*   **安全与治理**：结算安全由 **GT 质押 + 验证者网络**提供；治理/金库在 GateChain 侧。
*   **互操作**：在 L2 侧集成 **LayerZero** 与生态桥，提供跨链资产/消息互通。
*   **可用性与可观察性**：节点、索引、浏览器在 L2，读取 Blob/合约信息以实现可验证与回溯。

---

## 交易与结算流程

*请注意：下图是 Mermaid 格式的源码，我们推荐使用您提供的 SVG 矢量图版本以获得最佳显示效果。*
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

## 组件交互图

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

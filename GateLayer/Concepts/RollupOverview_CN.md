# Rollup 工作原理

## 什么是 Rollup？

Rollup 是一种强大的 Layer 2 扩容解决方案，其核心思想是将大量的交易计算和状态存储转移到 Layer 2（链下）处理，同时将每笔交易的少量数据发布回 Layer 1（链上），从而利用 Layer 1 的安全性和去中心化来保障 Layer 2 的安全。

Gate Layer 正是基于这种模型构建的 **Optimistic Rollup**，它以 GateChain 作为其 Layer 1。

## Gate Layer 的 Rollup 机制

Gate Layer 的工作流程可以分解为以下几个关键步骤，这些步骤共同确保了网络的高性能和与 GateChain 一致的安全性：

### 1. 链下执行 (Off-Chain Execution)

*   **快速处理**：用户提交的交易首先由 Gate Layer 的 **Sequencer (定序器)** 接收。Sequencer 负责对交易进行排序、处理，并立即将它们打包成 L2 区块。
*   **高性能**：由于这个过程发生在链下，它避免了 L1 慢速的共识过程，从而使 Gate Layer 能够实现极高的 TPS（每秒交易处理量）和毫秒级的交易确认。

### 2. 数据发布至 GateChain (Data Publication)

*   **数据可用性**：为了确保 L2 的状态可以被任何人验证，交易数据必须是公开可用的。`op-batcher` 组件会将多个 L2 区块的交易数据进行压缩，然后通过 GateChain 支持的 **EIP-4844 `blobs`** 发布到 L1。
*   **低成本**：使用 `blobs` 而不是传统的 `calldata` 来发布数据，极大地降低了 L1 的存储成本，这是 Gate Layer 交易费用低廉的关键原因。

### 3. 状态承诺 (State Commitment)

*   **状态根**：当 L2 交易被执行后，会产生一个新的 L2 状态。`op-proposer` 组件会计算这个新状态的唯一加密指纹，即**状态根 (State Root)**。
*   **提交至 L1**：`op-proposer` 会定期将这个新的状态根提交到部署在 GateChain (L1) 上的 `L2OutputOracle` 智能合约中。这个状态根就像是 L2 在某个时间点的“快照摘要”。

### 4. 乐观验证 (Optimistic Verification)

*   **“乐观”假设**：Gate Layer 作为一个 **Optimistic Rollup**，它“乐观地”假设所有提交到 L1 的状态根都是正确的，无需立即进行验证。
*   **故障挑战期**：每个状态根在提交后都会进入一个**故障挑战期 (Fault Challenge Period)**。在此期间，网络中的任何人（验证者）都可以检查 L1 上的交易数据，并重新计算状态根。
*   **故障证明**：如果验证者发现 L1 上的状态根与自己计算的不符，他们可以提交一个**故障证明 (Fraud Proof)** 来发起挑战。如果挑战成功，错误的状态根将被移除，提交者将受到惩罚。如果挑战期内无人挑战，该状态根就被认为是最终确认的。

## 总结

通过这套机制，Gate Layer 巧妙地将计算密集型的工作放在了 L2，同时利用 L1 GateChain 作为数据的发布层和最终的仲裁层，从而在不牺牲安全性的前提下，实现了区块链性能的巨大飞跃。


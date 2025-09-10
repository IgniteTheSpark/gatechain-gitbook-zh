# Rollup Overview

## What is a Rollup?

A Rollup is a powerful Layer 2 scaling solution. Its core idea is to move the bulk of transaction computation and state storage off-chain (to Layer 2) while posting a small amount of data for each transaction back on-chain (to Layer 1). By doing this, a Rollup leverages the security and decentralization of its Layer 1 to secure the Layer 2 network.

Gate Layer is built on this model as an **Optimistic Rollup**, using GateChain as its Layer 1.

## How Gate Layer's Rollup Works

Gate Layer's workflow can be broken down into several key steps that together ensure high performance and security consistent with GateChain:

### 1. Off-Chain Execution

*   **Fast Processing**: User-submitted transactions are first received by Gate Layer's **Sequencer**. The Sequencer is responsible for ordering and processing transactions, and immediately bundling them into L2 blocks.
*   **High Performance**: Because this process occurs off-chain, it avoids the slower consensus process of the L1, allowing Gate Layer to achieve extremely high TPS (Transactions Per Second) and millisecond-level transaction confirmations.

### 2. Data Publication to GateChain

*   **Data Availability**: To ensure the state of L2 can be verified by anyone, the transaction data must be publicly available. The `op-batcher` component compresses the transaction data from multiple L2 blocks and then posts it to the L1 using **EIP-4844 `blobs`**, which are supported by GateChain.
*   **Low Cost**: Using `blobs` instead of traditional `calldata` for data posting significantly reduces L1 storage costs, which is a key reason for Gate Layer's low transaction fees.

### 3. State Commitment

*   **State Root**: After L2 transactions are executed, a new L2 state is generated. The `op-proposer` component calculates a unique cryptographic fingerprint of this new state, known as the **State Root**.
*   **Submission to L1**: The `op-proposer` periodically submits this new state root to the `L2OutputOracle` smart contract deployed on GateChain (L1). This state root acts like a "snapshot summary" of the L2 at a specific point in time.

### 4. Optimistic Verification

*   **The "Optimistic" Assumption**: As an **Optimistic Rollup**, Gate Layer "optimistically" assumes that all state roots submitted to the L1 are correct without needing immediate verification.
*   **Fault Challenge Period**: Each state root, upon submission, enters a **Fault Challenge Period**. During this window, anyone (a Verifier) can check the transaction data on L1 and re-calculate the state root.
*   **Fraud Proofs**: If a verifier finds that the state root on L1 does not match their own calculation, they can submit a **Fraud Proof** to issue a challenge. If the challenge is successful, the incorrect state root is removed, and its proposer is penalized. If no one challenges the state root within the challenge period, it is considered final.

## Summary

Through this mechanism, Gate Layer cleverly offloads computation-intensive work to L2 while leveraging L1 GateChain as the data publication layer and the ultimate arbiter of truth. This achieves a massive leap in blockchain performance without sacrificing security.


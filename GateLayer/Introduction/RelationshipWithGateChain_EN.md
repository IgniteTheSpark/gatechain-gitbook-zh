# Relationship with GateChain

## Fees and Economic Flow

Transaction fees on Gate Layer are designed as a sum of two components to accurately reflect the cost of execution on L2 and data storage on L1.

*   **L2 Execution Fee**:
    *   This fee covers the computational resources required to execute a transaction in the Gate Layer EVM.
    *   It follows the standard **EIP-1559 model**, consisting of a `baseFee` and a `priorityFee`. This portion is the primary revenue for the Sequencer.

*   **L1 Data Fee**:
    *   This fee covers the cost of publishing transaction data as **Blobs** to GateChain (L1), ensuring data security and availability.
    *   This is the key reason L2 fees can be orders of magnitude lower than L1, as Blob storage is significantly cheaper than traditional `calldata`.

*   **Fee Flow and Governance**:
    *   The total fee paid by the user (`L2 Execution Fee + L1 Data Fee`) is collected by the Sequencer.
    *   The Sequencer then pays the cost to submit data to L1 (`L1 Data Fee`), retaining the remainder (`L2 Execution Fee`) as its operational profit.
    *   In the future, governance can implement mechanisms to redistribute Sequencer revenue, for instance, by directing a portion to an ecosystem treasury or using it to incentivize network participants.

## Cross-Chain Interoperability

*   The application layer achieves multi-chain asset/message interoperability through **LayerZero and ecosystem bridges**.

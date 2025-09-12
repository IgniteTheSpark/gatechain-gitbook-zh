# Gas and Fee Mechanism

## Fee Overview

Gate Layer uses the Fjord release of OP-Stack and posts all L2 transaction data to GateChain via EIP-4844 `blobs` (not using `calldata` or external DA). The total fee for a transaction is the sum of three components:

*   **L2 Execution Fee**
    *   Based on the EIP-1559 model, reflecting the computational resources consumed for executing the transaction on Gate Layer.

*   **L1 Data Fee**
    *   The cost required to post transaction data to GateChain `blobs`. This fee is proportional to the size of the transaction data and the current Blob Base Fee on GateChain.

*   **Operator Fee**
    *   A configurable fee, either linear with gas used or constant, intended to cover the operational costs of the chain (not applied to deposit transactions).

---

## Fee Formulas

The total fee (`totalFee`) for a transaction is calculated as follows:

```math
totalFee = l2ExecutionFee + l1DataFee + operatorFee
```

The components are calculated as follows:

*   **L2 Execution Fee**: 
    ```math
    l2ExecutionFee = gasUsed * (baseFee + priorityFee)
    ```
    This is a standard EIP-1559 model where `gasUsed` is the gas consumed by the transaction, `baseFee` is the L2 network's base fee, and `priorityFee` is an optional tip for the sequencer.

*   **L1 Data Fee**: 
    ```math
    l1DataFee = estimatedSizeScaled * (baseFeeScalar * l1BaseFee * 16 + blobFeeScalar * l1BlobBaseFee) / 10^{12}
    ```

*   **Operator Fee**: 
    ```math
    operatorFee = (gasUsed * operatorFeeScalar / 10^6) + operatorFeeConstant
    ```

---

## Fee Component Breakdown

### L2 Execution Fee

The execution fee model is identical to what you would pay for the same transaction on Ethereum or GateChain. Because Gate Layer is EVM-equivalent, the `gasUsed` for a transaction on Gate Layer is exactly the same as it would be on Ethereum.

**The core difference is the Gas Price.** The gas price on Gate Layer is significantly lower than on Ethereum, so even with the same gas usage, the final fee in USD is much cheaper.

### L1 Data Fee

Gate Layer integrates the **FastLZ compression estimator** from the Fjord upgrade, which allows for a more accurate calculation of the L1 data fee for each transaction.

The calculation process is as follows:

1.  **Estimate the compressed transaction size**:
    ```math
    estimatedSizeScaled = max(minTxSize * 10^6, intercept + fastlzCoef * fastlzSize)
    ```
    *   `fastlzSize`: The actual size of the transaction after FastLZ compression post-signing.
    *   `intercept` and `fastlzCoef`: Model parameters inherited from the OP-Stack.

2.  **Calculate the weighted gas price multiplier**:
    ```math
    l1FeeScaled = baseFeeScalar * l1BaseFee * 16 + blobFeeScalar * l1BlobBaseFee
    ```
    *   `l1BaseFee`: The current base fee of GateChain (L1).
    *   `l1BlobBaseFee`: The current blob base fee of GateChain (L1).
    *   `baseFeeScalar` and `blobFeeScalar`: Chain configuration parameters used to adjust the fee.
        *   **Current Gate Layer Parameter Settings**: `baseFeeScalar` = `1368`, `blobFeeScalar` = `810949`.

3.  **Calculate the final L1 data fee**:
    The results from the first two steps are multiplied and scaled to get the final `l1DataFee`.
    ```math
    l1DataFee = estimatedSizeScaled * l1FeeScaled / 10^{12}
    ```

### Operator Fee

This fee provides an additional revenue model for the chain operator to cover specific operational costs.

*   **Deposit transactions are exempt from the operator fee**.
*   The fee is determined by two operator-configurable parameters: `operatorFeeScalar` and `operatorFeeConstant`.

**Current Gate Layer Parameters**:
*   `operatorFeeScalar`: `0`
*   `operatorFeeConstant`: `0`

This means that **Gate Layer currently does not charge any operator fees**.


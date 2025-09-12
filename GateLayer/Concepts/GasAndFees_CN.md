# Gas 和费用机制

## 费用概览

Gate Layer 采用了 OP-Stack 的 Fjord 版本，并将所有 L2 交易数据通过 EIP-4844 `blobs` 的形式发布到 GateChain（不使用 `calldata` 或外部 DA）。一笔交易的总费用由三部分组成：

*   **L2 执行费 (L2 Execution Fee)**
    *   基于 EIP-1559 模型，反映了在 Gate Layer 上执行交易所消耗的计算资源。

*   **L1 数据费 (L1 Data Fee)**
    *   将交易数据发布到 GateChain `blobs` 所需的费用。该费用与交易数据的大小和当前 GateChain 的 Blob 基础费用成正比。

*   **运营费 (Operator Fee)**
    *   一个可配置的、与 Gas 用量线性相关或恒定的费用，用于覆盖链的运营成本（存款交易除外）。

---

## 费用公式

一笔交易的总费用 (`totalFee`) 计算方式如下：

```math
totalFee = l2ExecutionFee + l1DataFee + operatorFee
```

其中，各个部分的计算方式为：

*   **L2 执行费**: 
    ```math
    l2ExecutionFee = gasUsed * (baseFee + priorityFee)
    ```
    这是一个标准的 EIP-1559 模型，`gasUsed` 是交易消耗的 Gas 量，`baseFee` 是 L2 网络的基础费用，`priorityFee` 是用户可选的优先费。

*   **L1 数据费**: 
    ```math
    l1DataFee = estimatedSizeScaled * l1FeeScaled / 10^{12}
    ```

*   **运营费**: 
    ```math
    operatorFee = (gasUsed * operatorFeeScalar / 10^6) + operatorFeeConstant
    ```

---

## 各部分详解

### L2 执行费

交易的执行费与您在以太坊或 GateChain 上支付相同交易的费用模型完全相同。由于 Gate Layer 是 EVM 等效的，一笔交易在 Gate Layer 上消耗的 `gasUsed` 与在以太坊上完全一致。

**核心区别在于 Gas Price**。Gate Layer 上的 Gas Price 远低于以太坊，因此即使用量相同，最终以美元计价的费用也会便宜得多。

### L1 数据费

Gate Layer 集成了 Fjord 升级中的 **FastLZ 压缩估算器**，能够更精确地计算每笔交易应支付的 L1 数据费用。

计算过程如下：

1.  **估算压缩后的交易大小**：
    ```math
    estimatedSizeScaled = max(minTxSize * 10^6, intercept + fastlzCoef * fastlzSize)
    ```
    *   `fastlzSize`: 交易签名后经过 FastLZ 压缩的实际大小。
    *   `intercept` 和 `fastlzCoef`: 从 OP-Stack 继承的模型参数。

2.  **计算加权的 Gas 价格乘数**：
    ```math
    l1FeeScaled = baseFeeScalar * l1BaseFee * 16 + blobFeeScalar * l1BlobBaseFee
    ```
    *   `l1BaseFee`: GateChain (L1) 的当前基础费。
    *   `l1BlobBaseFee`: GateChain (L1) 的当前 Blob 基础费。
    *   `baseFeeScalar` 和 `blobFeeScalar`: 用于调节费用的链配置参数。
        *   **当前 Gate Layer 参数设置**: `baseFeeScalar` = `1368`, `blobFeeScalar` = `810949`。

3.  **计算最终 L1 数据费**：
    将前两步的结果相乘并进行缩放，得到最终的 `l1DataFee`。
    ```math
    l1DataFee = estimatedSizeScaled * l1FeeScaled / 10^{12}
    ```

### 运营费

该费用为链的运营方提供了额外的收入模型，以覆盖特定的运营成本。

*   **存款交易不收取运营费**。
*   费用由两个运营方可配置的参数决定：`operatorFeeScalar` 和 `operatorFeeConstant`。

**当前 Gate Layer 参数设置**:
*   `operatorFeeScalar`: `0`
*   `operatorFeeConstant`: `0`

这意味着**目前 Gate Layer 不收取任何运营费**。


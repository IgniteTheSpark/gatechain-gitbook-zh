# 常见问题 (FAQ)

### 什么是 Gate Layer？
GateLayer 是一个基于 Optimism OP Stack 构建的高性能 Layer 2 网络。它完全兼容 EVM，由 GateChain 作为结算层提供安全保障，并为低成本、高速度的交易进行了优化。

### GateLayer 与其他基于 OP Stack 的 L2 有何不同？
GateLayer 的独特之处在于它利用 GateChain 的原生 Blob 存储来存放 Rollup 的状态根，从而降低了成本并提升了可扩展性。它由 GT 质押保障，在确保强大结算安全性的同时，实现了超过 5,700 的 TPS 和 1 秒的出块时间。

### 如何估算 Gate Layer 的 TPS？
TPS 可以通过一个原生转账交易来估算。每笔转账通常消耗 21,000 Gas，而 Gate Layer 的区块 Gas 上限是 1.2 亿。这意味着每个区块可以处理 120,000,000 ÷ 21,000 ≈ 5,714 笔交易，因此 TPS 超过 5,700。

### 开发者和用户如何开始使用 GateLayer？
开发者可以使用现有的以太坊工具无需许可地部署应用，而用户将享受到低成本、高速度的交易。一旦网络上线，我们将提供文档、区块浏览器和跨链桥作为核心的入门工具。

### GateLayer 上的交易费为什么这么低？
GateLayer 上的交易费由两部分组成：L2 执行费和 L1 数据费。其低成本的关键在于，L1 数据费是通过将交易数据作为 `Blobs` 发布到 GateChain 来支付的。Blob 是专为 Rollup 设计的、极具成本效益的数据存储方案，使其比在主网上使用传统的 `calldata` 便宜得多。

### GateLayer 上的“交易最终性”是什么意思？
GateLayer 上的一笔交易会经历两个确认阶段：
1.  **快速确认 (unsafe)**：交易几乎被 L2 的 Sequencer 即时确认（约 1 秒内），提供了流畅的用户体验。
2.  **最终确认 (finalized)**：只有当交易数据被发布到 GateChain (L1) 并且经过了额外的 10 个区块的安全缓冲期后，该交易才变得不可逆转。这个由 L2 强制执行的等待期确保了最高级别的安全性。

### GateLayer 的安全性是如何保障的？
GateLayer 的安全性是多层次的：
- **执行安全**：作为一个基于 OP Stack 的 Rollup，它依赖于故障证明机制来确保 L2 状态转换的正确性。
- **数据可用性与结算安全**：它继承了 GateChain 的安全性，GateChain 通过 GT 质押和验证者网络来保障所有 L2 交易数据和结算操作的安全。
- **互操作安全**：跨链操作由 LayerZero 等协议保障，并以 GateChain 作为最终性的安全锚点。

### GateLayer 上的原生 Gas 代币是什么？
GateLayer 上的原生 Gas 代币是 **GT (GateToken)**，用于支付网络上的所有交易费用。

### GateLayer 是否兼容 EVM？这对开发者意味着什么？
是的，GateLayer **完全兼容 EVM**。这意味着任何在以太坊上运行的智能合约、DApp 或工具，几乎无需任何修改就可以在 GateLayer 上运行。开发者可以利用整个以太坊工具链（如 Hardhat, Foundry, Remix, Ethers.js 等）和他们现有的 Solidity 代码库，无缝地构建和部署应用。

### 如何将我现有的 DApp 迁移到 GateLayer？
迁移一个基于 EVM 的 DApp 是一个非常简单的过程：
1.  **配置网络**：在您的开发框架（如 Hardhat）中，添加 GateLayer 的网络详情（RPC URL, Chain ID）。
2.  **部署合约**：将您现有的、未经修改的智能合约重新部署到 GateLayer。
3.  **更新前端**：更新您 DApp 的前端，使其指向新的合约地址并连接到 GateLayer 的 RPC 端点。
由于通常不需要更改代码，迁移过程既快速又高效。

### 为什么我应该在 GateLayer 上构建我的 DApp？
GateLayer 为开发者提供了几个核心优势：
- **极低的成本**：通过使用 GateChain 的 Blob 进行数据可用性保障，交易费用远低于以太坊甚至其他 L2，使您的 DApp 对用户更具吸引力。
- **高性能**：拥有超过 5,700 的 TPS 和 1 秒的出块时间，您的 DApp 可以提供响应迅速、近乎即时的用户体验。
- **熟悉的工具**：无需学习新的语言或工具，您可以使用您所熟知的 EVM 生态系统进行构建。
- **互操作性**：通过原生集成 LayerZero 等协议，您的 DApp 可以轻松地与多链生态系统连接。

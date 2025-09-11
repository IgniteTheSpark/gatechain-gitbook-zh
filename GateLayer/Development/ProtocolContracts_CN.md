# 协议合约地址

本文档列出了 Gate Layer 部署在 GateChain (L1) 上的核心协议智能合约地址。这些合约共同构成了 Gate Layer 系统的基础。

---

## 核心 L1 合约

这些是与 Gate Layer 功能直接相关的主要合约。

| 合约名称                       | 地址                                       | 简介                                                               |
| ------------------------------ | ------------------------------------------ | ------------------------------------------------------------------ |
| **L1StandardBridge**           | `0x75ded3be4d69e8403c9079ae5cf5e059a23f6a67` | 处理 ETH 和标准 ERC20 代币在 L1 和 L2 之间的存取款。                 |
| **L1CrossDomainMessenger**     | `0x60cad8d7d622be3d62786b2e7a6617765d0f4d94` | 在 L1 和 L2 之间传递任意消息，是跨链通信的基础。                   |
| **OptimismPortal**             | `0x0d69cdd07cf93075fe53769945575274fc563ba9` | L2 交易数据提交到 L1 的入口点，并处理提款证明，是系统的核心。      |
| **DisputeGameFactory**         | `0x6f419bb5d754033e39f758d20133d7fea8e2bb55` | 故障证明系统的核心，用于创建和管理针对 L2 产出根的挑战。           |
| **SystemConfig**               | `0xaeb4b93732c30c5d74ab3220cd73aa0fcdd275f8` | 存储 L2 的 Gas 相关参数，如 `overhead` 和 `scalar`。                 |
| **L1ERC721Bridge**             | `0xa19cbb87428a6b943912f2f44241ca1cb3a8cec1` | 处理 ERC721 NFT 代币在 L1 和 L2 之间的跨链。                         |
| **OptimismMintableERC20Factory** | `0xca1bbd1187ccbf2297c828c48f5c0896d9985636` | 在 L2 上为存款的 ERC20 代币创建对应的“可铸造”版本。               |
| **ProxyAdmin**                 | `0x1be6f451a7578cb48ddbd8fea8d2fd25462e5d16` | 管理以上核心合约的升级权限。                                       |
| **AddressManager**             | `0x6cbd08ae08136ab974ae121a0895d3836c8a00ce` | (旧版) 用于解析和管理系统合约地址的注册表。                        |

---

## 逻辑实现合约 (Implementations)

以下是上述代理合约 (Proxy Contracts) 当前指向的逻辑实现合约地址。这些地址可能会随着协议升级而改变。

| 合约名称                            | 实现地址                                   |
| ----------------------------------- | ------------------------------------------ |
| **L1StandardBridge Impl**           | `0x0b09ba359a106c9ea3b181cbc5f394570c7d2a7a` |
| **L1CrossDomainMessenger Impl**     | `0x5d5a095665886119693f0b41d8dfee78da033e8b` |
| **OptimismPortal Impl**             | `0xb1dfde4e7c3018b97fa68b12f7d5648c96e4674e` |
| **DisputeGameFactory Impl**         | `0x4bba758f006ef09402ef31724203f316ab74e4a0` |
| **SystemConfig Impl**               | `0xec6c6d47ec88f474bffa4defd38930fb2e79084c` |
| **L1ERC721Bridge Impl**             | `0x7ae1d3bd877a4c5ca257404ce26be93a02c98013` |
| **OptimismMintableERC20Factory Impl** | `0x5493f4677a186f64805fe7317d6993ba4863988f` |

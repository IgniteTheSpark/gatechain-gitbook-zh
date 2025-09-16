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

---

## 与 OP Stack 对齐：建议补充的清单（对标 opBNB 等）

基于 OP Stack 架构，除上述 L1 核心合约外，通常还会包含以下组件（部分为 L1 合约/账户，部分为 L2 预部署系统合约）。考虑到网络尚未完全上线，以下地址以“待公布 (TBD)”或“依系统参数推导”为占位，待主网/测试网发布后更新。

### 1) 其他 L1 合约 / 账户（建议补充）

| 名称 | 地址 | 说明 |
| --- | --- | --- |
| **ProtocolVersions** | TBD | 管理协议版本与兼容性门控，用于治理与组件版本约束（新版本 OP Stack 常见）。 |
| **BatchInbox（批次收件账户）** | 由 `SystemConfig.batcherHash` 推导 | Batcher 在 L1 发布交易批次/Blobs 的目标账户，通常为账户而非合约，用于数据可用性锚定。 |

> 说明：不同 OP Stack 版本（含故障证明演进）在 L1 侧还可能出现 `OptimismPortal2`、或与挑战/争议游戏相关的扩展合约，后续按 GateLayer 实际落地版本补充。

### 2) L2 核心与预部署系统合约（建议新增一节列出）

| 合约名称 | 预期地址/占位 | 简介 |
| --- | --- | --- |
| **L2CrossDomainMessenger** | 预部署（默认固定地址，待确认） | L2 ↔ L1 跨域消息通道在 L2 侧的对应端。 |
| **L2StandardBridge** | 预部署（默认固定地址，待确认） | 标准资产跨链桥的 L2 端。 |
| **L2ERC721Bridge** | 预部署（默认固定地址，待确认） | ERC721 NFT 跨链桥的 L2 端。 |
| **OptimismMintableERC20Factory (L2)** | 预部署（默认固定地址，待确认） | 为存款资产在 L2 上创建可铸造镜像代币。 |
| **L2ToL1MessagePasser** | 预部署（默认固定地址，待确认） | L2 → L1 消息/提款的消息通道基础设施。 |
| **GasPriceOracle** | 预部署（默认固定地址，待确认） | 提供 L2 费用参数（如 baseFee、overhead/scalar 等）的只读接口。 |
| **SequencerFeeVault** | 预部署（默认固定地址，待确认） | 存放 Sequencer 收取的 L2 费用收入。 |

> 注：OP Stack Bedrock 起，以上多为“预部署（predeploy）”，地址通常使用 `0x4200…` 前缀的固定值（若 GateLayer 未自定义，则与上游一致）。为避免误导，这里暂以“预部署（待确认）”占位，待网络发布后统一更新为确切地址。

---

## 后续维护建议

- 网络发布/升级后，将本页地址分为：`L1 合约`、`L2 预部署`、`实现合约（Implementation）` 三个小节逐项更新；并在每次重大升级（例如故障证明/挑战机制版本更迭）后同步维护。
- 在 `SystemConfig` 变更（如 batcher/sequencer/proposer 身份或参数）后，及时更新由其推导得到的账户/地址说明（如 BatchInbox）。
- 为方便开发者自助校验，可在文末附“检验脚本示例”（如通过 JSON-RPC 读取预部署合约代码哈希、读取 `SystemConfig` 参数等）。

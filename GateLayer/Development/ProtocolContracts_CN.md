# 协议合约地址

本文档汇总 Gate Layer 在 GateChain（L1）与 Gate Layer（L2）上的系统合约与地址。为便于查阅，按照“测试网 / 主网（占位）/ 通用预部署”区分。

## 概述

Gate Layer 系统合约是构建在 GateChain L1 与 Gate Layer L2 之间的核心基础设施合约集合，负责跨链通信、资产桥接、系统配置与协议升级等关键功能。

## 系统架构

系统合约分为两个层次：
- L1 系统合约：部署在 GateChain 主网（或测试网）上，承担跨域消息、桥接、状态提交与配置等职能
- L2 预部署合约：预置于 Gate Layer L2 上，地址固定（0x4200… 段），负责 L2 侧消息、桥接、费用与只读参数等

---

## 测试网 · L1 核心合约

以下为当前测试网已部署、与 Gate Layer 功能直接相关的主要合约：

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

## 测试网 · 逻辑实现合约 (Implementations)

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

## 主网（占位）· 逻辑实现合约 (Implementations)

主网上线后将公布各代理合约当前指向的实现地址，现保留占位以便对齐结构：

| 合约名称 | 实现地址 |
| --- | --- |
| **L1StandardBridge Impl** | TBD |
| **L1CrossDomainMessenger Impl** | TBD |
| **OptimismPortal Impl** | TBD |
| **DisputeGameFactory Impl** | TBD |
| **SystemConfig Impl** | TBD |
| **L1ERC721Bridge Impl** | TBD |
| **OptimismMintableERC20Factory Impl** | TBD |

## 测试网 · L2 预部署合约（标准固定地址）

以下为 Gate Layer 测试网的 L2 预部署（predeploy）合约标准固定地址：

| 合约名称 | 地址 | 简介 |
| --- | --- | --- |
| **L2CrossDomainMessenger** | `0x4200000000000000000000000000000000000007` | L2 侧跨域消息通道，与 L1 XDM 配对。 |
| **L2StandardBridge** | `0x4200000000000000000000000000000000000010` | 标准资产桥 L2 端，处理 ETH/ERC20 跨链与铸销。 |
| **L2ToL1MessagePasser** | `0x4200000000000000000000000000000000000016` | L2→L1 提款/消息的底层通道与存证。 |
| **L1Block** | `0x4200000000000000000000000000000000000015` | 提供 L1 区块信息与基础费信息（只读）。 |
| **GasPriceOracle** | `0x420000000000000000000000000000000000000F` | L2 费用只读接口（baseFee、overhead/scalar 等）。 |
| **L2ERC721Bridge** | `0x4200000000000000000000000000000000000014` | L2 侧 ERC721 桥。 |
| **OptimismMintableERC20Factory (L2)** | `0x4200000000000000000000000000000000000012` | L2 上为存款资产创建可铸造镜像代币。 |
| **SequencerFeeVault** | `0x4200000000000000000000000000000000000011` | 序列器费用金库。 |
| **BaseFeeVault** | `0x4200000000000000000000000000000000000019` | 基础费用金库。 |
| **L1FeeVault** | `0x420000000000000000000000000000000000001A` | L1 费用金库。 |
| **OperatorFeeVault** | `0x420000000000000000000000000000000000001B` | 运营费用金库。 |
| **ProxyAdmin** | `0x4200000000000000000000000000000000000018` | 代理管理员（升级管理）。 |

## 主网 · L2 预部署合约（标准固定地址）

当前与测试网一致；如后续因自定义或版本差异发生变化，将在此处更新。

| 合约名称 | 地址 | 简介 |
| --- | --- | --- |
| **L2CrossDomainMessenger** | `0x4200000000000000000000000000000000000007` | L2 侧跨域消息通道，与 L1 XDM 配对。 |
| **L2StandardBridge** | `0x4200000000000000000000000000000000000010` | 标准资产桥 L2 端，处理 ETH/ERC20 跨链与铸销。 |
| **L2ToL1MessagePasser** | `0x4200000000000000000000000000000000000016` | L2→L1 提款/消息的底层通道与存证。 |
| **L1Block** | `0x4200000000000000000000000000000000000015` | 提供 L1 区块信息与基础费信息（只读）。 |
| **GasPriceOracle** | `0x420000000000000000000000000000000000000F` | L2 费用只读接口（baseFee、overhead/scalar 等）。 |
| **L2ERC721Bridge** | `0x4200000000000000000000000000000000000014` | L2 侧 ERC721 桥。 |
| **OptimismMintableERC20Factory (L2)** | `0x4200000000000000000000000000000000000012` | L2 上为存款资产创建可铸造镜像代币。 |
| **SequencerFeeVault** | `0x4200000000000000000000000000000000000011` | 序列器费用金库。 |
| **BaseFeeVault** | `0x4200000000000000000000000000000000000019` | 基础费用金库。 |
| **L1FeeVault** | `0x420000000000000000000000000000000000001A` | L1 费用金库。 |
| **OperatorFeeVault** | `0x420000000000000000000000000000000000001B` | 运营费用金库。 |
| **ProxyAdmin** | `0x4200000000000000000000000000000000000018` | 代理管理员（升级管理）。 |

---

## 主网（占位）· L1 合约地址

主网 L1 合约将在上线后公布；此处提供占位以便对齐目录结构：

| 合约名称 | 地址 | 简介 |
| --- | --- | --- |
| **L1StandardBridge** | TBD | 处理 ETH 与标准 ERC20 在 L1/L2 的存取款。 |
| **L1CrossDomainMessenger** | TBD | L1/L2 任意消息通道。 |
| **OptimismPortal / OptimismPortal2** | TBD | L2 数据提交入口与提款证明处理。 |
| **DisputeGameFactory** | TBD | 故障证明/挑战工厂。 |
| **SystemConfig** | TBD | L2 费用参数与系统配置。 |
| **L1ERC721Bridge** | TBD | ERC721 跨链桥 L1 端。 |
| **OptimismMintableERC20Factory** | TBD | L2 镜像代币工厂对应 L1 端。 |
| **ProxyAdmin** | TBD | 升级权限管理。 |

---

（上线后回填）


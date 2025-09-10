# 钱包

GateChain 钱包允许您监控您在 GateChain 上的资产。资产可以是 GateChain 的原生代币，也可以是来自 Ethereum、Solana、Polygon 和各种支持 IBC 的链的跨链资产。

GateChain 支持多种不同的钱包。用户可以选择使用 Ethereum 或 Cosmos 原生钱包在 GateChain 上提交交易。

## 概述

GateChain 的账户类型使用 Ethereum 的 ECDSA secp256k1 曲线来生成密钥。简单来说，GateChain 的账户与 Ethereum 账户是原生兼容的，允许 Ethereum 原生钱包（如 MetaMask）与 GateChain 交互。流行的 Cosmos 钱包如 Keplr 和 Leap 也已经集成了 GateChain。

## 基于 Ethereum 的钱包

如上所述，基于 Ethereum 的钱包可以用来与 GateChain 交互。目前，GateChain 支持最流行的基于 Ethereum 的钱包，包括：

* [MetaMask](https://metamask.io/zh-CN/)
* [Ledger](https://www.ledger.com/)
* [Trezor](https://www.trezor.io/)
* [Torus](https://tor.us/)

使用 Ethereum 原生钱包在 GateChain 上签名交易的过程包括：

1. 将交易转换为 EIP712 TypedData
2. 使用 Ethereum 原生钱包签名 EIP712 TypedData
3. 将交易打包成原生 Cosmos 交易（包括签名），并将交易广播到链上

这个过程对最终用户来说是透明的。如果您之前使用过 Ethereum 原生钱包，用户体验将是相同的。

## GateChain Web 钱包

访问 [GateChain Web 钱包](https://www.gate.io/zh/web3)

## GateChain CLI 钱包

```bash
gatecli account create
```

响应：

```bash
- name: "1739429752"
  type: local
  address: gt11p2ddfwz82ezguvwhjued5y6lw4wgk89hqylcxtevkfcf45u9pdk04u2nn99cezv9e9a0zl
  pubkey: gt1pub1u8s6p73qrernvfy4drmnwhtuc3qd5t2sfu8l2lmww6l2hjjmpc2cutqd8gxsn6l9xj
  mnemonic: ""
  threshold: 0
  pubkeys: []


**重要提示** 请将此助记词短语保存在安全的地方。
如果您忘记了密码，这是恢复账户的唯一方式。

theme ghost symbol also quit vessel antenna token close copper plastic patch wolf payment cluster nasty elbow that theory slogan hybrid furnace anger always
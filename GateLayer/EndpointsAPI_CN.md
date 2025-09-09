# Endpoints & API

本文档提供了连接到 Gate Layer 网络所需的所有关键端点，包括 JSON-RPC 和区块浏览器 API。

---

## JSON-RPC Endpoints

### Gate Layer Testnet
- **Chain ID**: `10087`
- **RPC URLs**:
  - `http://gatelayer-testnet.gatenode.cc`
  - `(备用 RPC 1 待定)`
  - `(备用 RPC 2 待定)`
- **浏览器 URL**: `https://gatescan.org/gatelayer-testnet`

### Gate Layer Mainnet
- **Chain ID**: `(待定)`
- **RPC URLs**:
  - `(主网 RPC 1 待定)`
  - `(主网 RPC 2 待定)`
- **浏览器 URL**: `(待定)`

---

## JSON-RPC API 兼容性

Gate Layer 支持标准的以太坊 JSON-RPC API。开发者可以使用任何与以太坊兼容的工具（如 Hardhat, Foundry, Ethers.js, Web3.js）与 Gate Layer 节点进行交互。

### 请求示例 (`curl`)

以下是一个使用 `curl` 调用 `eth_blockNumber` 方法来获取最新区块号的示例：

```bash
curl -X POST \
  -H "Content-Type: application/json" \
  --data '{"jsonrpc":"2.0","method":"eth_blockNumber","params":[],"id":1}' \
  http://gatelayer-testnet.gatenode.cc
```

### 完整文档

要获取所有可用 JSON-RPC 方法的完整列表和详细说明，请参阅[以太坊官方文档](https://ethereum.org/en/developers/docs/apis/json-rpc/)。

---

## 区块浏览器 API

Gate Layer 的区块浏览器 GateScan 提供了一套与 Etherscan 完全兼容的 API。

### API 端点

**Testnet:**
```
https://gatescan.org/gatelayer-testnet/api
```

**Mainnet:**
```
(待定)
```

### 完整文档

要获取所有可用端点和参数的完整列表，请参阅 [Blockscout API 文档](https://eth.blockscout.com/api-docs)。只需将基础 URL 替换为 GateScan 的 URL 即可。

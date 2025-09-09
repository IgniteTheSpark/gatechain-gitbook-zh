# Endpoints & API

This document provides all the key endpoints required to connect to the Gate Layer network, including JSON-RPC and the block explorer API.

---

## JSON-RPC Endpoints

### Gate Layer Testnet
- **Chain ID**: `10087`
- **RPC URLs**:
  - `http://gatelayer-testnet.gatenode.cc`
  - `(Standby RPC 1 TBD)`
  - `(Standby RPC 2 TBD)`
- **Explorer URL**: `https://gatescan.org/gatelayer-testnet`

### Gate Layer Mainnet
- **Chain ID**: `(TBD)`
- **RPC URLs**:
  - `(Mainnet RPC 1 TBD)`
  - `(Mainnet RPC 2 TBD)`
- **Explorer URL**: `(TBD)`

---

## JSON-RPC API Compatibility

Gate Layer supports the standard Ethereum JSON-RPC API. Developers can interact with the Gate Layer node using any Ethereum-compatible tool (such as Hardhat, Foundry, Ethers.js, Web3.js).

### Request Example (`curl`)

Below is an example of using `curl` to call the `eth_blockNumber` method to get the latest block number:

```bash
curl -X POST \
  -H "Content-Type: application/json" \
  --data '{"jsonrpc":"2.0","method":"eth_blockNumber","params":[],"id":1}' \
  http://gatelayer-testnet.gatenode.cc
```

### Full Documentation

For a complete list and detailed explanation of all available JSON-RPC methods, please refer to the [official Ethereum documentation](https://ethereum.org/en/developers/docs/apis/json-rpc/).

---

## Block Explorer API

Gate Layer's block explorer, GateScan, provides a set of APIs that are fully compatible with Etherscan.

### API Endpoint

**Testnet:**
```
https://gatescan.org/gatelayer-testnet/api
```

**Mainnet:**
```
(TBD)
```

### Full Documentation

For a complete list of all available endpoints and parameters, please refer to the [Blockscout API Documentation](https://eth.blockscout.com/api-docs). Simply replace the base URL with the GateScan URL.

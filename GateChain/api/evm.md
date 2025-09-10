# JSON-RPC 服务器

由于 GateChain EVM 基本兼容以太坊，本文档中的大部分内容来自以太坊 EVM 文档的贡献。

## JSON-RPC 方法

| 方法 | 命名空间 | 是否已实现 | 备注 |
|--------|-----------|-------------|--------|
| [web3_clientVersion](#web3_clientVersion) | Web3 | ✔ | |
| [web3_sha3](#web3_sha3) | Web3 | ✔ | |
| [net_version](#net_version) | Net | ✔ | |
| [eth_protocolVersion](#eth_protocolVersion) | Eth | ✔ | |
| [eth_syncing](#eth_syncing) | Eth | ✔ | |
| [eth_gasPrice](#eth_gasPrice) | Eth | ✔ | |
| [eth_accounts](#eth_accounts) | Eth | ✔ | |
| [eth_blockNumber](#eth_blockNumber) | Eth | ✔ | |
| [eth_getBalance](#eth_getBalance) | Eth | ✔ | |
| [eth_getStorageAt](#eth_getStorageAt) | Eth | ✔ | |
| [eth_getTransactionCount](#eth_getTransactionCount) | Eth | ✔ | |
| [eth_getBlockTransactionCountByNumber](#eth_getBlockTransactionCountByNumber) | Eth | ✔ | |
| [eth_getBlockTransactionCountByHash](#eth_getBlockTransactionCountByHash) | Eth | ✔ | |
| [eth_getCode](#eth_getCode) | Eth | ✔ | |
| [eth_sign](#eth_sign) | Eth | ✔ | |
| [eth_sendTransaction](#eth_sendTransaction) | Eth | ✔ | |
| [eth_sendRawTransaction](#eth_sendRawTransaction) | Eth | ✔ | |
| [eth_call](#eth_call) | Eth | ✔ | |
| [eth_estimateGas](#eth_estimateGas) | Eth | ✔ | |
| [eth_getBlockByNumber](#eth_getBlockByNumber) | Eth | ✔ | |
| [eth_getBlockByHash](#eth_getBlockByHash) | Eth | ✔ | |
| [eth_getTransactionByHash](#eth_getTransactionByHash) | Eth | ✔ | |
| [eth_getTransactionByBlockHashAndIndex](#eth_getTransactionByBlockHashAndIndex) | Eth | ✔ | |
| [eth_getTransactionReceipt](#eth_getTransactionReceipt) | Eth | ✔ | |

### web3_clientVersion

返回当前客户端版本。

#### 参数
无

#### 返回值
String - 当前客户端版本

#### 示例
```json
// 请求
curl -X POST --data '{"jsonrpc":"2.0","method":"web3_clientVersion","params":[],"id":67}'

// 结果
{
    "jsonrpc": "2.0",
    "id": 1,
    "result": "gate enhanced-1.0.5-135-gf00ae80"
}
```

### web3_sha3

返回给定数据的 Keccak-256 哈希值（注意：这不是标准的 SHA3-256）。

#### 参数
DATA - 需要转换为 SHA3 哈希的数据
```json
params: [
  "0x68656c6c6f20776f726c64"
]
```

#### 返回值
DATA - 给定字符串的 SHA3 结果。

#### 示例
```json
// 请求
curl -X POST --data '{"jsonrpc":"2.0","method":"web3_sha3","params":["0x68656c6c6f20776f726c64"],"id":64}'

// 结果
{
  "id":64,
  "jsonrpc": "2.0",
  "result": "0x47173285a8d7341e5e972fc677286384f802f8ef42a5ec5f03bbfa254cb01fad"
}
```

### net_version

返回当前网络 ID。

#### 参数
无

#### 返回值
String - 当前网络 ID
- "86": 主网

#### 示例
```json
// 请求
curl -X POST --data '{"jsonrpc":"2.0","method":"net_version","params":[],"id":67}'

// 结果
{
  "id":67,
  "jsonrpc": "2.0",
  "result": "86"
}
```

### eth_protocolVersion

返回当前以太坊协议版本。

#### 参数
无

#### 返回值
String - 当前以太坊协议版本

#### 示例
```json
// 请求
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_protocolVersion","params":[],"id":67}'

// 结果
{
  "id":67,
  "jsonrpc": "2.0",
  "result": "0x41"
}
```

### eth_syncing

返回一个包含同步状态数据的对象，如果未在同步则返回 false。

#### 参数
无

#### 返回值
Object|Boolean，一个包含同步状态数据的对象，或者当未在同步时返回 FALSE：
- startingBlock: QUANTITY - 导入开始时的区块（仅在同步到达区块头后重置）
- currentBlock: QUANTITY - 当前区块，与 eth_blockNumber 相同
- highestBlock: QUANTITY - 预估的最高区块

#### 示例
```json
// 请求
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_syncing","params":[],"id":1}'

// 结果
{
    "jsonrpc": "2.0",
    "id": 1,
    "result": {
        "currentBlock": 106699
    }
}

// 或者当未在同步时
{
  "id":1,
  "jsonrpc": "2.0",
  "result": false
}
```

### eth_gasPrice

返回当前的 gas 价格（以 wei 为单位）。

#### 参数
无

#### 返回值
QUANTITY - 当前 gas 价格的整数值（以 wei 为单位）。

#### 示例
```json
// 请求
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_gasPrice","params":[],"id":73}'

// 结果
{
  "id":73,
  "jsonrpc": "2.0",
  "result": "0x09184e72a000" // 10000000000000
}
```

### eth_accounts

返回客户端拥有的地址列表。

#### 参数
无

#### 返回值
Array of DATA, 20 字节 - 客户端拥有的地址列表。

#### 示例
```json
// 请求
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_accounts","params":[],"id":1}'

// 结果
{
    "jsonrpc": "2.0",
    "id": 1,
    "result": [
        "0xd9a549bcca822b696718802ec4aad4e1acc24367",
        "0xcfe9c51a8039e74b0cce28642be0504fc898094a",
        "0xa1968292d334ef64b56b01862d6ac1b2a8548b91"
    ]
}
```

### eth_blockNumber

返回最新区块的编号。

#### 参数
无

#### 返回值
QUANTITY - 客户端当前所在的区块号整数值。

#### 示例
```json
// 请求
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_blockNumber","params":[],"id":83}'

// 结果
{
  "id":83,
  "jsonrpc": "2.0",
  "result": "0x1a0e1" // 106721
}
```

### eth_getBalance

返回指定地址的账户余额。

#### 参数
- DATA, 20 字节 - 要查询余额的地址
- QUANTITY|TAG - 区块号整数，或字符串 "latest"、"earliest" 或 "pending"

示例：
```json
params: [
   '0x407d73d8a49eeb85d32cf465507dd71d507100c1',
   'latest'
]
```

#### 返回值
QUANTITY - 当前余额的整数值（以 wei 为单位）。

#### 示例
```json
// 请求
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_getBalance","params":["0x407d73d8a49eeb85d32cf465507dd71d507100c1", "latest"],"id":1}'

// 结果
{
  "id":1,
  "jsonrpc": "2.0",
  "result": "0x6c3a050b1aedb4000" // 124776820000000000000
}
```

### eth_getStorageAt

返回给定地址的存储位置的值。

#### 参数
- DATA, 20 字节 - 存储的地址
- QUANTITY - 存储中位置的整数值
- QUANTITY|TAG - 区块号整数，或字符串 "latest"、"earliest" 或 "pending"

#### 返回值
DATA - 该存储位置的值。

#### 示例
计算正确的位置取决于要检索的存储。考虑以下由地址 `0x295a70b2de5e3953354a6a8344e616ed314d7251` 部署在 `0x391694e7e0b0cce554cb130d723a9d27458f9298` 的合约。

```solidity
contract Storage {
    uint pos0;
    mapping(address => uint) pos1;

    function Storage() {
        pos0 = 1234;
        pos1[msg.sender] = 5678;
    }
}
```

检索 pos0 的值很直接：

```json
// 请求
curl -X POST --data '{"jsonrpc":"2.0", "method": "eth_getStorageAt", "params": ["0x295a70b2de5e3953354a6a8344e616ed314d7251", "0x0", "latest"], "id": 1}' localhost:8545

// 结果
{
  "jsonrpc":"2.0",
  "id":1,
  "result":"0x00000000000000000000000000000000000000000000000000000000000004d2"
}
```

检索映射中的元素较为复杂。映射中元素的位置计算方式为：
```
keccack(LeftPad32(key, 0), LeftPad32(map position, 0))
```

这意味着要检索 pos1["0x391694e7e0b0cce554cb130d723a9d27458f9298"] 的存储，我们需要用以下方式计算位置：
```
keccak(decodeHex("000000000000000000000000391694e7e0b0cce554cb130d723a9d27458f9298" + "0000000000000000000000000000000000000000000000000000000000000001"))
```

可以使用带有 web3 库的 geth 控制台进行计算：
```javascript
> var key = "000000000000000000000000391694e7e0b0cce554cb130d723a9d27458f9298" + "0000000000000000000000000000000000000000000000000000000000000001"
undefined
> web3.sha3(key, {"encoding": "hex"})
"0x6661e9d6d8b923d5bbaab1b96e1dd51ff6ea2a93520fdc9eb75d059238b8c5e9"
```

现在获取存储：
```json
// 请求
curl -X POST --data '{"jsonrpc":"2.0", "method": "eth_getStorageAt", "params": ["0x295a70b2de5e3953354a6a8344e616ed314d7251", "0x6661e9d6d8b923d5bbaab1b96e1dd51ff6ea2a93520fdc9eb75d059238b8c5e9", "latest"], "id": 1}' localhost:8545

// 结果
{
  "jsonrpc":"2.0",
  "id":1,
  "result":"0x000000000000000000000000000000000000000000000000000000000000162e"
}
```

### eth_getTransactionCount

返回从一个地址发送的交易数量。

#### 参数
- DATA, 20 字节 - 地址
- QUANTITY|TAG - 区块号整数，或字符串 "latest"、"earliest" 或 "pending"

示例：
```json
params: [
   '0x407d73d8a49eeb85d32cf465507dd71d507100c1',
   'latest' // 最新区块的状态
]
```

#### 返回值
QUANTITY - 从该地址发送的交易数量的整数值。

#### 示例
```json
// 请求
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_getTransactionCount","params":["0x407d73d8a49eeb85d32cf465507dd71d507100c1","latest"],"id":1}'

// 结果
{
  "id":1,
  "jsonrpc": "2.0",
  "result": "0x1" // 1
}
```

### eth_getBlockTransactionCountByNumber

返回与给定区块号匹配的区块中的交易数量。

#### 参数
QUANTITY|TAG - 区块号整数，或字符串 "earliest"、"latest" 或 "pending"

示例：
```json
params: [
   '0xe8', // 232
]
```

#### 返回值
QUANTITY - 该区块中的交易数量的整数值。

#### 示例
```json
// 请求
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_getBlockTransactionCountByNumber","params":["0xe8"],"id":1}'

// 结果
{
  "id":1,
  "jsonrpc": "2.0",
  "result": "0xa" // 10
}
```

### eth_getBlockTransactionCountByHash

返回与给定区块哈希匹配的区块中的交易数量。

#### 参数
DATA, 32 字节 - 区块的哈希值

示例：
```json
params: [
   '0xb903239f8543d04b5dc1ba6579132b143087c68db1b2168786408fcbce568238'
]
```

#### 返回值
QUANTITY - 该区块中的交易数量的整数值。

#### 示例
```json
// 请求
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_getBlockTransactionCountByHash","params":["0xb903239f8543d04b5dc1ba6579132b143087c68db1b2168786408fcbce568238"],"id":1}'

// 结果
{
  "id":1,
  "jsonrpc": "2.0",
  "result": "0xb" // 11
}
```

### eth_getCode

返回给定地址的代码。

#### 参数
- DATA, 20 字节 - 地址
- QUANTITY|TAG - 区块号整数，或字符串 "latest"、"earliest" 或 "pending"

示例：
```json
params: [
   '0xa94f5374fce5edbc8e2a8697c15331677e6ebf0b',
   '0x2'  // 2
]
```

#### 返回值
DATA - 给定地址的代码。

#### 示例
```json
// 请求
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_getCode","params":["0xa94f5374fce5edbc8e2a8697c15331677e6ebf0b", "0x2"],"id":1}'

// 结果
{
  "id":1,
  "jsonrpc": "2.0",
  "result": "0x600160008035811a818181146012578301005b601b6001356025565b8060005260206000f25b600060078202905091905056"
}
```

### eth_sign

sign 方法使用以下方式计算以太坊特定的签名：`sign(keccak256("\x19Ethereum Signed Message:\n" + len(message) + message)))`。

通过在消息前添加前缀，使计算出的签名可识别为以太坊特定的签名。这可以防止恶意 DApp 签署任意数据（例如交易）并使用签名冒充受害者的滥用情况。

> 注意：用于签名的地址必须处于解锁状态。

#### 参数
- account, message
- DATA, 20 字节 - 地址
- DATA, N 字节 - 要签名的消息

#### 返回值
DATA: 签名

#### 示例
```json
// 请求
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_sign","params":["0x9b2055d370f73ec7d8a03e965129118dc8f5bf83", "0xdeadbeaf"],"id":1}'

// 结果
{
  "id":1,
  "jsonrpc": "2.0",
  "result": "0xa3f20717a250c2b0b729b7e5becbff67fdaef7e0699da4de7ca5895b02a170a12d887fd3b17bfdce3481f10bea41f45ba9f709d39ce8325427b57afcfc994cee1b"
}
```

### eth_sendTransaction

创建新的消息调用交易或合约创建（如果数据字段包含代码）。

#### 参数
Object - 交易对象：
- from: DATA, 20 字节 - 交易发送方的地址
- to: DATA, 20 字节 - （创建新合约时可选）交易接收方的地址
- gas: QUANTITY - （可选，默认值：90000）为交易执行提供的 gas 整数值。未使用的 gas 将被返回
- gasPrice: QUANTITY - gas price provided by the sender in Wei.
- value: QUANTITY - （可选）随交易发送的值的整数值
- data: DATA - 合约的编译代码或被调用方法签名的哈希值和编码参数
- nonce: QUANTITY - （可选）nonce 的整数值。这允许覆盖使用相同 nonce 的自己的待处理交易
- gas: QUANTITY - gas provided by the sender.
- input: DATA - the data send along with the transaction.

示例：
```json
params: [{
  "from": "0xb60e8dd61c5d32be8058bb8eb970870f07233155",
  "to": "0xd46e8dd67c5d32be8058bb8eb970870f07244567",
  "gas": "0x76c0", // 30400
  "gasPrice": "0x9184e72a000", // 10000000000000
  "value": "0x9184e72a", // 2441406250
  "data": "0xd46e8dd67c5d32be8d46e8dd67c5d32be8058bb8eb970870f072445675058bb8eb970870f072445675",
  "gas": "0x76c0", // 30400
  "input": "0xd46e8dd67c5d32be8d46e8dd67c5d32be8058bb8eb970870f072445675058bb8eb970870f072445675"
}]
```

#### 返回值
DATA, 32 字节 - 交易哈希，如果交易尚不可用则返回零哈希。

如果您创建了一个合约，请在交易被挖矿后使用 eth_getTransactionReceipt 获取合约地址。

#### 示例
```json
// 请求
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_sendTransaction","params":[{见上文}],"id":1}'

// 结果
{
  "id":1,
  "jsonrpc": "2.0",
  "result": "0xe670ec64341771606e55d6b4ca35a1a6b75ee3d5145a99d05921026d1527331"
}
```

### eth_sendRawTransaction

为已签名的交易创建新的消息调用交易或合约创建。

#### 参数
DATA - 已签名的交易数据。

示例：
```json
params: ["0xd46e8dd67c5d32be8d46e8dd67c5d32be8058bb8eb970870f072445675058bb8eb970870f072445675"]
```

#### 返回值
DATA, 32 字节 - 交易哈希，如果交易尚不可用则返回零哈希。

如果您创建了一个合约，请在交易被挖矿后使用 eth_getTransactionReceipt 获取合约地址。

#### 示例
```json
// 请求
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_sendRawTransaction","params":[{见上文}],"id":1}'

// 结果
{
  "id":1,
  "jsonrpc": "2.0",
  "result": "0xe670ec64341771606e55d6b4ca35a1a6b75ee3d5145a99d05921026d1527331"
}
```

### eth_call

立即执行新的消息调用，而不在区块链上创建交易。

#### 参数
1. Object - 交易调用对象：
   - from: DATA, 20 字节 - （可选）交易发送方的地址
   - to: DATA, 20 字节 - 交易接收方的地址
   - gas: QUANTITY - （可选）为交易执行提供的 gas 整数值
   - gasPrice: QUANTITY - （可选）每个已支付 gas 使用的 gasPrice 整数值
   - value: QUANTITY - （可选）随交易发送的值的整数值
   - data: DATA - （可选）方法签名的哈希值和编码参数

2. QUANTITY|TAG - 区块号整数，或字符串 "latest"、"earliest" 或 "pending"

#### 返回值
DATA - 已执行合约的返回值。

#### 示例
```json
// 请求
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_call","params":[{见上文}],"id":1}'

// 结果
{
  "id":1,
  "jsonrpc": "2.0",
  "result": "0x"
}
```

### eth_estimateGas

生成并返回完成交易所需的 gas 估算值。该交易不会被添加到区块链中。

> 注意：由于各种原因（包括 EVM 机制和节点性能），估算值可能会显著高于交易实际使用的 gas 量。

#### 参数
参见 eth_call 参数，但所有属性都是可选的。如果未指定 gas 限制，geth 将使用待处理区块的区块 gas 限制作为上限。因此，当 gas 数量高于待处理区块 gas 限制时，返回的估算值可能不足以执行调用/交易。

#### 返回值
QUANTITY - 使用的 gas 量。

#### 示例
```json
// 请求
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_estimateGas","params":[{见上文}],"id":1}'

// 结果
{
  "id":1,
  "jsonrpc": "2.0",
  "result": "0x5208" // 21000
}
```

### eth_getBlockByHash

通过哈希返回关于区块的信息。

#### 参数
- DATA, 32 字节 - 区块的哈希值
- Boolean - 如果为 true 则返回完整的交易对象，如果为 false 则只返回交易的哈希值

示例：
```json
params: [
   '0xdc0818cf78f21a8e70579cb46a43643f78291264dda342ae31049421c82d21ae',
   false
]
```

#### 返回值
Object - 区块对象，如果未找到区块则返回 null：

- number: QUANTITY - 区块号。当其为待处理区块时为 null
- hash: DATA, 32 字节 - 区块的哈希值。当其为待处理区块时为 null
- parentHash: DATA, 32 字节 - 父区块的哈希值
- nonce: DATA, 8 字节 - 生成的工作量证明的哈希值。当其为待处理区块时为 null
- sha3Uncles: DATA, 32 字节 - 区块中叔块数据的 SHA3 值
- logsBloom: DATA, 256 字节 - 区块日志的布隆过滤器。当其为待处理区块时为 null
- transactionsRoot: DATA, 32 字节 - 区块的交易树根
- stateRoot: DATA, 32 字节 - 区块的最终状态树根
- receiptsRoot: DATA, 32 字节 - 区块的收据树根
- miner: DATA, 20 字节 - 获得挖矿奖励的受益人地址
- difficulty: QUANTITY - 该区块的难度整数值
- totalDifficulty: QUANTITY - 截至该区块的链总难度整数值
- extraData: DATA - 该区块的"额外数据"字段
- size: QUANTITY - 该区块大小的整数值（以字节为单位）
- gasLimit: QUANTITY - 该区块允许的最大 gas 值
- gasUsed: QUANTITY - 该区块中所有交易使用的总 gas 值
- timestamp: QUANTITY - 区块打包时的 unix 时间戳
- transactions: Array - 交易对象数组，或根据最后给定参数返回 32 字节的交易哈希值数组
- uncles: Array - 叔块哈希值数组

#### 示例
```json
// 请求
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_getBlockByHash","params":["0xe670ec64341771606e55d6b4ca35a1a6b75ee3d5145a99d05921026d1527331", true],"id":1}'

// 结果
{
"id":1,
"jsonrpc":"2.0",
"result": {
    "number": "0x1b4", // 436
    "hash": "0xe670ec64341771606e55d6b4ca35a1a6b75ee3d5145a99d05921026d1527331",
    "parentHash": "0x9646252be9520f6e71339a8df9c55e4d7619deeb018d2a3f2d21fc165dde5eb5",
    "nonce": "0xe04d296d2460cfb8472af2c5fd05b5a214109c25688d3704aed5484f9a7792f2",
    "sha3Uncles": "0x1dcc4de8dec75d7aab85b567b6ccd41ad312451b948a7413f0a142fd40d49347",
    "logsBloom": "0xe670ec64341771606e55d6b4ca35a1a6b75ee3d5145a99d05921026d1527331",
    "transactionsRoot": "0x56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421",
    "stateRoot": "0xd5855eb08b3387c0af375e9cdb6acfc05eb8f519e419b874b6ff2ffda7ed1dff",
    "miner": "0x4e65fda2159562a496f9f3522f89122a3088497a",
    "difficulty": "0x027f07", // 163591
    "totalDifficulty":  "0x027f07", // 163591
    "extraData": "0x0000000000000000000000000000000000000000000000000000000000000000",
    "size":  "0x027f07", // 163591
    "gasLimit": "0x9f759", // 653145
    "gasUsed": "0x9f759", // 653145
    "timestamp": "0x54e34e8e", // 1424182926
    "transactions": [{...},{ ... }],
    "uncles": ["0x1606e5...", "0xd5145a9..."]
  }
}
```

### eth_getBlockByNumber

通过区块号返回关于区块的信息。

#### 参数
- QUANTITY|TAG - 区块号整数，或字符串 "earliest"、"latest" 或 "pending"，作为默认区块参数
- Boolean - 如果为 true 则返回完整的交易对象，如果为 false 则只返回交易的哈希值

示例：
```json
params: [
   '0x1b4', // 436
   true
]
```

#### 返回值
参见 eth_getBlockByHash

#### 示例
```json
// 请求
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_getBlockByNumber","params":["0x1b4", true],"id":1}'

// 结果
参见 eth_getBlockByHash
```

### eth_getTransactionByHash

通过交易哈希返回关于交易的信息。

#### 参数
DATA, 32 字节 - 交易的哈希值

示例：
```json
params: [
   "0xb903239f8543d04b5dc1ba6579132b143087c68db1b2168786408fcbce568238"
]
```

#### 返回值
Object - 交易对象，如果未找到交易则返回 null：

- hash: DATA, 32 字节 - 交易的哈希值
- nonce: QUANTITY - 发送方在此之前进行的交易数量
- blockHash: DATA, 32 字节 - 该交易所在区块的哈希值。当其为待处理时为 null
- blockNumber: QUANTITY - 该交易所在的区块号。当其为待处理时为 null
- transactionIndex: QUANTITY - 交易在区块中的索引位置的整数值。当其为待处理时为 null
- from: DATA, 20 字节 - 发送方地址
- to: DATA, 20 字节 - 接收方地址。当其为合约创建交易时为 null
- value: QUANTITY - 以 Wei 为单位转移的值
- gas: QUANTITY - gas provided by the sender.
- gasPrice: QUANTITY - gas price provided by the sender in Wei.
- input: DATA - the data send along with the transaction.

#### 示例
```json
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_getTransactionByHash","params":["0xb903239f8543d04b5dc1ba6579132b143087c68db1b2168786408fcbce568238"],"id":1}'

// Result
{
"id":1,
"jsonrpc":"2.0",
"result": {
    "hash":"0xc6ef2fc5426d6ad6fd9e2a26abeab0aa2411b7ab17f30a99d3cb96aed1d1055b",
    "nonce":"0x",
    "blockHash": "0xbeab0aa2411b7ab17f30a99d3cb9c6ef2fc5426d6ad6fd9e2a26a6aed1d1055b",
    "blockNumber": "0x15df", // 5599
    "transactionIndex":  "0x1", // 1
    "from":"0x407d73d8a49eeb85d32cf465507dd71d507100c1",
    "to":"0x85h43d8a49eeb85d32cf465507dd71d507100c1",
    "value":"0x7f110", // 520464
    "gas": "0x7f110", // 520464
    "gasPrice":"0x09184e72a000",
    "input":"0x603880600c6000396000f300603880600c6000396000f3603880600c6000396000f360"
  }
}
```

### eth_getTransactionByBlockHashAndIndex

通过区块哈希和交易索引位置返回关于交易的信息。

#### 参数
- DATA, 32 字节 - 区块的哈希值
- QUANTITY - 交易索引位置的整数值

示例：
```json
params: [
   '0xe670ec64341771606e55d6b4ca35a1a6b75ee3d5145a99d05921026d1527331',
   '0x0' // 0
]
```

#### 返回值
参见 eth_getTransactionByHash

#### 示例
```json
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_getTransactionByBlockHashAndIndex","params":["0xc6ef2fc5426d6ad6fd9e2a26abeab0aa2411b7ab17f30a99d3cb96aed1d1055b", "0x0"],"id":1}'

// Result
参见 eth_getTransactionByHash
```

### eth_getTransactionReceipt

通过交易哈希返回交易的收据。

> 注意：收据不可用于待处理交易。

#### 参数
DATA, 32 字节 - 交易的哈希值

示例：
```json
params: [
   '0xb903239f8543d04b5dc1ba6579132b143087c68db1b2168786408fcbce568238'
]
```

#### 返回值
Object - 交易收据对象，如果未找到收据则返回 null：

- transactionHash: DATA, 32 字节 - 交易的哈希值
- transactionIndex: QUANTITY - 交易在区块中的索引位置的整数值
- blockHash: DATA, 32 字节 - 该交易所在区块的哈希值
- blockNumber: QUANTITY - 该交易所在的区块号
- from: DATA, 20 字节 - 发送方地址
- to: DATA, 20 字节 - 接收方地址。当其为合约创建交易时为 null
- cumulativeGasUsed: QUANTITY - 该交易在区块中执行时使用的总 gas 量
- gasUsed: QUANTITY - 该特定交易单独使用的 gas 量
- contractAddress: DATA, 20 字节 - 如果交易为合约创建交易，则为创建的合约地址，否则为 null
- logs: Array - 该交易生成的日志对象数组
- logsBloom: DATA, 256 字节 - 轻客户端快速检索相关日志的布隆过滤器

它还返回：
- root: DATA 32 字节，交易后状态根（pre Byzantium）
- status: QUANTITY 1（成功）或 0（失败）

#### 示例
```json
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"eth_getTransactionReceipt","params":["0xb903239f8543d04b5dc1ba6579132b143087c68db1b2168786408fcbce568238"],"id":1}'

// Result
{
"id":1,
"jsonrpc":"2.0",
"result": {
     transactionHash: '0xb903239f8543d04b5dc1ba6579132b143087c68db1b2168786408fcbce568238',
     transactionIndex:  '0x1', // 1
     blockNumber: '0xb', // 11
     blockHash: '0xc6ef2fc5426d6ad6fd9e2a26abeab0aa2411b7ab17f30a99d3cb96aed1d1055b',
     cumulativeGasUsed: '0x33bc', // 13244
     gasUsed: '0x4dc', // 1244
     contractAddress: '0xb60e8dd61c5d32be8058bb8eb970870f07233155', // 如果没有创建合约则为 null
     logs: [{
         // 日志，与 getFilterLogs 等返回的格式相同
     }, ...],
     logsBloom: "0x00...0", // 256 字节布隆过滤器
     status: '0x1'
  }
}
```
# GateChain CLI 参考

> 重要提示：
> - 所有 CLI 命令都需要设置大于 0NANOGT 的交易费用
> - 资产金额是通过乘以 10E18 转换的正整数值
> - 资产金额支持科学计数法，例如：10E9 NANOGT = 1GT

## 基本命令

### 1. 启动本地 RPC 服务

**命令**
```bash
gatecli rest-server
```

**说明**
- 当 RPC 服务启动时，由于存储锁定，命令行无法执行

### 2. 查询节点状态

**命令**
```bash
gatecli status
```

**说明**
- 查询本地节点服务状态

### 3. 查询版本

**命令**
```bash
gatecli version
```

**说明**
- 查询命令行的版本信息

### 4. 获取帮助

**命令**
```bash
gatecli [Help1] [Help2] [Help3]... --help
```

**说明**
- 使用帮助命令获取有关命令行操作的更多信息

#### 错误
```bash
Must specify these options: --chain-id  when --trust-node is false
```

如果信任此节点，请输入 '--trust-node=true'，否则输入 '--chain-id' 来解决上述错误。


## 账户管理

### 1. 账户类型
- 单签名账户
  - 前缀：'gt1'
  - 示例：'gt11twm7dma44k7wg5jppeyphrct9nx2l4m8szy44h72qv9eatyla3hkaevg3vx99mlslwsnfq'
- 多重签名账户
  - 前缀：'gt2'
  - 示例：'g211twm7dma44k7wg5jppeyphrct9nx2l4m8szy44h72qv9eatyla3hkaevg3vx99mlslwsnfq'
- 单签名保险箱账户
  - 前缀：'vault1'
  - 示例：'vault11fg056uaatk9s3k6l34eqkc7qchjup8e39afauf9naz4jmpklsze3uwx6405vll88l5lvww'
- 多重签名保险箱账户
  - 前缀：'vault2'
  - 示例：'vault21fg056uaatk9s3k6l34eqkc7qchjup8e39afauf9naz4jmpklsze3uwx6405vll88l5lvww'

### 2. 创建单签名账户

**命令**
```bash
gatecli account create
```

**响应示例**
```
- name: vault
  type: local
  address: gt1124j4d2tjt0c7vrq5er4n2ksyp539gkv28krrsye5ym2y2mf5ekvg2s7neydamthz7a7xld
  pubkey: gt1pub1u8s6p73qf92hfdy6sp3s3657ssqrnpjtwevc86ht6d9txyrnt7239528z6wqxjhlfn
  mnemonic: ""
  threshold: 0
  pubkeys: []

重要提示：请将此助记词短语保存在安全的地方。
如果您忘记了密码，这是恢复账户的唯一方式。

flee agree charge truth answer flush inflict shove nice valid auto love laugh review frame sword later man inside couch slogan level guitar diet
```

**注意**
- 助记词短语仅在创建账户时显示，使用 show 或 list 命令时不会显示
- 请将助记词短语保存在安全的位置，因为它是恢复账户所必需的

### 3. 创建多重签名账户

**命令**
```bash
gatecli account create [account] --multisig [account list] --multisig-threshold [minimum effective amount]
```

**参数**

| 参数 | 类型 | 描述 |
|------|------|------|
| account | String | 要创建的账户名称 |
| account list | String | 以逗号分隔的现有账户地址列表 |
| minimum effective amount | Int| 所需的最小签名数量 |

**示例**
```bash
gatecli account create gt11twm7dma44k7wg5jppeyphrct9nx2l4m8szy44h72qv9eatyla3hkaevg3vx99mlslwsnfq --multisig gt110nxr6.....,gt113454xdr..... --multisig-threshold 2
```

**注意**
- 账户列表必须引用已创建的账户
- 创建后，使用 `gatecli account publish-multisig` 获取以 'gt2' 开头的最终多重签名地址

### 4. 查询账户列表

**命令**
```bash
gatecli account list
```

**响应示例**
```
- name: con
  type: local
  address: gt11ewdeyjdlxs2x4yglnhtulp0tclgdp7c2t8d5c6495r83qvrfr7p0qt40uddu3k44s7rxg6
  pubkey: gt1pub1u8s6p73qg4vmn6pjsnfehrh0nph8sqnp0nta0clfayjhsgtmjpfeek289gdsmr7au2
  mnemonic: ""
  threshold: 0
  pubkeys: []
```

### 5. 查询账户信息

**命令**
```bash
gatecli account show [account] --chain-id [chain ID]
```

**参数**

| 参数 | 类型 | 描述 |
|------|------|------|
| account | String | 要查询的账户地址 |
| chain-id | String | 链 ID |

**示例**
```bash
gatecli account show gt11twm7dma44k7wg5jppeyphrct9nx2l4m8szy44h72qv9eatyla3hkaevg3vx99mlslwsnfq --chain-id testnet
```

**响应示例**
```
Account:
  Address:        gt11dsck7f7txdc7hz5wdsspey0w5eyhw7yj4xrc3a6r4w4t39z9k45r86htlzxjf6z57an2r7
  Pubkey:        gt1pub1u8s6p73q634mnsu7knd4fz23ng3gwvcyjfldrhlyffty3mnpx5phtr6ydyyqlj3uv8
  Tokens:        69998999899999967NANOGT
  AccountNumber: 4
  Sequence:      0
  AccountType:   0
  VaultAddress:  [vault1124j4d2tjt0c7vrq5er4n2ksyp539gkv28krrsye5ym2y2mf5ekvg2s7neydamthzwag272]
  ReceivedRevocableTokens:
```

**注意**
- AccountType: 0=单签名标准账户, 1=单签名保险箱账户, 2=多重签名标准账户, 3=多重签名保险箱账户
- VaultAddress: 如果存在，表示此账户是列出的保险箱账户的找回账户

### 6. 修改账户密码

**命令**
```bash
gatecli account update [name]
```

**示例**
```bash
gatecli account update 1583472684
```

**响应示例**
```
密码更新成功！
```

### 7. 删除账户

**命令**
```bash
gatecli account delete [name]
```

**示例**
```bash
gatecli account delete 1583472684
```

**响应示例**
```
账户已永久删除！
```

**注意**
- 账户私钥将被永久删除

### 8. 查询账户余额

**命令**
```bash
gatecli account balance [account] --chain-id [chain ID]
```

**参数**

| 参数 | 类型 | 描述 |
|------|------|------|
| account | String | 要查询的账户地址 |
| chain-id | String | 链 ID |

**示例**
```bash
gatecli account balance gt11twm7dma44k7wg5jppeyphrct9nx2l4m8szy44h72qv9eatyla3hkaevg3vx99mlslwsnfq --chain-id testnet
```

**响应示例**
```
1000000NANOGT //账户余额
```

### 9. 查询账户公钥

**命令**
```bash
gatecli account show-key [name]
```

**示例**
```bash
gatecli account show-key 1583472684
```

### 10. 发布多重签名账户

**命令**
```bash
gatecli account publish-multisig [account] [public key] --from [sender account] --fees [tx fees] --chain-id [chain ID]
```

**参数**

| 参数 | 类型 | 描述 |
|------|------|------|
| account | String | 要发布的多重签名账户地址 |
| public key | String | 多重签名账户的公钥 |
| sender account | String | 发送交易的账户地址 |
| tx fees | String | 交易费用 |
| chain-id | String | 链 ID |

**示例**
```bash
gatecli account publish-multisig gt11twm7dma44k7wg5jppeyphrct9nx2l4m8szy44h72qv9eatyla3hkaevg3vx99mlslwsnfq gt1pub1ytql0csgqgfzd666axrjzqegteuuxvghau9u0q67lltpjqla3ykzz3t8efmh6sqhyt4uhnh3q5fzd666axrjzqkhwmygytf0grzudhv69h9ttcy4xhze0v4mtf4jza6mrp0j3lq68qfzd666axrjzqn6wmq0uuyvxr8tywehal0zyzhpy5tv4h5tpryvc449jmznnzdruqy68ks2 --from gt11twm7dma44k7wg5jppeyphrct9nx2l4m8szy44h72qv9eatyla3hkaevg3vx99mlslwsnfq --fees 100000NANOGT --chain-id testnet
```

**响应示例**
```
TxHash: BASIC-9F685A8362E6218E372CE60E306E8BC35B66006D82F9B3381A6AECE26FA6355CA38CD75AFFDF597794159D9356BE0376
```

## 区块信息

### 1. 查询区块信息

**命令**
```bash
gatecli block show [block height] --chain-id [chain ID]
```

**参数**

| 参数 | 类型 | 描述 |
|------|------|------|
| block height | Int | 要查询的区块高度 |
| chain-id | String | 链 ID |

**示例**
```bash
gatecli block show 10001 --chain-id testnet
```

**响应示例**
```json
{
    "hash": "H44ZPPWLRIXI3TJ64UZSHKKSJ4XRJMWIGU6FHFQXWFTRY3QHW3426MWP3IA44KJHZHBEHSOYYEWNO",
    "previousBlockHash": "BF4BVNISMRNALPYXZAPPK4LKLVGTUDVWGNA5BQYJVHPDRZXTVGQG3LA4FB6RPHSYX5IEA44BGD3DE",
    "seed": "HSIWSUEVTY4VRJCZMJ4VE7MFTDBA6SGIM7LY6YOWJ624ON7LFOINBIT6A3STY4GV52D5IIMNZDVNO",
    "proposer": "gt11ynwpc98gkh9uxxnkld7gjp5knu5ht75f0223uf298ap6ug6vtx5ank5vfde0pylmu09wzh",
    "height": "959",
    "period": "0",
    "txnRoot": "LHHTNT65QQOXUV6MQM4LIZ5PRGGY32XQRFTHIO4WXY4BPVBCWEXQFZ2NZU37AYKWOOUFSL4YTX2A4",
    "txnps": {
        "proxyTransactions": null
    },
    "timestamp": "1591788058",
    "UpgradeState": {
        "currentProtocol": "v1",
        "nextProtocol": "",
        "nextProtocolApprovals": "0",
        "nextProtocolVoteBefore": "0",
        "nextProtocolSwitchOn": "0"
    },
    "UpgradeVote": {
        "upgradePropose": "",
        "upgradeApprove": false
    },
    "CertCommitteeInfo": [
        "gt117h3k4uaftcql9zaqll5vg2x3gn67576qyklnvmzh9xldczryqpzpdj69xygrm0hvklufun",
        "singleCommitteePower": "90000341"
    ],
    "BlockCommitteeInfo": [
        "gt117h3k4uaftcql9zaqll5vg2x3gn67576qyklnvmzh9xldczryqpzpdj69xygrm0hvklufun",
        "singleCommitteePower": "90000323"
    ],
    "autoOfflineAccounts": []
}
```

**字段描述**
- hash: 当前区块的哈希值
- previousBlockHash: 前一个区块的哈希值
- seed: VRF种子用于抽取
- proposer: 提议区块的共识账户
- height: 区块高度
- period: 区块阶段
- txnRoot: 区块中交易的哈希值
- timestamp: 区块生成的时间
- UpgradeState: 共识协议版本状态
- UpgradeVote: 共识协议升级投票状态
- CertCommitteeInfo: 区块委员会（下一轮潜在奖励得主）
- BlockCommitteeInfo: 区块链委员会（本轮奖励得主）
- autoOfflineAccounts: 指定轮次自动离线的共识账户

## 共识账户

### 1. 创建共识账户

**命令**
```bash
gatecli con-account create [account address] --chain-id [chain ID]
```

**参数**

| Parameter | Type | Description |
|------|------|------|
| account address | String | Address of the consensus account to create |
| chain-id | String | Chain ID of the network |

**示例**
```bash
gatecli con-account create gt11twm7dma44k7wg5jppeyphrct9nx2l4m8szy44h72qv9eatyla3hkaevg3vx99mlslwsnfq --chain-id testnet
```

**响应示例**
```
ParticipationKey响应Data:
 ConAccount:
  Code:	0
  Log:	success //logs
  ParticipationKey:	ParticipationData:
  Address: 		AAAAAAAAAAAABXJ3QGZLVBIMEGOHDOOTCELOEO437DZQTRW5VBB3QVMSZ23VKLZVGSFOMUGDNLZQZVIEBFXQ //gatemint consensus account address
  VoteID: 		hgnhy7jmd8jy19j2hhcageqdj1s4xt7kpnv4388zxt8jaqbyrew0 //VRF public key
  SelectionID: 		293rc5xc7fsz81txx35686x0n4arbvx4cvy7n01e1emtghybhhg0 //the one-time signing public key
  FileName:	AAAAAAAAAAAABXJ3QGZLVBIMEGOHDOOTCELOEO437DZQTRW5VBB3QVMSZ23VKLZVGSFOMUGDNLZQZVIEBFXQ.0.0.partkey //local partkeyFile Name
```
**描述**
- 创建共识账户时，请确保账户有足够的 'NANOGT' 代币。


### 2. 获取共识账户在线

**命令**
```bash
gatecli con-account online 
--from [sender account] 
--pubkey [sender account public key]
--moniker [name]
--fees [tx fees]
--commission-max-change-rate [max commission rate change daily]
--commission-max-rate [max commission rate ]
--commission-rate [commission rate]
--chain-id [chain ID] 
```

**参数**

| 参数 | 类型 | 描述 |
|------|------|------|
| sender account | String | 发送者账户地址 |
| sender account public key | String | 发送者账户公钥 |
| name | String | 共识账户名称 |
| tx fees | String | 交易手续费 (e.g., 100000NANOGT) |
| max commission rate change daily | String | 最大佣金率变化每日 |
| max commission rate | String | 最大佣金率 |
| commission rate | String | 佣金率 |
| chain-id | String | 链 ID |

**示例**
```bash
gatecli con-account online --from gt11twm7dma44k7wg5jppeyphrct9nx2l4m8szy44h72qv9eatyla3hkaevg3vx99mlslwsnfq --pubkey gt1pub1u8s6p73qp3ac00a628mk7g4utllp6cvrl54vqgdazg3q6d8e3lzxxycrgw3qk3q7wn --moniker newcon-account --fees 100000NANOGT --commission-max-change-rate "0.01" --commission-max-rate "0.01" --commission-rate "0.01" --chain-id testnet
```

**响应示例**
```
 TxHash: BASIC-9F685A8362E6218E372CE60E306E8BC35B66006D82F9B3381A6AECE26FA6355CA38CD75AFFDF597794159D9356BE0376 
  //transaction  has,using gatecli tx show {hash} to query detailed information of this transaction 
  Data: rQO5zc/tCu8BYPD/ggoo3TuBsrqFDCGccbnTERbiO5v48wnG3ahDuFWSzrdVLzU0iuZQw2rzDBIoHk1VTbZ0J94UnjHi3aO8fwO1V5rK5I2NZvxNF1lFstSU9JD3J18JbxqUAWd0MXB1YjE4cTJmZ3VnZ3F5Znp0YzBwNXJhenEwZnRwdXplNzJwOXRwN25lZ2plZTl6amtjaGx2MHFwNThyZTdyZGduajNqd2x3d3JscjN6Z2o3cmNkcWxnc2Z3Y2V2YWRqaGE0ZXZoOThkejdzN3pjYzh5MHZhZnY3amh1ajNobXR1M2ZtajM2eXdqZWNtbnF1OWgSEgoMCgZOQU5PR1QSAjExEMCaDBowMXrG9msevrtuVTHWuZdFIixl5hSO4tWOvIZV01T/p+Pbg1sPeBgWGHbKUcm1064KImkKJeHhoPogZ32xdJvDkmTqENs7tchCbHrQ1z1n7Eeh1/ud9weWADUSQJRr9hYE0jvDKTx9IsfYAh3myFPQaYV9pt+TEi+IKdFm2KOZGYckVEbFx9ydMn2F6UbhopD5Y5HbrKJzf0fF9woyBNcEqQY=
  Raw Log: sync broadcast tx success //transaction  is successfully sent
```

### 3. 获取离线共识账户

**命令**
```bash
gatecli con-account offline --from [sender account] --fees [tx fees] --chain-id [chain ID]
```

**参数**

| 参数 | 类型 | 描述 |
|------|------|------|
| sender account | String | 发送者的账户地址 |
| tx fees | String | 交易费用（例如，100000NANOGT） |
| chain ID | String | 链 ID |

**示例**
```bash
gatecli con-account offline --from gt11twm7dma44k7wg5jppeyphrct9nx2l4m8szy44h72qv9eatyla3hkaevg3vx99mlslwsnfq --fees 100000NANOGT --chain-id testnet
```

**响应示例**
```
  TxHash: BASIC-9F685A8362E6218E372CE60E306E8BC35B66006D82F9B3381A6AECE26FA6355CA38CD75AFFDF597794159D9356BE0376 
  //transaction hash,using gatecli tx show {hash} to query detailed transaction information
  Data: rQO5zc/tCu8BYPD/ggoo3TuBsrqFDCGccbnTERbiO5v48wnG3ahDuFWSzrdVLzU0iuZQw2rzDBIoHk1VTbZ0J94UnjHi3aO8fwO1V5rK5I2NZvxNF1lFstSU9JD3J18JbxqUAWd0MXB1YjE4cTJmZ3VnZ3F5Znp0YzBwNXJhenEwZnRwdXplNzJwOXRwN25lZ2plZTl6amtjaGx2MHFwNThyZTdyZGduajNqd2x3d3JscjN6Z2o3cmNkcWxnc2Z3Y2V2YWRqaGE0ZXZoOThkejdzN3pjYzh5MHZhZnY3amh1ajNobXR1M2ZtajM2eXdqZWNtbnF1OWgSEgoMCgZOQU5PR1QSAjExEMCaDBowMXrG9msevrtuVTHWuZdFIixl5hSO4tWOvIZV01T/p+Pbg1sPeBgWGHbKUcm1064KImkKJeHhoPogZ32xdJvDkmTqENs7tchCbHrQ1z1n7Eeh1/ud9weWADUSQJRr9hYE0jvDKTx9IsfYAh3myFPQaYV9pt+TEi+IKdFm2KOZGYckVEbFx9ydMn2F6UbhopD5Y5HbrKJzf0fF9woyBNcEqQY=
  Raw Log: sync broadcast tx success //transaction  is successfully sent
```

### 4. 修改共识账户的最大佣金率

**命令**
```bash
gatecli con-account max-rate -- commission-max-rate [max commission rate] --commission-max-change-rate [max commission rate change daily] --from [sender account] --fees [tx fees] --chain-id [chain ID]
```

**参数**

| 参数 | 类型 | 描述 |
|------|------|------|
| max commission rate | String | 最大佣金率 |
| max commission rate change daily | String | 每日最大佣金率变化 |
| sender account | String | 发送者的账户地址 |
| tx fees | String | 交易费用（例如，100000NANOGT） |
| chain ID | String | 链 ID |

**示例**
```bash
gatecli con-account max-rate -- commission-max-rate 0.1 --commission-max-change-rate 0.2 --from gt11twm7dma44k7wg5jppeyphrct9nx2l4m8szy44h72qv9eatyla3hkaevg3vx99mlslwsnfq --fees 10000000NANOGT --chain-id testnet
```

**响应示例**
```
 TxHash: BASIC-9F685A8362E6218E372CE60E306E8BC35B66006D82F9B3381A6AECE26FA6355CA38CD75AFFDF597794159D9356BE0376 
  //transaction hash,using gatecli tx show {hash} to query
  Data: rQO5zc/tCu8BYPD/ggoo3TuBsrqFDCGccbnTERbiO5v48wnG3ahDuFWSzrdVLzU0iuZQw2rzDBIoHk1VTbZ0J94UnjHi3aO8fwO1V5rK5I2NZvxNF1lFstSU9JD3J18JbxqUAWd0MXB1YjE4cTJmZ3VnZ3F5Znp0YzBwNXJhenEwZnRwdXplNzJwOXRwN25lZ2plZTl6amtjaGx2MHFwNThyZTdyZGduajNqd2x3d3JscjN6Z2o3cmNkcWxnc2Z3Y2V2YWRqaGE0ZXZoOThkejdzN3pjYzh5MHZhZnY3amh1ajNobXR1M2ZtajM2eXdqZWNtbnF1OWgSEgoMCgZOQU5PR1QSAjExEMCaDBowMXrG9msevrtuVTHWuZdFIixl5hSO4tWOvIZV01T/p+Pbg1sPeBgWGHbKUcm1064KImkKJeHhoPogZ32xdJvDkmTqENs7tchCbHrQ1z1n7Eeh1/ud9weWADUSQJRr9hYE0jvDKTx9IsfYAh3myFPQaYV9pt+TEi+IKdFm2KOZGYckVEbFx9ydMn2F6UbhopD5Y5HbrKJzf0fF9woyBNcEqQY=
  Raw Log: sync broadcast tx success //transaction  is successfully sent
```

### 5. 编辑共识账户

**命令**
```bash
gatecli con-account edit --moniker [con-account name] --commission-rate [commission rate] --from [sender account] --fees [tx fees] --chain-id [chain ID]
```

**参数**

| 参数 | 类型 | 描述 |
|------|------|------|
| con-account name | String | 共识账户的名称 |
| commission rate | String | 佣金率 |
| sender account | String | 发送者的账户地址 |
| tx fees | String | 交易费用（例如，100000NANOGT） |
| chain ID | String | 链 ID |

**示例**
```bash
gatecli con-account edit --moniker con1 --commission-rate 0.03 --from gt11twm7dma44k7wg5jppeyphrct9nx2l4m8szy44h72qv9eatyla3hkaevg3vx99mlslwsnfq --fees 100000NANOGT --chain-id testnet
```

**响应示例**
```
  TxHash: BASIC-9F685A8362E6218E372CE60E306E8BC35B66006D82F9B3381A6AECE26FA6355CA38CD75AFFDF597794159D9356BE0376 
  //transaction hash,using gatecli tx show {hash} to query
  Data: rQO5zc/tCu8BYPD/ggoo3TuBsrqFDCGccbnTERbiO5v48wnG3ahDuFWSzrdVLzU0iuZQw2rzDBIoHk1VTbZ0J94UnjHi3aO8fwO1V5rK5I2NZvxNF1lFstSU9JD3J18JbxqUAWd0MXB1YjE4cTJmZ3VnZ3F5Znp0YzBwNXJhenEwZnRwdXplNzJwOXRwN25lZ2plZTl6amtjaGx2MHFwNThyZTdyZGduajNqd2x3d3JscjN6Z2o3cmNkcWxnc2Z3Y2V2YWRqaGE0ZXZoOThkejdzN3pjYzh5MHZhZnY3amh1ajNobXR1M2ZtajM2eXdqZWNtbnF1OWgSEgoMCgZOQU5PR1QSAjExEMCaDBowMXrG9msevrtuVTHWuZdFIixl5hSO4tWOvIZV01T/p+Pbg1sPeBgWGHbKUcm1064KImkKJeHhoPogZ32xdJvDkmTqENs7tchCbHrQ1z1n7Eeh1/ud9weWADUSQJRr9hYE0jvDKTx9IsfYAh3myFPQaYV9pt+TEi+IKdFm2KOZGYckVEbFx9ydMn2F6UbhopD5Y5HbrKJzf0fF9woyBNcEqQY=
  Raw Log: sync broadcast tx success //transaction  is successfully sent
```

**描述**
- 佣金率必须满足以下要求：
- - 必须在共识账户设置的最大佣金率（commission-max-rate）范围内；
- - 每天只能更改一次，且更改范围不得超过共识账户设置的最大佣金率变化（commission-max-change-rate）。


### 6. 查询共识账户信息

**命令**
```bash
gatecli con-account show-key [consensus account pending online] --chain-id [chain ID]
```

**参数**

| 参数 | 类型 | 描述 |
|------|------|------|
| consensus account pending online | String | 要查询的共识账户地址 |
| chain ID | String | 链 ID |

**示例**
```bash
gatecli con-account show-key gt11twm7dma44k7wg5jppeyphrct9nx2l4m8szy44h72qv9eatyla3hkaevg3vx99mlslwsnfq --chain-id testnet
```

**响应示例**
```
ParticipationKey响应Data:
 ConAccount:
  Code: 0
  Log: success
  ParticipationKey: ParticipationData:
  Address:     AAAAAAAAAAAABXJ3QGZLVBIMEGOHDOOTCELOEO437DZQTRW5VBB3QVMSZ23VKLZVGSFOMUGDNLZQZVIEBFXQ
  VoteID:     hgnhy7jmd8jy19j2hhcageqdj1s4xt7kpnv4388zxt8jaqbyrew0
  SelectionID:     293rc5xc7fsz81txx35686x0n4arbvx4cvy7n01e1emtghybhhg0
  FileName: AAAAAAAAAAAABXJ3QGZLVBIMEGOHDOOTCELOEO437DZQTRW5VBB3QVMSZ23VKLZVGSFOMUGDNLZQZVIEBFXQ.0.0.partkey
```

### 7. 查询本地共识账户信息

**命令**
```bash
gatecli con-account show-key [consensus account pending online] --chain-id [chain ID]
```

**参数**

| 参数 | 类型 | 描述 |
|------|------|------|
| consensus account pending online | String | 要查询的共识账户地址 |
| chain ID | String | 链 ID |

**示例**
```bash
gatecli con-account show-key gt11twm7dma44k7wg5jppeyphrct9nx2l4m8szy44h72qv9eatyla3hkaevg3vx99mlslwsnfq --chain-id testnet
```

**响应示例**
```
ParticipationKey响应Data:
 ConAccount:
  Code:	0
  Log:	success //logs
  ParticipationKey:	ParticipationData:
  Address: 		AAAAAAAAAAAABXJ3QGZLVBIMEGOHDOOTCELOEO437DZQTRW5VBB3QVMSZ23VKLZVGSFOMUGDNLZQZVIEBFXQ //gatemint consensus account address
  VoteID: 		hgnhy7jmd8jy19j2hhcageqdj1s4xt7kpnv4388zxt8jaqbyrew0 //VRF public key
  SelectionID: 		293rc5xc7fsz81txx35686x0n4arbvx4cvy7n01e1emtghybhhg0 //the one-time signing public key
  FileName:	AAAAAAAAAAAABXJ3QGZLVBIMEGOHDOOTCELOEO437DZQTRW5VBB3QVMSZ23VKLZVGSFOMUGDNLZQZVIEBFXQ.0.0.partkey //local partkeyFile Name
```

### 8. 查询共识账户列表

**命令**
```bash
gatecli con-account list --chain-id [chain ID]
```

**参数**

| 参数 | 类型 | 描述 |
|------|------|------|
| chain-id | String | 链 ID |

**示例**
```bash
gatecli con-account list --chain-id testnet
```

**响应示例**
```
Con-account
  Address:                    gt11dsck7f7txdc7hz5wdsspey0w5eyhw7yj4xrc3a6r4w4t39z9k45r86htlzxjf6z57an2r7
  Pubkey:                     gt1pub1u8s6p73q634mnsu7knd4fz23ng3gwvcyjfldrhlyffty3mnpx5phtr6ydyyqlj3uv8
  Status:                     online
  Power:                      70009656
  Tokens:                     100000000
  Delegator Shares:           100000000.000000000000000000
  Power Rate:                 1.000152230759201952
  Description:                {   }
  Commission:                 rate: 0.000000000000000000, maxRate: 0.000000000000000000, maxChangeRate: 0.000000000000000000, updateTime: 1970-01-01 00:00:00 +0000 UTC
```

## 分配管理

### 1. 设置账户以获取收入

**命令**
```bash
gatecli distribution set-withdraw-addr [account to fetch income] --from [sender account] --fees [tx fees] --chain-id [chain ID]
```

**参数**

| 参数 | 类型 | 描述 |
|------|------|------|
| account to fetch income | String | 收入的账户地址 |
| sender account | String | 发送者的账户地址 |
| tx fees | String | 交易费用（例如，100000NANOGT） |
| chain ID | String | 链 ID |

**示例**
```bash
gatecli distribution set-withdraw-addr gt11twm7dma44k7wg5jppeyphrct9nx2l4m8szy44h72qv9eatyla3hkaevg3vx99mlslwsnfq --from gt11twm7dma44k7wg5jppeyphrct9nx2l4m8szy44h72qv9eatyla3hkaevg3vx99mlslwsnfq --fees 100000NANOGT --chain-id testnet
```

**响应示例**
```
TxHash: BASIC-9F685A8362E6218E372CE60E306E8BC35B66006D82F9B3381A6AECE26FA6355CA38CD75AFFDF597794159D9356BE0376
Data: rQO5zc/tCu8BYPD/ggoo3TuBsrqFDCGccbnTERbiO5v48wnG3ahDuFWSzrdVLzU0iuZQw2rzDBIoHk1VTbZ0J94UnjHi3aO8fwO1V5rK5I2NZvxNF1lFstSU9JD3J18JbxqUAWd0MXB1YjE4cTJmZ3VnZ3F5Znp0YzBwNXJhenEwZnRwdXplNzJwOXRwN25lZ2plZTl6amtjaGx2MHFwNThyZTdyZGduajNqd2x3d3JscjN6Z2o3cmNkcWxnc2Z3Y2V2YWRqaGE0ZXZoOThkejdzN3pjYzh5MHZhZnY3amh1ajNobXR1M2ZtajM2eXdqZWNtbnF1OWgSEgoMCgZOQU5PR1QSAjExEMCaDBowMXrG9msevrtuVTHWuZdFIixl5hSO4tWOvIZV01T/p+Pbg1sPeBgWGHbKUcm1064KImkKJeHhoPogZ32xdJvDkmTqENs7tchCbHrQ1z1n7Eeh1/ud9weWADUSQJRr9hYE0jvDKTx9IsfYAh3myFPQaYV9pt+TEi+IKdFm2KOZGYckVEbFx9ydMn2F6UbhopD5Y5HbrKJzf0fF9woyBNcEqQY=
Raw Log: sync broadcast tx success
```

**注意**：保险箱账户只能将收入和本金转移到这个账户，不能单独设置。

### 2. 委托人账户从共识账户中提取部分收入

**命令**
```bash
gatecli distribution withdraw-rewards [consensus account address] --from [sender account] --fees [tx fees] --chain-id [chain ID]
```

**参数**

| 参数 | 类型 | 描述 |
|------|------|------|
| consensus account address | String | 共识账户的地址 |
| sender account | String | 发送者的账户地址 |
| tx fees | String | 交易费用（例如，100000NANOGT） |
| chain ID | String | 链 ID |

**示例**
```bash
gatecli distribution withdraw-rewards gt11twm7dma44k7wg5jppeyphrct9nx2l4m8szy44h72qv9eatyla3hkaevg3vx99mlslwsnfq --from gt11twm7dma44k7wg5jppeyphrct9nx2l4m8szy44h72qv9eatyla3hkaevg3vx99mlslwsnfq --fees 100000NANOGT --chain-id testnet
```

**响应示例**
```
  TxHash: BASIC-9F685A8362E6218E372CE60E306E8BC35B66006D82F9B3381A6AECE26FA6355CA38CD75AFFDF597794159D9356BE0376 
  //transaction hash, using gatecli tx show {hash}to query details of this transaction
  Data: rQO5zc/tCu8BYPD/ggoo3TuBsrqFDCGccbnTERbiO5v48wnG3ahDuFWSzrdVLzU0iuZQw2rzDBIoHk1VTbZ0J94UnjHi3aO8fwO1V5rK5I2NZvxNF1lFstSU9JD3J18JbxqUAWd0MXB1YjE4cTJmZ3VnZ3F5Znp0YzBwNXJhenEwZnRwdXplNzJwOXRwN25lZ2plZTl6amtjaGx2MHFwNThyZTdyZGduajNqd2x3d3JscjN6Z2o3cmNkcWxnc2Z3Y2V2YWRqaGE0ZXZoOThkejdzN3pjYzh5MHZhZnY3amh1ajNobXR1M2ZtajM2eXdqZWNtbnF1OWgSEgoMCgZOQU5PR1QSAjExEMCaDBowMXrG9msevrtuVTHWuZdFIixl5hSO4tWOvIZV01T/p+Pbg1sPeBgWGHbKUcm1064KImkKJeHhoPogZ32xdJvDkmTqENs7tchCbHrQ1z1n7Eeh1/ud9weWADUSQJRr9hYE0jvDKTx9IsfYAh3myFPQaYV9pt+TEi+IKdFm2KOZGYckVEbFx9ydMn2F6UbhopD5Y5HbrKJzf0fF9woyBNcEqQY=
  Raw Log: sync broadcast tx success transaction is sent successfully
```

### 3. 委托人账户从共识账户中提取所有收入

**命令**
```bash
gatecli distribution withdraw-all-rewards --from [sender account] --fees [tx fees] --chain-id [chain ID]
```

**参数**

| 参数 | 类型 | 描述 |
|------|------|------|
| sender account | String | 发送者的账户地址 |
| tx fees | String | 交易费用（例如，100000NANOGT） |
| chain ID | String | 链 ID |

**示例**
```bash
gatecli distribution withdraw-all-rewards --from gt11twm7dma44k7wg5jppeyphrct9nx2l4m8szy44h72qv9eatyla3hkaevg3vx99mlslwsnfq --fees 100000NANOGT --chain-id testnet
```

**响应示例**
```
  TxHash: BASIC-9F685A8362E6218E372CE60E306E8BC35B66006D82F9B3381A6AECE26FA6355CA38CD75AFFDF597794159D9356BE0376
  //transaction hash, using gatecli tx show {hash}to query details of this transaction
  Data: rQO5zc/tCu8BYPD/ggoo3TuBsrqFDCGccbnTERbiO5v48wnG3ahDuFWSzrdVLzU0iuZQw2rzDBIoHk1VTbZ0J94UnjHi3aO8fwO1V5rK5I2NZvxNF1lFstSU9JD3J18JbxqUAWd0MXB1YjE4cTJmZ3VnZ3F5Znp0YzBwNXJhenEwZnRwdXplNzJwOXRwN25lZ2plZTl6amtjaGx2MHFwNThyZTdyZGduajNqd2x3d3JscjN6Z2o3cmNkcWxnc2Z3Y2V2YWRqaGE0ZXZoOThkejdzN3pjYzh5MHZhZnY3amh1ajNobXR1M2ZtajM2eXdqZWNtbnF1OWgSEgoMCgZOQU5PR1QSAjExEMCaDBowMXrG9msevrtuVTHWuZdFIixl5hSO4tWOvIZV01T/p+Pbg1sPeBgWGHbKUcm1064KImkKJeHhoPogZ32xdJvDkmTqENs7tchCbHrQ1z1n7Eeh1/ud9weWADUSQJRr9hYE0jvDKTx9IsfYAh3myFPQaYV9pt+TEi+IKdFm2KOZGYckVEbFx9ydMn2F6UbhopD5Y5HbrKJzf0fF9woyBNcEqQY=
  Raw Log: sync broadcast tx success transaction is sent successfully
```
### 4. 将委托账户的收益再投资到共识账户中

**命令**
```bash
gatecli distribution reward-reinvestment [consensus account address] --from [sender account] --fees [tx fees] --chain-id [chain ID]
```

**参数**

| 参数 | 类型 | 描述 |
|------|------|------|
| consensus account address | String | 共识账户的地址 |
| sender account | String | 发送者的账户地址 |
| tx fees | String | 交易费用（例如，100000NANOGT） |
| chain ID | String | 链 ID |

**示例**
```bash
gatecli distribution reward-reinvestment gt11rjq598t8vte64ff2tnesdvsfazv38atpenufj4zl0ljhw2q28jnlxnh6mqgmn6elje6fzc --from gt11mtehamaw8wktwpppx9klrhhfu5upmutzthl0kkpwsa0slw8h4xd3p2ane2zt262dlsyy3m --fees 100000NANOGT --chain-id testnet
```

**响应示例**
```
  TxHash: BASIC-9F685A8362E6218E372CE60E306E8BC35B66006D82F9B3381A6AECE26FA6355CA38CD75AFFDF597794159D9356BE0376
  //transaction hash, using gatecli tx show {hash}to query details of this transaction
  Data: rQO5zc/tCu8BYPD/ggoo3TuBsrqFDCGccbnTERbiO5v48wnG3ahDuFWSzrdVLzU0iuZQw2rzDBIoHk1VTbZ0J94UnjHi3aO8fwO1V5rK5I2NZvxNF1lFstSU9JD3J18JbxqUAWd0MXB1YjE4cTJmZ3VnZ3F5Znp0YzBwNXJhenEwZnRwdXplNzJwOXRwN25lZ2plZTl6amtjaGx2MHFwNThyZTdyZGduajNqd2x3d3JscjN6Z2o3cmNkcWxnc2Z3Y2V2YWRqaGE0ZXZoOThkejdzN3pjYzh5MHZhZnY3amh1ajNobXR1M2ZtajM2eXdqZWNtbnF1OWgSEgoMCgZOQU5PR1QSAjExEMCaDBowMXrG9msevrtuVTHWuZdFIixl5hSO4tWOvIZV01T/p+Pbg1sPeBgWGHbKUcm1064KImkKJeHhoPogZ32xdJvDkmTqENs7tchCbHrQ1z1n7Eeh1/ud9weWADUSQJRr9hYE0jvDKTx9IsfYAh3myFPQaYV9pt+TEi+IKdFm2KOZGYckVEbFx9ydMn2F6UbhopD5Y5HbrKJzf0fF9woyBNcEqQY=
  Raw Log: sync broadcast tx success transaction is sent successfully
```
### 5. 查询某个委托人账户在某个共识账户上的委托收入

**命令**
```bash
gatecli distribution rewards [delegator account address] [consensus account address] --chain-id [chain ID]
```

**参数**

| 参数 | 类型 | 描述 |
|------|------|------|
| delegator account address | String | 委托人账户的地址 |
| consensus account address | String | 共识账户的地址 |
| chain ID | String | 链 ID |

**示例**
```bash
gatecli distribution rewards gt11twm7dma44k7wg5jppeyphrct9nx2l4m8szy44h72qv9eatyla3hkaevg3vx99mlslwsnfq gt11twm7dma44k7wg5jppeyphrct9nx2l4m8szy44h72qv9eatyla3hkaevg3vx99mlslwsnfq --chain-id testnet
```

**响应示例**
```
Delegator Total Rewards:
  Rewards:
	Con-account: gt11dsck7f7txdc7hz5wdsspey0w5eyhw7yj4xrc3a6r4w4t39z9k45r86htlzxjf6z57an2r7 // consensus account address
	Reward: 21.644616251838158370NANOGT //commission and mining earnings
```
### 6. 查询所有共识账户的委托收益

**命令**
```bash
gatecli distribution rewards [delegator account address] --chain-id [chain ID]
```

**参数**

| 参数 | 类型 | 描述 |
|------|------|------|
| delegator account address | String | 委托人账户的地址 |
| chain ID | String | 链 ID |

**示例**
```bash
gatecli distribution rewards gt11twm7dma44k7wg5jppeyphrct9nx2l4m8szy44h72qv9eatyla3hkaevg3vx99mlslwsnfq --chain-id testnet
```

**响应示例**
```
Delegator Total Rewards:
  Rewards:
	Con-account: gt11dsck7f7txdc7hz5wdsspey0w5eyhw7yj4xrc3a6r4w4t39z9k45r86htlzxjf6z57an2r7 // consensus account address
	Reward: 21.644616251838158370NANOGT //commission and mining earnings
	Con-account:...
Total: 11997269037.266215577925788000NANOGT //total delegation income of a delegator account
```
### 7. 查询共识账户待支付的委托收入

**命令**
```bash
gatecli distribution outstanding-rewards [consensus account address] --chain-id [chain ID]
```

**参数**

| 参数 | 类型 | 描述 |
|------|------|------|
| consensus account address | String | 共识账户的地址 |
| chain ID | String | 链 ID |

**示例**
```bash
gatecli distribution outstanding-rewards gt11twm7dma44k7wg5jppeyphrct9nx2l4m8szy44h72qv9eatyla3hkaevg3vx99mlslwsnfq --chain-id testnet
```

**响应示例**
```
660467079334.408199257068561630NANOGT //待支付的委托收入
```

### 8. 查询共识账户收入

**命令**
```bash
gatecli distribution commission [consensus account address] --chain-id [chain ID]
```

**参数**

| Parameter | Type | Description |
|------|------|------|
| consensus account address | String | Address of the consensus account |
| chain-id | String | Chain ID of the network |

**示例**
```bash
gatecli distribution commission gt11twm7dma44k7wg5jppeyphrct9nx2l4m8szy44h72qv9eatyla3hkaevg3vx99mlslwsnfq --chain-id testnet
```

**响应示例**
```
660467079334.408199257068561630NANOGT //commission and mining earnings
```

### 9. 查询分布和基础

**命令**
```bash
gatecli distribution params --chain-id [chain ID]
```

**参数**

| Parameter | Type | Description |
|------|------|------|
| chain-id | String | Chain ID of the network |

**响应示例**
```
Distribution Params:
  Community Tax:          "0.020000000000000000" //Community Tax rate
  Withdraw Addr Enabled:  true
  First CommitteeReward:  "0.400000000000000000" //The first Committee mining reward rate
  Second CommitteeReward:  "0.350000000000000000" //The second Committee mining reward rate
  Third CommitteeReward:  "0.250000000000000000" //The third Committee mining reward rate
```


## 可撤销交易管理

### 1.发送可撤销交易

**命令**
```bash
gatecli revocable-tx send [recipient account] [transfer token amount] --from [sender account] --fees [tx fees] --chain-id [chain ID]
```

**参数**

| Parameter | Type | Description |
|------|------|------|
| recipient account | String | Account address of the recipient |
| transfer token amount | String | Amount of tokens to transfer |
| sender account | String | Account address of the sender |
| tx fees | String | Transaction fees (e.g., 100000NANOGT) |
| chain-id | String | Chain ID of the network |

**示例**
```bash
gatecli revocable-tx send gt11twm7dma44k7wg5jppeyphrct9nx2l4m8szy44h72qv9eatyla3hkaevg3vx99mlslwsnfq 10NANOGT --from vault11fg056uaatk9s3k6l34eqkc7qchjup8e39afauf9naz4jmpklsze3uwx6405vll88l5lvww --fees 100000NANOGT --chain-id testnet
```

**响应示例**
```
TxHash: REVOCABLEPAY-0FD8A17798EC2CC637252687CA7DACA39BE0C555496EC3242F90C7C0BBBFE5F10A34A51D866BEC91A9866275BED9B522
Data: rQO5zc/tCu8BYPD/ggoo3TuBsrqFDCGccbnTERbiO5v48wnG3ahDuFWSzrdVLzU0iuZQw2rzDBIoHk1VTbZ0J94UnjHi3aO8fwO1V5rK5I2NZvxNF1lFstSU9JD3J18JbxqUAWd0MXB1YjE4cTJmZ3VnZ3F5Znp0YzBwNXJhenEwZnRwdXplNzJwOXRwN25lZ2plZTl6amtjaGx2MHFwNThyZTdyZGduajNqd2x3d3JscjN6Z2o3cmNkcWxnc2Z3Y2V2YWRqaGE0ZXZoOThkejdzN3pjYzh5MHZhZnY3amh1ajNobXR1M2ZtajM2eXdqZWNtbnF1OWgSEgoMCgZOQU5PR1QSAjExEMCaDBowMXrG9msevrtuVTHWuZdFIixl5hSO4tWOvIZV01T/p+Pbg1sPeBgWGHbKUcm1064KImkKJeHhoPogZ32xdJvDkmTqENs7tchCbHrQ1z1n7Eeh1/ud9weWADUSQJRr9hYE0jvDKTx9IsfYAh3myFPQaYV9pt+TEi+IKdFm2KOZGYckVEbFx9ydMn2F6UbhopD5Y5HbrKJzf0fF9woyBNcEqQY=
Raw Log: sync broadcast tx success
```

**说明**
- 只有金库账户可以发送可撤销交易

### 2. 查询单笔交易

**命令**
```bash
gatecli revocable-tx show [transaction Hash] --chain-id [chain ID]
```

**参数**

| 参数 | 类型 | 描述 |
|------|------|------|
| transaction Hash | String | 要查询的交易哈希值 |
| chain-id | String | 链 ID |

**示例**
```bash
gatecli revocable-tx show 03190D3F56D6B65CC11BFE3F9CD961729B134D224A35AC731728601C9DD3A3C7 --chain-id testnet
```

**响应示例**
```
Response:
  Height: 773
  TxHash: REVOCABLEPAY-082A18896CD1397563569D939C884D446B303865012C750ACB40E3911CF8FD69144F836537C97B77EFF473776D7889F5
  Data: A402B9CDCFED0A6747B161CE0A284A1F4D73BD5D8B08DB5F8D720B63C0C5E5C09F312F53DE24B3E8AB2D86DF80B31E38DAABE8CFFCE712286C316F27CB3371EB8A8E6C201C91EEA649777892A98788F743ABAAB89445B56833EAEBF88D24E8541A0D0A064E414E4F4754120331303012120A0C0A064E414E4F47541202313110C09A0C1A30B94B240CB12506E73813D1B31BFE5B6A508398678996D6AA39D38E592BC35322FBD134B76728A831A3C9059153F6109B22690A25E1E1A0FA200C7B87BFBA51F76F22BC5FFE1D6183FD2AC021BD12220D34F98FC463130343A212403F2B5EC54D272161E400E6247226E03D2A82BDA2977CFC78AD2C7685F728DAF67E5CE2FE7E715D8118BF19AED1C54DDEFB51CE486C68C1867B532592C3C0900F3204FA05CC07
  Raw Log: [{"msg_index":0,"success":true,"log":"","events":[{"type":"message","attributes":[{"key":"sender","value":"gt11fg056uaatk9s3k6l34eqkc7qchjup8e39afauf9naz4jmpklsze3uwx6405vll8805fq0f"},{"key":"module","value":"revocable"},{"key":"action","value":"revocable"}]},{"type":"transfer","attributes":[{"key":"recipient","value":"gt11dsck7f7txdc7hz5wdsspey0w5eyhw7yj4xrc3a6r4w4t39z9k45r86htlzxjf6z57an2r7"}]}]}]
  Logs: [{"msg_index":0,"success":true,"log":"","events":[{"type":"message","attributes":[{"key":"sender","value":"vault11fg056uaatk9s3k6l34eqkc7qchjup8e39afauf9naz4jmpklsze3uwx6405vll88l5lvww"},{"key":"module","value":"revocable"},{"key":"action","value":"revocable"}]},{"type":"transfer","attributes":[{"key":"recipient","value":"gt11dsck7f7txdc7hz5wdsspey0w5eyhw7yj4xrc3a6r4w4t39z9k45r86htlzxjf6z57an2r7"}]}]}]
  GasWanted: 200000
  GasUsed: 95467
  Timestamp: 2020-06-19T07:56:11+08:00
  Events:
    - message
      - sender: vault11fg056uaatk9s3k6l34eqkc7qchjup8e39afauf9naz4jmpklsze3uwx6405vll88l5lvww
      - module: revocable
      - action: revocable
    - transfer
      - recipient: gt11dsck7f7txdc7hz5wdsspey0w5eyhw7yj4xrc3a6r4w4t39z9k45r86htlzxjf6z57an2r7
```

### 3. 查询可撤销交易列表

**命令**
```bash
gatecli revocable-tx list [Vault Account] --chain-id [chain ID]
```

**参数**

| 参数 | 类型 | 描述 |
|------|------|------|
| Vault Account | String | 要查询的 Vault 账户地址 |
| chain-id | String | 链 ID |

**示例**
```bash
gatecli revocable-tx list vault11fg056uaatk9s3k6l34eqkc7qchjup8e39afauf9naz4jmpklsze3uwx6405vll88l5lvww --chain-id testnet
```

**响应示例**
```
Txs: count  1
TxHash:         REVOCABLEPAY-082A18896CD1397563569D939C884D446B303865012C750ACB40E3911CF8FD69144F836537C97B77EFF473776D7889F5
Index:          0
Height:         873
Tokens:          100NANOGT
```

### 4. 撤销可撤销交易

**命令**
```bash
gatecli revocable-tx revoke [transaction Hash] --from [sender account] --fees [tx fees] --chain-id [chain ID]
```

**参数**

| 参数 | 类型 | 描述 |
|------|------|------|
| transaction Hash | String | 要撤销的交易哈希值 |
| sender account | String | 发送者的账户地址 |
| tx fees | String | 交易费用（例如，100000NANOGT） |
| chain-id | String | 链 ID |

**示例**
```bash
gatecli revocable-tx revoke 0E3B67C685C271632CE6F4DAA2AB06AF7E8077509E1CB5310F63F6C147786E12 --from vault11fg056uaatk9s3k6l34eqkc7qchjup8e39afauf9naz4jmpklsze3uwx6405vll88l5lvww --fees 100000NANOGT --chain-id testnet
```

**响应示例**
```
TxHash: REVOKE-9F685A8362E6218E372CE60E306E8BC35B66006D82F9B3381A6AECE26FA6355CA38CD75AFFDF597794159D9356BE0376
Data: rQO5zc/tCu8BYPD/ggoo3TuBsrqFDCGccbnTERbiO5v48wnG3ahDuFWSzrdVLzU0iuZQw2rzDBIoHk1VTbZ0J94UnjHi3aO8fwO1V5rK5I2NZvxNF1lFstSU9JD3J18JbxqUAWd0MXB1YjE4cTJmZ3VnZ3F5Znp0YzBwNXJhenEwZnRwdXplNzJwOXRwN25lZ2plZTl6amtjaGx2MHFwNThyZTdyZGduajNqd2x3d3JscjN6Z2o3cmNkcWxnc2Z3Y2V2YWRqaGE0ZXZoOThkejdzN3pjYzh5MHZhZnY3amh1ajNobXR1M2ZtajM2eXdqZWNtbnF1OWgSEgoMCgZOQU5PR1QSAjExEMCaDBowMXrG9msevrtuVTHWuZdFIixl5hSO4tWOvIZV01T/p+Pbg1sPeBgWGHbKUcm1064KImkKJeHhoPogZ32xdJvDkmTqENs7tchCbHrQ1z1n7Eeh1/ud9weWADUSQJRr9hYE0jvDKTx9IsfYAh3myFPQaYV9pt+TEi+IKdFm2KOZGYckVEbFx9ydMn2F6UbhopD5Y5HbrKJzf0fF9woyBNcEqQY=
Raw Log: sync broadcast tx success
```

## 委托管理

### 1. 委托代币到共识账户

**命令**
```bash
gatecli staking delegate [consensus account address] [amount] --from [sender account] --fees [tx fees] --chain-id [chain ID]
```

**参数**

| 参数 | 类型 | 描述 |
|------|------|------|
| consensus account address | String | 要委托的共识账户地址 |
| amount | String | 要委托的代币数量 |
| sender account | String | 发送者的账户地址 |
| tx fees | String | 交易费用（例如，100000NANOGT） |
| chain-id | String | 链 ID |

**示例**
```bash
gatecli staking delegate gt11twm7dma44k7wg5jppeyphrct9nx2l4m8szy44h72qv9eatyla3hkaevg3vx99mlslwsnfq 100000NANOGT --from gt11twm7dma44k7wg5jppeyphrct9nx2l4m8szy44h72qv9eatyla3hkaevg3vx99mlslwsnfq --fees 100000NANOGT --chain-id testnet
```

**响应示例**
```
TxHash: DELEGATE-9F685A8362E6218E372CE60E306E8BC35B66006D82F9B3381A6AECE26FA6355CA38CD75AFFDF597794159D9356BE0376
Data: rQO5zc/tCu8BYPD/ggoo3TuBsrqFDCGccbnTERbiO5v48wnG3ahDuFWSzrdVLzU0iuZQw2rzDBIoHk1VTbZ0J94UnjHi3aO8fwO1V5rK5I2NZvxNF1lFstSU9JD3J18JbxqUAWd0MXB1YjE4cTJmZ3VnZ3F5Znp0YzBwNXJhenEwZnRwdXplNzJwOXRwN25lZ2plZTl6amtjaGx2MHFwNThyZTdyZGduajNqd2x3d3JscjN6Z2o3cmNkcWxnc2Z3Y2V2YWRqaGE0ZXZoOThkejdzN3pjYzh5MHZhZnY3amh1ajNobXR1M2ZtajM2eXdqZWNtbnF1OWgSEgoMCgZOQU5PR1QSAjExEMCaDBowMXrG9msevrtuVTHWuZdFIixl5hSO4tWOvIZV01T/p+Pbg1sPeBgWGHbKUcm1064KImkKJeHhoPogZ32xdJvDkmTqENs7tchCbHrQ1z1n7Eeh1/ud9weWADUSQJRr9hYE0jvDKTx9IsfYAh3myFPQaYV9pt+TEi+IKdFm2KOZGYckVEbFx9ydMn2F6UbhopD5Y5HbrKJzf0fF9woyBNcEqQY=
Raw Log: sync broadcast tx success
```
### 2. 查询共识帐户中某个委托帐户的委托信息

**命令**
```bash
gatecli staking delegation [delegator account address] [consensus account address] --chain-id [chain ID]
```

**参数**

| Parameter | Type | Description |
|------|------|------|
| delegator account address | String | Address of the delegator account |
| consensus account address | String | Address of the consensus account |
| chain-id | String | Chain ID of the network |

**示例**
```bash
gatecli staking delegation gt11twm7dma44k7wg5jppeyphrct9nx2l4m8szy44h72qv9eatyla3hkaevg3vx99mlslwsnfq gt11dsck7f7txdc7hz5wdsspey0w5eyhw7yj4xrc3a6r4w4t39z9k45r86htlzxjf6z57an2r7 --chain-id testnet
```

**响应示例**
```
Delegation:
  Delegator: gt11twm7dma44k7wg5jppeyphrct9nx2l4m8szy44h72qv9eatyla3hkaevg3vx99mlslwsnfq
  Con-account: gt11dsck7f7txdc7hz5wdsspey0w5eyhw7yj4xrc3a6r4w4t39z9k45r86htlzxjf6z57an2r7
  Shares: 100000000.000000000000000000
```
### 3. 查询委托人账户在所有共识账户中的委托信息

**命令**
```bash
gatecli staking delegations [delegator account address] --chain-id [chain ID]
```

**参数**

| 参数 | 类型 | 描述 |
|------|------|------|
| delegator account address | String | 委托人账户的地址 |
| chain-id | String | 链 ID |

**示例**
```bash
gatecli staking delegations gt11twm7dma44k7wg5jppeyphrct9nx2l4m8szy44h72qv9eatyla3hkaevg3vx99mlslwsnfq --chain-id testnet
```

**响应示例**
```
Delegation:
  Delegator: gt11twm7dma44k7wg5jppeyphrct9nx2l4m8szy44h72qv9eatyla3hkaevg3vx99mlslwsnfq
  Con-account: gt11dsck7f7txdc7hz5wdsspey0w5eyhw7yj4xrc3a6r4w4t39z9k45r86htlzxjf6z57an2r7
  Shares: 100000000.000000000000000000
```

### 4. 转移委托

**命令**
```bash
gatecli staking redelegate [source consensus account address] [target consensus account address] [delegation token amount ] --from [delegator account address] --fees [tx fees] --chain-id [chain ID]
```

**参数**

| 参数 | 类型 | 描述 |
|------|------|------|
| source consensus account address | String | 源共识账户地址 |
| target consensus account address | String | 目标共识账户地址 |
| delegation token amount | String | 委托代币数量 |
| delegator account address | String | 委托人账户地址 |
| tx fees | String | 交易费用（例如，100000NANOGT） |
| chain-id | String | 链 ID |

**示例**
```bash
gatecli staking redelegate gt11m5acrv46s5xzr8r3h8f3z9hz8wdl3ucfcmw6ssac2kfvad649u6nfzhx2rpk4ucvrxla6s gt11m5acrv46s5xzr8r3h8f3z9hz8wdl3ucfcmw6ssac2kfvad649u6nfzhx2rpk4ucvrxla54 100000000NANOGT --from gt11m5acrv46s5xzr8r3h8f3z9hz8wdl3ucfcmw6ssac2kfvad649u6nfzhx2rpk4ucvrxla6s --fees 100000NANOGT --chain-id testnet
```

**响应示例**
```
 TxHash: BASIC-9F685A8362E6218E372CE60E306E8BC35B66006D82F9B3381A6AECE26FA6355CA38CD75AFFDF597794159D9356BE0376 
  //Transaction hash,using gatecli tx show {hash}to query details of this transaction.
  Data: rQO5zc/tCu8BYPD/ggoo3TuBsrqFDCGccbnTERbiO5v48wnG3ahDuFWSzrdVLzU0iuZQw2rzDBIoHk1VTbZ0J94UnjHi3aO8fwO1V5rK5I2NZvxNF1lFstSU9JD3J18JbxqUAWd0MXB1YjE4cTJmZ3VnZ3F5Znp0YzBwNXJhenEwZnRwdXplNzJwOXRwN25lZ2plZTl6amtjaGx2MHFwNThyZTdyZGduajNqd2x3d3JscjN6Z2o3cmNkcWxnc2Z3Y2V2YWRqaGE0ZXZoOThkejdzN3pjYzh5MHZhZnY3amh1ajNobXR1M2ZtajM2eXdqZWNtbnF1OWgSEgoMCgZOQU5PR1QSAjExEMCaDBowMXrG9msevrtuVTHWuZdFIixl5hSO4tWOvIZV01T/p+Pbg1sPeBgWGHbKUcm1064KImkKJeHhoPogZ32xdJvDkmTqENs7tchCbHrQ1z1n7Eeh1/ud9weWADUSQJRr9hYE0jvDKTx9IsfYAh3myFPQaYV9pt+TEi+IKdFm2KOZGYckVEbFx9ydMn2F6UbhopD5Y5HbrKJzf0fF9woyBNcEqQY=
  Raw Log: sync broadcast tx success //transaction is sent successfully
```

### 5. 查询特定委托人的所有转移委托

**命令**
```bash
gatecli staking redelegations [delegator account address] --chain-id [chain ID]
```

**参数**

| 参数 | 类型 | 描述 |
|------|------|------|
| delegator account address | String | 委托人账户地址 |
| chain-id | String | 链 ID |

**示例**
```bash
gatecli staking redelegations gt11p8qmx5q8h7h3es3wl4x0y4efgme552hk7x5g6ppeelel2v2vvsthxk0ce65gw9mfls9ugp --chain-id testnet
```

**响应示例**
```
Redelegations between:
  Delegator:                   gt11m5acrv46s5xzr8r3h8f3z9hz8wdl3ucfcmw6ssac2kfvad649u6nfzhx2rpk4ucvrxla6s //delegator account address
  Source Con-account:          gt11dsck7f7txdc7hz5wdsspey0w5eyhw7yj4xrc3a6r4w4t39z9k45r86htlzxjf6z57an2r7 //source consensus account address
  Destination Con-account:     gt11ewdeyjdlxs2x4yglnhtulp0tclgdp7c2t8d5c6495r83qvrfr7p0qt40uddu3k44s7rxg6 //target consensus account address
  Entries:
    Redelegation Entry #0:
      Creation height:           876 //height  at which the shift delegation transaction is initiated 
      Min time to undelegate (unix): 2020-07-10 02:47:51 +0000 UTC //time finishing undelegating  at source consensus account.
      Initial Balance:           10 //initial token amount of delegation shift  
      Shares:                    10.000000000000000000 //amount of the delegation shift
      Balance:                   10 //token amount of shift delegation 
Redelegations between:
 ...
```

### 7. 查询特定委托人的两个共识账户之间的转移委托

**命令**
```bash
gatecli staking redelegation [delegator account address] [source consensus account address] [target consensus account address] --chain-id [chain ID]
```

**参数**

| 参数 | 类型 | 描述 |
|------|------|------|
| delegator account address | String | 委托人账户地址 |
| source consensus account address | String | 源共识账户地址 |
| target consensus account address | String | 目标共识账户地址 |
| chain-id | String | 链 ID |

**示例**
```bash
gatecli staking redelegation gt11m5acrv46s5xzr8r3h8f3z9hz8wdl3ucfcmw6ssac2kfvad649u6nfzhx2rpk4ucvrxla6s gt11dsck7f7txdc7hz5wdsspey0w5eyhw7yj4xrc3a6r4w4t39z9k45r86htlzxjf6z57an2r7 gt11ewdeyjdlxs2x4yglnhtulp0tclgdp7c2t8d5c6495r83qvrfr7p0qt40uddu3k44s7rxg6 --chain-id testnet
```

**响应示例**
```
Redelegations between:
  Delegator:                   gt11m5acrv46s5xzr8r3h8f3z9hz8wdl3ucfcmw6ssac2kfvad649u6nfzhx2rpk4ucvrxla6s //delegator account address
  Source Con-account:          gt11dsck7f7txdc7hz5wdsspey0w5eyhw7yj4xrc3a6r4w4t39z9k45r86htlzxjf6z57an2r7 //source consensus account address
  Destination Con-account:     gt11ewdeyjdlxs2x4yglnhtulp0tclgdp7c2t8d5c6495r83qvrfr7p0qt40uddu3k44s7rxg6 //target consensus account address
  Entries:
    Redelegation Entry #0:
      Creation height:           876 //height  at which the shift delegation transaction is initiated 
      Min time to undelegate (unix): 2020-07-10 02:47:51 +0000 UTC //time finishing undelegating  at source consensus account.
      Initial Balance:           10 //initial token amount of delegation shift 
      Shares:                    10.000000000000000000 //delegation shift quantity 
      Balance:                   10 //token amount of shift delegation 
```

### 8. 从共识账户取消委托

**命令**
```bash
gatecli staking undelegate [consensus account address] [amount] --from [sender account] --fees [tx fees] --chain-id [chain ID]
```

**参数**

| 参数 | 类型 | 描述 |
|------|------|------|
| consensus account address | String | 共识账户地址 |
| amount | String | 取消委托的代币数量 |
| sender account | String | 发送者账户地址 |
| tx fees | String | 交易费用（例如，100000NANOGT） |
| chain-id | String | 链 ID |

**示例**
```bash
gatecli staking undelegate gt11twm7dma44k7wg5jppeyphrct9nx2l4m8szy44h72qv9eatyla3hkaevg3vx99mlslwsnfq 100000NANOGT --from gt11twm7dma44k7wg5jppeyphrct9nx2l4m8szy44h72qv9eatyla3hkaevg3vx99mlslwsnfq --fees 100000NANOGT --chain-id testnet
```

**响应示例**
```
TxHash: UNDELEGATE-9F685A8362E6218E372CE60E306E8BC35B66006D82F9B3381A6AECE26FA6355CA38CD75AFFDF597794159D9356BE0376
Data: rQO5zc/tCu8BYPD/ggoo3TuBsrqFDCGccbnTERbiO5v48wnG3ahDuFWSzrdVLzU0iuZQw2rzDBIoHk1VTbZ0J94UnjHi3aO8fwO1V5rK5I2NZvxNF1lFstSU9JD3J18JbxqUAWd0MXB1YjE4cTJmZ3VnZ3F5Znp0YzBwNXJhenEwZnRwdXplNzJwOXRwN25lZ2plZTl6amtjaGx2MHFwNThyZTdyZGduajNqd2x3d3JscjN6Z2o3cmNkcWxnc2Z3Y2V2YWRqaGE0ZXZoOThkejdzN3pjYzh5MHZhZnY3amh1ajNobXR1M2ZtajM2eXdqZWNtbnF1OWgSEgoMCgZOQU5PR1QSAjExEMCaDBowMXrG9msevrtuVTHWuZdFIixl5hSO4tWOvIZV01T/p+Pbg1sPeBgWGHbKUcm1064KImkKJeHhoPogZ32xdJvDkmTqENs7tchCbHrQ1z1n7Eeh1/ud9weWADUSQJRr9hYE0jvDKTx9IsfYAh3myFPQaYV9pt+TEi+IKdFm2KOZGYckVEbFx9ydMn2F6UbhopD5Y5HbrKJzf0fF9woyBNcEqQY=
Raw Log: sync broadcast tx success
```
### 9.通过安全账户解除委托

**命令**
```bash
gatecli staking undelegate-by-retrieval-account [vault account1 vault account2...] --from [sender account] --fees [tx fees] --chain-id [chain ID]
```

**参数**

| 参数 | 类型 | 描述 |
|------|------|------|
| vault account1 vault account2... | String | 安全账户地址 |
| sender account | String | 发送者账户地址 |
| tx fees | String | 交易费用（例如，100000NANOGT） |
| chain-id | String | 链 ID |

**示例**
```bash
gatecli staking undelegate-by-retrieval-account vault11d9t6... vault11w8c3v... --from gt11dsck7f7txdc7hz5wdsspey0w5eyhw7yj4xrc3a6r4w4t39z9k45r86htlzxjf6z57an2r7 --fees 100000NANOGT --chain-id testnet
```

**响应示例**
```
  TxHash: BASIC-9F685A8362E6218E372CE60E306E8BC35B66006D82F9B3381A6AECE26FA6355CA38CD75AFFDF597794159D9356BE0376 
  //Transaction hash,using gatecli tx show {hash}to query details of this transaction.
  Data: rQO5zc/tCu8BYPD/ggoo3TuBsrqFDCGccbnTERbiO5v48wnG3ahDuFWSzrdVLzU0iuZQw2rzDBIoHk1VTbZ0J94UnjHi3aO8fwO1V5rK5I2NZvxNF1lFstSU9JD3J18JbxqUAWd0MXB1YjE4cTJmZ3VnZ3F5Znp0YzBwNXJhenEwZnRwdXplNzJwOXRwN25lZ2plZTl6amtjaGx2MHFwNThyZTdyZGduajNqd2x3d3JscjN6Z2o3cmNkcWxnc2Z3Y2V2YWRqaGE0ZXZoOThkejdzN3pjYzh5MHZhZnY3amh1ajNobXR1M2ZtajM2eXdqZWNtbnF1OWgSEgoMCgZOQU5PR1QSAjExEMCaDBowMXrG9msevrtuVTHWuZdFIixl5hSO4tWOvIZV01T/p+Pbg1sPeBgWGHbKUcm1064KImkKJeHhoPogZ32xdJvDkmTqENs7tchCbHrQ1z1n7Eeh1/ud9weWADUSQJRr9hYE0jvDKTx9IsfYAh3myFPQaYV9pt+TEi+IKdFm2KOZGYckVEbFx9ydMn2F6UbhopD5Y5HbrKJzf0fF9woyBNcEqQY=
  Raw Log: sync broadcast tx success //transaction is sent successfully
```
### 10. 查询特定委托人在共识账户中的取消委托

**命令**
```bash
gatecli staking undelegation [delegator account address] [consensus account address] --chain-id [chain ID]
```

**参数**

| 参数 | 类型 | 描述 |
|------|------|------|
| delegator account address | String | 委托人账户地址 |
| consensus account address | String | 共识账户地址 |
| chain-id | String | 链 ID |

**示例**
```bash
gatecli staking undelegation gt11m5acrv46s5xzr8r3h8f3z9hz8wdl3ucfcmw6ssac2kfvad649u6nfzhx2rpk4ucvrxla6s gt11ewdeyjdlxs2x4yglnhtulp0tclgdp7c2t8d5c6495r83qvrfr7p0qt40uddu3k44s7rxg6 --chain-id testnet
```

**响应示例**
```
Undelegations between:
  Delegator:   gt11m5acrv46s5xzr8r3h8f3z9hz8wdl3ucfcmw6ssac2kfvad649u6nfzhx2rpk4ucvrxla6s //delegator account address
  Con-account: gt11ewdeyjdlxs2x4yglnhtulp0tclgdp7c2t8d5c6495r83qvrfr7p0qt40uddu3k44s7rxg6 //consensus account address
	Entries:
		Undelegation 0:
		Creation Height:           904 //block height  of the undelegate transaction
		Min time to undelegate (unix): 2020-07-10 03:34:31 +0000 UTC //time finishing undelegating
		Expected balance:          10 //undelegated amount
```

### 11. 查询特定委托人在所有共识账户中的取消委托

**命令**
```bash
gatecli staking undelegations [delegator account address] --chain-id [chain ID]
```

**参数**

| 参数 | 类型 | 描述 |
|------|------|------|
| delegator account address | String | 委托人账户地址 |
| chain-id | String | 链 ID |

**示例**
```bash
gatecli staking undelegations gt11m5acrv46s5xzr8r3h8f3z9hz8wdl3ucfcmw6ssac2kfvad649u6nfzhx2rpk4ucvrxla6s --chain-id testnet
```

**响应示例**
```
Undelegations between:
  Delegator:   gt11m5acrv46s5xzr8r3h8f3z9hz8wdl3ucfcmw6ssac2kfvad649u6nfzhx2rpk4ucvrxla6s //delegator  account address
  Con-account: gt11ewdeyjdlxs2x4yglnhtulp0tclgdp7c2t8d5c6495r83qvrfr7p0qt40uddu3k44s7rxg6 //consensus account address
	Entries:
		Undelegation 0:
		Creation Height:           904 //block height  of undelegate transaction
		Min time to undelegate (unix): 2020-07-10 03:34:31 +0000 UTC //time finishing undelegating
		Expected balance:          10 //undelegated amount
Undelegations between:
	...
```

### 12. 查询特定共识账户的所有委托

**命令**
```bash
gatecli staking delegations-to [consensus account address] --chain-id [chain ID]
```

**参数**

| 参数 | 类型 | 描述 |
|------|------|------|
| consensus account address | String | 共识账户地址 |
| chain-id | String | 链 ID |

**示例**
```bash
gatecli staking delegations-to gt11m5acrv46s5xzr8r3h8f3z9hz8wdl3ucfcmw6ssac2kfvad649u6nfzhx2rpk4ucvrxla6s --chain-id testnet
```

**响应示例**
```
Delegation:
  Delegator:   gt11m5acrv46s5xzr8r3h8f3z9hz8wdl3ucfcmw6ssac2kfvad649u6nfzhx2rpk4ucvrxla6s //delegator account address
  Con-account: gt11ewdeyjdlxs2x4yglnhtulp0tclgdp7c2t8d5c6495r83qvrfr7p0qt40uddu3k44s7rxg6 //consensus account address
  Shares:      1000.000000000000000000 //delegation quantity
  Balance:   1000 //delegation token amount 
	...
```

### 13. 查询特定共识账户的所有委托转移

**命令**
```bash
gatecli staking redelegations-from [consensus account address] --chain-id [chain ID]
```

**参数**

| 参数 | 类型 | 描述 |
|------|------|------|
| consensus account address | String | 共识账户地址 |
| chain-id | String | 链 ID |

**示例**
```bash
gatecli staking redelegations-from gt11dsck7f7txdc7hz5wdsspey0w5eyhw7yj4xrc3a6r4w4t39z9k45r86htlzxjf6z57an2r7 --chain-id testnet
```

**响应示例**
```
Redelegations between:
  Delegator:                   gt11m5acrv46s5xzr8r3h8f3z9hz8wdl3ucfcmw6ssac2kfvad649u6nfzhx2rpk4ucvrxla6s //delegator account address
  Source Con-account:          gt11dsck7f7txdc7hz5wdsspey0w5eyhw7yj4xrc3a6r4w4t39z9k45r86htlzxjf6z57an2r7 //source consensus account address
  Destination Con-account:     gt11ewdeyjdlxs2x4yglnhtulp0tclgdp7c2t8d5c6495r83qvrfr7p0qt40uddu3k44s7rxg6 //target consensus account address
  Entries:
    Redelegation Entry #0:
      Creation height:           876 //block height of shift  delegation transaction
      Min time to Undelegations (unix): 2020-07-10 02:47:51 +0000 UTC //time finishing undelegating from source consensus account 
      Initial Balance:           10 //initial token amount of delegation shift 
      Shares:                    10.000000000000000000 //amount of the delegation shift
      Balance:                   10 //token amount of shift delegation 
```

### 14. 查询特定共识账户的所有取消委托

**命令**
```bash
gatecli staking undelegations-from  [consensus account address] --chain-id [chain ID]
```

**参数**

| 参数 | 类型 | 描述 |
|------|------|------|
| consensus account address | String | 共识账户地址 |
| chain-id | String | 链 ID |

**示例**
```bash
gatecli staking undelegations-from gt11dsck7f7txdc7hz5wdsspey0w5eyhw7yj4xrc3a6r4w4t39z9k45r86htlzxjf6z57an2r7 --chain-id testnet
```

**响应示例**
```
Undelegations between:
  Delegator:   gt11m5acrv46s5xzr8r3h8f3z9hz8wdl3ucfcmw6ssac2kfvad649u6nfzhx2rpk4ucvrxla6s //delegator account address
  Con-account: gt11ewdeyjdlxs2x4yglnhtulp0tclgdp7c2t8d5c6495r83qvrfr7p0qt40uddu3k44s7rxg6 //consensus account address
	Entries:
		Undelegation 0:
		Creation Height:           904 //block height of undelegate transaction
		Min time to undelegate (unix): 2020-07-10 03:34:31 +0000 UTC //time finishing undelegating
		Expected balance:          10 //delegated token amount
Undelegations between:
	...
```


### 15. 查询质押池

**命令**
```bash
gatecli staking pool --chain-id [chain ID]
```

**参数**

| 参数 | 类型 | 描述 |
|------|------|------|
| chain-id | String | 链 ID |

**响应示例**
```
Pool:
  Not Bonded Tokens:  10000
  Bonded Tokens:      0
```

### 16. 查询质押参数

**命令**
```bash
gatecli staking params --chain-id [chain ID]
```

**参数**

| 参数 | 类型 | 描述 |
|------|------|------|
| chain-id | String | 链 ID |

**响应示例**
```
Params:
  Undelegating Time:    504h0m0s
  Max Con-accounts:    100
  Max Entries:       7
  Bonded Coin Denom: NANOGT
  Pow Rate:          1
  Max Pow Rate:      2
```

## 基金会管理


### 1. 初始化

**命令**
```bash
gated add-foundation [config file path]
```

**参数**

| 参数 | 类型 | 描述 |
|------|------|------|
| config file path | String | 配置文件路径 |

**示例**
```bash
gated add-foundation foundation.json
```

**响应示例**
```
{
    "members":[
        {
            "address":"gt21twm7dma44k7wg5jppeyphrct9nx2l4m8szy44h72qv9eatyla3hkaevg3vx99mlsw2cxe3", 
            "proportion":"1",
            "funds_pool":[],
            "withdraw":[],
            "released":null
        },
        {
            "address":"gt11dsck7f7txdc7hz5wdsspey0w5eyhw7yj4xrc3a6r4w4t39z9k45r86htlzxjf6z57an2r7",
            "proportion":"2",
            "funds_pool":[],
            "withdraw":[],
            "released":null
        }
    ],
    "params":{
        "total_reward":[
            {
                "denom":"NANOGT",
                "amount":"20000000000000000"
            }
        ],
        "max_release_height":"15000000",
        "max_members":"20"
    }
}
```

**描述**
- 基金会初始化必须在创世块初始化时执行。
- max_members: 表示基金会允许的最大成员数。
- members: 基金会成员，地址（成员账户地址），比例（成员持有的比例）。


### 2. 查询基金会成员列表

**命令**
```bash
gatecli foundation members --chain-id [chain ID]
```

**参数**

| 参数 | 类型 | 描述 |
|------|------|------|
| chain-id | String | 链 ID |

**示例**
```bash
gatecli foundation members --chain-id testnet
```

**响应示例**
```
Member:
  Address:     gt11twm7dma44k7wg5jppeyphrct9nx2l4m8szy44h72qv9eatyla3hkaevg3vx99mlslwsnfq
  Proportion:  1
  FundsPool:   6666666666666666NANOGT
  Released:    373777777777.773295955555555556NANOGT
  Withdraw:
Member:
  ...
```

### 3. 基金会成员提款

**命令**
```bash
gatecli foundation withdraw [amount] --from [foundation members] --fees [tx fees] --chain-id [chain ID]
```

**参数**

| 参数 | 类型 | 描述 |
|------|------|------|
| amount | String | 提款金额 |
| foundation members | String | 基金会成员地址 |
| tx fees | String | 交易费用（例如100000NANOGT） |
| chain-id | String | 链 ID |

**示例**
```bash
gatecli foundation withdraw 10NANOGT --from gt11twm7dma44k7wg5jppeyphrct9nx2l4m8szy44h72qv9eatyla3hkaevg3vx99mlslwsnfq --fees 100000NANOGT --chain-id testnet
```

**响应示例**
```
TxHash: BASIC-9F685A8362E6218E372CE60E306E8BC35B66006D82F9B3381A6AECE26FA6355CA38CD75AFFDF597794159D9356BE0376
Data: rQO5zc/tCu8BYPD/ggoo3TuBsrqFDCGccbnTERbiO5v48wnG3ahDuFWSzrdVLzU0iuZQw2rzDBIoHk1VTbZ0J94UnjHi3aO8fwO1V5rK5I2NZvxNF1lFstSU9JD3J18JbxqUAWd0MXB1YjE4cTJmZ3VnZ3F5Znp0YzBwNXJhenEwZnRwdXplNzJwOXRwN25lZ2plZTl6amtjaGx2MHFwNThyZTdyZGduajNqd2x3d3JscjN6Z2o3cmNkcWxnc2Z3Y2V2YWRqaGE0ZXZoOThkejdzN3pjYzh5MHZhZnY3amh1ajNobXR1M2ZtajM2eXdqZWNtbnF1OWgSEgoMCgZOQU5PR1QSAjExEMCaDBowMXrG9msevrtuVTHWuZdFIixl5hSO4tWOvIZV01T/p+Pbg1sPeBgWGHbKUcm1064KImkKJeHhoPogZ32xdJvDkmTqENs7tchCbHrQ1z1n7Eeh1/ud9weWADUSQJRr9hYE0jvDKTx9IsfYAh3myFPQaYV9pt+TEi+IKdFm2KOZGYckVEbFx9ydMn2F6UbhopD5Y5HbrKJzf0fF9woyBNcEqQY=
Raw Log: sync broadcast tx success
```

## 代币管理

### 1. 发行代币

**命令**
```bash
gatecli token issue [token symbol] [token amount] --from [sender account] --fees [tx fees] --chain-id [chain ID]
```

**参数**

| 参数 | 类型 | 描述 |
|------|------|------|
| token symbol | String | 代币符号 |
| token amount | String | 代币数量 |
| sender account | String | 发送者账户地址 |
| tx fees | String | 交易费用（例如100000NANOGT） |
| chain-id | String | 链 ID |

**示例**
```bash
gatecli token issue "ABC" 100000000 --from gt11twm7dma44k7wg5jppeyphrct9nx2l4m8szy44h72qv9eatyla3hkaevg3vx99mlslwsnfq --fees 100000NANOGT --chain-id testnet
```

**响应示例**
```
TxHash: ISSUE-9F685A8362E6218E372CE60E306E8BC35B66006D82F9B3381A6AECE26FA6355CA38CD75AFFDF597794159D9356BE0376
Data: rQO5zc/tCu8BYPD/ggoo3TuBsrqFDCGccbnTERbiO5v48wnG3ahDuFWSzrdVLzU0iuZQw2rzDBIoHk1VTbZ0J94UnjHi3aO8fwO1V5rK5I2NZvxNF1lFstSU9JD3J18JbxqUAWd0MXB1YjE4cTJmZ3VnZ3F5Znp0YzBwNXJhenEwZnRwdXplNzJwOXRwN25lZ2plZTl6amtjaGx2MHFwNThyZTdyZGduajNqd2x3d3JscjN6Z2o3cmNkcWxnc2Z3Y2V2YWRqaGE0ZXZoOThkejdzN3pjYzh5MHZhZnY3amh1ajNobXR1M2ZtajM2eXdqZWNtbnF1OWgSEgoMCgZOQU5PR1QSAjExEMCaDBowMXrG9msevrtuVTHWuZdFIixl5hSO4tWOvIZV01T/p+Pbg1sPeBgWGHbKUcm1064KImkKJeHhoPogZ32xdJvDkmTqENs7tchCbHrQ1z1n7Eeh1/ud9weWADUSQJRr9hYE0jvDKTx9IsfYAh3myFPQaYV9pt+TEi+IKdFm2KOZGYckVEbFx9ydMn2F6UbhopD5Y5HbrKJzf0fF9woyBNcEqQY=
Raw Log: sync broadcast tx success
```

**描述**
- 发行代币会产生200000000000NANOGT的费用，请确保您的账户有足够的NANOGT代币。
- 代币符号必须全部大写，长度为2-15个字符。
- 在创建代币时，还有另外两个标志：
  - - '--mintable' 是否可以额外发行代币
  - - '--freezable' 是否可以冻结代币


### 2. 发行额外代币

**命令**
```bash
gatecli token mint [token amount] --from [sender account] --fees [tx fees] --chain-id [chain ID]
```

**参数**

| 参数 | 类型 | 描述 |
|------|------|------|
| token amount | String | 代币数量 |
| sender account | String | 发送者账户地址 |
| tx fees | String | 交易费用（例如100000NANOGT） |
| chain-id | String | 链 ID |

**示例**
```bash
gatecli token mint 100000000000AAA-94f --from gt11dsck7f7txdc7hz5wdsspey0w5eyhw7yj4xrc3a6r4w4t39z9k45r86htlzxjf6z57an2r7 --fees 100000NANOGT --chain-id testnet
```
**响应示例**
```
  TxHash: BASIC-9F685A8362E6218E372CE60E306E8BC35B66006D82F9B3381A6AECE26FA6355CA38CD75AFFDF597794159D9356BE0376 
  //transaction hash, using gatecli tx show {hash}to query details of this transaction
  Data: rQO5zc/tCu8BYPD/ggoo3TuBsrqFDCGccbnTERbiO5v48wnG3ahDuFWSzrdVLzU0iuZQw2rzDBIoHk1VTbZ0J94UnjHi3aO8fwO1V5rK5I2NZvxNF1lFstSU9JD3J18JbxqUAWd0MXB1YjE4cTJmZ3VnZ3F5Znp0YzBwNXJhenEwZnRwdXplNzJwOXRwN25lZ2plZTl6amtjaGx2MHFwNThyZTdyZGduajNqd2x3d3JscjN6Z2o3cmNkcWxnc2Z3Y2V2YWRqaGE0ZXZoOThkejdzN3pjYzh5MHZhZnY3amh1ajNobXR1M2ZtajM2eXdqZWNtbnF1OWgSEgoMCgZOQU5PR1QSAjExEMCaDBowMXrG9msevrtuVTHWuZdFIixl5hSO4tWOvIZV01T/p+Pbg1sPeBgWGHbKUcm1064KImkKJeHhoPogZ32xdJvDkmTqENs7tchCbHrQ1z1n7Eeh1/ud9weWADUSQJRr9hYE0jvDKTx9IsfYAh3myFPQaYV9pt+TEi+IKdFm2KOZGYckVEbFx9ydMn2F6UbhopD5Y5HbrKJzf0fF9woyBNcEqQY=
  Raw Log: sync broadcast tx success //transaction is sent successfully
```

**描述**
- 额外发行的代币必须使用参数 --mintable 在发行时使用。
- 代币数量的单位是链上代币符号（AAA-94f）
- 额外发行的代币数量和之前发行的代币数量之和不应超过默认上限（100亿）
  
### 3. 冻结代币

**命令**
```bash
gatecli token freeze [onchain token symbol] --from [sender account] --fees [tx fees] --chain-id [chain ID] 
```

**参数**

| 参数 | 类型 | 描述 |
|------|------|------|
| onchain token symbol | String | 代币符号 |
| sender account | String | 发送者账户地址 |
| tx fees | String | 交易费用（例如100000NANOGT） |
| chain-id | String | 链 ID |

**示例**
```bash
gatecli token freeze AAA-94f --from gt11dsck7f7txdc7hz5wdsspey0w5eyhw7yj4xrc3a6r4w4t39z9k45r86htlzxjf6z57an2r7 --fees 100000NANOGT --chain-id testnet
```
**响应示例**
```
 TxHash: BASIC-9F685A8362E6218E372CE60E306E8BC35B66006D82F9B3381A6AECE26FA6355CA38CD75AFFDF597794159D9356BE0376 
  //transaction hash, using gatecli tx show {hash}to query details of this transaction
  Data: rQO5zc/tCu8BYPD/ggoo3TuBsrqFDCGccbnTERbiO5v48wnG3ahDuFWSzrdVLzU0iuZQw2rzDBIoHk1VTbZ0J94UnjHi3aO8fwO1V5rK5I2NZvxNF1lFstSU9JD3J18JbxqUAWd0MXB1YjE4cTJmZ3VnZ3F5Znp0YzBwNXJhenEwZnRwdXplNzJwOXRwN25lZ2plZTl6amtjaGx2MHFwNThyZTdyZGduajNqd2x3d3JscjN6Z2o3cmNkcWxnc2Z3Y2V2YWRqaGE0ZXZoOThkejdzN3pjYzh5MHZhZnY3amh1ajNobXR1M2ZtajM2eXdqZWNtbnF1OWgSEgoMCgZOQU5PR1QSAjExEMCaDBowMXrG9msevrtuVTHWuZdFIixl5hSO4tWOvIZV01T/p+Pbg1sPeBgWGHbKUcm1064KImkKJeHhoPogZ32xdJvDkmTqENs7tchCbHrQ1z1n7Eeh1/ud9weWADUSQJRr9hYE0jvDKTx9IsfYAh3myFPQaYV9pt+TEi+IKdFm2KOZGYckVEbFx9ydMn2F6UbhopD5Y5HbrKJzf0fF9woyBNcEqQY=
  Raw Log: sync broadcast tx success //transaction is sent successfully
```

**描述**
- 冻结代币时，必须使用参数 --freezable 在发行时使用。


### 4. 解冻代币

**命令**
```bash
gatecli token unfreeze [token amount] --from [sender account] --fees [tx fees] --chain-id [chain ID]
```

**参数**

| 参数 | 类型 | 描述 |
|------|------|------|
| token amount | String | 代币数量 |
| sender account | String | 发送者账户地址 |
| tx fees | String | 交易费用（例如100000NANOGT） |
| chain-id | String | 链 ID |

**示例**
```bash
gatecli token unfreeze 100000NANOGT --from gt11twm7dma44k7wg5jppeyphrct9nx2l4m8szy44h72qv9eatyla3hkaevg3vx99mlslwsnfq --fees 100000NANOGT --chain-id testnet
```

**响应示例**
```
TxHash: BASIC-9F685A8362E6218E372CE60E306E8BC35B66006D82F9B3381A6AECE26FA6355CA38CD75AFFDF597794159D9356BE0376
Data: rQO5zc/tCu8BYPD/ggoo3TuBsrqFDCGccbnTERbiO5v48wnG3ahDuFWSzrdVLzU0iuZQw2rzDBIoHk1VTbZ0J94UnjHi3aO8fwO1V5rK5I2NZvxNF1lFstSU9JD3J18JbxqUAWd0MXB1YjE4cTJmZ3VnZ3F5Znp0YzBwNXJhenEwZnRwdXplNzJwOXRwN25lZ2plZTl6amtjaGx2MHFwNThyZTdyZGduajNqd2x3d3JscjN6Z2o3cmNkcWxnc2Z3Y2V2YWRqaGE0ZXZoOThkejdzN3pjYzh5MHZhZnY3amh1ajNobXR1M2ZtajM2eXdqZWNtbnF1OWgSEgoMCgZOQU5PR1QSAjExEMCaDBowMXrG9msevrtuVTHWuZdFIixl5hSO4tWOvIZV01T/p+Pbg1sPeBgWGHbKUcm1064KImkKJeHhoPogZ32xdJvDkmTqENs7tchCbHrQ1z1n7Eeh1/ud9weWADUSQJRr9hYE0jvDKTx9IsfYAh3myFPQaYV9pt+TEi+IKdFm2KOZGYckVEbFx9ydMn2F6UbhopD5Y5HbrKJzf0fF9woyBNcEqQY=
Raw Log: sync broadcast tx success
```

**注释**
- 代币数量必须使用链上代币符号（例如AAA-94f）


### 5. 燃烧代币

**命令**
```bash
gatecli token burn [token amount] --from [sender account] --fees [tx fees] --chain-id [chain ID] 
```

**参数**

| Parameter | Type | Description |
|------|------|------|
| token amount | String | Amount of tokens to unfreeze (e.g., 100000NANOGT) |
| sender account | String | Account address of the sender |
| tx fees | String | Transaction fees (e.g., 100000NANOGT) |
| chain-id | String | Chain ID of the network |

**示例**
```bash
gatecli token burn 100000000000AAA-94f  --from gt11dsck7f7txdc7hz5wdsspey0w5eyhw7yj4xrc3a6r4w4t39z9k45r86htlzxjf6z57an2r7 --fees 100000NANOGT --chain-id testnet
```

**响应示例**
```
TxHash: BASIC-9F685A8362E6218E372CE60E306E8BC35B66006D82F9B3381A6AECE26FA6355CA38CD75AFFDF597794159D9356BE0376 
//transaction hash, using gatecli tx show {hash}to query details of this transaction
Data: rQO5zc/tCu8BYPD/ggoo3TuBsrqFDCGccbnTERbiO5v48wnG3ahDuFWSzrdVLzU0iuZQw2rzDBIoHk1VTbZ0J94UnjHi3aO8fwO1V5rK5I2NZvxNF1lFstSU9JD3J18JbxqUAWd0MXB1YjE4cTJmZ3VnZ3F5Znp0YzBwNXJhenEwZnRwdXplNzJwOXRwN25lZ2plZTl6amtjaGx2MHFwNThyZTdyZGduajNqd2x3d3JscjN6Z2o3cmNkcWxnc2Z3Y2V2YWRqaGE0ZXZoOThkejdzN3pjYzh5MHZhZnY3amh1ajNobXR1M2ZtajM2eXdqZWNtbnF1OWgSEgoMCgZOQU5PR1QSAjExEMCaDBowMXrG9msevrtuVTHWuZdFIixl5hSO4tWOvIZV01T/p+Pbg1sPeBgWGHbKUcm1064KImkKJeHhoPogZ32xdJvDkmTqENs7tchCbHrQ1z1n7Eeh1/ud9weWADUSQJRr9hYE0jvDKTx9IsfYAh3myFPQaYV9pt+TEi+IKdFm2KOZGYckVEbFx9ydMn2F6UbhopD5Y5HbrKJzf0fF9woyBNcEqQY=
Raw Log: sync broadcast tx success //transaction is sent successfully
```

**注释**
- 代币数量必须使用链上代币符号（例如AAA-94f）

### 6. 查询代币信息

**命令**
```bash
gatecli token show [token symbol] --chain-id [chain ID]
```

**参数**

| 参数 | 类型 | 描述 |
|------|------|------|
| token symbol | String | 代币符号 |
| chain-id | String | 链 ID |

**示例**
```bash
gatecli token show ABC-94F --chain-id testnet
```

**响应示例**
```
Token:
  Symbol:          ABC-94F
  SourceAddress:   gt11twm7dma44k7wg5jppeyphrct9nx2l4m8szy44h72qv9eatyla3hkaevg3vx99mlslwsnfq
  TokenType:       1
  FreezeType:      1
  TotalSupply:     100000000
  SendLock:        0
  WithdrawLock:    0
```

### 7. 查询所有代币

**命令**
```bash
gatecli token list --chain-id [chain ID]
```

**参数**

| 参数 | 类型 | 描述 |
|------|------|------|
| chain-id | String | 链 ID |

**示例**
```bash
gatecli token list --chain-id testnet
```

**响应示例**
```
Token:
  Symbol:          ABC-94F
  SourceAddress:   gt11twm7dma44k7wg5jppeyphrct9nx2l4m8szy44h72qv9eatyla3hkaevg3vx99mlslwsnfq
  TokenType:       1
  FreezeType:      1
  TotalSupply:     100000000
  SendLock:        0
  WithdrawLock:    0
Token:
  ...
```

## 转账管理

### 1. 发送交易

**命令**
```bash
gatecli tx send [recipient account] [amount] --from [sender account] --fees [tx fees] --chain-id [chain ID]
```

**参数**

| 参数 | 类型 | 描述 |
|------|------|------|
| recipient account | String | 接收者账户地址 |
| amount | String | 转账金额 |
| sender account | String | 发送者账户地址 |
| tx fees | String | 交易费用（例如100000NANOGT） |
| chain-id | String | 链 ID |


**示例**
```bash
gatecli tx send gt11twm7dma44k7wg5jppeyphrct9nx2l4m8szy44h72qv9eatyla3hkaevg3vx99mlslwsnfq 100000NANOGT --from gt11twm7dma44k7wg5jppeyphrct9nx2l4m8szy44h72qv9eatyla3hkaevg3vx99mlslwsnfq --fees 100000NANOGT --chain-id testnet 
```

**响应示例**
```
TxHash: TRANSFER-9F685A8362E6218E372CE60E306E8BC35B66006D82F9B3381A6AECE26FA6355CA38CD75AFFDF597794159D9356BE0376
Data: rQO5zc/tCu8BYPD/ggoo3TuBsrqFDCGccbnTERbiO5v48wnG3ahDuFWSzrdVLzU0iuZQw2rzDBIoHk1VTbZ0J94UnjHi3aO8fwO1V5rK5I2NZvxNF1lFstSU9JD3J18JbxqUAWd0MXB1YjE4cTJmZ3VnZ3F5Znp0YzBwNXJhenEwZnRwdXplNzJwOXRwN25lZ2plZTl6amtjaGx2MHFwNThyZTdyZGduajNqd2x3d3JscjN6Z2o3cmNkcWxnc2Z3Y2V2YWRqaGE0ZXZoOThkejdzN3pjYzh5MHZhZnY3amh1ajNobXR1M2ZtajM2eXdqZWNtbnF1OWgSEgoMCgZOQU5PR1QSAjExEMCaDBowMXrG9msevrtuVTHWuZdFIixl5hSO4tWOvIZV01T/p+Pbg1sPeBgWGHbKUcm1064KImkKJeHhoPogZ32xdJvDkmTqENs7tchCbHrQ1z1n7Eeh1/ud9weWADUSQJRr9hYE0jvDKTx9IsfYAh3myFPQaYV9pt+TEi+IKdFm2KOZGYckVEbFx9ydMn2F6UbhopD5Y5HbrKJzf0fF9woyBNcEqQY=
Raw Log: sync broadcast tx success
```

### 2. 查询单笔交易

**命令**
```bash
gatecli tx show [transaction  Hash] --chain-id [chain ID]
```

**参数**

| 参数 | 类型 | 描述 |
|------|------|------|
| transaction hash | String | 交易哈希 |
| chain-id | String | 链 ID |

**示例**
```bash
gatecli tx show 03190D3F56D6B65CC11BFE3F9CD961729B134D224A35AC731728601C9DD3A3C7 --chain-id testnet
```

**响应示例**
```
Response:
  Height: 761 //block height of transaction
  TxHash: IRREVOCABLEPAY-9D8A3C2103C7B9BA5C99A1F52E28034F42A28404F1BF9E92C533B6A0B17E81F0FBDA199F4E2661FB420D2E216257AAB8 //transaction hash
  Data: A702B9CDCFED0A6ADC2AA0850A286C316F27CB3371EB8A8E6C201C91EEA649777892A98788F743ABAAB89445B56833EAEBF88D24E8541228556556A9725BF1E60C14C8EB355A040D2254598A3D8638133426D4456D34CD988543D3C91BDDAEE21A100A064E414E4F4754120631303030303012120A0C0A064E414E4F47541202313110C09A0C1A30B7BEC61FBCE269F041C962481652F2A6763659FB783D40B23FF202C1800AE34E7D6498F28DBBE0E9DE8992F93CC9424122690A25E1E1A0FA20D46BB9C39EB4DB5489519A22873304927ED1DFE44A5648EE613503758F446908124047FF53D27F08FA3B7C478F560B95570759AA9EBC8CBBD5690A51A00A07CBBB9353A8328973BA9B44D426AECD0DEB8E5382704D785EBFD9838CAF0791A5FC12063204ED05BF07
  Raw Log: [{"msg_index":0,"success":true,"log":"","events":[{"type":"message","attributes":[{"key":"sender","value":"gt11dsck7f7txdc7hz5wdsspey0w5eyhw7yj4xrc3a6r4w4t39z9k45r86htlzxjf6z57an2r7"},{"key":"module","value":"bank"},{"key":"action","value":"send"}]},{"type":"transfer","attributes":[{"key":"recipient","value":"gt1124j4d2tjt0c7vrq5er4n2ksyp539gkv28krrsye5ym2y2mf5ekvg2s7neydamthz7a7xld"},{"key":"amount","value":"100000NANOGT"}]}]}]
  Logs: [{"msg_index":0,"success":true,"log":"","events":[{"type":"message","attributes":[{"key":"sender","value":"gt11dsck7f7txdc7hz5wdsspey0w5eyhw7yj4xrc3a6r4w4t39z9k45r86htlzxjf6z57an2r7"},{"key":"module","value":"bank"},{"key":"action","value":"send"}]},{"type":"transfer","attributes":[{"key":"recipient","value":"vault1124j4d2tjt0c7vrq5er4n2ksyp539gkv28krrsye5ym2y2mf5ekvg2s7neydamthzwag272"},{"key":"amount","value":"100000NANOGT"}]}]}]
  GasWanted: 200000 //Gas consumed by this transaction
  GasUsed: 75207 
  Timestamp: 2020-06-19T07:36:11+08:00 //UTC transaction time
  Events:
		- message
			- sender: gt11dsck7f7txdc7hz5wdsspey0w5eyhw7yj4xrc3a6r4w4t39z9k45r86htlzxjf6z57an2r7 //sender account
			- module: bank //transaction module
			- action: send //transaction action
		- transfer
			- recipient:  vault1124j4d2tjt0c7vrq5er4n2ksyp539gkv28krrsye5ym2y2mf5ekvg2s7neydamthzwag272 //recipient account
			- amount: 100000NANOGT //transfer token amount 

```


### 3. 根据条件查询交易列表

**命令**
```bash
gatecli tx search 
--tags [<tag1>:<value1>&<tag2>:<value2>] 
--page [page number] 
--limit [entries per page] 
--chain-id [chain ID]
```

**参数**

| 参数 | 类型 | 描述 |
|------|------|------|
| tags | String | 交易标签 |
| page | String | 页码 |
| limit | String | 每页条数 |
| chain-id | String | 链 ID |

**示例**
```bash
gatecli tx search --tags 'tx.height:771&message.sender:gt11dsck7f7txdc7hz5wdsspey0w5eyhw7yj4xrc3a6r4w4t39z9k45r86htlzxjf6z57an2r7' --page 1 --limit 30 --chain-id testnet
```

**响应示例**
```
[
    {
        "count": "1", //the number of queries
        "limit": "100", //entries per page
        "page_number": "1", //page number
        "page_total": "1", //total pages
        "total_count": "1", //total entries
        "txs": [
            {
                "data": "A802B9CDCFED0A6BB9AE6FBE0A280273979685CF46967B3CEF04E3FDE9FC89B748D1AEF505630C95F81C50AD416399F657E2571C828F1228817243C326F338B53826CC93443332C45EFB00C1E81311FAD6B5A1AECF0E7CA43762221BB9FC03FD1A110A064E414E4F475412073130303030303012120A0C0A064E414E4F47541202313110C09A0C1A30E3F427451551B57618A238092EF6B7CFF333D55AC23A7F07D58AE9845E2E49A65B019F71D4F393B29DAFB4E8E605E40922690A25E1E1A0FA20C939025C7A4DF13E0525E30C425B4A89A4DDBC38B1373B00C5F4D6CCEC77F27B1240F8369BB605D0DD32277F9370EF9FC7D0028D50E62CD7892594C6C1E0689551D9B8A4765F244528C436796F3300A5FDEC7817DE5A286C78EB485C4EAB9D259B0C3204CD209F22",
                "events": [
                    {
                        "attributes": [
                            {
                                "key": "con-account",
                                "value": "gt11s9ey8sex7vut2wpxejf5gvejc300kqxpaqf3r7kkkks6ancw0jjrwc3zrwulcqlamjqzwd" //consensus account address
                            },
                            {
                                "key": "amount",
                                "value": "1000000" //delegation token amount
                            }
                        ],
                        "type": "delegate" //transaction tyep
                    },
                    {
                        "attributes": [
                            {
                                "key": "module",
                                "value": "staking"
                            },
                            {
                                "key": "sender",
                                "value": "gt11qfee0959earfv7euauzw8l0fljymwjx34m6s2ccvjhupc59dg93enajhuft3eq50tvz39x" //sender
                            },
                            {
                                "key": "action",
                                "value": "delegation"
                            }
                        ],
                        "type": "message"
                    }
                ],
                "gas_used": "107880",
                "gas_wanted": "200000",
                "height": "4185", //block height of tranasction
                "logs": [
                    {
                        "events": [
                            {
                                "attributes": [
                                    {
                                        "key": "con-account",
                                        "value": "gt11s9ey8sex7vut2wpxejf5gvejc300kqxpaqf3r7kkkks6ancw0jjrwc3zrwulcqlamjqzwd" //consensus account address
                                    },
                                    {
                                        "key": "amount",
                                        "value": "1000000"
                                    }
                                ],
                                "type": "delegation"
                            },
                            {
                                "attributes": [
                                    {
                                        "key": "module",
                                        "value": "staking"
                                    },
                                    {
                                        "key": "sender",
                                        "value": "gt11qfee0959earfv7euauzw8l0fljymwjx34m6s2ccvjhupc59dg93enajhuft3eq50tvz39x" //sender
                                    },
                                    {
                                        "key": "action",
                                        "value": "delegation"
                                    }
                                ],
                                "type": "message"
                            }
                        ],
                        "log": "",
                        "msg_index": 0,
                        "success": true
                    }
                ],
                "raw_log": "[{\"msg_index\":0,\"success\":true,\"log\":\"\",\"events\":[{\"type\":\"delegate\",\"attributes\":[{\"key\":\"con-account\",\"value\":\"gt11s9ey8sex7vut2wpxejf5gvejc300kqxpaqf3r7kkkks6ancw0jjrwc3zrwulcqlamjqzwd\"},{\"key\":\"amount\",\"value\":\"1000000\"}]},{\"type\":\"message\",\"attributes\":[{\"key\":\"module\",\"value\":\"staking\"},{\"key\":\"sender\",\"value\":\"gt11qfee0959earfv7euauzw8l0fljymwjx34m6s2ccvjhupc59dg93enajhuft3eq50tvz39x\"},{\"key\":\"action\",\"value\":\"delegate\"}]}]}]",
                "timestamp": "2020-06-06T03:28:28+08:00",
                "tx": {
                    "type": "StdTx",
                    "value": {
                        "fee": {
                            "amount": [
                                {
                                    "amount": "11", //transaction commission
                                    "denom": "NANOGT"
                                }
                            ],
                            "gas": "200000"
                        },
                        "memo": "",
                        "msg": [
                            {
                                "type": "MsgDelegate",
                                "value": {
                                    "amount": {
                                        "amount": "1000000", //delegation token amount 
                                        "denom": "NANOGT"
                                    },
                                    "delegator_address": "gt11qfee0959earfv7euauzw8l0fljymwjx34m6s2ccvjhupc59dg93enajhuft3eq50tvz39x", //delegator address
                                    "con-account_address": "gt11s9ey8sex7vut2wpxejf5gvejc300kqxpaqf3r7kkkks6ancw0jjrwc3zrwulcqlamjqzwd" //consensus account address
                                }
                            }
                        ],
                        "nonces": [
                            "4/QnRRVRtXYYojgJLva3z/Mz1VrCOn8H1YrphF4uSaZbAZ9x1POTsp2vtOjmBeQJ"
                        ],
                        "signatures": [
                            {
                                "pub_key": {
                                    "type": "gatechain/PubKeyEd25519",
                                    "value": "yTkCXHpN8T4FJeMMQltKiaTdvDixNzsAxfTWzOx38ns="
                                },
                                "signature": "+DabtgXQ3TInf5Nw75/H0AKNUOYs14kllMbB4GiVUdm4pHZfJEUoxDZ5bzMApf3seBfeWihseOtIXE6rnSWbDA==" //Signature
                            }
                        ],
                        "valid_height": [ 
                            "4173",
                            "4383" //height the transaction takes effect
                        ]
                    }
                },
                "txhash": "BASIC-57884EB3E55CD2BDA7E912D6B2851CB539A4C4ED40DFC164B0AF57E9A49D512883E353D38677EC051055A17A948415A7" //transaction hash
            }
        ]
    }
]
```
### 4. 单签

**命令**
```bash
gatecli tx sign [tx file] --from [sender account] --chain-id [chain ID] --generate-only
```

**参数**

| 参数 | 类型 | 描述 |
|------|------|------|
| tx file | String | 交易文件路径 |
| sender account | String | 发送者账户地址 |
| chain-id | String | 链 ID |

**示例**
```bash
gatecli tx sign unsignedTx.json --from gt11twm7dma44k7wg5jppeyphrct9nx2l4m8szy44h72qv9eatyla3hkaevg3vx99mlslwsnfq --chain-id testnet --generate-only > signedTx.json
```

**注释**
- 单签交易将保存到指定输出文件

### 5. 多签

**命令**
```bash
gatecli tx multisign [tx file] [multisig account name] [signer-1 account] [signer-2 account] ... [signer-n account] --chain-id [chain ID]
```

**参数**

| 参数 | 类型 | 描述 |
|------|------|------|
| tx file | String | 交易文件路径 |
| multisig account name | String | 多签账户名称 |
| signer account | String | 每个签名者的账户地址 |
| chain-id | String | 链 ID |

**示例**
```bash
gatecli tx multisign unsignedTx.json gt11twm7dma44k7wg5jppeyphrct9nx2l4m8szy44h72qv9eatyla3hkaevg3vx99mlslwsnfq gt11dsck7f7txdc7hz5wdsspey0w5eyhw7yj4xrc3a6r4w4t39z9k45r86htlzxjf6z57an2r7 --chain-id testnet
```

**响应示例**
```
TxHash: MULTISIGN-9F685A8362E6218E372CE60E306E8BC35B66006D82F9B3381A6AECE26FA6355CA38CD75AFFDF597794159D9356BE0376
Data: rQO5zc/tCu8BYPD/ggoo3TuBsrqFDCGccbnTERbiO5v48wnG3ahDuFWSzrdVLzU0iuZQw2rzDBIoHk1VTbZ0J94UnjHi3aO8fwO1V5rK5I2NZvxNF1lFstSU9JD3J18JbxqUAWd0MXB1YjE4cTJmZ3VnZ3F5Znp0YzBwNXJhenEwZnRwdXplNzJwOXRwN25lZ2plZTl6amtjaGx2MHFwNThyZTdyZGduajNqd2x3d3JscjN6Z2o3cmNkcWxnc2Z3Y2V2YWRqaGE0ZXZoOThkejdzN3pjYzh5MHZhZnY3amh1ajNobXR1M2ZtajM2eXdqZWNtbnF1OWgSEgoMCgZOQU5PR1QSAjExEMCaDBowMXrG9msevrtuVTHWuZdFIixl5hSO4tWOvIZV01T/p+Pbg1sPeBgWGHbKUcm1064KImkKJeHhoPogZ32xdJvDkmTqENs7tchCbHrQ1z1n7Eeh1/ud9weWADUSQJRr9hYE0jvDKTx9IsfYAh3myFPQaYV9pt+TEi+IKdFm2KOZGYckVEbFx9ydMn2F6UbhopD5Y5HbrKJzf0fF9woyBNcEqQY=
Raw Log: sync broadcast tx success
```



### 6. 广播交易

**命令**
```bash
gatecli tx broadcast [tx file] --chain-id [chain ID]
```

**参数**

| 参数 | 类型 | 描述 |
|------|------|------|
| tx file | String | 交易文件路径 |
| chain-id | String | 链 ID |

**示例**
```bash
gatecli tx broadcast signedTx.json --chain-id testnet
```

**响应示例**
```
TxHash: BROADCAST-9F685A8362E6218E372CE60E306E8BC35B66006D82F9B3381A6AECE26FA6355CA38CD75AFFDF597794159D9356BE0376
Data: rQO5zc/tCu8BYPD/ggoo3TuBsrqFDCGccbnTERbiO5v48wnG3ahDuFWSzrdVLzU0iuZQw2rzDBIoHk1VTbZ0J94UnjHi3aO8fwO1V5rK5I2NZvxNF1lFstSU9JD3J18JbxqUAWd0MXB1YjE4cTJmZ3VnZ3F5Znp0YzBwNXJhenEwZnRwdXplNzJwOXRwN25lZ2plZTl6amtjaGx2MHFwNThyZTdyZGduajNqd2x3d3JscjN6Z2o3cmNkcWxnc2Z3Y2V2YWRqaGE0ZXZoOThkejdzN3pjYzh5MHZhZnY3amh1ajNobXR1M2ZtajM2eXdqZWNtbnF1OWgSEgoMCgZOQU5PR1QSAjExEMCaDBowMXrG9msevrtuVTHWuZdFIixl5hSO4tWOvIZV01T/p+Pbg1sPeBgWGHbKUcm1064KImkKJeHhoPogZ32xdJvDkmTqENs7tchCbHrQ1z1n7Eeh1/ud9weWADUSQJRr9hYE0jvDKTx9IsfYAh3myFPQaYV9pt+TEi+IKdFm2KOZGYckVEbFx9ydMn2F6UbhopD5Y5HbrKJzf0fF9woyBNcEqQY=
Raw Log: sync broadcast tx success
```

### 7. 编码

**命令**
```bash
gatecli tx encode [file path]
```

**参数**

| 参数 | 类型 | 描述 |
|------|------|------|
| file path | String | 交易文件路径 |

**示例**
```bash
gatecli tx encode tx_sign.json
```

**响应示例**
```
vAG5zc/tCmrcKqCFCihsMW8nyzNx64qObCAcke6mSXd4kqmHiPdDq6q4lEW1aDPq6/iNJOhUEihVZVapclvx5gwUyOs1WgQNIlRZij2GOBM0JtRFbTTNmIVD08kb3a7iGhAKBk5BTk9HVBIGMTAwMDAwEhIKDAoGTkFOT0dUEgIxMRDAmgwaMHZI3vYAiiIVpsyR/xtwvbu9zTfr0vxZUmT7lhu3unO0lOsHGyPZ3GqlJ9La3c5L7DIEoAboBw==
```

### 8. 预估gas

**命令**
```bash
gatecli tx send [recipient account] [amount] --from [sender account]  --chain-id [chain ID] ----dry-run --gas-adjustment [value]
```

**参数**

| 参数 | 类型 | 描述 |
|------|------|------|
| recipient account | String | 接收者账户地址 |
| amount | String | 转账金额 |
| sender account | String | 发送者账户地址 |
| chain-id | String | 链 ID |
| gas-adjustment  | float | 预估系数 |

**示例**
```bash
gatecli tx send gt11twm7dma44k7wg5jppeyphrct9nx2l4m8szy44h72qv9eatyla3hkaevg3vx99mlslwsnfq 100000NANOGT --from gt11twm7dma44k7wg5jppeyphrct9nx2l4m8szy44h72qv9eatyla3hkaevg3vx99mlslwsnfq  --chain-id testnet --dry-run --gas-adjustment 1.4
```

**响应示例**
```
gas estimate: 71349
```
### 9. 根据预估gas发送交易

**命令**
```bash
gatecli tx send [recipient account] [amount] --from [sender account] --fees [tx fees] --gas [gaslimit] --chain-id [chain ID]
```

**参数**

| 参数 | 类型 | 描述 |
|------|------|------|
| recipient account | String | 接收者账户地址 |
| amount | String | 转账金额 |
| sender account | String | 发送者账户地址 |
| tx fees | String | 交易费用（例如100000NANOGT） |
| gas| String | gaslimit |
| chain-id | String | 链 ID |

**示例**
```bash
gatecli tx send gt11twm7dma44k7wg5jppeyphrct9nx2l4m8szy44h72qv9eatyla3hkaevg3vx99mlslwsnfq 100000NANOGT --from gt11twm7dma44k7wg5jppeyphrct9nx2l4m8szy44h72qv9eatyla3hkaevg3vx99mlslwsnfq --fees 713490NANOGT --gas 71349 --chain-id testnet 
```

**响应示例**
```
Response:
  TxHash: IRREVOCABLEPAY-2C8F3C621A31CC84D11FAE2F3CA3F2A9315F0BBCC6EC4D17FA76920E0957B43ECC488C51952BDB592249D64E575AB02E
  Data: ngK5zc/tCl7cKqCFCiiJ3719mjDKkN+wByfBhTyFH31nlWIIfDLjF0nFlNa8uKExO8jRNFKBEhQ/qxhGItwZthCTSblIEUk78qRTYhoYCgZOQU5PR1QSDjEwMDAwMDAwMDAwMDAwEhYKEAoGTkFOT0dUEgY3MTM0OTAQta0EGjAAFFGQ8fF+ak+aFnmJafU2/01YaB5viwFYTYsrDuDBfau8+rUyHdtO9D4tLA/4e84iaQol4eGg+iBx0YPKICxemJ0xaakoVkgfJ8HG4FOYs1WKjCM5QPA1aBJA9I57PDH02c8vd1VpQvfiEqNkCXH5crGvPMos/zzj/SfNJTlcqJjl/Hu0Wawh65A/yUCHqo9fvZBbQDA3foGPCzIDFOYB
  Raw Log: sync broadcast tx success
```
**注释**
- gasprice=fees/gas，gasprice必须大于basefee, gas 默认值为200000
- basefee 可以通过evm rpc eth_getBlockByNumber方法查询


## 保险账户
### 1. 创建保险账户
**命令**
```bash
gatecli vault-account create [基础账户] [找回账户] [延迟生效高度] [清算高度] [转移代币数量] --from [发送者账户] --fees [交易手续费] --chain-id [链ID]
```

**参数**

| 参数 | 类型 | 描述 |
|------|------|------|
| 基础账户 | String | 基础账户地址 |
| 找回账户 | String | 找回账户地址 |
| 延迟生效高度 | Int | 保险账户延迟生效高度 |
| 清算高度 | Int | 保险账户清算高度 |
| 转移代币数量 | String | 转移到保险账户的代币数量 |
| 发送者账户 | String | 发送者账户 |
| 交易手续费 | String | 交易手续费 |
| 链 ID | String | 链 ID |

**示例**
```bash
gatecli vault-account create gt11dsck7f7txdc7hz5wdsspey0w5eyhw7yj4xrc3a6r4w4t39z9k45r86htlzxjf6z57an2r7 gt11dsck7f7txdc7hz5wdsspey0w5eyhw7yj4xrc3a6r4w4t39z9k45r86htlzxjf6z57a3457 38 300 30NANOGT --from gt11dsck7f7txdc7hz5wdsspey0w5eyhw7yj4xrc3a6r4w4t39z9k45r86htlzxjf6z57a3457 --fees 10000000NANOGT --chain-id testnet
```

**响应示例**
```
TxHash: BASIC-9F685A8362E6218E372CE60E306E8BC35B66006D82F9B3381A6AECE26FA6355CA38CD75AFFDF597794159D9356BE0376 
//交易hash，使用gatecli tx show {hash}查询此交易详细信息
Data: rQO5zc/tCu8BYPD/ggoo3TuBsrqFDCGccbnTERbiO5v48wnG3ahDuFWSzrdVLzU0iuZQw2rzDBIoHk1VTbZ0J94UnjHi3aO8fwO1V5rK5I2NZvxNF1lFstSU9JD3J18JbxqUAWd0MXB1YjE4cTJmZ3VnZ3F5Znp0YzBwNXJhenEwZnRwdXplNzJwOXRwN25lZ2plZTl6amtjaGx2MHFwNThyZTdyZGduajNqd2x3d3JscjN6Z2o3cmNkcWxnc2Z3Y2V2YWRqaGE0ZXZoOThkejdzN3pjYzh5MHZhZnY3amh1ajNobXR1M2ZtajM2eXdqZWNtbnF1OWgSEgoMCgZOQU5PR1QSAjExEMCaDBowMXrG9msevrtuVTHWuZdFIixl5hSO4tWOvIZV01T/p+Pbg1sPeBgWGHbKUcm1064KImkKJeHhoPogZ32xdJvDkmTqENs7tchCbHrQ1z1n7Eeh1/ud9weWADUSQJRr9hYE0jvDKTx9IsfYAh3myFPQaYV9pt+TEi+IKdFm2KOZGYckVEbFx9ydMn2F6UbhopD5Y5HbrKJzf0fF9woyBNcEqQY=
Raw Log: sync broadcast tx success //交易发送成功
```

**注释**
- 必须使用新生成的账户作为基础账户，用来发送创建保险账户交易
- 如果先向该账户发送了普通交易，该账户即为普通账户，不能用来再发送创建保险账户交易
- 清算高度需要大于（当前高度+延迟生效高度）

### 2. 修改清算高度

**命令**
```bash
gatecli vault-account update-clearing-height [新的清算高度] --from [发送者账户] --fees [交易手续费] --chain-id [链ID]
```

**参数**

| 参数 | 类型 | 描述 |
|------|------|------|
| 新的清算高度 | Int | 要设置的新清算高度 |
| 发送者账户 | String | 发送者账户 |
| 交易手续费 | String | 交易手续费 |
| 链 ID | String | 链 ID |

**示例**
```bash
gatecli vault-account update-clearing-height 100000 --from vault11fg056uaatk9s3k6l34eqkc7qchjup8e39afauf9naz4jmpklsze3uwx6405vll88l5lvww --fees 10000000NANOGT --chain-id testnet
```

**响应示例**
```
TxHash: ACCOUNTSET-9F685A8362E6218E372CE60E306E8BC35B66006D82F9B3381A6AECE26FA6355CA38CD75AFFDF597794159D9356BE0376 
//交易hash，使用gatecli tx show {hash}查询此交易详细信息
Data: rQO5zc/tCu8BYPD/ggoo3TuBsrqFDCGccbnTERbiO5v48wnG3ahDuFWSzrdVLzU0iuZQw2rzDBIoHk1VTbZ0J94UnjHi3aO8fwO1V5rK5I2NZvxNF1lFstSU9JD3J18JbxqUAWd0MXB1YjE4cTJmZ3VnZ3F5Znp0YzBwNXJhenEwZnRwdXplNzJwOXRwN25lZ2plZTl6amtjaGx2MHFwNThyZTdyZGduajNqd2x3d3JscjN6Z2o3cmNkcWxnc2Z3Y2V2YWRqaGE0ZXZoOThkejdzN3pjYzh5MHZhZnY3amh1ajNobXR1M2ZtajM2eXdqZWNtbnF1OWgSEgoMCgZOQU5PR1QSAjExEMCaDBowMXrG9msevrtuVTHWuZdFIixl5hSO4tWOvIZV01T/p+Pbg1sPeBgWGHbKUcm1064KImkKJeHhoPogZ32xdJvDkmTqENs7tchCbHrQ1z1n7Eeh1/ud9weWADUSQJRr9hYE0jvDKTx9IsfYAh3myFPQaYV9pt+TEi+IKdFm2KOZGYckVEbFx9ydMn2F6UbhopD5Y5HbrKJzf0fF9woyBNcEqQY=
Raw Log: sync broadcast tx success //交易发送成功
```

**注释**
- 新的清算高度必须大于区块链当前高度 + 保险账户延迟生效高度


### 3. 账户清算交易

**命令**
```bash
gatecli vault-account clear [保险账户1]([保险账户2] [保险账户3]...) --from [发送者账户] --fees [交易手续费] --chain-id [链ID]
```

**参数**

| 参数 | 类型 | 描述 |
|------|------|------|
| 保险账户 | String | 要清算的保险账户地址列表 |
| 发送者账户 | String | 发送者账户 |
| 交易手续费 | String | 交易手续费 |
| 链 ID | String | 链 ID |

**示例**
```bash
gatecli vault-account clear vault11d9t6... vault11w8c3v... vault11v5s2... --from gt11dsck7f7txdc7hz5wdsspey0w5eyhw7yj4xrc3a6r4w4t39z9k45r86htlzxjf6z57an2r7 --fees 10000000NANOGT --chain-id testnet
```

**响应示例**
```
TxHash: VAULTCLEAR-9F685A8362E6218E372CE60E306E8BC35B66006D82F9B3381A6AECE26FA6355CA38CD75AFFDF597794159D9356BE0376 
//交易hash，使用gatecli tx show {hash}查询此交易详细信息
Data: rQO5zc/tCu8BYPD/ggoo3TuBsrqFDCGccbnTERbiO5v48wnG3ahDuFWSzrdVLzU0iuZQw2rzDBIoHk1VTbZ0J94UnjHi3aO8fwO1V5rK5I2NZvxNF1lFstSU9JD3J18JbxqUAWd0MXB1YjE4cTJmZ3VnZ3F5Znp0YzBwNXJhenEwZnRwdXplNzJwOXRwN25lZ2plZTl6amtjaGx2MHFwNThyZTdyZGduajNqd2x3d3JscjN6Z2o3cmNkcWxnc2Z3Y2V2YWRqaGE0ZXZoOThkejdzN3pjYzh5MHZhZnY3amh1ajNobXR1M2ZtajM2eXdqZWNtbnF1OWgSEgoMCgZOQU5PR1QSAjExEMCaDBowMXrG9msevrtuVTHWuZdFIixl5hSO4tWOvIZV01T/p+Pbg1sPeBgWGHbKUcm1064KImkKJeHhoPogZ32xdJvDkmTqENs7tchCbHrQ1z1n7Eeh1/ud9weWADUSQJRr9hYE0jvDKTx9IsfYAh3myFPQaYV9pt+TEi+IKdFm2KOZGYckVEbFx9ydMn2F6UbhopD5Y5HbrKJzf0fF9woyBNcEqQY=
Raw Log: sync broadcast tx success //交易发送成功
```

**注释**
- 发送者--from，必须是此保险账户的安全账户
- 所有的保险账户的清算高度都必须达到


### 4. 查询保险账户信息

**命令**
```bash
gatecli vault-account show [保险账户] --chain-id [链ID]
```

**参数**

| 参数 | 类型 | 描述 |
|------|------|------|
| 保险账户 | String | 保险账户地址 |
| 链 ID | String | 链 ID |

**示例**
```bash
gatecli vault-account show vault11fg056uaatk9s3k6l34eqkc7qchjup8e39afauf9naz4jmpklsze3uwx6405vll88l5lvww --chain-id testnet
```

**响应示例**
```
Account:
  Address:            vault1124j4d2tjt0c7vrq5er4n2ksyp539gkv28krrsye5ym2y2mf5ekvg2s7neydamthzwag272 //账户地址
  Pubkey: //账户公钥
  Tokens:        100000000NANOGT //账户余额
  AccountNumber: 9 //账户编号
  Sequence:      0
  AccountType:        1 //账户类型：0.单签普通账户、1.单签保险账户、2.多签普通账户、3.多签保险账户
  DelayHeight:        100 //保险账户交易延迟生效高度
  SecurityAddress:    gt11dsck7f7txdc7hz5wdsspey0w5eyhw7yj4xrc3a6r4w4t39z9k45r86htlzxjf6z57an2r7 //保险账户的找回账户
  LastClearingHeight: 0 //上一次设置的清算高度
  LastClearingEffectHeight:   0 //上一次设置清算高度时的交易高度
  NextClearingHeight:      700 //最新设置的清算高度
  NextClearingEffectHeight:   257 //最新设置清算高度时的交易高度
  ReceivedRevocableTokens: //仍可撤回的代币
  SentRevocableTokens: //已发送可撤销的代币
```


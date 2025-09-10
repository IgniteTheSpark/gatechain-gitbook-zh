# API 参考文档

> 重要提示：
> - 构建交易的所有接口请求参数必须设置大于0NANOGT的交易费
> - 资产金额（资产数量）是通过将10E18乘以转换的正整数值
> - 资产金额支持科学计数法，例如10E9 NANOGT = 1GT

## 节点 API

### 1. 查询节点状态

**接口路径**

```bash
GET /v1/status
```

**返回参数**

| 参数 | 类型 | 描述 |
|------|------|------|
| node_status | Object | 节点状态信息 |
| application_version | Object | 应用版本信息 |

**返回示例**

```json
{
    "node_status": {
        "lastHeight": "996",//the current height
        "lastConsensusVersion": "v1",//the current consensus version number
        "nextConsensusVersion": "v1",//version number of the next block consensus 
        "nextConsensusVersionRound": "997",//height  of the next consensus version
        "nextConsensusVersionSupported": true, //if support upgrading
        "timeSinceLastRound": "16",//time since the latest block is generated
        "catchupTime": "0",//time  spent to download block by this node
        "hasSyncedSinceStartup": false, //if have finished a round of consensus since startup?
        "step": "2", //step
        "period": "0", //phase
        "zeroTimeStamp": "2020-06-11T06:38:29.626739Z", //When  the  current step begins
        "deadline": "43", // time to timeout
        "fastRecoveryDeadline": "597", //timeout of quick recovery mode
        "minimumTxFee": "10" //the lowest fee required for packaging data by the current node
    },
    "application_version": {
        "name": "gate", //applicatoin name
        "server_name": "gated", //progress name
        "client_name": "gatecli", //client name
        "version": "0.9.0-236-g2531c037", //version number
        "commit": "2531c037c6d1324288f4609ca201c8429029243f", //submit information
        "build_tags": "netgo,ledger", //build tag
        "go": "go version go1.13.7 darwin/amd64" //golang version
    }
}
```

### 2. 查询账户信息

**接口路径**

```bash
GET /v1/account/{address}
```

**参数说明**

| 参数 | 类型 | 描述 |
|------|------|------|
| address | String | 账户地址 |

**返回示例**

```json
{
    "height":"5129", //block height
    "result":{
        "type":"AccountResp",
        "value":{
            "account_field":{
                "type":"VaultAccount",
                "value":{
                    "base_account":{
                        "account_number":"7", //account serial number
                        "address":"gt11ja8j8qskxvccwf3rchp9efxjdu6v5wfkj5uwu4cmktue7h7ufjwqlgqs9ja64xj9kgd5zj", //account address
                        "public_key": {
                            "type": "gatechain/PubKeyEd25519",
                            "value": "99gGX+9YCUV26rE+V/54v2graA/b4xKeqaYGOr6wWTU="
                        },                        
                        "sequence":"4",
                        "tokens":[
                            {
                                "amount":"89999999999999880", //account balance
                                "denom":"NANOGT" //unit
                            }
                        ]
                    },
                    "clearing_height":{ //clearing height of the account,  the value is only for  Vault Account. 
                        "last_clearing_effect_height":"0", //transaction height when setting clearing height last time
                        "last_clearing_height":"0", //clearing height set last time
                        "next_clearing_effect_height":"0", //transaction height when setting  the new clearing height 
                        "next_clearing_height":"0" //the new clearing height
                    },
                    "delay_height":"0", //delay height before a transaction takes effect for the Vault Account
                    "received_revocable_tokens":null, //token still can be revoked 
                    "security_address":"", //Retrieval Account
                    "sent_revocable_tokens":null, //token sent and revocable
                    "vault_address":[
                        "vault112t7hfsmd63a2nz0vwqhpy3msd98vvl35qeuej2uavh2ssjls4f8amqtwgpq3pwksgdqfe6" //Vault Account address,  a value in this field indicates that the  account queried is the Retrieval Account of the Vault Account 
                    ]
                }
            },
            "account_type":0 //Account Type：0.Single Signature Standard Account,1.Single Signature Vault Account, 2.Multisignature Standard Account,3.Multisignature Vault Account
        } 
    }
}
```

### 3. 查询账户余额

**接口路径**

```bash
GET /v1/account/balance/{account}
```

**参数说明**

| 参数 | 类型 | 描述 |
|------|------|------|
| address | String | 账户地址 |

**返回示例**

```json
{
    "height":"5483", //block height
    "result":[
        {
            "amount":"8999999989968", //account balance
            "denom":"NANOGT" //unit
        },
    ]
}
```

### 4. 发布多签账户

> 该接口生成"发布多签账户"的交易体,本地签名后,调用"发送交易"接口完成广播。

**接口路径**

```bash
POST /v1/account/publish-multisig/{address}
```

**请求体示例**

```json
{
  "base_req": {
    "from": "gt11ja8j8qskxvccwf3rchp9efxjdu6v5wfkj5uwu4cmktue7h7ufjwqlgqs9ja64xj9kgd5zj", //sender account
    "memo": "", //transaction remarks,The length of the remarks is limited to 85 characters in Chinese and 256 characters in English.
    "chain_id": "testnet",//chain ID
    "gas": "200000",//gas consumed by the transaction
    "fees": [
      {
        "denom": "NANOGT", //unit
        "amount": "20" // fee
      }
    ],
    "simulate": false, //whether calcuate  simulated  gas 
    "valid_height": [ //height when a tranasction takes effect
          "600",
          "900"
    ] 
  },
  "pubkey":"gt1pub1ytql0csgqgfzd666axrjzqegteuuxvghau9u0q67lltpjqla3ykzz3t8efmh6sqhyt4uhnh3q5fzd666axrjzqkhwmygytf0grzudhv69h9ttcy4xhze0v4mtf4jza6mrp0j3lq68qfzd666axrjzqn6wmq0uuyvxr8tywehal0zyzhpy5tv4h5tpryvc449jmznnzdruqy68ks2" //multisignature account public key
}
```

**返回示例**

```json
{
    "type":"StdTx",
    "value":{
        "msg":[
            {
                "type":"MsgPublishMultiSigAccount", //transaction type
                "value":{
                    "from_address":"gt11ja8j8qskxvccwf3rchp9efxjdu6v5wfkj5uwu4cmktue7h7ufjwqlgqs9ja64xj9kgd5zj", //Sender account
                    "to_address":"gt11ja8j8qskxvccwf3rchp9efxjdu6v5wfkj5uwu4cmktue7h7ufjwqlgqs9ja64xj9kgd5zj", //multisignature account 
						"pubkey":"gt1pub1ytql0csgqgfzd666axrjzqegteuuxvghau9u0q67lltpjqla3ykzz3t8efmh6sqhyt4uhnh3q5fzd666axrjzqkhwmygytf0grzudhv69h9ttcy4xhze0v4mtf4jza6mrp0j3lq68qfzd666axrjzqn6wmq0uuyvxr8tywehal0zyzhpy5tv4h5tpryvc449jmznnzdruqy68ks2" //multisignature account public key
                }
            }
        ],
        "fee":{
            "amount":[
                {
                    "denom":"NANOGT", //unit
                    "amount":"20" //fee
                }
            ],
            "gas":"200000" //gas consumed by the transaction
        },
        "signatures":null, //Signature
        "memo":"",
        "valid_height": [ //height when a tranasction takes effect
           "600",
           "900"
        ] 
    }
}
```

### 5. 查询共识账户列表

**接口路径**

```bash
GET /v1/staking/con-accounts
```

**返回示例**

```json
{
    "height": "5203", //block height
    "result": [
        {
            "commission": {
                "commission_rates": {
                    "max_change_rate": "0.000000000000000000", //max commission rate change 
                    "max_rate": "0.000000000000000000", //max commission rate 
                    "rate": "0.000000000000000000" //commission 
                },
                "update_time": "1970-01-01T00:00:00Z" //time commission updated at
            },
            "delegator_shares": "11000000.000000000000000000", //delegation quantity of a consensus account
            "description": { //a collection of consensus account attributes
                "details": "",
                "identity": "",
                "moniker": "",
                "website": ""
            },
            "operator_address": "gt116h05fjhaay7sx3zl9w5ej3tpx3s94yhcsmt0gqcqsq26w2qvsyt4l82vftygtff0pfsr93", //consensus account address
            "power": "39000934", //consensus account power
            "power_rate": "1.000922559773909156", //loyalty coefficient  of a  consensus account
            "pubkey": "gt1pub1u8s6p73qzlye3d4mljgt3auxhz4shj43w2eu0evladd03rr2auyrhc87aynqpwdz6w", //consensus account public key
            "status": "online", //online status of consensus account          
            "tokens": "11000000" //total tokens delegated to consensus account
        },
        ...
    ]
}
```

### 6. 创建保险账户

> 该接口生成"发布多签账户"的交易体,本地签名后,调用"发送交易"接口完成广播。


**接口路径**

```bash
POST  /v1/vault-account/create/{base-account}
```

**参数说明**

| 参数 | 类型 | 描述 |
|------|------|------|
| base-account | String | 基础账户地址 |


**请求体示例**

```json
{
  "base_req": {
    "from": "gt11ja8j8qskxvccwf3rchp9efxjdu6v5wfkj5uwu4cmktue7h7ufjwqlgqs9ja64xj9kgd5zj", //sender account
    "memo": "", ////transaction remarks,The length of the remarks is limited to 85 characters in Chinese and 256 characters in English.
    "chain_id": "testnet", //chain ID
    "gas": "200000", //gas consumed by this transaction
    "fees": [
      {
        "denom": "NANOGT", //unit
        "amount": "5000" //fee
      }
    ],
    "simulate": false, //if calculate simulated gas?
    "valid_height":[ //Block height at which a transaction takes effect
         "600",
         "900"
    ]
  },
  "amount": [
    {
      "denom": "NANOGT", //unit
      "amount": "500000000" //transfer token amount
    }
  ],
  "vault_req":{
    "security": "gt11ja8j8qskxvccwf3rchp9efxjdu6v5wfkj5uwu4cmktue7h7ufjwqlgqs9ja64xj9kgd5zj", //Retrieval Account
    "pubkey": "", //When creating a multisignature Vault Account,fill the public key of a  multi signature Standard Account; when creating a single signature Vault Account, leave it blank.
    "delay_height": "100", //delay height of transactions for the Vault Account
    "clear_height": "50000" //clearing height of the account
  }
}
```

**返回示例**

```json
{
    "type":"StdTx",
    "value":{
        "msg":[
            {
                "type":"MsgCreateVault", //Transaction type
                "value":{
                    "from_address":"gt11ja8j8qskxvccwf3rchp9efxjdu6v5wfkj5uwu4cmktue7h7ufjwqlgqs9ja64xj9kgd5zj", //sender account
                    "to_address":"gt11ja8j8qskxvccwf3rchp9efxjdu6v5wfkj5uwu4cmktue7h7ufjwqlgqs9ja64xj9kgd5zj", // the base account to the Vault Account, or the recipient
                    "security_address":"gt11ja8j8qskxvccwf3rchp9efxjdu6v5wfkj5uwu4cmktue7h7ufjwqlgqs9ja64xj9kgd5zj", //Retrieval Account
                    "delay_height":"100", //delay height of transactions for the Vault Account
                    "clearing_height":"50000", //clearing height of the account
                    "amount":[
                        {
                            "denom":"NANOGT", //unit
                            "amount":"500000000" //transfer token amount
                        }
                    ],
                    "pubkey":"" //When creating a multisignature Vault Account,fill the public key of a  multi signature Standard Account; when creating a single signature Vault Account, leave it blank.
                }
            }
        ],
        "fee":{
            "amount":[
                {
                    "denom":"NANOGT", //unit
                    "amount":"5000" //fee
                }
            ],
            "gas":"200000" //gas consumed by this transaction
        },
        "signatures":null, //Signature
        "memo":"",
        "valid_height": [ //block height at which the transaction takes effect
            "600",
            "900"
        ] 
    }
}
```

### 7. 修改清算高度

> 该接口生成"修改清算高度"的交易体,本地签名后,调用"发送交易"接口完成广播。


**接口路径**

```bash
POST /v1/vault-account/update-clearing-height
```


**请求体示例**

```json
{
  "base_req": {
    "from": "vault112t7hfsmd63a2nz0vwqhpy3msd98vvl35qeuej2uavh2ssjls4f8amqtwgpq3pwksgdqfe6", //sender account
    "memo": "", ////transaction remarks,The length of the remarks is limited to 85 characters in Chinese and 256 characters in English.
    "chain_id": "testnet", //chain ID
    "gas": "200000", //gas consumed by this transaction
    "fees": [
      {
        "denom": "NANOGT", //unit
        "amount": "5000" //fee
      }
    ],
    "simulate": false, //if calculate simulated gas?
    "valid_height": [ //block height at which the transaction takes effect
         "600",
         "900"
   ] 
  },
  "clearing_height": "6200" //new clearing height
}
```

**返回示例**

```json
{
    "type":"StdTx",
    "value":{
        "msg":[
            {
                "type":"MsgUpdateClearingHeight", //Transaction type
                "value":{
                    "vault_address":"vault112t7hfsmd63a2nz0vwqhpy3msd98vvl35qeuej2uavh2ssjls4f8amqtwgpq3pwksgdqfe6", //sender account
                    "clearing_height":"6200" //new clearing height
                }
            }
        ],
        "fee":{
            "amount":[
                {
                    "denom":"NANOGT", //unit
                    "amount":"5000" //fee
                }
            ],
            "gas":"200000" //gas consumed by the transaction
        },
        "signatures":null, //Signature
        "memo":"",
    	 "valid_height": [ //block height at which the transaction takes effect
           "600",
           "900"
   		] 
    }
}
```

### 8. 清空账户交易

> 该接口生成"修改清算高度"的交易体,本地签名后,调用"发送交易"接口完成广播。


**接口路径**

```bash
POST /v1/vault-account/clear
```


**请求体示例**

```json
{
    "base_req": {
    "from": "gt11ja8j8qskxvccwf3rchp9efxjdu6v5wfkj5uwu4cmktue7h7ufjwqlgqs9ja64xj9kgd5zj", //sender account,which is the Retrieval Account to the Vault Account
    "memo": "", //transaction remarks
    "chain_id": "testnet", //chain ID
    "gas": "200000", //gas consumed by this transaction
    "fees": [ 
      {
        "denom": "NANOGT", //unit
        "amount": "5000" //fee
      }
    ],
    "simulate": false, //if calculate simulated gas?
    "valid_height": [ //block height at which the transaction takes effect
         "600",
         "900"
   ] 
  },
  "vaults": ["vault112t7hfsmd63a2nz0vwqhpy3msd98vvl35qeuej2uavh2ssjls4f8amqtwgpq3pwksgdqfe6"] //Vault Account address
}
```

**返回示例**

```json
{
    "type":"StdTx",
    "value":{
        "msg":[
            {
                "type":"MsgClearVaultAccount", //Transaction type
                "value":{
                    "from_address":"gt11ja8j8qskxvccwf3rchp9efxjdu6v5wfkj5uwu4cmktue7h7ufjwqlgqs9ja64xj9kgd5zj", //sender account,which is Retrieval Account to the Vault Account
                    "vault_address":[
                        "vault112t7hfsmd63a2nz0vwqhpy3msd98vvl35qeuej2uavh2ssjls4f8amqtwgpq3pwksgdqfe6" //Vault Account
                    ]
                }
            }
        ],
        "fee":{
            "amount":[
                {
                    "denom":"NANOGT", //unit
                    "amount":"5000" //fee
                }
            ],
            "gas":"200000" //gas consumed by this transaction
        },
        "signatures":null, //Signature
        "memo":"",
        "valid_height": [ //block height at which the transaction takes effect
            "600",
            "900"
        ] 
    }
}
```

### 9. 查询保险账户信息

**接口路径**

```bash
GET  /v1/vault-account/{address}
```

**参数说明**

| 参数 | 类型 | 描述 |
|------|------|------|
| address | String |保险账户 |


**返回示例**

```json
{
    "height": "4618", //block height
    "result": {
        "type": "AccountResp",
        "value": {
            "account_field": {
                "type": "VaultAccount",
                "value": {
                    "base_account": {
                        "account_number": "12",
                        "address": "vault112t7hfsmd63a2nz0vwqhpy3msd98vvl35qeuej2uavh2ssjls4f8amqtwgpq3pwksgdqfe6", //保险账户
                        "public_key": {
                            "type": "gatechain/PubKeyEd25519",
                            "value": "IK8RZV4tqj/m/s9eEY9agWXF42yA5U3s31Q0D6Zp1rI="
                        },
                        "sequence": "0",
                        "tokens": [
                            {
                                "amount": "9889", //account balance
                                "denom": "NANOGT" //unit
                            }
                        ]
                    },
                    "clearing_height": {
                        "last_clearing_effect_height": "0", //transaction height when set up clearing height last time
                        "last_clearing_height": "0", //clearing height set last time
                        "next_clearing_effect_height": "3693", //transaction height when set up  new clearing height
                        "next_clearing_height": "100000" //new clearing height
                    },
                    "delay_height": "100", //delay height before a transaction takes effect
                    "received_revocable_tokens": null, //token that can be revoked
                    "security_address": "gt116h05fjhaay7sx3zl9w5ej3tpx3s94yhcsmt0gqcqsq26w2qvsyt4l82vftygtff0pfsr93", //Retrieval Account
                    "sent_revocable_tokens": [], //token sent and revocable
                    "vault_address": null //保险账户 
                }
            },
            "account_type": 1 //account type：0.ingle signature Standard Account,1.single signature Vault Account,2.multisignature Standard Account,3.multisignature Vault Account 
        }
    }
}
```

### 10. 查询可撤回交易列表从保险账户

**接口路径**

```bash
GET  /v1/vault-account/list-revocable-txs/{vault-account}
```

**参数说明**

| 参数 | 类型 | 描述 |
|------|------|------|
| vault-account | String |保险账户 |


**返回示例**

```json
{
    "height":"6947", //block height
    "result":[
        {
            "height":"6947", //block height at which  the transaction takes effect
            "msg_index":"0",
            "tokens":[
                {
                    "amount":"5", //transfer token amount
                    "denom":"NANOGT" //unit
                }
            ],
            "tx_hash":"REVOCABLEPAY-BB042E7853D6E32C6F81E0205A3CDD5FDA6545F2A7E92627E50EA19F86EFD6B8" //transaction hash
        }
    ]
}
```



### 11. 查询最新区块信息

**接口路径**

```bash
GET /v1/block/latest
```

**返回示例**

```json
{
    "hash": "H44ZPPWLRIXI3TJ64UZSHKKSJ4XRJMWIGU6FHFQXWFTRY3QHW3426MWP3IA44KJHZHBEHSOYYEWNO",//hash of the current block 
    "previousBlockHash": "BF4BVNISMRNALPYXZAPPK4LKLVGTUDVWGNA5BQYJVHPDRZXTVGQG3LA4FB6RPHSYX5IEA44BGD3DE",//hash of the preceding block 
    "seed": "HSIWSUEVTY4VRJCZMJ4VE7MFTDBA6SGIM7LY6YOWJ624ON7LFOINBIT6A3STY4GV52D5IIMNZDVNO",//VRF seed for draw
    "proposer": "gt11ynwpc98gkh9uxxnkld7gjp5knu5ht75f0223uf298ap6ug6vtx5ank5vfde0pylmu09wzh",//consensus account that make a  proposal in the block
    "height": "959",//block height
    "period": "0",//block phase
    "txnRoot": "LHHTNT65QQOXUV6MQM4LIZ5PRGGY32XQRFTHIO4WXY4BPVBCWEXQFZ2NZU37AYKWOOUFSL4YTX2A4",// hash of the transaction collection in a block
    "txnps": {//transaction collection
        "proxyTransactions": null
    },
    "timestamp": "1591788058",//time the block is generated at
    "UpgradeState": { //consensus protocol version status
        "currentProtocol": "v1", //current version number
        "nextProtocol": "", //target version number to upgrade to 
        "nextProtocolApprovals": "0", //votes already received by target version
        "nextProtocolVoteBefore": "0",//deadline height  to vote to  target version
        "nextProtocolSwitchOn": "0" // height to upgrade to target version
    },
    "UpgradeVote": { //voting status of consensus protocol upgrade 
        "upgradePropose": "", //target version number to upgrade to 
        "upgradeApprove": false //voting results of upgrading to target version
    },
    "CertCommitteeInfo": [ //block committee information(in locall certificate)（potential reward winners of next round）
       "gt117h3k4uaftcql9zaqll5vg2x3gn67576qyklnvmzh9xldczryqpzpdj69xygrm0hvklufun",
       "singleCommitteePower": "90000341" // consensus account power of this round
    ],
    "BlockCommitteeInfo": [ //block committee information（in block head）（reward winners of this round）
       "gt117h3k4uaftcql9zaqll5vg2x3gn67576qyklnvmzh9xldczryqpzpdj69xygrm0hvklufun",
       "singleCommitteePower": "90000323" //consensus account power of preceding round 
    ],
    "autoOfflineAccounts": [] //consensus accounts auto-offline at specified round
}
```

### 12. 查询特定区块信息

**接口路径**

```bash
GET /v1/block/{height}
```

**参数说明**

| 参数 | 类型 | 描述 |
|------|------|------|
| height | Int64 | block height(>=1) |

**返回示例**

```json
{
    "hash": "H44ZPPWLRIXI3TJ64UZSHKKSJ4XRJMWIGU6FHFQXWFTRY3QHW3426MWP3IA44KJHZHBEHSOYYEWNO",//hash of current block 
    "previousBlockHash": "BF4BVNISMRNALPYXZAPPK4LKLVGTUDVWGNA5BQYJVHPDRZXTVGQG3LA4FB6RPHSYX5IEA44BGD3DE",//hash of preceding block 
    "seed": "HSIWSUEVTY4VRJCZMJ4VE7MFTDBA6SGIM7LY6YOWJ624ON7LFOINBIT6A3STY4GV52D5IIMNZDVNO",//VRF seed for draw
    "proposer": "gt11ynwpc98gkh9uxxnkld7gjp5knu5ht75f0223uf298ap6ug6vtx5ank5vfde0pylmu09wzh",//consensus account to propose in a block
    "height": "959",//block height
    "period": "0",//block phase
    "txnRoot": "LHHTNT65QQOXUV6MQM4LIZ5PRGGY32XQRFTHIO4WXY4BPVBCWEXQFZ2NZU37AYKWOOUFSL4YTX2A4",// hash of the transaction collection in a block
    "txnps": {//transaction collection
        "proxyTransactions": null
    },
    "timestamp": "1591788058",//time the block is generated at
    "UpgradeState": { //consensus protocol version status
        "currentProtocol": "v1", //current version number
        "nextProtocol": "", //target version number to upgrade to 
        "nextProtocolApprovals": "0", //votes already received by target version
        "nextProtocolVoteBefore": "0",//deadline height  to vote to  target version
        "nextProtocolSwitchOn": "0" // height to upgrade to target version
    },
    "UpgradeVote": { //consensus protocol upgrade voting status
        "upgradePropose": "", //target version number to upgrade to 
        "upgradeApprove": false //voting results of upgrading to target version
    },
    "CertCommitteeInfo": [ //block committee information (in localcertificate)（potential reward winners of next round）
       "gt117h3k4uaftcql9zaqll5vg2x3gn67576qyklnvmzh9xldczryqpzpdj69xygrm0hvklufun",
       "singleCommitteePower": "90000341" //consensus account power of this round 
    ],
    "BlockCommitteeInfo": [ //block committee information（in block head）（reward winners of this round）
       "gt117h3k4uaftcql9zaqll5vg2x3gn67576qyklnvmzh9xldczryqpzpdj69xygrm0hvklufun",
       "singleCommitteePower": "90000323" //consensus account power of preceding round 
    ],
    "autoOfflineAccounts": [] //consensus accounts auto-offline  at specified round
}
```

### 13. 发送交易

**接口路径**

```bash
POST /v1/tx
```

**请求体示例**

```json
在本地签名交易后，只需要将签名文件作为请求体复制。无需复制以下内容。
{
    "type":"StdTx",
    "value":{
        "msg":[
            {
                "type":"MsgCreateVault", //transaction type（This transaction is to create a Vault Account. ）
                "value":{
                    "from_address":"gt11tgjslrwl0j35czlj0ayxq9t7hzd0gtckfwc57qsl3nftl7zyk8gccv5kexetmm3xsv2tj5", //sender account
                    "to_address":"gt11e02xkclka6h64c80az66ampjzzwe739tlnrkxya3ecxrj6e67plh626zad7tqz3w3m8xqt", //recipient account
                    "security_address":"gt11tgjslrwl0j35czlj0ayxq9t7hzd0gtckfwc57qsl3nftl7zyk8gccv5kexetmm3xsv2tj5", //Retrieval Account 
                    "delay_height":"100", //delay height before a transaction takes effect
                    "clearing_height":"50000", //clearing height
                    "amount":[
                        {
                            "denom":"NANOGT", //unit
                            "amount":"500" //transfer token amount
                        }
                    ],
                    "pubkey":""
                }
            }
        ],
        "fee":{
            "amount":[
                {
                    "denom":"NANOGT", //unit
                    "amount":"11" //fee
                }
            ],
            "gas":"200000" //gas consumed by the transaction
        },
        "nonces":[
            null
        ],
        "signatures":[
            {
                "pub_key":{
                    "type":"gatechain/PubKeyEd25519",
                    "value":"pIGmDHUICq/NrI6TYJj8/yGbJfa/EjrJ50ImqSVbPyw="
                },
                "signature":"CYUFLRQf2JCw1kR0AFIdUhMrc+qu5owzwh1outwoAn6AKf2joFScL2qxWFzHHLO/3uig87E7rFUQmajz47d/Dw==" //signature
            }
        ],
        "memo":"",
        "valid_height":[ //block height at which the transaction takes effect
            "2300",
            "2500"
        ]
    }
}
```
**返回示例**

```json
{
    "height":"0", //block height
    "txhash":"IRREVOCABLEPAY-79459C3708F7E38EA35977C12E5ECB659D7F23B772BEB5A58F52DFAA4C72D985", //transaction hash
    "data":"zgG5zc/tCkDcKqCFChTtdOB+KzokLvRctbEBeDtte5QIlxIUntatO0LfnWuAFDcHJkcXQrv6qA8aDgoGTkFOT0dDEgQ1MDAwEhQKDgoGTkFOT0dDEgQ1MDAwEMCaDBoAImkKJRYk3mQgGz3RMlEjEVSJCXrBPhyG5fov5EwzE/zamPMLcE4rol4SQPpCFMKSXR0kQejB351r0ZDR05LgxFe9GsEsgyj/MKe2ia3Pv+aem3OhQnndisiEsZbCEKP2svCTwnWBG5KlJAAyAwHoBw==",
    "raw_log":"boradcast tx success", 
    "tx":{
        "type":"StdTx",
        "value":{
            "msg":[
                {
                    "type":"MsgSend", //transaction type
                    "value":{
                        "from_address":"gt11ja8j8qskxvccwf3rchp9efxjdu6v5wfkj5uwu4cmktue7h7ufjwqlgqs9ja64xj9kgd5zj", //sender account
                        "to_address":"gt11ja8j8qskxvccwf3rchp9efxjdu6v5wfkj5uwu4cmktue7h7ufjwqlgqs9ja64xj9kgd5zj", //recipient account
                        "amount":[
                            {
                                "denom":"NANOGT", //unit
                                "amount":"5000" //transfer token amount
                            }
                        ]
                    }
                }
            ],
            "fee":{
                "amount":[
                    {
                        "denom":"NANOGT", //unit
                        "amount":"5000" //fee
                    }
                ],
                "gas":"200000" //gas consumed by the transaction
            },
            "nonces":[
                null
            ],
            "signatures":[
                {
                    "pub_key":{
                        "type":"gatechain/PubKeyEd25519",
                        "value":"Gz3RMlEjEVSJCXrBPhyG5fov5EwzE/zamPMLcE4rol4="
                    },
                    "signature":"+kIUwpJdHSRB6MHfnWvRkNHTkuDEV70awSyDKP8wp7aJrc+/5p6bc6FCed2KyISxlsIQo/ay8JPCdYEbkqUkAA==" //signature
                }
            ],
            "memo":"",
            "valid_height":[ //block height at which a transaction takes effect
                "1",
                "1000"
            ]
        }
    }
}
```

### 14. 普通交易


**接口路径**

```bash
GET /v1/tx/send/{address}
```

**参数说明**

| 参数 | 类型 | 描述 |
|------|------|------|
| address | String | 账户地址 |


**请求体示例**

```json
{
    "base_req": {
        "from": "gt11ja8j8qskxvccwf3rchp9efxjdu6v5wfkj5uwu4cmktue7h7ufjwqlgqs9ja64xj9kgd5zj", //sender account
        "memo": "", ////transaction remarks,The length of the remarks is limited to 85 characters in Chinese and 256 characters in English.
        "chain_id": "testnet", //chain ID
        "gas": "200000", //gas consumed by the transaction
        "fees": [{ 
            "denom": "NANOGT", //unit
            "amount": "5000" //fee
        }],
        "simulate": false, //if calculate simulated gas
        "valid_height":[ //block height at which the transaction takes  effect
            "600",
            "900"
    	]
    },
    "amount": [{
        "denom": "NANOGT", //unit
        "amount": "5000" //transfer token amount
    }]
}
```

**返回示例**

```json
{
    "type":"StdTx",
    "value":{
        "msg":[
            {
                "type":"MsgSend", //transaction type
                "value":{
                    "from_address":"gt11ja8j8qskxvccwf3rchp9efxjdu6v5wfkj5uwu4cmktue7h7ufjwqlgqs9ja64xj9kgd5zj", //sender account
                    "to_address":"gt11ja8j8qskxvccwf3rchp9efxjdu6v5wfkj5uwu4cmktue7h7ufjwqlgqs9ja64xj9kgd5zj", //recipient account
                    "amount":[
                        {
                            "denom":"NANOGT", //unit
                            "amount":"5000" //transfer token amount
                        }
                    ]
                }
            }
        ],
        "fee":{
            "amount":[
                {
                    "denom":"NANOGT", //unit
                    "amount":"5000" //fee
                }
            ],
            "gas":"200000" //gas consumed by the transaction
        },
        "signatures":null, //signature
        "memo":"",
        "valid_height":[ //block height at which the transaction takes  effect
            "600",
            "900"
    	]
    }
}
```

### 15. 查询交易

**接口路径**

```bash
GET /v1/tx/{hash}
```

**参数**

| 参数 | 类型 | 描述 |
|------|------|------|
| hash | String | 交易哈希 |

**返回示例**

```json
{
"data":"AB02B9CDCFED0A6FDC2AA0850A28E151E865950BE2B1B0772852895F3D6DBF475F21B7A8C261E8A9817B8E3E56C97F5319BF6C59095C1228E019A5E99305917528D431DDC13C9776648B6244877F9F19B033DCE3B88B92D3A09586D05AC0E1281A150A064E414E4F4754120B313030303030303030303012120A0C0A064E414E4F47541202313110C09A0C1A3032B98683061DE03543FAB82EA8904E492FFDF1AC56F1BC855316B29E004EB3A02FCF7069CCEA0B1606C549498C28336022690A25E1E1A0FA201BAAF55B7BFB628A0FEFAE3A2BE6114F30A5A373CFF7744DE321FADB2CC202541240D72A4B77AA24B5702EF59BB98E0574F6A24541CEF08B498A8EA8F9532807339F9DBCA1B7176AB78D8C000B62EF18D5B5EFBC2E7EE05E05B800D2EC31AA7C190A320301CC01",
            "events": [
                {
                    "attributes": [ //attributes of the sender
                        {
                            "key": "sender",
                            "value": "gt11u9g7sev4p03trvrh9pfgjheadkl5whepk75vyc0g4xqhhr372myh75cehak9jz2u3ap0fp" //sender account
                        },
                        {
                            "key": "module",
                            "value": "bank"
                        },
                        {
                            "key": "action",
                            "value": "send"
                        }
                    ],
                    "type": "message"
                },
                {
                    "attributes": [ //attributes of the recipient
                        {
                            "key": "recipient",
                            "value": "gt11uqv6t6vnqkgh22x5x8wuz0yhwejgkcjysale7xdsx0ww8wytjtf6p9vx6pdvpcfgme5xar" // recipient account
                        },
                        {
                            "key": "amount", //unit
                            "value": "10000000000NANOGT" //transfer token amount
                        }
                    ],
                    "type": "transfer"
                }
            ],
            "gas_used": "60579",
            "gas_wanted": "200000",
            "height": "5", //block height of the transaction
            "logs": [
                {
                    "events": [
                        {
                            "attributes": [
                                {
                                    "key": "sender",
                                    "value": "gt11u9g7sev4p03trvrh9pfgjheadkl5whepk75vyc0g4xqhhr372myh75cehak9jz2u3ap0fp"
                                },
                                {
                                    "key": "module",
                                    "value": "bank"
                                },
                                {
                                    "key": "action",
                                    "value": "send"
                                }
                            ],
                            "type": "message"
                        },
                        {
                            "attributes": [
                                {
                                    "key": "recipient",
                                    "value": "gt11uqv6t6vnqkgh22x5x8wuz0yhwejgkcjysale7xdsx0ww8wytjtf6p9vx6pdvpcfgme5xar"
                                },
                                {
                                    "key": "amount",
                                    "value": "10000000000NANOGT"
                                }
                            ],
                            "type": "transfer"
                        }
                    ],
                    "log": "",
                    "msg_index": 0,
                    "success": true
                }
            ],
            "raw_log": "[{\"msg_index\":0,\"success\":true,\"log\":\"\",\"events\":[{\"type\":\"message\",\"attributes\":[{\"key\":\"sender\",\"value\":\"gt11u9g7sev4p03trvrh9pfgjheadkl5whepk75vyc0g4xqhhr372myh75cehak9jz2u3ap0fp\"},{\"key\":\"module\",\"value\":\"bank\"},{\"key\":\"action\",\"value\":\"send\"}]},{\"type\":\"transfer\",\"attributes\":[{\"key\":\"recipient\",\"value\":\"gt11uqv6t6vnqkgh22x5x8wuz0yhwejgkcjysale7xdsx0ww8wytjtf6p9vx6pdvpcfgme5xar\"},{\"key\":\"amount\",\"value\":\"10000000000NANOGT\"}]}]}]",
            "timestamp": "2020-06-15T20:53:28+08:00",
            "tx": {
                "type": "StdTx",
                "value": {
                    "fee": {
                        "amount": [
                            {
                                "amount": "11",
                                "denom": "NANOGT"
                            }
                        ],
                        "gas": "200000"
                    },
                    "memo": "",
                    "msg": [
                        {
                            "type": "MsgSend",
                            "value": {
                                "amount": [
                                    {
                                        "amount": "10000000000",
                                        "denom": "NANOGT"
                                    }
                                ],
                                "from_address": "gt11u9g7sev4p03trvrh9pfgjheadkl5whepk75vyc0g4xqhhr372myh75cehak9jz2u3ap0fp",
                                "to_address": "gt11uqv6t6vnqkgh22x5x8wuz0yhwejgkcjysale7xdsx0ww8wytjtf6p9vx6pdvpcfgme5xar"
                            }
                        }
                    ],
                    "nonces": [
                        "MrmGgwYd4DVD+rguqJBOSS/98axW8byFUxayngBOs6Avz3BpzOoLFgbFSUmMKDNg"
                    ],
                    "signatures": [ 
                        {
                            "pub_key": {
                                "type": "gatechain/PubKeyEd25519",
                                "value": "G6r1W3v7YooP7646K+YRTzClo3PP93RN4yH62yzCAlQ="
                            },
                            "signature": "1ypLd6oktXAu9Zu5jgV09qJFQc7wi0mKjqj5UygHM5+dvKG3F2q3jYwAC2LvGNW177wufuBeBbgA0uwxqnwZCg==" //signature
                        }
                    ],
                    "valid_height": [ //block height at which a transaction takes effect
                        "1",
                        "204"
                    ]
                }
            },
            "txhash": "IRREVOCABLEPAY-DA2D28BEE24FF2CCF7FB45064BE316D6AB6962CD1C79A53DB427C83EC94A746C0C0A07FC6B74EFBC2C3B1F086F123E34" //transaction hash
}
```

### 16. 发送可撤回交易
> 该接口生成"更改清算高度"的交易体。签名后，可以调用"发送交易"接口完成广播。

**接口路径**

```bash
POST  /v1/revocable-tx/send/{address}
```

**参数**

| 参数 | 类型 | 描述 |
|------|------|------|
| address | String | the recipient account, which can be a Standard Account or a Vault Account. |


**请求体示例**

```json
{
  "base_req": {
	"from": "gt11ja8j8qskxvccwf3rchp9efxjdu6v5wfkj5uwu4cmktue7h7ufjwqlgqs9ja64xj9kgd5zj", //sender account 
	"memo": "", ////transaction remarks,The length of the remarks is limited to 85 characters in Chinese and 256 characters in English.
	"chain_id": "testnet", //chain ID
	"gas": "200000", //gas consumed by this transaction
	"fees": [
	  {
		"denom": "NANOGT", //unit
		"amount": "5000" //fee
	  }
	],
	"simulate": false, // If calculate simulated gas?
    "valid_height":[ //height at which the transaction takes effect
        "600",
        "900"
   	 ]
  },
  "amount": [
	{
	  "denom": "NANOGT", //unit
	  "amount": "5000" //transfer token amount 
	}
  ]
}
```

**返回示例**

```json
{
    "type":"StdTx",
    "value":{
        "msg":[
            {
                "type":"MsgRevocableSend", //transaction type
                "value":{
                    "from_address":"vault112t7hfsmd63a2nz0vwqhpy3msd98vvl35qeuej2uavh2ssjls4f8amqtwgpq3pwksgdqfe6", //sender account
                    "to_address":"gt11ja8j8qskxvccwf3rchp9efxjdu6v5wfkj5uwu4cmktue7h7ufjwqlgqs9ja64xj9kgd5zj", //recipient account 
                    "amount":[
                        {
                            "denom":"NANOGT", //unit
                            "amount":"5000" //transfer token amount 
                        }
                    ]
                }
            }
        ],
        "fee":{
            "amount":[
                {
                    "denom":"NANOGT", //unit
                    "amount":"5000" //fee
                }
            ],
            "gas":"200000" //gas consumed by this transaction
        },
        "signatures":null, //signature
        "memo":"", 
        "valid_height":[ //height at which the transaction takes effect
            "600",
            "900"
    	]
    }
}
```

### 17. 撤回可撤回交易
> 该接口生成"更改清算高度"的交易体。签名后，可以调用"发送交易"接口完成广播。


**接口路径**

```bash
POST /v1/revocable-tx/revoke/{tx-hash}
```

**参数**

| 参数 | 类型 | 描述 |
|------|------|------|
| tx-hash | String | 可撤回交易哈希 |

**请求体示例**

```json
{
  "base_req": {
    "from": "vault112t7hfsmd63a2nz0vwqhpy3msd98vvl35qeuej2uavh2ssjls4f8amqtwgpq3pwksgdqfe6", //sender account
    "memo": "", ////transaction remarks,The length of the remarks is limited to 85 characters in Chinese and 256 characters in English.
    "chain_id": "testnet", //chain ID
    "gas": "200000", //gas consumed by this transaction
    "fees": [
      {
        "denom": "NANOGT", //unit
        "amount": "1" //fee
      }
    ],
    "simulate": false, //If calculate simulated gas?
    "valid_height":[ //height at which the transaction takes effect
        "600",
        "900"
   	]
  },
  "index": "0" //serial number of the message
}
```

**返回示例**

```json
{
    "type":"StdTx",
    "value":{
        "msg":[
            {
                "type":"MsgRevoke", //transaction type
                "value":{
                    "vault_address":"gt11ja8j8qskxvccwf3rchp9efxjdu6v5wfkj5uwu4cmktue7h7ufjwqlgqs9ja64xj9kgd5zj", //base account address of the Vault Account
                    "security_address":"gt11ja8j8qskxvccwf3rchp9efxjdu6v5wfkj5uwu4cmktue7h7ufjwqlgqs9ja64xj9kgd5zj", //Retrieval Account address
                    "revoke_address":"gt11ja8j8qskxvccwf3rchp9efxjdu6v5wfkj5uwu4cmktue7h7ufjwqlgqs9ja64xj9kgd5zj", //Account address  transaction is revoked to
                    "height":"6947", //height at which the revoke transaction takes effect
                    "tx_hash":"BB042E7853D6E32C6F81E0205A3CDD5FDA6545F2A7E92627E50EA19F86EFD6B8", //transaction hash of the revocable transaction
                    "msg_index":"0", //serial number of the message
                    "amount":[
                        {
                            "denom":"NANOGT", //unit
                            "amount":"5" //revoked token amount 
                        }
                    ]
                }
            }
        ],
        "fee":{
            "amount":[
                {
                    "denom":"NANOGT", //unit
                    "amount":"1" //fee of the revoke transaction
                }
            ],
            "gas":"200000" //gas consumded by this transaction
        },
        "signatures":null, //signature
        "memo":"", 
        "valid_height":[ //height at which the transaction takes effect
            "600",
            "900"
    	]
    }
}
```

### 18. 查询可撤回交易状态

**接口路径**

```bash
GET /v1/revocable-tx/status/{hash}
```


**参数**

| 参数 | 类型 | 描述 |
|------|------|------|
| tx-hash | String | 可撤回交易哈希 |


**返回示例**

```json
{
    "status": 1, //1：revocable,0：irrevocable
    "revoke_hash": "" //transaction Hash of the Revoke transaction
}
```

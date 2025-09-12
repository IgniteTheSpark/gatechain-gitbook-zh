# API Reference Document

> Important Note:
> - All interface request parameters for constructing transactions must set a transaction fee greater than 0NANOGT
> - The asset amount (Asset amount) is a positive integer value converted by multiplying 10E18
> - The asset amount supports scientific notation, for example 10E9 NANOGT = 1GT

## Node API

### 1. Query A Node Status

**Interface Path**

```bash
GET /v1/status
```

**Return Parameters**

| Parameter | Type | Description |
|------|------|------|
| node_status | Object | Node status information |
| application_version | Object | Application version information |

**Return Example**

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

### 2. Query Account Information

**Interface Path**

```bash
GET /v1/account/{address}
```

**Parameter Explanation**

| Parameter | Type | Description |
|------|------|------|
| address | String | Account address |

**Return Example**

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


### 3. Query Account Balance

**Interface Path**

```bash
GET /v1/account/balance/{account}
```

**Parameter Explanation**

| Parameter | Type | Description |
|------|------|------|
| address | String | Account address |

**Return Example**

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

### 4. Publish Multisignature Account 

> This interfaces generates the transaction body of "publishing a multisignature account". After locally signing it, you can invoke "Send Transaction" interface to finish broadcasting。

**Interface Path**

```bash
POST /v1/account/publish-multisig/{address}
```

**Request Body Example**

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

**Return Example**

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

### 5. Query Consensus Accounts List

**Interface Path**

```bash
GET /v1/staking/con-accounts
```

**Return Example**

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

### 6. Create a Vault Account

> This interfaces generates the transaction body of "publishing a multisignature account". After locally signing it, you can invoke "Send Transaction" interface to finish broadcasting。


**Interface Path**

```bash
POST  /v1/vault-account/create/{base-account}
```

**Parameter Explanation**

| Parameter | Type | Description |
|------|------|------|
| base-account | String | Base account address |


**Request Body Example**

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

**Return Example**

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

### 7. Change Clearing Height

> The interface generates transaction body for " Change Clearing Height".After locally signing it, you can invoke "Send Transaction" interface to finish broadcasting.


**Interface Path**

```bash
POST /v1/vault-account/update-clearing-height
```


**Request Body Example**

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

**Return Example**

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

### 8. Clear Account Transaction

> The interface generates transaction body for " Change Clearing Height".After locally signing it, you can invoke "Send Transaction" interface to finish broadcasting.


**Interface Path**

```bash
POST /v1/vault-account/clear
```


**Request Body Example**

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

**Return Example**

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

### 9. Query Vault Account Information

**Interface Path**

```bash
GET  /v1/vault-account/{address}
```

**Parameter Explanation**

| Parameter | Type | Description |
|------|------|------|
| address | String |Vault Account |


**Return Example**

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
                        "address": "vault112t7hfsmd63a2nz0vwqhpy3msd98vvl35qeuej2uavh2ssjls4f8amqtwgpq3pwksgdqfe6", //Vault Account
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
                    "vault_address": null //Vault Account 
                }
            },
            "account_type": 1 //account type：0.ingle signature Standard Account,1.single signature Vault Account,2.multisignature Standard Account,3.multisignature Vault Account 
        }
    }
}
```

### 10. Query Revocable Transaction List From A Vault Account 

**Interface Path**

```bash
GET  /v1/vault-account/list-revocable-txs/{vault-account}
```

**Parameter Explanation**

| Parameter | Type | Description |
|------|------|------|
| vault-account | String |Vault Account |


**Return Example**

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



### 11. Query The Latest Block Information

**Interface Path**

```bash
GET /v1/block/latest
```

**Return Example**

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

### 12. Query A Specific BLock Information

**Interface Path**

```bash
GET /v1/block/{height}
```

**Parameter Explanation**

| Parameter | Type | Description |
|------|------|------|
| height | Int64 | block height(>=1) |

**Return Example**

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

### 13. Send Transaction

**Interface Path**

```bash
POST /v1/tx
```

**Request Body Example**

```json
After locally singing a transaction,  you just need to copy the signatures file as request body. No need to copy below content.
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
**Return Example**

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

### 14. Normal Transaction


**Interface Path**

```bash
GET /v1/tx/send/{address}
```

**Parameter Explanation**

| Parameter | Type | Description |
|------|------|------|
| address | String | Account address |


**Request Body Example**

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

**Return Example**

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

### 15. Query Transaction

**Interface Path**

```bash
GET /v1/tx/{hash}
```

**Parameters**

| Parameter | Type | Description |
|------|------|------|
| hash | String | Transaction hash |

**Return Example**

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

### 16. Send a Revocable Transaction
> The interface generates transaction body for " Change Clearing Height".After locally signing it, you can invoke "Send Transaction" interface to finish broadcasting.

**Interface Path**

```bash
POST  /v1/revocable-tx/send/{address}
```

**Parameters**

| Parameter | Type | Description |
|------|------|------|
| address | String | the recipient account, which can be a Standard Account or a Vault Account. |


**Request Body Example**

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

**Return Example**

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

### 17. Revoke a Revocable Transaction
> The interface generates transaction body for " Change Clearing Height".After locally signing it, you can invoke "Send Transaction" interface to finish broadcasting.


**Interface Path**

```bash
POST /v1/revocable-tx/revoke/{tx-hash}
```

**Parameters**

| Parameter | Type | Description |
|------|------|------|
| tx-hash | String | Hash of the revocable transaction |

**Request Body Example**

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

**Return Example**

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

### 18. Query Revocable Transaction Status

**Interface Path**

```bash
GET /v1/revocable-tx/status/{hash}
```


**Parameters**

| Parameter | Type | Description |
|------|------|------|
| tx-hash | String | transaction Hash of the Revocable Transaction |


**Return Example**

```json
{
    "status": 1, //1：revocable,0：irrevocable
    "revoke_hash": "" //transaction Hash of the Revoke transaction
}
```

### 19. Issue Token 
> The interface generates transaction body for " Change Clearing Height".After locally signing it, you can invoke "Send Transaction" interface to finish broadcasting.


**Interface Path**

```bash
POST  /v1/token/issue/{symbol}
```

**Parameters**

| Parameter | Type | Description |
|------|------|------|
| symbol | String | token symbol, (upper case letter, 2-15 characters long)|

> Note：Token issuance incurs a fee of 200000000000NANOGT, please make sure you have adequate NANOGT token at account.
>
**Request Body Example**

```json
{
  "base_req": {
    "from": "gt11ja8j8qskxvccwf3rchp9efxjdu6v5wfkj5uwu4cmktue7h7ufjwqlgqs9ja64xj9kgd5zj", //sender account
    "memo": "", ////transaction remarks,The length of the remarks is limited to 85 characters in Chinese and 256 characters in English.
    "chain_id": "testnet", //chain ID
    "gas": "80445444", //gas consumed by the transaction
    "fees": [
      {
        "denom": "NANOGT", //unit
        "amount": "5000" //fee
      }
    ],
    "simulate": false, //if calculate simulated gas
    "valid_height":[ //block height at which  the transaction takes effect
         "600",
         "900"
    ]
  },
  "token_name": "test token", //token name
  "total_supply": "1000000000000000", //total supply
  "mintable": true, //if allows additionally issuing
  "freezable": true //if allows freezing
}
```

**Return Example**

```json
{
    "type":"StdTx",
    "value":{
        "msg":[
            {
                "type":"IssueToken", //transaction type
                "value":{
                    "source_address":"gt11ja8j8qskxvccwf3rchp9efxjdu6v5wfkj5uwu4cmktue7h7ufjwqlgqs9ja64xj9kgd5zj", //sender account
                    "token_name":"test token", //token name
                    "symbol":"YJ", //token symbol
                    "total_supply":"1000000000000000", //total supply
                    "mintable":true, //if allows additionally issuing
                    "freezable":true //if allows freezing
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
            "gas":"80445444" //gas consumed by the transaction
        },
        "signatures":null, //signature
        "memo":"",
    	 "valid_height":[ //block height at which  the transaction takes effect
            "600",
            "900"
        ]
    }
}
```


### 20. Issue Additional Token
> The interface generates transaction body for " Change Clearing Height".After locally signing it, you can invoke "Send Transaction" interface to finish broadcasting.


**Interface Path**

```bash
POST /v1/token/mint/{symbol}
```

**Parameters**

| Parameter | Type | Description |
|------|------|------|
| symbol | String | onchain token symbol (token symbol-[random string])|


**Request Body Example**

```json
{
  "base_req": {
	"from": "gt11ja8j8qskxvccwf3rchp9efxjdu6v5wfkj5uwu4cmktue7h7ufjwqlgqs9ja64xj9kgd5zj", //sender account
	"memo": "", ////transaction remarks,The length of the remarks is limited to 85 characters in Chinese and 256 characters in English.
	"chain_id": "testnet", //chain ID
	"gas": "200000", //gas consumed by the transaction
	"fees": [
	  {
		"denom": "NANOGT", //unit
		"amount": "5000" //fee
	  }
	],
	"simulate": false, //if calculate simulated gas
   "valid_height":[ //block height at which  the transaction takes effect
   		"600",
   		"900"
   ] 
  },
  "amount": "10000000" //amount to additionally issue
}
```

**Return Example**

```json
{
    "type":"StdTx",
    "value":{
        "msg":[
            {
                "type":"MintToken", //transaction type
                "value":{
                  "source_address":"gt11ja8j8qskxvccwf3rchp9efxjdu6v5wfkj5uwu4cmktue7h7ufjwqlgqs9ja64xj9kgd5zj", //sender account
                    "amount":{
                        "denom":"YJ-9D3", //unit of additionally issued token
                        "amount":"10000000" //amount to additionally issue
                    }
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
  		 "valid_height":[ //block height at which  the transaction takes effect
   			 "600",
   			 "900"
 		 ] 
    }
}
```


### 21. Freeze Token
> The interface generates transaction body for " Change Clearing Height".After locally signing it, you can invoke "Send Transaction" interface to finish broadcasting.


**Interface Path**

```bash
POST /v1/token/freeze/{symbol}
```

**Parameters**

| Parameter | Type | Description |
|------|------|------|
| symbol | String | onchain token symbol (token symbol-[random string])|


**Request Body Example**

```json
{
    "from": "gt11ja8j8qskxvccwf3rchp9efxjdu6v5wfkj5uwu4cmktue7h7ufjwqlgqs9ja64xj9kgd5zj", //sender account
    "memo": "", ////transaction remarks,The length of the remarks is limited to 85 characters in Chinese and 256 characters in English.
    "chain_id": "testnet", //chain ID
    "gas": "200000", //gas consumed by the transaction
    "fees": [
      {
        "denom": "NANOGT", //unit
        "amount": "5000" //fee
      }
    ],
    "simulate": false, //if calculate simulated gas
    "valid_height":[ //block height at which  the transaction takes effect
        "600",
        "900"
    ]
}
```

**Return Example**

```json
{
    "type":"StdTx",
    "value":{
        "msg":[
            {
                "type":"FreezeToken", //transaction type
                "value":{
                  "source_address":"gt11ja8j8qskxvccwf3rchp9efxjdu6v5wfkj5uwu4cmktue7h7ufjwqlgqs9ja64xj9kgd5zj", //sender account
                    "symbol":"YY-A69" //unit of token to freeze
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
        "valid_height":[ //block height at which  the transaction takes effect
           "600",
           "900"
        ]
    }
}
```

### 22. Unfreeze Token
> The interface generates transaction body for " Change Clearing Height".After locally signing it, you can invoke "Send Transaction" interface to finish broadcasting.


**Interface Path**

```bash
POST /v1/token/unfreeze/{symbol}
```

**Parameters**

| Parameter | Type | Description |
|------|------|------|
| symbol | String | onchain token symbol (token symbol-[random string])|


**Request Body Example**

```json
{
    "from": "gt11ja8j8qskxvccwf3rchp9efxjdu6v5wfkj5uwu4cmktue7h7ufjwqlgqs9ja64xj9kgd5zj", //sender account
    "memo": "",  ////transaction remarks,The length of the remarks is limited to 85 characters in Chinese and 256 characters in English.
    "chain_id": "testnet", //chain ID
    "gas": "200000", //gas consumed by the transaction
    "fees": [
      {
        "denom": "NANOGT", //unit
        "amount": "5000" //fee
      }
    ],
    "simulate": false, //if calculate simulated gas
    "valid_height":[ //block height at which  the transaction takes effect
         "600",
         "900"
    ]
}
```

**Return Example**

```json
{
    "type":"StdTx",
    "value":{
        "msg":[
            {
                "type":"UnfreezeToken", //transaction type
                "value":{
                  "source_address":"gt11ja8j8qskxvccwf3rchp9efxjdu6v5wfkj5uwu4cmktue7h7ufjwqlgqs9ja64xj9kgd5zj", //sender account
                    "symbol":"YY-A69" //unit of token to unfreeze
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
        "valid_height":[ //block height at which  the transaction takes effect
           "600",
           "900"
        ]
    }
}
```

### 23. Burn Token 
> The interface generates transaction body for " Change Clearing Height".After locally signing it, you can invoke "Send Transaction" interface to finish broadcasting.


**Interface Path**

```bash
POST /v1/token/burn/{symbol}
```

**Parameters**

| Parameter | Type | Description |
|------|------|------|
| symbol | String | onchain token symbol (token symbol-[random string])|


**Request Body Example**

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
        "valid_height":[ //block height at which  the transaction takes effect
           "600",
           "900"
        ]
    },
    "amount": "10000" //token amount to burn
}
```

**Return Example**

```json
{
    "type":"StdTx",
    "value":{
        "msg":[
            {
                "type":"BurnToken", //transaction type
                "value":{
                    "from_address":"gt11ja8j8qskxvccwf3rchp9efxjdu6v5wfkj5uwu4cmktue7h7ufjwqlgqs9ja64xj9kgd5zj", //sender account
                    "sub":{
                        "denom":"YY-A69", //unit of token to burn
                        "amount":"10000" //burned token amount 
                    }
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
        "valid_height":[ //block height at which  the transaction takes effect
           "600",
           "900"
        ]
    }
}
```

### 24. Query Token Information 

**Interface Path**

```bash
GET  /v1/token/show/{symbol}
```

**Parameters**

| Parameter | Type | Description |
|------|------|------|
| symbol | String | onchain token symbol (token symbol-[random string])|


**Return Example**

```json
{
    "height":"0", //block height
    "result":{
        "type":"Token", //token type
        "value":{
            "freezable":true, //if allows freezing
            "freezed":false, //if freeze
            "mintable":true, //if allows additionally issuing
            "source_address":"gt11ja8j8qskxvccwf3rchp9efxjdu6v5wfkj5uwu4cmktue7h7ufjwqlgqs9ja64xj9kgd5zj", //Issuer account address
            "symbol":"YJ-9D3", //onchain token symbol
            "token_name":"test token", //token name
            "total_supply":"1000000000000000" //total supply
        }
    }
}
```

### 25. Query All Token

**Interface Path**

```bash
GET  /v1/token/list
```

**Parameters**

| Parameter | Type | Description |
|------|------|------|
| symbol | String | onchain token symbol (token symbol-[random string])|


**Return Example**

```json
{
    "height":"0",
    "result":{
        "tokens":[
            {
                "type":"Token", //token type
                "value":{
                    "freezable":true, //if allows freezing
                    "freezed":false, //if freeze
                    "mintable":true, //if allows additionally issuing
                  "source_address":"gt11ja8j8qskxvccwf3rchp9efxjdu6v5wfkj5uwu4cmktue7h7ufjwqlgqs9ja64xj9kgd5zj", //Issuer account address
                    "symbol":"YJ-9D3", //onchain token symbol
                    "token_name":"test token", //token name
                    "total_supply":"1000000000000000" //total supply
                }
            },
            ...
        ]
    }
}
```


### 26. Query Foundation Member List

**Interface Path**

```bash
GET  v1/foundation/members
```

**Return Example**

```json
{
    "height": "0", //block height
    "result": [
        {
            "address": "gt11tgjslrwl0j35czlj0ayxq9t7hzd0gtckfwc57qsl3nftl7zyk8gccv5kexetmm3xsv2tj5", //foundation member account
            "funds_pool": [
                {
                    "amount": "10000000000000000", //total token quantity
                    "denom": "NANOGT" //unit
                }
            ],
            "proportion": "1", //the  proportion held by the foundation member
            "released": [
                {
                    "amount": "4666666666.660000000000000000", //token already released
                    "denom": "NANOGT" //unit
                }
            ],
            "withdraw": [] //revoked token amount
        }
    ]
}
```

### 27. Query Foundation Member Information

**Interface Path**

```bash
GET  /v1/foundation/member/{address}
```
**Parameters**

| Parameter | Type | Description |
|------|------|------|
| address | String | foundation member account address|

**Return Example**

```json
{
    "height": "0", //block height
    "result": {
        "type": "Member",
        "value": {
            "address": "gt11twm7dma44k7wg5jppeyphrct9nx2l4m8szy44h72qv9eatyla3hkaevg3vx99mlslwsnfq", //foundation member account
            "funds_pool": [
                {
                    "amount": "10000000000000000",//total token quantity
                    "denom": "NANOGT" //unit
                }
            ],
            "proportion": "1", //the  proportion held by the foundation member
            "released": [
                {
                    "amount": "11333333333.330000000000000000", //token already released
                    "denom": "NANOGT" //unit
                }
            ],
            "withdraw": [] //revoked token amount
        }
    }
}
```

### 28. Delegate Token To Consensus Account
> The interface generates transaction body for " Change Clearing Height".After locally signing it, you can invoke "Send Transaction" interface to finish broadcasting.


**Interface Path**

```bash
POST /v1/staking/delegator/{delegatorAddr}/delegate
```

**Parameters**

| Parameter | Type | Description |
|------|------|------|
| delegatorAddr | String |delegator’s account|


**Request Body Example**

```json
{
    "base_req":{
        "from":"gt110hwwuh7chle04dk38ut7l0uz8estmnsl78kmdqppnhrvvyrps92lzqh5q52ny4ztv5gaq9", //sender account
        "memo":"", ////transaction remarks,The length of the remarks is limited to 85 characters in Chinese and 256 characters in English.
        "chain_id":"testnet", //chain ID
        "gas":"200000", //gas consumed by the transaction
        "fees":[
            {
                "denom":"NANOGT", //unit
                "amount":"500" //fee
            }
        ],
        "simulate":false, //if calculate simulated gas
        "valid_height":[ //the block height at which the transaction takes effect
            "600",
            "900"
    	]
    },
    "con-account_address":"gt116h05fjhaay7sx3zl9w5ej3tpx3s94yhcsmt0gqcqsq26w2qvsyt4l82vftygtff0pfsr93", //consensus account address
    "delegator_address":"gt110hwwuh7chle04dk38ut7l0uz8estmnsl78kmdqppnhrvvyrps92lzqh5q52ny4ztv5gaq9", //delegator's account address
    "amount":{
        "denom":"NANOGT", //unit
        "amount":"100000000" //delegation token amount 
    }
}
```

**Return Example**

```json
{
    "type":"StdTx",
    "value":{
        "msg":[
            {
                "type":"MsgDelegate", //transaction type
                "value":{
                    "delegator_address":"gt110hwwuh7chle04dk38ut7l0uz8estmnsl78kmdqppnhrvvyrps92lzqh5q52ny4ztv5gaq9", //delegator's account address
                    "con-account_address":"gt116h05fjhaay7sx3zl9w5ej3tpx3s94yhcsmt0gqcqsq26w2qvsyt4l82vftygtff0pfsr93", //consensus account address
                    "amount":{
                        "denom":"NANOGT",  //unit
                        "amount":"100000000" //delegation token amount 
                    }
                }
            }
        ],
        "fee":{
            "amount":[
                {
                    "denom":"NANOGT", //unit
                    "amount":"500" //fee
                }
            ],
            "gas":"200000" //gas consumed by the transaction
        },
        "nonces":[
            null
        ],
        "signatures":null, //signature
        "memo":"",
        "valid_height":[ //the block height at which the transaction takes effect
            "600",
            "900"
    	]
    }
}
```

### 29. Query Delegation Information of A Delegator Account In A Consensus Account 

**Interface Path**

```bash
GET  /v1/staking/delegator/{delegatorAddr}/{con-account}/delegations

```
**Parameters**

| Parameter | Type | Description |
|------|------|------|
| delegatorAddr | String | delegator’s account|
| con-account | String | consensus account|

**Return Example**

```json
{
    "height": "103", //block height
    "result": {
        "balance": "1000000000", //delegation token amount 
        "con-account_address": "gt11ja8j8qskxvccwf3rchp9efxjdu6v5wfkj5uwu4cmktue7h7ufjwqlgqs9ja64xj9kgd5zj", //consensus account address
        "delegator_address": "gt11923wtfrfea85w9pklkkmpff7ctllhyjfyed54amdnmtteerk4jrl0tl58khd300jvgnsma", //delegator's account address
        "shares": "1000000000.000000000000000000" //delegation amount
    }
}
```

### 30. Query Delegation Information of A Delegator Account In All Consensus Accounts

**Interface Path**

```bash
GET  /v1/staking/delegator/{delegatorAddr}/delegations
```
**Parameters**

| Parameter | Type | Description |
|------|------|------|
| delegatorAddr | String | delegator’s account|

**Return Example**

```json
{
    "height": "117", //block height
    "result": [
        {
            "balance": "200000", //delegation token amount 
            "con-account_address": "gt11fd299ajlray3ltuj0jmzvwylzafscymk9nc98trr5peedf9q3s90wnczpa7qr6f5d6y3ny", //consensus account address
            "delegator_address": "gt11923wtfrfea85w9pklkkmpff7ctllhyjfyed54amdnmtteerk4jrl0tl58khd300jvgnsma", //delegator's account address
            "shares": "200000.000000000000000000" //delegation amount
        },
		...
    ]
}
```

### 31. Query Consensus Accounts Information For All Delegations of An Account

**Interface Path**

```bash
GET /v1/staking/delegator/{delegatorAddr}/con-accounts
```
**Parameters**

| Parameter | Type | Description |
|------|------|------|
| delegatorAddr | String | delegator’s account|

**Return Example**

```json
{
    "height":"77842",//block height
    "result":[
        {
            "commission":{
                "commission_rates":{
                    "max_change_rate":"0.010000000000000000",//fee maximum change  range
                    "max_rate":"0.010000000000000000",//maximum fee
                    "rate":"0.010000000000000000" //fee
            },
                "update_time":"2020-05-27T08:13:47Z"// fee updated at
            },
            "delegator_shares":"100000000.000000000000000000",//delegation amount of a consensus account 
            "description":{//consensus account attributes collection
                "details":"",
                "identity":"",
                "moniker":"contwo",
                "website":""
            },
            "operator_address":
"gt11h3ugxuhhljffqyvj7sm08u3507ykdr5w67d9dkuv5tktv2vyc5xqrsxv7ujd8r6xvpwpt7",//consensus account address
			  "power": "39000934", //consensus account power
            "power_rate":"1.029615402961540000",//consensus account loyalty coefficient
            "pubkey": "gt1pub1u8s6p73qzlye3d4mljgt3auxhz4shj43w2eu0evladd03rr2auyrhc87aynqpwdz6w", //consensus account public key
            "status":"online", //consensus account online status
            "tokens":"100000000"//total token amount delegated to the consensus account
        }
    ]
}
```


### 32. Query A List of Delegation Transactions of A Delegator Account

**Interface Path**

```bash
GET  /v1/tx?message.sender={delegatorAddr}&limit=1&page=1&message.action={delegation/redelegation/undelegation}
```
**Parameters**

| Parameter | Type | Description |
|------|------|------|
| delegatorAddr | String | delegator’s account|
| action | String | action type:delegate/shift delegation/undelegate|
**Return Example**

```json
[
    {
        "count": "1", //Query entries
        "limit": "100", //entries per page
        "page_number": "1", // page number
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
                        "type": "delegate" //transaction type
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
                "height": "4185", //block height of the transaction
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
                                    "amount": "11", //transaction fee
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
                                "signature": "+DabtgXQ3TInf5Nw75/H0AKNUOYs14kllMbB4GiVUdm4pHZfJEUoxDZ5bzMApf3seBfeWihseOtIXE6rnSWbDA==" //signature
                            }
                        ],
                        "valid_height": [ 
                            "4173",
                            "4383" //the block height at which the transaction takes effect
                        ]
                    }
                },
                "txhash": "BASIC-57884EB3E55CD2BDA7E912D6B2851CB539A4C4ED40DFC164B0AF57E9A49D512883E353D38677EC051055A17A948415A7" //transaction hash
            }
        ]
    }
]
```

### 33. Shift Delegation
> The interface generates transaction body for " Change Clearing Height".After locally signing it, you can invoke "Send Transaction" interface to finish broadcasting.


**Interface Path**

```bash
POST /v1/staking/delegator/{delegatorAddr}/redelegate
```

**Parameters**

| Parameter | Type | Description |
|------|------|------|
| delegatorAddr | String |delegator’s account|


**Request Body Example**

```json
{
    "base_req":{
        "from":"gt110hwwuh7chle04dk38ut7l0uz8estmnsl78kmdqppnhrvvyrps92lzqh5q52ny4ztv5gaq9", //sender account
        "memo":"", ////transaction remarks,The length of the remarks is limited to 85 characters in Chinese and 256 characters in English.
        "chain_id":"testnet", //chain ID
        "gas":"200000", //gas consumed by the transaction
        "fees":[
            {
                "denom":"NANOGT", //unit
                "amount":"500" //fee
            }
        ],
        "simulate":false, //if calculate simulated gas
        "valid_height":[ //the block height at which transaction takes effect
            "600",
            "900"
    	]
    },
    "con-account_src_address": "gt116h05fjhaay7sx3zl9w5ej3tpx3s94yhcsmt0gqcqsq26w2qvsyt4l82vftygtff0pfsr93", //source consensus account
    "con-account_dst_address": "gt11la699nscvukjp5kj07nsgq2styuq63zgy8n04srcldx3dal6fkfa22y8a9fz9thuezvnls", //target consensus account
    "delegator_address":"gt110hwwuh7chle04dk38ut7l0uz8estmnsl78kmdqppnhrvvyrps92lzqh5q52ny4ztv5gaq9", //delegator  account
    "amount":
        {
            "denom":"NANOGT", //unit
            "amount":"100000000" //  token amount to shift 
        }
    
}
```

**Return Example**

```json
{
    "type": "StdTx",
    "value": {
        "msg": [
            {
                "type": "MsgBeginRedelegate", //transaction type
                "value": {
                    "delegator_address": "gt110hwwuh7chle04dk38ut7l0uz8estmnsl78kmdqppnhrvvyrps92lzqh5q52ny4ztv5gaq9", //delegator  account
                    "con-account_src_address": "gt116h05fjhaay7sx3zl9w5ej3tpx3s94yhcsmt0gqcqsq26w2qvsyt4l82vftygtff0pfsr93", //source consensus account
                    "con-account_dst_address": "gt11la699nscvukjp5kj07nsgq2styuq63zgy8n04srcldx3dal6fkfa22y8a9fz9thuezvnls", //target consensus account
                    "amount": {
                        "denom": "NANOGT", //unit
                        "amount": "100000000" // token amount to shift
                    }
                }
            }
        ],
        "fee": {
            "amount": [
                {
                    "denom": "NANOGT", //unit
                    "amount": "500" //fee
                }
            ],
            "gas": "200000"  //gas consumed by the transaction
        },
        "nonces": [
            null
        ],
        "signatures": null, //signature
        "memo": "",
        "valid_height":[ //the block height at which transaction takes effect
            "600",
            "900"
    	]
    }
}
```

### 34. Query Delegation Shifts 


**Interface Path**

```bash
GET /v1/staking/redelegations?delegator={delegatorAddr}&con-account_from={con-account_from}&con-account_to={con-account_to}
```

**Parameters**

| Parameter | Type | Description |
|------|------|------|
| delegatorAddr | String |delegator’s account|
| con-account_from | String | source consensus account|
| con-account_to | String | target consensus account|

**Return Example**

```json
{
    "height": "4573", //block height
    "result": [
        {
            "delegator_address": "gt11a0a2pcna4jmkuz4z8af7tejpyh0u8yh2wtktq8xpjt3qaualzdtwxw7r9cwh88pnkfk4xn", //delegator's account address
            "con-account_dst_address": "gt11a0a2pcna4jmkuz4z8af7tejpyh0u8yh2wtktq8xpjt3qaualzdtwxw7r9cwh88pnkfk4xn", //target consensus account
            "con-account_src_address": "gt11la699nscvukjp5kj07nsgq2styuq63zgy8n04srcldx3dal6fkfa22y8a9fz9thuezvnls", //source consensus account
            "entries": [
                {
                    "balance": "40000000", // shift delegation token amount 
                    "completion_time": "2020-06-26T19:18:28Z", //time the shift delegation finishes at
                    "creation_height": 0, //block height at which the shift delegation transaction is initiated
                    "initial_balance": "40000000", //initial token amount of shift delegation
                    "shares_dst": "40000000.000000000000000000" //delegation amount shifted to target consensus account
                }
            ]
        }
    ]
}
```


### 35. Undelegate From A Consensus Account
> The interface generates transaction body for " Change Clearing Height".After locally signing it, you can invoke "Send Transaction" interface to finish broadcasting.


**Interface Path**

```bash
POST  /v1/staking/delegator/{delegatorAddr}/undelegate
```

**Parameters**

| Parameter | Type | Description |
|------|------|------|
| delegatorAddr | String |delegator’s account|


**Request Body Example**

```json
{
    "base_req":{
        "from":"gt11a0a2pcna4jmkuz4z8af7tejpyh0u8yh2wtktq8xpjt3qaualzdtwxw7r9cwh88pnkfk4xn", //sender account
        "memo":"", ////transaction remarks,The length of the remarks is limited to 85 characters in Chinese and 256 characters in English.
        "chain_id":"testnet", //chain ID
        "gas":"200000", //gas consumed by the transaction
        "fees":[
            {
                "denom":"NANOGT",
                "amount":"500" //fee
            }
        ],
        "simulate":false, //if calculate simulated gas
        "valid_height":[ //the block height at which transaction takes effect
            "600",
            "900"
    	]
    },
    "con-account_address": "gt11la699nscvukjp5kj07nsgq2styuq63zgy8n04srcldx3dal6fkfa22y8a9fz9thuezvnls", //consensus account
    "delegator_address":"gt11a0a2pcna4jmkuz4z8af7tejpyh0u8yh2wtktq8xpjt3qaualzdtwxw7r9cwh88pnkfk4xn", //delegator  account
    "amount":
        {
            "denom":"NANOGT", //unit
            "amount":"10000000" // amount to undelegate
        }
}
```

**Return Example**

```json
{
    "type": "StdTx",
    "value": {
        "msg": [
            {
                "type": "MsgUndelegate", //transaction type
                "value": {
                    "delegator_address": "gt11a0a2pcna4jmkuz4z8af7tejpyh0u8yh2wtktq8xpjt3qaualzdtwxw7r9cwh88pnkfk4xn", //delegator  account
                    "con-account_address": "gt11la699nscvukjp5kj07nsgq2styuq63zgy8n04srcldx3dal6fkfa22y8a9fz9thuezvnls", //consensus account
                    "amount": {
                        "denom": "NANOGT", //unit
                        "amount": "10000000" //amount to undelegate
                    }
                }
            }
        ],
        "fee": {
            "amount": [
                {
                    "denom": "NANOGT", //unit
                    "amount": "500" //fee
                } 
            ],
            "gas": "200000" //gas consumed by the transaction
        },
        "nonces": [
            null
        ],
        "signatures": null, //signature
        "memo": "",
        "valid_height":[ //the block height at which transaction takes effect
            "600",
            "900"
    	]
    }
}
```

### 36. Release of delegation through security account
> The interface generates transaction body for " Change Clearing Height".After locally signing it, you can invoke "Send Transaction" interface to finish broadcasting.


**Interface Path**

```bash
POST  /v1/staking/delegator/undelegate_by_retrieval_account
```

**Request Body Example**

```json
{
    "base_req":{
        "from":"gt11la699nscvukjp5kj07nsgq2styuq63zgy8n04srcldx3dal6fkfa22y8a9fz9thuezvnls", //sender account
        "memo":"", ////transaction remarks,The length of the remarks is limited to 85 characters in Chinese and 256 characters in English.
        "chain_id":"testnet", //chain ID
        "gas":"200000", //gas consumed by the transaction
        "fees":[
            {
                "denom":"NANOGT",
                "amount":"500" //fee
            }
        ],
        "simulate":false, //if calculate simulated gas
        "valid_height":[ //the block height at which transaction takes effect
            "600",
            "900"
    	]
    },
    "security_address": "gt11la699nscvukjp5kj07nsgq2styuq63zgy8n04srcldx3dal6fkfa22y8a9fz9thuezvnls", //security address
    "delegator_address":["vault11556shquf76lunqu7hz05qtd2yda0gm8y0k2k3ku928nmyhgkjhrh95utu3h5c7wr6wuw7q"]//vault account
}
```

**Return Example**

```json
{
    "type": "StdTx",
    "value": {
        "msg": [
            {
                "type": "MsgUndelegateByRetrievalAccount", //transaction type
                "value": {
                    "security_address": "gt11a0a2pcna4jmkuz4z8af7tejpyh0u8yh2wtktq8xpjt3qaualzdtwxw7r9cwh88pnkfk4xn", //security address
                    "delegator_address": "vault11556shquf76lunqu7hz05qtd2yda0gm8y0k2k3ku928nmyhgkjhrh95utu3h5c7wr6wuw7q", //vault account
                }
            }
        ],
        "fee": {
            "amount": [
                {
                    "denom": "NANOGT", //unit
                    "amount": "500" //fee
                } 
            ],
            "gas": "200000" //gas consumed by the transaction
        },
        "nonces": [
            null
        ],
        "signatures": null, //signature
        "memo": "",
        "valid_height":[ //the block height at which transaction takes effect
            "600",
            "900"
    	]
    }
}
```

### 37. Query Undelegations of A Delegator Account in A consensus Account

**Interface Path**

```bash
GET /v1/staking/delegator/{delegatorAddr}/{con-account}/undelegations
```
**Parameters**

| Parameter | Type | Description |
|------|------|------|
| delegatorAddr | String |delegator’s account|
| con-account | String | consensus account|


**Return Example**

```json
{
    "height": "4595", //block height
    "result": {
            "con-account_address": "gt11la699nscvukjp5kj07nsgq2styuq63zgy8n04srcldx3dal6fkfa22y8a9fz9thuezvnls", //consensus account address
        "delegator_address": "gt11a0a2pcna4jmkuz4z8af7tejpyh0u8yh2wtktq8xpjt3qaualzdtwxw7r9cwh88pnkfk4xn", //delegator's account address
        "entries": [
            {
                "balance": "10000000", //undelegated amount 
                "completion_time": "2020-06-26T13:41:48Z", //time at which the undelegation finishes. That is, the time when principal is received
                "creation_height": "3977", //block height at which the undelegate  transaction is initiated
                "initial_balance": "10000000" //token amount at the time the undelegate transaction is initiated. If the consensus account is a bad actor during the undelegating time, the delegator's token will be deducted
            }
        ]
    }
}
```

### 38. Query Undelegations of A Delegator Account in All consensus Accounts

**Interface Path**

```bash
GET /v1/staking/delegator/{delegatorAddr}/undelegations
```
**Parameters**

| Parameter | Type | Description |
|------|------|------|
| delegatorAddr | String |delegator’s account|



**Return Example**

```json
{
    "height": "4595", //block height
    "result": {
            "con-account_address": "gt11la699nscvukjp5kj07nsgq2styuq63zgy8n04srcldx3dal6fkfa22y8a9fz9thuezvnls", //consensus account address
        "delegator_address": "gt11a0a2pcna4jmkuz4z8af7tejpyh0u8yh2wtktq8xpjt3qaualzdtwxw7r9cwh88pnkfk4xn", //delegator's account address
        "entries": [
            {
                "balance": "10000000", //Undelegation amount
                "completion_time": "2020-06-26T13:41:48Z", //time at which the undelegation finishes. That is, the time when principal is received
                "creation_height": "3977", //block height at which the undelegate  transaction is initiated
                "initial_balance": "10000000" //token amount at the time the undelegate transaction is initiated. If the consensus account is a bad actor during the undelegating time, the delegator's token will be deducted
            }
        ]
    },
    ...
}
```

### 39. Query All Delegations Of A Specific Consensus Account

**Interface Path**

```bash
GET /v1/staking/con-account/{con-account}/delegations
```
**Parameters**

| Parameter | Type | Description |
|------|------|------|
| con-account | String |consensus account|



**Return Example**

```json
{
    "height": "5273", //block height
    "result": [
        {
            "balance": "1000000", //delegation token amount
            "con-account_address": "gt116h05fjhaay7sx3zl9w5ej3tpx3s94yhcsmt0gqcqsq26w2qvsyt4l82vftygtff0pfsr93", //consensus account
            "delegator_address": "gt11qfee0959earfv7euauzw8l0fljymwjx34m6s2ccvjhupc59dg93enajhuft3eq50tvz39x", //delegator  account
            "shares": "1000000.000000000000000000" //delegation amount
        },
        ...
    ]
}
```


### 40. Query All Undelegations Of A Specific Consensus Account

**Interface Path**

```bash
GET /v1/staking/con-account/{con-account}/undelegations
```
**Parameters**

| Parameter | Type | Description |
|------|------|------|
| con-account | String |consensus account|



**Return Example**

```json
{
    "height": "5287", //block height
    "result": [
        {
            "con-account_address": "gt11la699nscvukjp5kj07nsgq2styuq63zgy8n04srcldx3dal6fkfa22y8a9fz9thuezvnls", //consensus account address
            "delegator_address": "gt11a0a2pcna4jmkuz4z8af7tejpyh0u8yh2wtktq8xpjt3qaualzdtwxw7r9cwh88pnkfk4xn", //delegator  account
            "entries": [
                {
                    "balance": "10000000", //Undelegation amount
                    "completion_time": "2020-06-26T13:41:48Z", //time at which the undelegation finishes. That is, the time when principal is received
                    "creation_height": "3977", //block height at which the undelegate  transaction is initiated
                    "initial_balance": "10000000" //token amount when initiating the undelegate transaction. If the consensus account is a bad actor during the undelegating time, the delegator's token will be deducted
                }
            ]
        }
    ]
}
```

### 41. Query Staking Pool

**Interface Path**

```bash
GET /v1/staking/pool
```

**Return Example**

```json
{
    "height": "43471",
    "result": {
        "bonded_tokens": "2000012230843453",
        "not_bonded_tokens": "2768799795"
    }
}
```

### 42. Query Staking Parameters

**Interface Path**

```bash
GET /v1/staking/parameters
```

**Return Example**

```json
{
    "height": "5290", //block height
    "result": {
        "bond_denom": "NANOGT", //token unit
        "max_entries": 7, //supported maximum number of businesses(undelegate and re-delegate businesses)
        "max_pow_rate": 2, //maximum loyalty coefficient
        "max_con-accounts": 100, //maximum number of consensus accounts 
        "pow_rate": 1, //minimum loyalty coefficient
        "reward_uint_gt": 18, //reward unit
        "undelegating_time": "1814400000000000" //time when the delegating finishes 
    }
}
```


### 43. Setup Account to Fetch Income
> The interface generates transaction body for " Change Clearing Height".After locally signing it, you can invoke "Send Transaction" interface to finish broadcasting.


**Interface Path**

```bash
POST /v1/distribution/delegator/{delegatorAddr}/withdraw_address
```
**Parameters**

| Parameter | Type | Description |
|------|------|------|
| delegatorAddr | String |delegator’s account|

**Request Body Example**

```json
{
    "base_req":{
        "from":"gt11a0a2pcna4jmkuz4z8af7tejpyh0u8yh2wtktq8xpjt3qaualzdtwxw7r9cwh88pnkfk4xn", //sender account
        "memo":"", ////transaction remarks,The length of the remarks is limited to 85 characters in Chinese and 256 characters in English.
        "chain_id":"testnet", //chain ID
        "gas":"200000", //gas consumed by this  transaction
        "fees":[
            {
                "denom":"NANOGT", //unit
                "amount":"500" //fee
            }
        ],
        "simulate":false, //If calculate simulated Gas
        "valid_height":[ //height at which the transaction takes effect
            "600",
            "900"
    	]
    },
    "withdraw_address": "gt11s9ey8sex7vut2wpxejf5gvejc300kqxpaqf3r7kkkks6ancw0jjrwc3zrwulcqlamjqzwd" //new account to fetch income
}
```

**Return Example**

```json
{
    "type": "StdTx",
    "value": {
        "msg": [
            {
                "type": "MsgModifyWithdrawAddress", //transaction type
                "value": {
                    "delegator_address": "gt11qfee0959earfv7euauzw8l0fljymwjx34m6s2ccvjhupc59dg93enajhuft3eq50tvz39x", //delegator's account
                    "withdraw_address": "gt11s9ey8sex7vut2wpxejf5gvejc300kqxpaqf3r7kkkks6ancw0jjrwc3zrwulcqlamjqzwd" //new account to fetch income
                }
            }
        ],
        "fee": {
            "amount": [
                {
                    "denom": "NANOGT", //unit
                    "amount": "500" //fee
                }
            ],
            "gas": "200000" //gas consumed by this  transaction
        },
        "nonces": [
            null
        ],
        "signatures": null, //signature
        "memo": "",
        "valid_height":[ //height at which the transaction takes effect
            "600",
            "900"
    	]
    }
}
```

Description：

> The vault account can only withdraw the income and principal to this account and cannot be set up separately.

### 44. Delegator Account Fetch Partial Income From A Consensus Account
> The interface generates transaction body for " Change Clearing Height".After locally signing it, you can invoke "Send Transaction" interface to finish broadcasting.


**Interface Path**

```bash
POST /v1/distribution/delegator/{delegatorAddr}/{con-account}/rewards
```
**Parameters**

| Parameter | Type | Description |
|------|------|------|
| delegatorAddr | String |delegator’s account|
| con-account | String |consensus account|
**Request Body Example**

```json
{
    "base_req":{
        "from":"gt11a0a2pcna4jmkuz4z8af7tejpyh0u8yh2wtktq8xpjt3qaualzdtwxw7r9cwh88pnkfk4xn", //sender account
        "memo":"", ////transaction remarks,The length of the remarks is limited to 85 characters in Chinese and 256 characters in English.
        "chain_id":"testnet", //chain ID
        "gas":"200000", //Gas consumed by this  transaction
        "fees":[
            {
                "denom":"NANOGT", //unit
                "amount":"500" //fee
            }
        ],
        "simulate":false, //If calculate simulated Gas
        "valid_height":[ //height at which the transaction takes effect
            "600",
            "900"
    	]
    }
}
```

**Return Example**

```json
{
    "type": "StdTx",
    "value": {
        "msg": [
            {
                "type": "MsgWithdrawDelegationReward", //transaction type
                "value": {
                    "delegator_address": "gt11a0a2pcna4jmkuz4z8af7tejpyh0u8yh2wtktq8xpjt3qaualzdtwxw7r9cwh88pnkfk4xn", //delegator account
                    "con-account_address": "gt11qfee0959earfv7euauzw8l0fljymwjx34m6s2ccvjhupc59dg93enajhuft3eq50tvz39x" //Consensus Account
                }
            }
        ],
        "fee": {
            "amount": [
                {
                    "denom": "NANOGT", //unit
                    "amount": "500" //fee
                }
            ],
            "gas": "200000" //gas consumed by this  transaction
        },
        "nonces": [
            null
        ],
        "signatures": null, //signature
        "memo": "",
        "valid_height":[ //height at which the transaction takes effect
            "600",
            "900"
    	]
    }
}
```

### 45.Delegator Account Fetch All Income From A Consensus Account
> The interface generates transaction body for " Change Clearing Height".After locally signing it, you can invoke "Send Transaction" interface to finish broadcasting.


**Interface Path**

```bash
POST /v1/distribution/delegator/{delegatorAddr}/rewards
```
**Parameters**

| Parameter | Type | Description |
|------|------|------|
| delegatorAddr | String |delegator’s account|

**Request Body Example**

```json
{
    "base_req":{
        "from":"gt11qfee0959earfv7euauzw8l0fljymwjx34m6s2ccvjhupc59dg93enajhuft3eq50tvz39x", //sender account
        "memo":"", ////transaction remarks,The length of the remarks is limited to 85 characters in Chinese and 256 characters in English.
        "chain_id":"testnet", //chain ID
        "gas":"200000", //Gas consumed by this  transaction
        "fees":[
            {
                "denom":"NANOGT", //unit
                "amount":"500" //fee
            }
        ],
        "simulate":false, //If calculate simulated Gas
        "valid_height":[ //height at which the transaction takes effect
            "600",
            "900"
    	]
    }
}
```

**Return Example**

```json
{
    "type": "StdTx",
    "value": {
        "msg": [
            {
                "type": "MsgWithdrawDelegationReward", //transaction type
                "value": {
                    "delegator_address": "gt11qfee0959earfv7euauzw8l0fljymwjx34m6s2ccvjhupc59dg93enajhuft3eq50tvz39x", //delegator's account
                    "con-account_address": "gt116h05fjhaay7sx3zl9w5ej3tpx3s94yhcsmt0gqcqsq26w2qvsyt4l82vftygtff0pfsr93" //consensus account
                }
            }
        ],
        "fee": {
            "amount": [
                {
                    "denom": "NANOGT", //unit
                    "amount": "500" //fee
                }
            ],
            "gas": "200000" //Gas consumed by this  transaction
        },
        "nonces": [
            null
        ],
        "signatures": null, //signature
        "memo": "",
        "valid_height":[ //height at which the transaction takes effect
            "600",
            "900"
    	]
    }
}
```
### 46.Reinvestment the rewards of the delegator account into the consensus account
> The interface generates transaction body for " Change Clearing Height".After locally signing it, you can invoke "Send Transaction" interface to finish broadcasting.


**Interface Path**

```bash
POST /v1/distribution/delegator/{delegatorAddr}/{con-account}/rewards_reinvestment
```
**Parameters**

| Parameter | Type | Description |
|------|------|------|
| delegatorAddr | String |delegator’s account|
| con-account | String |consensus account|

**Request Body Example**

```json
{
    "base_req":{
        "from":"gt11qfee0959earfv7euauzw8l0fljymwjx34m6s2ccvjhupc59dg93enajhuft3eq50tvz39x", //sender account
        "memo":"", ////transaction remarks,The length of the remarks is limited to 85 characters in Chinese and 256 characters in English.
        "chain_id":"testnet", //chain ID
        "gas":"200000", //Gas consumed by this  transaction
        "fees":[
            {
                "denom":"NANOGT", //unit
                "amount":"100000" //fee
            }
        ],
        "simulate":false, //If calculate simulated Gas
        "valid_height":[ //height at which the transaction takes effect
            "600",
            "900"
    	]
    }
}
```

**Return Example**

```json
{
    "type": "StdTx",
    "value": {
        "msg": [
            {
                "type": "MsgRewardReinvestment",
                "value": {
                    "delegator_address": "gt11mtehamaw8wktwpppx9klrhhfu5upmutzthl0kkpwsa0slw8h4xd3p2ane2zt262dlsyy3m",  //sender account
                    "con-account_address": "gt11rjq598t8vte64ff2tnesdvsfazv38atpenufj4zl0ljhw2q28jnlxnh6mqgmn6elje6fzc"  //consensus account
                }
            }
        ],
        "fee": {
            "amount": [
                {
                    "denom": "NANOGT", //unit
                    "amount": "10000000"  //fee
                }
            ],
            "gas": "10000000" //Gas consumed by this  transaction
        },
        "nonces": [
            null
        ],
        "signatures": null,  //If calculate simulated Gas
        "memo": "",
        "valid_height": [  //height at which the transaction takes effect
            "600",
            "900"
        ]
    }
}
```

### 47.Query Delegation Income of A Delegator Account At A Consensus Account 

**Interface Path**

```bash
GET  /v1/distribution/delegator/{delegatorAddr}/{con-account}/rewards
```
**Parameters**

| Parameter | Type | Description |
|------|------|------|
| delegatorAddr | String |delegator’s account|
| con-account | String |consensus account|


**Return Example**

```json
{
    "height": "0", //block height
    "result": [
        {
            "amount": "109475.336424214100340000", //delegation income
            "denom": "NANOGT" //unit
        }
    ]
}
```
### 48.Query Delegation Income of A Delegator Account At All Consensus Account

**Interface Path**

```bash
GET  /v1/distribution/delegator/{delegatorAddr}/rewards
```
**Parameters**

| Parameter | Type | Description |
|------|------|------|
| delegatorAddr | String |delegator’s account|



**Return Example**

```json
{
    "height": "0", //block height
    "result": {
        "rewards": [
            {
                "con-account_address": "gt11dsck7f7txdc7hz5wdsspey0w5eyhw7yj4xrc3a6r4w4t39z9k45r86htlzxjf6z57an2r7", //Consensus Account address
                "reward": [
                    {
                        "amount": "105.030990305699919120", //delegation income
                        "denom": "NANOGT" //unit
                    }
                ]
            },
            ...
         ],
        "total": [
            {
                "amount": "105.035150523300345120", //total income from delegation 
                "denom": "NANOGT" //unit
            }
        ]
    }
}
```

### 49.Query Delegation Income Pending Paying By A Consensus Account

**Interface Path**

```bash
GET /v1/distribution/con-account/{con-account}/outstanding_rewards
```
**Parameters**

| Parameter | Type | Description |
|------|------|------|
| con-account | String |consensus account|



**Return Example**

```json
{
    "height": "5365", //block height
    "result": [
        {
            "amount": "12227087603299.775054901980000000", //outstanding delegation income pending paying
            "denom": "NANOGT" //unit
        }
    ]
}
```
### 50.Query Consensus Account Income

**Interface Path**

```bash
GET /v1/distribution/con-account/{con-account}/rewards
```
**Parameters**

| Parameter | Type | Description |
|------|------|------|
| con-account | String |consensus account|



**Return Example**

```json
{
    "height": "0", //block height
    "result": [
        {
            "amount": "1341380880051.597973936430380729", //commission and mining earnings
            "denom": "NANOGT" //unit
        }
    ]
}
```

### 51.Query Distribution And Foundation Parameters

**Interface Path**

```bash
GET /v1/distribution/parameters
```

**Return Example**

```json
{
    "height": "0", //block height
    "result": {
        "community_tax": "0.020000000000000000", //Community Tax rate
        "first_committee_reward": "0.400000000000000000", //The first Committee mining reward rate
        "second_committee_reward": "0.350000000000000000", //The second Committee mining reward rate
        "third_committee_reward": "0.250000000000000000", //The third Committee mining reward rate
        "withdraw_addr_enabled": true 
    }
}
```

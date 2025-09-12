# GateChain CLI Reference

> Important Note:
> - All CLI commands require setting a transaction fee greater than 0NANOGT
> - Asset amounts are positive integer values converted by multiplying by 10E18
> - Asset amounts support scientific notation, e.g., 10E9 NANOGT = 1GT

## Basic Commands

### 1. Start Local RPC Service

**Command**
```bash
gatecli rest-server
```

**Notes**
- When RPC service starts, command line cannot be executed due to storage lock

### 2. Query Node Status

**Command**
```bash
gatecli status
```

**Notes**
- Query local node service status

### 3. Query Version

**Command**
```bash
gatecli version
```

**Notes**
- Query version information of the command line

### 4. Get Help

**Command**
```bash
gatecli [Help1] [Help2] [Help3]... --help
```

**Notes**
- For more information about command line operations, use the help command

#### Errors
```bash
Must specify these options: --chain-id  when --trust-node is false
```

If trust this node, please enter '--trust-node=true'，Otherwise enter'--chain-id'to solve the above error.



## Account Management

### 1. Account Types
- Single Signature Account
  - Prefix: 'gt1'
  - Example: 'gt11twm7dma44k7wg5jppeyphrct9nx2l4m8szy44h72qv9eatyla3hkaevg3vx99mlslwsnfq'
- Multisignature Account
  - Prefix: 'gt2'
  - Example: 'g211twm7dma44k7wg5jppeyphrct9nx2l4m8szy44h72qv9eatyla3hkaevg3vx99mlslwsnfq'
- Single Signature Vault Account
  - Prefix: 'vault1'
  - Example: 'vault11fg056uaatk9s3k6l34eqkc7qchjup8e39afauf9naz4jmpklsze3uwx6405vll88l5lvww'
- Multisignature Vault Account
  - Prefix: 'vault2'
  - Example: 'vault21fg056uaatk9s3k6l34eqkc7qchjup8e39afauf9naz4jmpklsze3uwx6405vll88l5lvww'

### 2. Create Single Signature Account

**Command**
```bash
gatecli account create
```

**Response Example**
```
- name: vault
  type: local
  address: gt1124j4d2tjt0c7vrq5er4n2ksyp539gkv28krrsye5ym2y2mf5ekvg2s7neydamthz7a7xld
  pubkey: gt1pub1u8s6p73qf92hfdy6sp3s3657ssqrnpjtwevc86ht6d9txyrnt7239528z6wqxjhlfn
  mnemonic: ""
  threshold: 0
  pubkeys: []

Important: Write this mnemonic phrase in a safe place.
It is the only way to recover your account if you ever forget your password.

flee agree charge truth answer flush inflict shove nice valid auto love laugh review frame sword later man inside couch slogan level guitar diet
```

**Notes**
- The mnemonic phrase is only displayed when creating the account and will not show when using show or list commands
- Save the mnemonic phrase in a secure location as it's required for account recovery

### 3. Create Multisignature Account

**Command**
```bash
gatecli account create [account] --multisig [account list] --multisig-threshold [minimum effective amount]
```

**Parameters**

| Parameter | Type | Description |
|------|------|------|
| account | String | Name of the account to create |
| account list | String | Comma-separated list of existing account addresses |
| minimum effective amount | Integer | Minimum number of signatures required |

**Example**
```bash
gatecli account create gt11twm7dma44k7wg5jppeyphrct9nx2l4m8szy44h72qv9eatyla3hkaevg3vx99mlslwsnfq --multisig gt110nxr6.....,gt113454xdr..... --multisig-threshold 2
```

**Notes**
- Account list must refer to already created accounts
- After creation, use `gatecli account publish-multisig` to get the final multisig address starting with 'gt2'

### 4. Query Account List

**Command**
```bash
gatecli account list
```

**Response Example**
```
- name: con
  type: local
  address: gt11ewdeyjdlxs2x4yglnhtulp0tclgdp7c2t8d5c6495r83qvrfr7p0qt40uddu3k44s7rxg6
  pubkey: gt1pub1u8s6p73qg4vmn6pjsnfehrh0nph8sqnp0nta0clfayjhsgtmjpfeek289gdsmr7au2
  mnemonic: ""
  threshold: 0
  pubkeys: []
```

### 5. Query Account Information

**Command**
```bash
gatecli account show [account] --chain-id [chain ID]
```

**Parameters**

| Parameter | Type | Description |
|------|------|------|
| account | String | Account address to query |
| chain-id | String | Chain ID of the network |

**Example**
```bash
gatecli account show gt11twm7dma44k7wg5jppeyphrct9nx2l4m8szy44h72qv9eatyla3hkaevg3vx99mlslwsnfq --chain-id testnet
```

**Response Example**
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

**Notes**
- AccountType: 0=Single signature standard account, 1=Single signature Vault Account, 2=Multisignature standard account, 3=Multisignature Vault Account
- VaultAddress: If present, indicates this account is a Retrieval Account for the listed Vault Account

### 6. Change Account Password

**Command**
```bash
gatecli account update [name]
```

**Example**
```bash
gatecli account update 1583472684
```

**Response Example**
```
Password successfully updated!
```

### 7. Delete Account

**Command**
```bash
gatecli account delete [name]
```

**Example**
```bash
gatecli account delete 1583472684
```

**Response Example**
```
Key deleted forever (uh oh!)
```

**Notes**
- Account private key will be deleted permanently

### 8. Query Account Balance

**Command**
```bash
gatecli account balance [account] --chain-id [chain ID]
```

**Parameters**

| Parameter | Type | Description |
|------|------|------|
| account | String | Account address to query |
| chain-id | String | Chain ID of the network |

**Example**
```bash
gatecli account balance gt11twm7dma44k7wg5jppeyphrct9nx2l4m8szy44h72qv9eatyla3hkaevg3vx99mlslwsnfq --chain-id testnet
```

**Response Example**
```
1000000NANOGT //account balance
```

### 9. Query Account Public Key

**Command**
```bash
gatecli account show-key [name]
```

**Example**
```bash
gatecli account show-key 1583472684
```

**Response Example**
```
- name: delegate
  type: local
  address: gt11m5acrv46s5xzr8r3h8f3z9hz8wdl3ucfcmw6ssac2kfvad649u6nfzhx2rpk4ucvrxla6s
  pubkey: gt1pub1u8s6p73qva7mzaymcwfxf6ssmvamtjzzd3adp4eavlky0gwhlwwlwpukqq6sqwuxfg
  mnemonic: ""
  threshold: 0
  pubkeys: []
```

### 10. Publish Multisignature Account

**Command**
```bash
gatecli account publish-multisig [account] [public key] --from [sender account] --fees [tx fees] --chain-id [chain ID]
```

**Parameters**

| Parameter | Type | Description |
|------|------|------|
| account | String | Account address to publish |
| public key | String | Public key of the multisig account |
| sender account | String | Account address of the sender |
| tx fees | String | Transaction fees (e.g., 100000NANOGT) |
| chain-id | String | Chain ID of the network |

**Example**
```bash
gatecli account publish-multisig gt11twm7dma44k7wg5jppeyphrct9nx2l4m8szy44h72qv9eatyla3hkaevg3vx99mlslwsnfq gt1pub1ytql0csgqgfzd666axrjzqegteuuxvghau9u0q67lltpjqla3ykzz3t8efmh6sqhyt4uhnh3q5fzd666axrjzqkhwmygytf0grzudhv69h9ttcy4xhze0v4mtf4jza6mrp0j3lq68qfzd666axrjzqn6wmq0uuyvxr8tywehal0zyzhpy5tv4h5tpryvc449jmznnzdruqy68ks2 --from gt11twm7dma44k7wg5jppeyphrct9nx2l4m8szy44h72qv9eatyla3hkaevg3vx99mlslwsnfq --fees 100000NANOGT --chain-id testnet
```

**Response Example**
```
TxHash: BASIC-9F685A8362E6218E372CE60E306E8BC35B66006D82F9B3381A6AECE26FA6355CA38CD75AFFDF597794159D9356BE0376
Data: rQO5zc/tCu8BYPD/ggoo3TuBsrqFDCGccbnTERbiO5v48wnG3ahDuFWSzrdVLzU0iuZQw2rzDBIoHk1VTbZ0J94UnjHi3aO8fwO1V5rK5I2NZvxNF1lFstSU9JD3J18JbxqUAWd0MXB1YjE4cTJmZ3VnZ3F5Znp0YzBwNXJhenEwZnRwdXplNzJwOXRwN25lZ2plZTl6amtjaGx2MHFwNThyZTdyZGduajNqd2x3d3JscjN6Z2o3cmNkcWxnc2Z3Y2V2YWRqaGE0ZXZoOThkejdzN3pjYzh5MHZhZnY3amh1ajNobXR1M2ZtajM2eXdqZWNtbnF1OWgSEgoMCgZOQU5PR1QSAjExEMCaDBowMXrG9msevrtuVTHWuZdFIixl5hSO4tWOvIZV01T/p+Pbg1sPeBgWGHbKUcm1064KImkKJeHhoPogZ32xdJvDkmTqENs7tchCbHrQ1z1n7Eeh1/ud9weWADUSQJRr9hYE0jvDKTx9IsfYAh3myFPQaYV9pt+TEi+IKdFm2KOZGYckVEbFx9ydMn2F6UbhopD5Y5HbrKJzf0fF9woyBNcEqQY=
Raw Log: sync broadcast tx success
```

## Block Information

### 1. Query Block Information

**Command**
```bash
gatecli block show [block height] --chain-id [chain ID]
```

**Parameters**

| Parameter | Type | Description |
|------|------|------|
| block height | Integer | Height of the block to query |
| chain-id | String | Chain ID of the network |

**Example**
```bash
gatecli block show 10001 --chain-id testnet
```

**Response Example**
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

**Field Descriptions**
- hash: Hash of the current block
- previousBlockHash: Hash of the preceding block
- seed: VRF seed for draw
- proposer: Consensus account that proposed the block
- height: Block height
- period: Block phase
- txnRoot: Hash of the collection of transactions in the block
- timestamp: Time block was generated
- UpgradeState: Version status of consensus protocol
- UpgradeVote: Voting status for consensus protocol upgrading
- CertCommitteeInfo: Block committee (potential reward winners of next round)
- BlockCommitteeInfo: Blockchain committee (reward winners of this round)
- autoOfflineAccounts: Consensus accounts auto-offline at specified rounds

## Consensus Account 

### 1. Create a Consensus Account

**Command**
```bash
gatecli con-account create [account address] --chain-id [chain ID]
```

**Parameters**

| Parameter | Type | Description |
|------|------|------|
| account address | String | Address of the consensus account to create |
| chain-id | String | Chain ID of the network |

**Example**
```bash
gatecli con-account create gt11twm7dma44k7wg5jppeyphrct9nx2l4m8szy44h72qv9eatyla3hkaevg3vx99mlslwsnfq --chain-id testnet
```

**Response Example**
```
ParticipationKeyResponse Data:
 ConAccount:
  Code:	0
  Log:	success //logs
  ParticipationKey:	ParticipationData:
  Address: 		AAAAAAAAAAAABXJ3QGZLVBIMEGOHDOOTCELOEO437DZQTRW5VBB3QVMSZ23VKLZVGSFOMUGDNLZQZVIEBFXQ //gatemint consensus account address
  VoteID: 		hgnhy7jmd8jy19j2hhcageqdj1s4xt7kpnv4388zxt8jaqbyrew0 //VRF public key
  SelectionID: 		293rc5xc7fsz81txx35686x0n4arbvx4cvy7n01e1emtghybhhg0 //the one-time signing public key
  FileName:	AAAAAAAAAAAABXJ3QGZLVBIMEGOHDOOTCELOEO437DZQTRW5VBB3QVMSZ23VKLZVGSFOMUGDNLZQZVIEBFXQ.0.0.partkey //local partkeyFile Name
```
**Description**
- When creating a consensus account, please ensure the account has adequate 'NANOGT' tokens.


### 2. Get Consensus Account Online

**Command**
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

**Parameters**

| Parameter | Type | Description |
|------|------|------|
| sender account | String | Account address of the sender |
| sender account public key | String | Public key of the sender account |
| name | String | Name of the consensus account |
| tx fees | String | Transaction fees (e.g., 100000NANOGT) |
| max commission rate change daily | String | Maximum commission rate change daily |
| max commission rate | String | Maximum commission rate |
| commission rate | String | Commission rate |
| chain-id | String | Chain ID of the network |

**Example**
```bash
gatecli con-account online --from gt11twm7dma44k7wg5jppeyphrct9nx2l4m8szy44h72qv9eatyla3hkaevg3vx99mlslwsnfq --pubkey gt1pub1u8s6p73qp3ac00a628mk7g4utllp6cvrl54vqgdazg3q6d8e3lzxxycrgw3qk3q7wn --moniker newcon-account --fees 100000NANOGT --commission-max-change-rate "0.01" --commission-max-rate "0.01" --commission-rate "0.01" --chain-id testnet
```

**Response Example**
```
 TxHash: BASIC-9F685A8362E6218E372CE60E306E8BC35B66006D82F9B3381A6AECE26FA6355CA38CD75AFFDF597794159D9356BE0376 
  //transaction  has,using gatecli tx show {hash} to query detailed information of this transaction 
  Data: rQO5zc/tCu8BYPD/ggoo3TuBsrqFDCGccbnTERbiO5v48wnG3ahDuFWSzrdVLzU0iuZQw2rzDBIoHk1VTbZ0J94UnjHi3aO8fwO1V5rK5I2NZvxNF1lFstSU9JD3J18JbxqUAWd0MXB1YjE4cTJmZ3VnZ3F5Znp0YzBwNXJhenEwZnRwdXplNzJwOXRwN25lZ2plZTl6amtjaGx2MHFwNThyZTdyZGduajNqd2x3d3JscjN6Z2o3cmNkcWxnc2Z3Y2V2YWRqaGE0ZXZoOThkejdzN3pjYzh5MHZhZnY3amh1ajNobXR1M2ZtajM2eXdqZWNtbnF1OWgSEgoMCgZOQU5PR1QSAjExEMCaDBowMXrG9msevrtuVTHWuZdFIixl5hSO4tWOvIZV01T/p+Pbg1sPeBgWGHbKUcm1064KImkKJeHhoPogZ32xdJvDkmTqENs7tchCbHrQ1z1n7Eeh1/ud9weWADUSQJRr9hYE0jvDKTx9IsfYAh3myFPQaYV9pt+TEi+IKdFm2KOZGYckVEbFx9ydMn2F6UbhopD5Y5HbrKJzf0fF9woyBNcEqQY=
  Raw Log: sync broadcast tx success //transaction  is successfully sent
```

### 3. Get Consensus Account Offline

**Command**
```bash
gatecli con-account offline --from [sender account] --fees [tx fees] --chain-id [chain ID]
```

**Parameters**

| Parameter | Type | Description |
|------|------|------|
| sender account | String | Account address of the sender |
| tx fees | String | Transaction fees (e.g., 100000NANOGT) |
| chain-id | String | Chain ID of the network |

**Example**
```bash
gatecli con-account offline --from gt11twm7dma44k7wg5jppeyphrct9nx2l4m8szy44h72qv9eatyla3hkaevg3vx99mlslwsnfq --fees 100000NANOGT --chain-id testnet
```

**Response Example**
```
  TxHash: BASIC-9F685A8362E6218E372CE60E306E8BC35B66006D82F9B3381A6AECE26FA6355CA38CD75AFFDF597794159D9356BE0376 
  //transaction hash,using gatecli tx show {hash} to query detailed transaction information
  Data: rQO5zc/tCu8BYPD/ggoo3TuBsrqFDCGccbnTERbiO5v48wnG3ahDuFWSzrdVLzU0iuZQw2rzDBIoHk1VTbZ0J94UnjHi3aO8fwO1V5rK5I2NZvxNF1lFstSU9JD3J18JbxqUAWd0MXB1YjE4cTJmZ3VnZ3F5Znp0YzBwNXJhenEwZnRwdXplNzJwOXRwN25lZ2plZTl6amtjaGx2MHFwNThyZTdyZGduajNqd2x3d3JscjN6Z2o3cmNkcWxnc2Z3Y2V2YWRqaGE0ZXZoOThkejdzN3pjYzh5MHZhZnY3amh1ajNobXR1M2ZtajM2eXdqZWNtbnF1OWgSEgoMCgZOQU5PR1QSAjExEMCaDBowMXrG9msevrtuVTHWuZdFIixl5hSO4tWOvIZV01T/p+Pbg1sPeBgWGHbKUcm1064KImkKJeHhoPogZ32xdJvDkmTqENs7tchCbHrQ1z1n7Eeh1/ud9weWADUSQJRr9hYE0jvDKTx9IsfYAh3myFPQaYV9pt+TEi+IKdFm2KOZGYckVEbFx9ydMn2F6UbhopD5Y5HbrKJzf0fF9woyBNcEqQY=
  Raw Log: sync broadcast tx success //transaction  is successfully sent
```

### 4. Modify the max commission rate of consensus account

**Command**
```bash
gatecli con-account max-rate -- commission-max-rate [max commission rate] --commission-max-change-rate [max commission rate change daily] --from [sender account] --fees [tx fees] --chain-id [chain ID]
```

**Parameters**

| Parameter | Type | Description |
|------|------|------|
| max commission rate | String | Maximum commission rate |
| max commission rate change daily | String | Maximum commission rate change daily |
| sender account | String | Account address of the sender |
| tx fees | String | Transaction fees (e.g., 100000NANOGT) |
| chain-id | String | Chain ID of the network |

**Example**
```bash
gatecli con-account max-rate -- commission-max-rate 0.1 --commission-max-change-rate 0.2 --from gt11twm7dma44k7wg5jppeyphrct9nx2l4m8szy44h72qv9eatyla3hkaevg3vx99mlslwsnfq --fees 10000000NANOGT --chain-id testnet
```

**Response Example**
```
 TxHash: BASIC-9F685A8362E6218E372CE60E306E8BC35B66006D82F9B3381A6AECE26FA6355CA38CD75AFFDF597794159D9356BE0376 
  //transaction hash,using gatecli tx show {hash} to query
  Data: rQO5zc/tCu8BYPD/ggoo3TuBsrqFDCGccbnTERbiO5v48wnG3ahDuFWSzrdVLzU0iuZQw2rzDBIoHk1VTbZ0J94UnjHi3aO8fwO1V5rK5I2NZvxNF1lFstSU9JD3J18JbxqUAWd0MXB1YjE4cTJmZ3VnZ3F5Znp0YzBwNXJhenEwZnRwdXplNzJwOXRwN25lZ2plZTl6amtjaGx2MHFwNThyZTdyZGduajNqd2x3d3JscjN6Z2o3cmNkcWxnc2Z3Y2V2YWRqaGE0ZXZoOThkejdzN3pjYzh5MHZhZnY3amh1ajNobXR1M2ZtajM2eXdqZWNtbnF1OWgSEgoMCgZOQU5PR1QSAjExEMCaDBowMXrG9msevrtuVTHWuZdFIixl5hSO4tWOvIZV01T/p+Pbg1sPeBgWGHbKUcm1064KImkKJeHhoPogZ32xdJvDkmTqENs7tchCbHrQ1z1n7Eeh1/ud9weWADUSQJRr9hYE0jvDKTx9IsfYAh3myFPQaYV9pt+TEi+IKdFm2KOZGYckVEbFx9ydMn2F6UbhopD5Y5HbrKJzf0fF9woyBNcEqQY=
  Raw Log: sync broadcast tx success //transaction  is successfully sent
```

### 5. Edit Consensus Account

**Command**
```bash
gatecli con-account edit --moniker [con-account name] --commission-rate [commission rate] --from [sender account] --fees [tx fees] --chain-id [chain ID]
```

**Parameters**

| Parameter | Type | Description |
|------|------|------|
| con-account name | String | Name of the consensus account |
| commission rate | String | Commission rate |
| sender account | String | Account address of the sender |
| tx fees | String | Transaction fees (e.g., 100000NANOGT) |
| chain-id | String | Chain ID of the network |

**Example**
```bash
gatecli con-account edit --moniker con1 --commission-rate 0.03 --from gt11twm7dma44k7wg5jppeyphrct9nx2l4m8szy44h72qv9eatyla3hkaevg3vx99mlslwsnfq --fees 100000NANOGT --chain-id testnet
```

**Response Example**
```
  TxHash: BASIC-9F685A8362E6218E372CE60E306E8BC35B66006D82F9B3381A6AECE26FA6355CA38CD75AFFDF597794159D9356BE0376 
  //transaction hash,using gatecli tx show {hash} to query
  Data: rQO5zc/tCu8BYPD/ggoo3TuBsrqFDCGccbnTERbiO5v48wnG3ahDuFWSzrdVLzU0iuZQw2rzDBIoHk1VTbZ0J94UnjHi3aO8fwO1V5rK5I2NZvxNF1lFstSU9JD3J18JbxqUAWd0MXB1YjE4cTJmZ3VnZ3F5Znp0YzBwNXJhenEwZnRwdXplNzJwOXRwN25lZ2plZTl6amtjaGx2MHFwNThyZTdyZGduajNqd2x3d3JscjN6Z2o3cmNkcWxnc2Z3Y2V2YWRqaGE0ZXZoOThkejdzN3pjYzh5MHZhZnY3amh1ajNobXR1M2ZtajM2eXdqZWNtbnF1OWgSEgoMCgZOQU5PR1QSAjExEMCaDBowMXrG9msevrtuVTHWuZdFIixl5hSO4tWOvIZV01T/p+Pbg1sPeBgWGHbKUcm1064KImkKJeHhoPogZ32xdJvDkmTqENs7tchCbHrQ1z1n7Eeh1/ud9weWADUSQJRr9hYE0jvDKTx9IsfYAh3myFPQaYV9pt+TEi+IKdFm2KOZGYckVEbFx9ydMn2F6UbhopD5Y5HbrKJzf0fF9woyBNcEqQY=
  Raw Log: sync broadcast tx success //transaction  is successfully sent
```

**Description**
- commission-rate The following requirements must be met:
- - Must be between 0 and commission-max-rate set by the con-account;
- - It can only be changed once a day,And the range of change shall not exceed the commission-max-change-rate set by the con-account.


### 6. Query Consensus Account Information

**Command**
```bash
gatecli con-account show-key [consensus account pending online] --chain-id [chain ID]
```

**Parameters**

| Parameter | Type | Description |
|------|------|------|
| consensus account pending online | String | Address of the consensus account to query |
| chain-id | String | Chain ID of the network |

**Example**
```bash
gatecli con-account show-key gt11twm7dma44k7wg5jppeyphrct9nx2l4m8szy44h72qv9eatyla3hkaevg3vx99mlslwsnfq --chain-id testnet
```

**Response Example**
```
ParticipationKeyResponse Data:
 ConAccount:
  Code: 0
  Log: success
  ParticipationKey: ParticipationData:
  Address:     AAAAAAAAAAAABXJ3QGZLVBIMEGOHDOOTCELOEO437DZQTRW5VBB3QVMSZ23VKLZVGSFOMUGDNLZQZVIEBFXQ
  VoteID:     hgnhy7jmd8jy19j2hhcageqdj1s4xt7kpnv4388zxt8jaqbyrew0
  SelectionID:     293rc5xc7fsz81txx35686x0n4arbvx4cvy7n01e1emtghybhhg0
  FileName: AAAAAAAAAAAABXJ3QGZLVBIMEGOHDOOTCELOEO437DZQTRW5VBB3QVMSZ23VKLZVGSFOMUGDNLZQZVIEBFXQ.0.0.partkey
```

### 7. Query Local Consensus Account Information

**Command**
```bash
gatecli con-account show-key [consensus account pending online] --chain-id [chain ID]
```

**Parameters**

| Parameter | Type | Description |
|------|------|------|
| consensus account pending online | String | Address of the consensus account to query |
| chain-id | String | Chain ID of the network |

**Example**
```bash
gatecli con-account show-key gt11twm7dma44k7wg5jppeyphrct9nx2l4m8szy44h72qv9eatyla3hkaevg3vx99mlslwsnfq --chain-id testnet
```

**Response Example**
```
ParticipationKeyResponse Data:
 ConAccount:
  Code:	0
  Log:	success //logs
  ParticipationKey:	ParticipationData:
  Address: 		AAAAAAAAAAAABXJ3QGZLVBIMEGOHDOOTCELOEO437DZQTRW5VBB3QVMSZ23VKLZVGSFOMUGDNLZQZVIEBFXQ //gatemint consensus account address
  VoteID: 		hgnhy7jmd8jy19j2hhcageqdj1s4xt7kpnv4388zxt8jaqbyrew0 //VRF public key
  SelectionID: 		293rc5xc7fsz81txx35686x0n4arbvx4cvy7n01e1emtghybhhg0 //the one-time signing public key
  FileName:	AAAAAAAAAAAABXJ3QGZLVBIMEGOHDOOTCELOEO437DZQTRW5VBB3QVMSZ23VKLZVGSFOMUGDNLZQZVIEBFXQ.0.0.partkey // local partkeyFile Name
```

### 8. Query Consensus Account List

**Command**
```bash
gatecli con-account list --chain-id [chain ID]
```

**Parameters**

| Parameter | Type | Description |
|------|------|------|
| chain-id | String | Chain ID of the network |

**Example**
```bash
gatecli con-account list --chain-id testnet
```

**Response Example**
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

## Distribution Management

### 1. Setup Account to Fetch Income

**Command**
```bash
gatecli distribution set-withdraw-addr [account to fetch income] --from [sender account] --fees [tx fees] --chain-id [chain ID]
```

**Parameters**

| Parameter | Type | Description |
|------|------|------|
| account to fetch income | String | Account address to receive income |
| sender account | String | Account address of the sender |
| tx fees | String | Transaction fees (e.g., 100000NANOGT) |
| chain-id | String | Chain ID of the network |

**Example**
```bash
gatecli distribution set-withdraw-addr gt11twm7dma44k7wg5jppeyphrct9nx2l4m8szy44h72qv9eatyla3hkaevg3vx99mlslwsnfq --from gt11twm7dma44k7wg5jppeyphrct9nx2l4m8szy44h72qv9eatyla3hkaevg3vx99mlslwsnfq --fees 100000NANOGT --chain-id testnet
```

**Response Example**
```
TxHash: BASIC-9F685A8362E6218E372CE60E306E8BC35B66006D82F9B3381A6AECE26FA6355CA38CD75AFFDF597794159D9356BE0376
Data: rQO5zc/tCu8BYPD/ggoo3TuBsrqFDCGccbnTERbiO5v48wnG3ahDuFWSzrdVLzU0iuZQw2rzDBIoHk1VTbZ0J94UnjHi3aO8fwO1V5rK5I2NZvxNF1lFstSU9JD3J18JbxqUAWd0MXB1YjE4cTJmZ3VnZ3F5Znp0YzBwNXJhenEwZnRwdXplNzJwOXRwN25lZ2plZTl6amtjaGx2MHFwNThyZTdyZGduajNqd2x3d3JscjN6Z2o3cmNkcWxnc2Z3Y2V2YWRqaGE0ZXZoOThkejdzN3pjYzh5MHZhZnY3amh1ajNobXR1M2ZtajM2eXdqZWNtbnF1OWgSEgoMCgZOQU5PR1QSAjExEMCaDBowMXrG9msevrtuVTHWuZdFIixl5hSO4tWOvIZV01T/p+Pbg1sPeBgWGHbKUcm1064KImkKJeHhoPogZ32xdJvDkmTqENs7tchCbHrQ1z1n7Eeh1/ud9weWADUSQJRr9hYE0jvDKTx9IsfYAh3myFPQaYV9pt+TEi+IKdFm2KOZGYckVEbFx9ydMn2F6UbhopD5Y5HbrKJzf0fF9woyBNcEqQY=
Raw Log: sync broadcast tx success
```

**Note**: The vault account can only withdraw the income and principal to this account and cannot be set up separately.

### 2. Delegator Account Fetch Partial Income From A Consensus Account

**Command**
```bash
gatecli distribution withdraw-rewards [consensus account address] --from [sender account] --fees [tx fees] --chain-id [chain ID]
```

**Parameters**

| Parameter | Type | Description |
|------|------|------|
| consensus account address | String | Address of the consensus account |
| sender account | String | Account address of the sender |
| tx fees | String | Transaction fees (e.g., 100000NANOGT) |
| chain-id | String | Chain ID of the network |

**Example**
```bash
gatecli distribution withdraw-rewards gt11twm7dma44k7wg5jppeyphrct9nx2l4m8szy44h72qv9eatyla3hkaevg3vx99mlslwsnfq --from gt11twm7dma44k7wg5jppeyphrct9nx2l4m8szy44h72qv9eatyla3hkaevg3vx99mlslwsnfq --fees 100000NANOGT --chain-id testnet
```

**Response Example**
```
  TxHash: BASIC-9F685A8362E6218E372CE60E306E8BC35B66006D82F9B3381A6AECE26FA6355CA38CD75AFFDF597794159D9356BE0376 
  //transaction hash, using gatecli tx show {hash}to query details of this transaction
  Data: rQO5zc/tCu8BYPD/ggoo3TuBsrqFDCGccbnTERbiO5v48wnG3ahDuFWSzrdVLzU0iuZQw2rzDBIoHk1VTbZ0J94UnjHi3aO8fwO1V5rK5I2NZvxNF1lFstSU9JD3J18JbxqUAWd0MXB1YjE4cTJmZ3VnZ3F5Znp0YzBwNXJhenEwZnRwdXplNzJwOXRwN25lZ2plZTl6amtjaGx2MHFwNThyZTdyZGduajNqd2x3d3JscjN6Z2o3cmNkcWxnc2Z3Y2V2YWRqaGE0ZXZoOThkejdzN3pjYzh5MHZhZnY3amh1ajNobXR1M2ZtajM2eXdqZWNtbnF1OWgSEgoMCgZOQU5PR1QSAjExEMCaDBowMXrG9msevrtuVTHWuZdFIixl5hSO4tWOvIZV01T/p+Pbg1sPeBgWGHbKUcm1064KImkKJeHhoPogZ32xdJvDkmTqENs7tchCbHrQ1z1n7Eeh1/ud9weWADUSQJRr9hYE0jvDKTx9IsfYAh3myFPQaYV9pt+TEi+IKdFm2KOZGYckVEbFx9ydMn2F6UbhopD5Y5HbrKJzf0fF9woyBNcEqQY=
  Raw Log: sync broadcast tx success transaction is sent successfully
```

### 3. Delegator Account Fetch All Income From A Consensus Account

**Command**
```bash
gatecli distribution withdraw-all-rewards --from [sender account] --fees [tx fees] --chain-id [chain ID]
```

**Parameters**

| Parameter | Type | Description |
|------|------|------|
| sender account | String | Account address of the sender |
| tx fees | String | Transaction fees (e.g., 100000NANOGT) |
| chain-id | String | Chain ID of the network |

**Example**
```bash
gatecli distribution withdraw-all-rewards --from gt11twm7dma44k7wg5jppeyphrct9nx2l4m8szy44h72qv9eatyla3hkaevg3vx99mlslwsnfq --fees 100000NANOGT --chain-id testnet
```

**Response Example**
```
  TxHash: BASIC-9F685A8362E6218E372CE60E306E8BC35B66006D82F9B3381A6AECE26FA6355CA38CD75AFFDF597794159D9356BE0376
  //transaction hash, using gatecli tx show {hash}to query details of this transaction
  Data: rQO5zc/tCu8BYPD/ggoo3TuBsrqFDCGccbnTERbiO5v48wnG3ahDuFWSzrdVLzU0iuZQw2rzDBIoHk1VTbZ0J94UnjHi3aO8fwO1V5rK5I2NZvxNF1lFstSU9JD3J18JbxqUAWd0MXB1YjE4cTJmZ3VnZ3F5Znp0YzBwNXJhenEwZnRwdXplNzJwOXRwN25lZ2plZTl6amtjaGx2MHFwNThyZTdyZGduajNqd2x3d3JscjN6Z2o3cmNkcWxnc2Z3Y2V2YWRqaGE0ZXZoOThkejdzN3pjYzh5MHZhZnY3amh1ajNobXR1M2ZtajM2eXdqZWNtbnF1OWgSEgoMCgZOQU5PR1QSAjExEMCaDBowMXrG9msevrtuVTHWuZdFIixl5hSO4tWOvIZV01T/p+Pbg1sPeBgWGHbKUcm1064KImkKJeHhoPogZ32xdJvDkmTqENs7tchCbHrQ1z1n7Eeh1/ud9weWADUSQJRr9hYE0jvDKTx9IsfYAh3myFPQaYV9pt+TEi+IKdFm2KOZGYckVEbFx9ydMn2F6UbhopD5Y5HbrKJzf0fF9woyBNcEqQY=
  Raw Log: sync broadcast tx success transaction is sent successfully
```
### 4. Reinvestment the rewards of the delegator account into the consensus account

**Command**
```bash
gatecli distribution reward-reinvestment [consensus account address] --from [sender account] --fees [tx fees] --chain-id [chain ID]
```

**Parameters**

| Parameter | Type | Description |
|------|------|------|
| consensus account address | String | Address of the consensus account |
| sender account | String | Account address of the sender |
| tx fees | String | Transaction fees (e.g., 100000NANOGT) |
| chain-id | String | Chain ID of the network |

**Example**
```bash
gatecli distribution reward-reinvestment gt11rjq598t8vte64ff2tnesdvsfazv38atpenufj4zl0ljhw2q28jnlxnh6mqgmn6elje6fzc --from gt11mtehamaw8wktwpppx9klrhhfu5upmutzthl0kkpwsa0slw8h4xd3p2ane2zt262dlsyy3m --fees 100000NANOGT --chain-id testnet
```

**Response Example**
```
  TxHash: BASIC-9F685A8362E6218E372CE60E306E8BC35B66006D82F9B3381A6AECE26FA6355CA38CD75AFFDF597794159D9356BE0376
  //transaction hash, using gatecli tx show {hash}to query details of this transaction
  Data: rQO5zc/tCu8BYPD/ggoo3TuBsrqFDCGccbnTERbiO5v48wnG3ahDuFWSzrdVLzU0iuZQw2rzDBIoHk1VTbZ0J94UnjHi3aO8fwO1V5rK5I2NZvxNF1lFstSU9JD3J18JbxqUAWd0MXB1YjE4cTJmZ3VnZ3F5Znp0YzBwNXJhenEwZnRwdXplNzJwOXRwN25lZ2plZTl6amtjaGx2MHFwNThyZTdyZGduajNqd2x3d3JscjN6Z2o3cmNkcWxnc2Z3Y2V2YWRqaGE0ZXZoOThkejdzN3pjYzh5MHZhZnY3amh1ajNobXR1M2ZtajM2eXdqZWNtbnF1OWgSEgoMCgZOQU5PR1QSAjExEMCaDBowMXrG9msevrtuVTHWuZdFIixl5hSO4tWOvIZV01T/p+Pbg1sPeBgWGHbKUcm1064KImkKJeHhoPogZ32xdJvDkmTqENs7tchCbHrQ1z1n7Eeh1/ud9weWADUSQJRr9hYE0jvDKTx9IsfYAh3myFPQaYV9pt+TEi+IKdFm2KOZGYckVEbFx9ydMn2F6UbhopD5Y5HbrKJzf0fF9woyBNcEqQY=
  Raw Log: sync broadcast tx success transaction is sent successfully
```
### 5. Query Delegation Income of A Delegator Account At A Consensus Account

**Command**
```bash
gatecli distribution rewards [delegator account address] [consensus account address] --chain-id [chain ID]
```

**Parameters**

| Parameter | Type | Description |
|------|------|------|
| delegator account address | String | Address of the delegator account |
| consensus account address | String | Address of the consensus account |
| chain-id | String | Chain ID of the network |

**Example**
```bash
gatecli distribution rewards gt11twm7dma44k7wg5jppeyphrct9nx2l4m8szy44h72qv9eatyla3hkaevg3vx99mlslwsnfq gt11twm7dma44k7wg5jppeyphrct9nx2l4m8szy44h72qv9eatyla3hkaevg3vx99mlslwsnfq --chain-id testnet
```

**Response Example**
```
Delegator Total Rewards:
  Rewards:
	Con-account: gt11dsck7f7txdc7hz5wdsspey0w5eyhw7yj4xrc3a6r4w4t39z9k45r86htlzxjf6z57an2r7 // consensus account address
	Reward: 21.644616251838158370NANOGT //commission and mining earnings
```
### 6. Query Delegation Income of A Delegator Account At All Consensus Account

**Command**
```bash
gatecli distribution rewards [delegator account address] --chain-id [chain ID]
```

**Parameters**

| Parameter | Type | Description |
|------|------|------|
| delegator account address | String | Address of the delegator account |
| chain-id | String | Chain ID of the network |

**Example**
```bash
gatecli distribution rewards gt11twm7dma44k7wg5jppeyphrct9nx2l4m8szy44h72qv9eatyla3hkaevg3vx99mlslwsnfq --chain-id testnet
```

**Response Example**
```
Delegator Total Rewards:
  Rewards:
	Con-account: gt11dsck7f7txdc7hz5wdsspey0w5eyhw7yj4xrc3a6r4w4t39z9k45r86htlzxjf6z57an2r7 // consensus account address
	Reward: 21.644616251838158370NANOGT //commission and mining earnings
	Con-account:...
Total: 11997269037.266215577925788000NANOGT //total delegation income of a delegator account
```
### 7. Query Delegation Income Pending Paying By A Consensus Account

**Command**
```bash
gatecli distribution outstanding-rewards [consensus account address] --chain-id [chain ID]
```

**Parameters**

| Parameter | Type | Description |
|------|------|------|
| consensus account address | String | Address of the consensus account |
| chain-id | String | Chain ID of the network |

**Example**
```bash
gatecli distribution outstanding-rewards gt11twm7dma44k7wg5jppeyphrct9nx2l4m8szy44h72qv9eatyla3hkaevg3vx99mlslwsnfq --chain-id testnet
```

**Response Example**
```
660467079334.408199257068561630NANOGT //delegation income yet to be paid
```

### 8. Query Consensus Account Income

**Command**
```bash
gatecli distribution commission [consensus account address] --chain-id [chain ID]
```

**Parameters**

| Parameter | Type | Description |
|------|------|------|
| consensus account address | String | Address of the consensus account |
| chain-id | String | Chain ID of the network |

**Example**
```bash
gatecli distribution commission gt11twm7dma44k7wg5jppeyphrct9nx2l4m8szy44h72qv9eatyla3hkaevg3vx99mlslwsnfq --chain-id testnet
```

**Response Example**
```
660467079334.408199257068561630NANOGT //commission and mining earnings
```

### 9. Query Distribution And Foundation Parameters

**Command**
```bash
gatecli distribution params --chain-id [chain ID]
```

**Parameters**

| Parameter | Type | Description |
|------|------|------|
| chain-id | String | Chain ID of the network |

**Response Example**
```
Distribution Params:
  Community Tax:          "0.020000000000000000" //Community Tax rate
  Withdraw Addr Enabled:  true
  First CommitteeReward:  "0.400000000000000000" //The first Committee mining reward rate
  Second CommitteeReward:  "0.350000000000000000" //The second Committee mining reward rate
  Third CommitteeReward:  "0.250000000000000000" //The third Committee mining reward rate
```


## Revocable Transaction Management

### 1. Send Revocable Transaction

**Command**
```bash
gatecli revocable-tx send [recipient account] [transfer token amount] --from [sender account] --fees [tx fees] --chain-id [chain ID]
```

**Parameters**

| Parameter | Type | Description |
|------|------|------|
| recipient account | String | Account address of the recipient |
| transfer token amount | String | Amount of tokens to transfer |
| sender account | String | Account address of the sender |
| tx fees | String | Transaction fees (e.g., 100000NANOGT) |
| chain-id | String | Chain ID of the network |

**Example**
```bash
gatecli revocable-tx send gt11twm7dma44k7wg5jppeyphrct9nx2l4m8szy44h72qv9eatyla3hkaevg3vx99mlslwsnfq 10NANOGT --from vault11fg056uaatk9s3k6l34eqkc7qchjup8e39afauf9naz4jmpklsze3uwx6405vll88l5lvww --fees 100000NANOGT --chain-id testnet
```

**Response Example**
```
TxHash: REVOCABLEPAY-0FD8A17798EC2CC637252687CA7DACA39BE0C555496EC3242F90C7C0BBBFE5F10A34A51D866BEC91A9866275BED9B522
Data: rQO5zc/tCu8BYPD/ggoo3TuBsrqFDCGccbnTERbiO5v48wnG3ahDuFWSzrdVLzU0iuZQw2rzDBIoHk1VTbZ0J94UnjHi3aO8fwO1V5rK5I2NZvxNF1lFstSU9JD3J18JbxqUAWd0MXB1YjE4cTJmZ3VnZ3F5Znp0YzBwNXJhenEwZnRwdXplNzJwOXRwN25lZ2plZTl6amtjaGx2MHFwNThyZTdyZGduajNqd2x3d3JscjN6Z2o3cmNkcWxnc2Z3Y2V2YWRqaGE0ZXZoOThkejdzN3pjYzh5MHZhZnY3amh1ajNobXR1M2ZtajM2eXdqZWNtbnF1OWgSEgoMCgZOQU5PR1QSAjExEMCaDBowMXrG9msevrtuVTHWuZdFIixl5hSO4tWOvIZV01T/p+Pbg1sPeBgWGHbKUcm1064KImkKJeHhoPogZ32xdJvDkmTqENs7tchCbHrQ1z1n7Eeh1/ud9weWADUSQJRr9hYE0jvDKTx9IsfYAh3myFPQaYV9pt+TEi+IKdFm2KOZGYckVEbFx9ydMn2F6UbhopD5Y5HbrKJzf0fF9woyBNcEqQY=
Raw Log: sync broadcast tx success
```

**Notes**
- Only Vault Account can send a Revocable Transaction

### 2. Query Single Transaction

**Command**
```bash
gatecli revocable-tx show [transaction Hash] --chain-id [chain ID]
```

**Parameters**

| Parameter | Type | Description |
|------|------|------|
| transaction Hash | String | Hash of the transaction to query |
| chain-id | String | Chain ID of the network |

**Example**
```bash
gatecli revocable-tx show 03190D3F56D6B65CC11BFE3F9CD961729B134D224A35AC731728601C9DD3A3C7 --chain-id testnet
```

**Response Example**
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

### 3. Query Revocable Transaction List

**Command**
```bash
gatecli revocable-tx list [Vault Account] --chain-id [chain ID]
```

**Parameters**

| Parameter | Type | Description |
|------|------|------|
| Vault Account | String | Address of the Vault Account |
| chain-id | String | Chain ID of the network |

**Example**
```bash
gatecli revocable-tx list vault11fg056uaatk9s3k6l34eqkc7qchjup8e39afauf9naz4jmpklsze3uwx6405vll88l5lvww --chain-id testnet
```

**Response Example**
```
Txs: count  1
TxHash:         REVOCABLEPAY-082A18896CD1397563569D939C884D446B303865012C750ACB40E3911CF8FD69144F836537C97B77EFF473776D7889F5
Index:          0
Height:         873
Tokens:          100NANOGT
```

### 4. Revoke Revocable Transaction

**Command**
```bash
gatecli revocable-tx revoke [transaction Hash] --from [sender account] --fees [tx fees] --chain-id [chain ID]
```

**Parameters**

| Parameter | Type | Description |
|------|------|------|
| transaction Hash | String | Hash of the transaction to revoke |
| sender account | String | Account address of the sender |
| tx fees | String | Transaction fees (e.g., 100000NANOGT) |
| chain-id | String | Chain ID of the network |

**Example**
```bash
gatecli revocable-tx revoke 0E3B67C685C271632CE6F4DAA2AB06AF7E8077509E1CB5310F63F6C147786E12 --from vault11fg056uaatk9s3k6l34eqkc7qchjup8e39afauf9naz4jmpklsze3uwx6405vll88l5lvww --fees 100000NANOGT --chain-id testnet
```

**Response Example**
```
TxHash: REVOKE-9F685A8362E6218E372CE60E306E8BC35B66006D82F9B3381A6AECE26FA6355CA38CD75AFFDF597794159D9356BE0376
Data: rQO5zc/tCu8BYPD/ggoo3TuBsrqFDCGccbnTERbiO5v48wnG3ahDuFWSzrdVLzU0iuZQw2rzDBIoHk1VTbZ0J94UnjHi3aO8fwO1V5rK5I2NZvxNF1lFstSU9JD3J18JbxqUAWd0MXB1YjE4cTJmZ3VnZ3F5Znp0YzBwNXJhenEwZnRwdXplNzJwOXRwN25lZ2plZTl6amtjaGx2MHFwNThyZTdyZGduajNqd2x3d3JscjN6Z2o3cmNkcWxnc2Z3Y2V2YWRqaGE0ZXZoOThkejdzN3pjYzh5MHZhZnY3amh1ajNobXR1M2ZtajM2eXdqZWNtbnF1OWgSEgoMCgZOQU5PR1QSAjExEMCaDBowMXrG9msevrtuVTHWuZdFIixl5hSO4tWOvIZV01T/p+Pbg1sPeBgWGHbKUcm1064KImkKJeHhoPogZ32xdJvDkmTqENs7tchCbHrQ1z1n7Eeh1/ud9weWADUSQJRr9hYE0jvDKTx9IsfYAh3myFPQaYV9pt+TEi+IKdFm2KOZGYckVEbFx9ydMn2F6UbhopD5Y5HbrKJzf0fF9woyBNcEqQY=
Raw Log: sync broadcast tx success
```

## Delegation Management

### 1. Delegate Token To Consensus Account

**Command**
```bash
gatecli staking delegate [consensus account address] [amount] --from [sender account] --fees [tx fees] --chain-id [chain ID]
```

**Parameters**

| Parameter | Type | Description |
|------|------|------|
| consensus account address | String | Address of the consensus account to delegate to |
| amount | String | Amount of tokens to delegate |
| sender account | String | Account address of the sender |
| tx fees | String | Transaction fees (e.g., 100000NANOGT) |
| chain-id | String | Chain ID of the network |

**Example**
```bash
gatecli staking delegate gt11twm7dma44k7wg5jppeyphrct9nx2l4m8szy44h72qv9eatyla3hkaevg3vx99mlslwsnfq 100000NANOGT --from gt11twm7dma44k7wg5jppeyphrct9nx2l4m8szy44h72qv9eatyla3hkaevg3vx99mlslwsnfq --fees 100000NANOGT --chain-id testnet
```

**Response Example**
```
TxHash: DELEGATE-9F685A8362E6218E372CE60E306E8BC35B66006D82F9B3381A6AECE26FA6355CA38CD75AFFDF597794159D9356BE0376
Data: rQO5zc/tCu8BYPD/ggoo3TuBsrqFDCGccbnTERbiO5v48wnG3ahDuFWSzrdVLzU0iuZQw2rzDBIoHk1VTbZ0J94UnjHi3aO8fwO1V5rK5I2NZvxNF1lFstSU9JD3J18JbxqUAWd0MXB1YjE4cTJmZ3VnZ3F5Znp0YzBwNXJhenEwZnRwdXplNzJwOXRwN25lZ2plZTl6amtjaGx2MHFwNThyZTdyZGduajNqd2x3d3JscjN6Z2o3cmNkcWxnc2Z3Y2V2YWRqaGE0ZXZoOThkejdzN3pjYzh5MHZhZnY3amh1ajNobXR1M2ZtajM2eXdqZWNtbnF1OWgSEgoMCgZOQU5PR1QSAjExEMCaDBowMXrG9msevrtuVTHWuZdFIixl5hSO4tWOvIZV01T/p+Pbg1sPeBgWGHbKUcm1064KImkKJeHhoPogZ32xdJvDkmTqENs7tchCbHrQ1z1n7Eeh1/ud9weWADUSQJRr9hYE0jvDKTx9IsfYAh3myFPQaYV9pt+TEi+IKdFm2KOZGYckVEbFx9ydMn2F6UbhopD5Y5HbrKJzf0fF9woyBNcEqQY=
Raw Log: sync broadcast tx success
```
### 2. Query Delegation Information of A Delegator Account In A Consensus Account

**Command**
```bash
gatecli staking delegation [delegator account address] [consensus account address] --chain-id [chain ID]
```

**Parameters**

| Parameter | Type | Description |
|------|------|------|
| delegator account address | String | Address of the delegator account |
| consensus account address | String | Address of the consensus account |
| chain-id | String | Chain ID of the network |

**Example**
```bash
gatecli staking delegation gt11twm7dma44k7wg5jppeyphrct9nx2l4m8szy44h72qv9eatyla3hkaevg3vx99mlslwsnfq gt11dsck7f7txdc7hz5wdsspey0w5eyhw7yj4xrc3a6r4w4t39z9k45r86htlzxjf6z57an2r7 --chain-id testnet
```

**Response Example**
```
Delegation:
  Delegator: gt11twm7dma44k7wg5jppeyphrct9nx2l4m8szy44h72qv9eatyla3hkaevg3vx99mlslwsnfq
  Con-account: gt11dsck7f7txdc7hz5wdsspey0w5eyhw7yj4xrc3a6r4w4t39z9k45r86htlzxjf6z57an2r7
  Shares: 100000000.000000000000000000
```
### 3. Query Delegation Information of A Delegator Account In All Consensus Accounts 

**Command**
```bash
gatecli staking delegations [delegator account address] --chain-id [chain ID]
```

**Parameters**

| Parameter | Type | Description |
|------|------|------|
| delegator account address | String | Address of the delegator account |
| chain-id | String | Chain ID of the network |

**Example**
```bash
gatecli staking delegations gt11twm7dma44k7wg5jppeyphrct9nx2l4m8szy44h72qv9eatyla3hkaevg3vx99mlslwsnfq --chain-id testnet
```

**Response Example**
```
Delegation:
  Delegator: gt11twm7dma44k7wg5jppeyphrct9nx2l4m8szy44h72qv9eatyla3hkaevg3vx99mlslwsnfq
  Con-account: gt11dsck7f7txdc7hz5wdsspey0w5eyhw7yj4xrc3a6r4w4t39z9k45r86htlzxjf6z57an2r7
  Shares: 100000000.000000000000000000
Delegation:
  ...
```

### 4. Shift Delegation

**Command**
```bash
gatecli staking redelegate [source consensus account address] [target consensus account address] [delegation token amount ] --from [delegator account address] --fees [tx fees] --chain-id [chain ID]
```

**Parameters**

| Parameter | Type | Description |
|------|------|------|
| source consensus account address | String | Address of the source consensus account |
| target consensus account address | String | Address of the target consensus account |
| delegation token amount | String | Amount of tokens to delegate |
| delegator account address | String | Address of the delegator account |
| tx fees | String | Transaction fees (e.g., 100000NANOGT) |
| chain-id | String | Chain ID of the network |

**Example**
```bash
gatecli staking redelegate gt11m5acrv46s5xzr8r3h8f3z9hz8wdl3ucfcmw6ssac2kfvad649u6nfzhx2rpk4ucvrxla6s gt11m5acrv46s5xzr8r3h8f3z9hz8wdl3ucfcmw6ssac2kfvad649u6nfzhx2rpk4ucvrxla54 100000000NANOGT --from gt11m5acrv46s5xzr8r3h8f3z9hz8wdl3ucfcmw6ssac2kfvad649u6nfzhx2rpk4ucvrxla6s --fees 100000NANOGT --chain-id testnet
```

**Response Example**
```
 TxHash: BASIC-9F685A8362E6218E372CE60E306E8BC35B66006D82F9B3381A6AECE26FA6355CA38CD75AFFDF597794159D9356BE0376 
  //Transaction hash，using gatecli tx show {hash}to query details of this transaction.
  Data: rQO5zc/tCu8BYPD/ggoo3TuBsrqFDCGccbnTERbiO5v48wnG3ahDuFWSzrdVLzU0iuZQw2rzDBIoHk1VTbZ0J94UnjHi3aO8fwO1V5rK5I2NZvxNF1lFstSU9JD3J18JbxqUAWd0MXB1YjE4cTJmZ3VnZ3F5Znp0YzBwNXJhenEwZnRwdXplNzJwOXRwN25lZ2plZTl6amtjaGx2MHFwNThyZTdyZGduajNqd2x3d3JscjN6Z2o3cmNkcWxnc2Z3Y2V2YWRqaGE0ZXZoOThkejdzN3pjYzh5MHZhZnY3amh1ajNobXR1M2ZtajM2eXdqZWNtbnF1OWgSEgoMCgZOQU5PR1QSAjExEMCaDBowMXrG9msevrtuVTHWuZdFIixl5hSO4tWOvIZV01T/p+Pbg1sPeBgWGHbKUcm1064KImkKJeHhoPogZ32xdJvDkmTqENs7tchCbHrQ1z1n7Eeh1/ud9weWADUSQJRr9hYE0jvDKTx9IsfYAh3myFPQaYV9pt+TEi+IKdFm2KOZGYckVEbFx9ydMn2F6UbhopD5Y5HbrKJzf0fF9woyBNcEqQY=
  Raw Log: sync broadcast tx success //transaction is sent successfully
```

### 5. Query All Delegation Shifts of A Specific Delegator Account

**Command**
```bash
gatecli staking redelegations [delegator account address] --chain-id [chain ID]
```

**Parameters**

| Parameter | Type | Description |
|------|------|------|
| delegator account address | String | Address of the delegator account |
| chain-id | String | Chain ID of the network |

**Example**
```bash
gatecli staking redelegations gt11p8qmx5q8h7h3es3wl4x0y4efgme552hk7x5g6ppeelel2v2vvsthxk0ce65gw9mfls9ugp --chain-id testnet
```

**Response Example**
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

### 7. Query Delegation Shifts Between Two Consensus Accounts For A Delegator Account

**Command**
```bash
gatecli staking redelegation [delegator account address] [source consensus account address] [target consensus account address] --chain-id [chain ID]
```

**Parameters**

| Parameter | Type | Description |
|------|------|------|
| delegator account address | String | Address of the delegator account |
| source consensus account address | String | Address of the source consensus account |
| target consensus account address | String | Address of the target consensus account |
| chain-id | String | Chain ID of the network |

**Example**
```bash
gatecli staking redelegation gt11m5acrv46s5xzr8r3h8f3z9hz8wdl3ucfcmw6ssac2kfvad649u6nfzhx2rpk4ucvrxla6s gt11dsck7f7txdc7hz5wdsspey0w5eyhw7yj4xrc3a6r4w4t39z9k45r86htlzxjf6z57an2r7 gt11ewdeyjdlxs2x4yglnhtulp0tclgdp7c2t8d5c6495r83qvrfr7p0qt40uddu3k44s7rxg6 --chain-id testnet
```

**Response Example**
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

### 8.Undelegate From A Consensus Account

**Command**
```bash
gatecli staking undelegate [consensus account address] [amount] --from [sender account] --fees [tx fees] --chain-id [chain ID]
```

**Parameters**

| Parameter | Type | Description |
|------|------|------|
| consensus account address | String | Address of the consensus account to undelegate from |
| amount | String | Amount of tokens to undelegate |
| sender account | String | Account address of the sender |
| tx fees | String | Transaction fees (e.g., 100000NANOGT) |
| chain-id | String | Chain ID of the network |

**Example**
```bash
gatecli staking undelegate gt11twm7dma44k7wg5jppeyphrct9nx2l4m8szy44h72qv9eatyla3hkaevg3vx99mlslwsnfq 100000NANOGT --from gt11twm7dma44k7wg5jppeyphrct9nx2l4m8szy44h72qv9eatyla3hkaevg3vx99mlslwsnfq --fees 100000NANOGT --chain-id testnet
```

**Response Example**
```
TxHash: UNDELEGATE-9F685A8362E6218E372CE60E306E8BC35B66006D82F9B3381A6AECE26FA6355CA38CD75AFFDF597794159D9356BE0376
Data: rQO5zc/tCu8BYPD/ggoo3TuBsrqFDCGccbnTERbiO5v48wnG3ahDuFWSzrdVLzU0iuZQw2rzDBIoHk1VTbZ0J94UnjHi3aO8fwO1V5rK5I2NZvxNF1lFstSU9JD3J18JbxqUAWd0MXB1YjE4cTJmZ3VnZ3F5Znp0YzBwNXJhenEwZnRwdXplNzJwOXRwN25lZ2plZTl6amtjaGx2MHFwNThyZTdyZGduajNqd2x3d3JscjN6Z2o3cmNkcWxnc2Z3Y2V2YWRqaGE0ZXZoOThkejdzN3pjYzh5MHZhZnY3amh1ajNobXR1M2ZtajM2eXdqZWNtbnF1OWgSEgoMCgZOQU5PR1QSAjExEMCaDBowMXrG9msevrtuVTHWuZdFIixl5hSO4tWOvIZV01T/p+Pbg1sPeBgWGHbKUcm1064KImkKJeHhoPogZ32xdJvDkmTqENs7tchCbHrQ1z1n7Eeh1/ud9weWADUSQJRr9hYE0jvDKTx9IsfYAh3myFPQaYV9pt+TEi+IKdFm2KOZGYckVEbFx9ydMn2F6UbhopD5Y5HbrKJzf0fF9woyBNcEqQY=
Raw Log: sync broadcast tx success
```
### 9.Release of delegation through security account

**Command**
```bash
gatecli staking undelegate-by-retrieval-account [vault account1 vault account2...] --from [sender account] --fees [tx fees] --chain-id [chain ID]
```

**Parameters**

| Parameter | Type | Description |
|------|------|------|
| vault account1 vault account2... | String | Address of the vault account |
| sender account | String | Account address of the sender |
| tx fees | String | Transaction fees (e.g., 100000NANOGT) |
| chain-id | String | Chain ID of the network |

**Example**
```bash
gatecli staking undelegate-by-retrieval-account vault11d9t6... vault11w8c3v... --from gt11dsck7f7txdc7hz5wdsspey0w5eyhw7yj4xrc3a6r4w4t39z9k45r86htlzxjf6z57an2r7 --fees 100000NANOGT --chain-id testnet
```

**Response Example**
```
  TxHash: BASIC-9F685A8362E6218E372CE60E306E8BC35B66006D82F9B3381A6AECE26FA6355CA38CD75AFFDF597794159D9356BE0376 
  //Transaction hash,using gatecli tx show {hash}to query details of this transaction.
  Data: rQO5zc/tCu8BYPD/ggoo3TuBsrqFDCGccbnTERbiO5v48wnG3ahDuFWSzrdVLzU0iuZQw2rzDBIoHk1VTbZ0J94UnjHi3aO8fwO1V5rK5I2NZvxNF1lFstSU9JD3J18JbxqUAWd0MXB1YjE4cTJmZ3VnZ3F5Znp0YzBwNXJhenEwZnRwdXplNzJwOXRwN25lZ2plZTl6amtjaGx2MHFwNThyZTdyZGduajNqd2x3d3JscjN6Z2o3cmNkcWxnc2Z3Y2V2YWRqaGE0ZXZoOThkejdzN3pjYzh5MHZhZnY3amh1ajNobXR1M2ZtajM2eXdqZWNtbnF1OWgSEgoMCgZOQU5PR1QSAjExEMCaDBowMXrG9msevrtuVTHWuZdFIixl5hSO4tWOvIZV01T/p+Pbg1sPeBgWGHbKUcm1064KImkKJeHhoPogZ32xdJvDkmTqENs7tchCbHrQ1z1n7Eeh1/ud9weWADUSQJRr9hYE0jvDKTx9IsfYAh3myFPQaYV9pt+TEi+IKdFm2KOZGYckVEbFx9ydMn2F6UbhopD5Y5HbrKJzf0fF9woyBNcEqQY=
  Raw Log: sync broadcast tx success //transaction is sent successfully
```
### 10.Query Undelegations of A Delegator Account in A consensus Account

**Command**
```bash
gatecli staking undelegation [delegator account address] [consensus account address] --chain-id [chain ID]
```

**Parameters**

| Parameter | Type | Description |
|------|------|------|
| delegator account address | String | Address of the delegator account |
| consensus account address | String | Address of the consensus account |
| chain-id | String | Chain ID of the network |

**Example**
```bash
gatecli staking undelegation gt11m5acrv46s5xzr8r3h8f3z9hz8wdl3ucfcmw6ssac2kfvad649u6nfzhx2rpk4ucvrxla6s gt11ewdeyjdlxs2x4yglnhtulp0tclgdp7c2t8d5c6495r83qvrfr7p0qt40uddu3k44s7rxg6 --chain-id testnet
```

**Response Example**
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

### 11.Query Undelegations of A Delegator Account in All consensus Accounts 

**Command**
```bash
gatecli staking undelegations [delegator account address] --chain-id [chain ID]
```

**Parameters**

| Parameter | Type | Description |
|------|------|------|
| delegator account address | String | Address of the delegator account |
| chain-id | String | Chain ID of the network |

**Example**
```bash
gatecli staking undelegations gt11m5acrv46s5xzr8r3h8f3z9hz8wdl3ucfcmw6ssac2kfvad649u6nfzhx2rpk4ucvrxla6s --chain-id testnet
```

**Response Example**
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

### 12.Query All Delegations Of A Specific Consensus Account

**Command**
```bash
gatecli staking delegations-to [consensus account address] --chain-id [chain ID]
```

**Parameters**

| Parameter | Type | Description |
|------|------|------|
| consensus account address | String | Address of the consensus account |
| chain-id | String | Chain ID of the network |

**Example**
```bash
gatecli staking delegations-to gt11m5acrv46s5xzr8r3h8f3z9hz8wdl3ucfcmw6ssac2kfvad649u6nfzhx2rpk4ucvrxla6s --chain-id testnet
```

**Response Example**
```
Delegation:
  Delegator:   gt11m5acrv46s5xzr8r3h8f3z9hz8wdl3ucfcmw6ssac2kfvad649u6nfzhx2rpk4ucvrxla6s //delegator account address
  Con-account: gt11ewdeyjdlxs2x4yglnhtulp0tclgdp7c2t8d5c6495r83qvrfr7p0qt40uddu3k44s7rxg6 //consensus account address
  Shares:      1000.000000000000000000 //delegation quantity
  Balance:   1000 //delegation token amount 
	...
```

### 13.Query All Delegation Shifts For A Specific Consensus Account 

**Command**
```bash
gatecli staking redelegations-from [consensus account address] --chain-id [chain ID]
```

**Parameters**

| Parameter | Type | Description |
|------|------|------|
| consensus account address | String | Address of the consensus account |
| chain-id | String | Chain ID of the network |

**Example**
```bash
gatecli staking redelegations-from gt11dsck7f7txdc7hz5wdsspey0w5eyhw7yj4xrc3a6r4w4t39z9k45r86htlzxjf6z57an2r7 --chain-id testnet
```

**Response Example**
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

### 14.Query All Undelegations Of A Specific Consensus Account

**Command**
```bash
gatecli staking undelegations-from  [consensus account address] --chain-id [chain ID]
```

**Parameters**

| Parameter | Type | Description |
|------|------|------|
| consensus account address | String | Address of the consensus account |
| chain-id | String | Chain ID of the network |

**Example**
```bash
gatecli staking undelegations-from gt11dsck7f7txdc7hz5wdsspey0w5eyhw7yj4xrc3a6r4w4t39z9k45r86htlzxjf6z57an2r7 --chain-id testnet
```

**Response Example**
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


### 15. Query Staking Pool

**Command**
```bash
gatecli staking pool --chain-id [chain ID]
```

**Parameters**

| Parameter | Type | Description |
|------|------|------|
| chain-id | String | Chain ID of the network |

**Response Example**
```
Pool:
  Not Bonded Tokens:  10000
  Bonded Tokens:      0
```

### 16. Query Staking Parameters

**Command**
```bash
gatecli staking params --chain-id [chain ID]
```

**Parameters**

| Parameter | Type | Description |
|------|------|------|
| chain-id | String | Chain ID of the network |

**Response Example**
```
Params:
  Undelegating Time:    504h0m0s
  Max Con-accounts:    100
  Max Entries:       7
  Bonded Coin Denom: NANOGT
  Pow Rate:          1
  Max Pow Rate:      2
```

## Foundation Management


### 1. Initialize

**Command**
```bash
gated add-foundation [config file path]
```

**Parameters**

| Parameter | Type | Description |
|------|------|------|
| config file path | String | Path to the configuration file |

**Example**
```bash
gated add-foundation foundation.json
```

**Response Example**
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

**Description**
- Foundation initialization must be executed when genesis block is initializing.
- max_members:indicates maximum members the foundation allows
- members:foundation member,address(member account),proportion(proportion held by the member)


### 2. Query Foundation Member List

**Command**
```bash
gatecli foundation members --chain-id [chain ID]
```

**Parameters**

| Parameter | Type | Description |
|------|------|------|
| chain-id | String | Chain ID of the network |

**Example**
```bash
gatecli foundation members --chain-id testnet
```

**Response Example**
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

### 3. Foundation Members Withdraw Tokens

**Command**
```bash
gatecli foundation withdraw [amount] --from [foundation members] --fees [tx fees] --chain-id [chain ID]
```

**Parameters**

| Parameter | Type | Description |
|------|------|------|
| amount | String | Amount of tokens to withdraw |
| foundation members | String | Address of the foundation member |
| tx fees | String | Transaction fees (e.g., 100000NANOGT) |
| chain-id | String | Chain ID of the network |

**Example**
```bash
gatecli foundation withdraw 10NANOGT --from gt11twm7dma44k7wg5jppeyphrct9nx2l4m8szy44h72qv9eatyla3hkaevg3vx99mlslwsnfq --fees 100000NANOGT --chain-id testnet
```

**Response Example**
```
TxHash: BASIC-9F685A8362E6218E372CE60E306E8BC35B66006D82F9B3381A6AECE26FA6355CA38CD75AFFDF597794159D9356BE0376
Data: rQO5zc/tCu8BYPD/ggoo3TuBsrqFDCGccbnTERbiO5v48wnG3ahDuFWSzrdVLzU0iuZQw2rzDBIoHk1VTbZ0J94UnjHi3aO8fwO1V5rK5I2NZvxNF1lFstSU9JD3J18JbxqUAWd0MXB1YjE4cTJmZ3VnZ3F5Znp0YzBwNXJhenEwZnRwdXplNzJwOXRwN25lZ2plZTl6amtjaGx2MHFwNThyZTdyZGduajNqd2x3d3JscjN6Z2o3cmNkcWxnc2Z3Y2V2YWRqaGE0ZXZoOThkejdzN3pjYzh5MHZhZnY3amh1ajNobXR1M2ZtajM2eXdqZWNtbnF1OWgSEgoMCgZOQU5PR1QSAjExEMCaDBowMXrG9msevrtuVTHWuZdFIixl5hSO4tWOvIZV01T/p+Pbg1sPeBgWGHbKUcm1064KImkKJeHhoPogZ32xdJvDkmTqENs7tchCbHrQ1z1n7Eeh1/ud9weWADUSQJRr9hYE0jvDKTx9IsfYAh3myFPQaYV9pt+TEi+IKdFm2KOZGYckVEbFx9ydMn2F6UbhopD5Y5HbrKJzf0fF9woyBNcEqQY=
Raw Log: sync broadcast tx success
```

## Token Management

### 1. Issue Token

**Command**
```bash
gatecli token issue [token symbol] [token amount] --from [sender account] --fees [tx fees] --chain-id [chain ID]
```

**Parameters**

| Parameter | Type | Description |
|------|------|------|
| token symbol | String | Symbol of the token to issue |
| token amount | String | Amount of tokens to issue |
| sender account | String | Account address of the sender |
| tx fees | String | Transaction fees (e.g., 100000NANOGT) |
| chain-id | String | Chain ID of the network |

**Example**
```bash
gatecli token issue "ABC" 100000000 --from gt11twm7dma44k7wg5jppeyphrct9nx2l4m8szy44h72qv9eatyla3hkaevg3vx99mlslwsnfq --fees 100000NANOGT --chain-id testnet
```

**Response Example**
```
TxHash: ISSUE-9F685A8362E6218E372CE60E306E8BC35B66006D82F9B3381A6AECE26FA6355CA38CD75AFFDF597794159D9356BE0376
Data: rQO5zc/tCu8BYPD/ggoo3TuBsrqFDCGccbnTERbiO5v48wnG3ahDuFWSzrdVLzU0iuZQw2rzDBIoHk1VTbZ0J94UnjHi3aO8fwO1V5rK5I2NZvxNF1lFstSU9JD3J18JbxqUAWd0MXB1YjE4cTJmZ3VnZ3F5Znp0YzBwNXJhenEwZnRwdXplNzJwOXRwN25lZ2plZTl6amtjaGx2MHFwNThyZTdyZGduajNqd2x3d3JscjN6Z2o3cmNkcWxnc2Z3Y2V2YWRqaGE0ZXZoOThkejdzN3pjYzh5MHZhZnY3amh1ajNobXR1M2ZtajM2eXdqZWNtbnF1OWgSEgoMCgZOQU5PR1QSAjExEMCaDBowMXrG9msevrtuVTHWuZdFIixl5hSO4tWOvIZV01T/p+Pbg1sPeBgWGHbKUcm1064KImkKJeHhoPogZ32xdJvDkmTqENs7tchCbHrQ1z1n7Eeh1/ud9weWADUSQJRr9hYE0jvDKTx9IsfYAh3myFPQaYV9pt+TEi+IKdFm2KOZGYckVEbFx9ydMn2F6UbhopD5Y5HbrKJzf0fF9woyBNcEqQY=
Raw Log: sync broadcast tx success
```

**Description**
- Token issuance incurs a fee of 200000000000NANOGT, please make sure you have adequate NANOGT token at account.
- Token symbol must be all in upper case, 2-15 characters long.
- In token creation, there are another two flags:
  - - '--mintable' whether the token can be additionally issued or not
  - -'--freezable' whether the token can be frozen or not


### 2.Issue Additional Token

**Command**
```bash
gatecli token mint [token amount] --from [sender account] --fees [tx fees] --chain-id [chain ID]
```

**Parameters**

| Parameter | Type | Description |
|------|------|------|
| token amount | String | Amount of tokens to issue |
| sender account | String | Account address of the sender |
| tx fees | String | Transaction fees (e.g., 100000NANOGT) |
| chain-id | String | Chain ID of the network |

**Example**
```bash
gatecli token mint 100000000000AAA-94f --from gt11dsck7f7txdc7hz5wdsspey0w5eyhw7yj4xrc3a6r4w4t39z9k45r86htlzxjf6z57an2r7 --fees 100000NANOGT --chain-id testnet
```
**Response Example**
```
  TxHash: BASIC-9F685A8362E6218E372CE60E306E8BC35B66006D82F9B3381A6AECE26FA6355CA38CD75AFFDF597794159D9356BE0376 
  //transaction hash, using gatecli tx show {hash}to query details of this transaction
  Data: rQO5zc/tCu8BYPD/ggoo3TuBsrqFDCGccbnTERbiO5v48wnG3ahDuFWSzrdVLzU0iuZQw2rzDBIoHk1VTbZ0J94UnjHi3aO8fwO1V5rK5I2NZvxNF1lFstSU9JD3J18JbxqUAWd0MXB1YjE4cTJmZ3VnZ3F5Znp0YzBwNXJhenEwZnRwdXplNzJwOXRwN25lZ2plZTl6amtjaGx2MHFwNThyZTdyZGduajNqd2x3d3JscjN6Z2o3cmNkcWxnc2Z3Y2V2YWRqaGE0ZXZoOThkejdzN3pjYzh5MHZhZnY3amh1ajNobXR1M2ZtajM2eXdqZWNtbnF1OWgSEgoMCgZOQU5PR1QSAjExEMCaDBowMXrG9msevrtuVTHWuZdFIixl5hSO4tWOvIZV01T/p+Pbg1sPeBgWGHbKUcm1064KImkKJeHhoPogZ32xdJvDkmTqENs7tchCbHrQ1z1n7Eeh1/ud9weWADUSQJRr9hYE0jvDKTx9IsfYAh3myFPQaYV9pt+TEi+IKdFm2KOZGYckVEbFx9ydMn2F6UbhopD5Y5HbrKJzf0fF9woyBNcEqQY=
  Raw Log: sync broadcast tx success //transaction is sent successfully
```

**Description**
- additional issued tokens must use parameter --mintable when issuing.
- The unit of token amount is the onchain token symbol（AAA-94f）
- The sum of additional issued tokens and the previously issued tokens should not exceed the default cap （10 billion）
  
### 3.Freeze Token

**Command**
```bash
gatecli token freeze [onchain token symbol] --from [sender account] --fees [tx fees] --chain-id [chain ID] 
```

**Parameters**

| Parameter | Type | Description |
|------|------|------|
| onchain token symbol | String | Symbol of the token to freeze |
| sender account | String | Account address of the sender |
| tx fees | String | Transaction fees (e.g., 100000NANOGT) |
| chain-id | String | Chain ID of the network |

**Example**
```bash
gatecli token freeze AAA-94f --from gt11dsck7f7txdc7hz5wdsspey0w5eyhw7yj4xrc3a6r4w4t39z9k45r86htlzxjf6z57an2r7 --fees 100000NANOGT --chain-id testnet
```
**Response Example**
```
 TxHash: BASIC-9F685A8362E6218E372CE60E306E8BC35B66006D82F9B3381A6AECE26FA6355CA38CD75AFFDF597794159D9356BE0376 
  //transaction hash, using gatecli tx show {hash}to query details of this transaction
  Data: rQO5zc/tCu8BYPD/ggoo3TuBsrqFDCGccbnTERbiO5v48wnG3ahDuFWSzrdVLzU0iuZQw2rzDBIoHk1VTbZ0J94UnjHi3aO8fwO1V5rK5I2NZvxNF1lFstSU9JD3J18JbxqUAWd0MXB1YjE4cTJmZ3VnZ3F5Znp0YzBwNXJhenEwZnRwdXplNzJwOXRwN25lZ2plZTl6amtjaGx2MHFwNThyZTdyZGduajNqd2x3d3JscjN6Z2o3cmNkcWxnc2Z3Y2V2YWRqaGE0ZXZoOThkejdzN3pjYzh5MHZhZnY3amh1ajNobXR1M2ZtajM2eXdqZWNtbnF1OWgSEgoMCgZOQU5PR1QSAjExEMCaDBowMXrG9msevrtuVTHWuZdFIixl5hSO4tWOvIZV01T/p+Pbg1sPeBgWGHbKUcm1064KImkKJeHhoPogZ32xdJvDkmTqENs7tchCbHrQ1z1n7Eeh1/ud9weWADUSQJRr9hYE0jvDKTx9IsfYAh3myFPQaYV9pt+TEi+IKdFm2KOZGYckVEbFx9ydMn2F6UbhopD5Y5HbrKJzf0fF9woyBNcEqQY=
  Raw Log: sync broadcast tx success //transaction is sent successfully
```

**Description**
- For token to freeze, parameter--freezable must be used when issuing.


### 4. Unfreeze Token

**Command**
```bash
gatecli token unfreeze [token amount] --from [sender account] --fees [tx fees] --chain-id [chain ID]
```

**Parameters**

| Parameter | Type | Description |
|------|------|------|
| token amount | String | Amount of tokens to unfreeze (e.g., 100000NANOGT) |
| sender account | String | Account address of the sender |
| tx fees | String | Transaction fees (e.g., 100000NANOGT) |
| chain-id | String | Chain ID of the network |

**Example**
```bash
gatecli token unfreeze 100000NANOGT --from gt11twm7dma44k7wg5jppeyphrct9nx2l4m8szy44h72qv9eatyla3hkaevg3vx99mlslwsnfq --fees 100000NANOGT --chain-id testnet
```

**Response Example**
```
TxHash: BASIC-9F685A8362E6218E372CE60E306E8BC35B66006D82F9B3381A6AECE26FA6355CA38CD75AFFDF597794159D9356BE0376
Data: rQO5zc/tCu8BYPD/ggoo3TuBsrqFDCGccbnTERbiO5v48wnG3ahDuFWSzrdVLzU0iuZQw2rzDBIoHk1VTbZ0J94UnjHi3aO8fwO1V5rK5I2NZvxNF1lFstSU9JD3J18JbxqUAWd0MXB1YjE4cTJmZ3VnZ3F5Znp0YzBwNXJhenEwZnRwdXplNzJwOXRwN25lZ2plZTl6amtjaGx2MHFwNThyZTdyZGduajNqd2x3d3JscjN6Z2o3cmNkcWxnc2Z3Y2V2YWRqaGE0ZXZoOThkejdzN3pjYzh5MHZhZnY3amh1ajNobXR1M2ZtajM2eXdqZWNtbnF1OWgSEgoMCgZOQU5PR1QSAjExEMCaDBowMXrG9msevrtuVTHWuZdFIixl5hSO4tWOvIZV01T/p+Pbg1sPeBgWGHbKUcm1064KImkKJeHhoPogZ32xdJvDkmTqENs7tchCbHrQ1z1n7Eeh1/ud9weWADUSQJRr9hYE0jvDKTx9IsfYAh3myFPQaYV9pt+TEi+IKdFm2KOZGYckVEbFx9ydMn2F6UbhopD5Y5HbrKJzf0fF9woyBNcEqQY=
Raw Log: sync broadcast tx success
```

**Notes**
- Token amount must be specified with the onchain token symbol (e.g., AAA-94f)


### 5. Burn Token

**Command**
```bash
gatecli token burn [token amount] --from [sender account] --fees [tx fees] --chain-id [chain ID] 
```

**Parameters**

| Parameter | Type | Description |
|------|------|------|
| token amount | String | Amount of tokens to unfreeze (e.g., 100000NANOGT) |
| sender account | String | Account address of the sender |
| tx fees | String | Transaction fees (e.g., 100000NANOGT) |
| chain-id | String | Chain ID of the network |

**Example**
```bash
gatecli token burn 100000000000AAA-94f  --from gt11dsck7f7txdc7hz5wdsspey0w5eyhw7yj4xrc3a6r4w4t39z9k45r86htlzxjf6z57an2r7 --fees 100000NANOGT --chain-id testnet
```

**Response Example**
```
TxHash: BASIC-9F685A8362E6218E372CE60E306E8BC35B66006D82F9B3381A6AECE26FA6355CA38CD75AFFDF597794159D9356BE0376 
//transaction hash, using gatecli tx show {hash}to query details of this transaction
Data: rQO5zc/tCu8BYPD/ggoo3TuBsrqFDCGccbnTERbiO5v48wnG3ahDuFWSzrdVLzU0iuZQw2rzDBIoHk1VTbZ0J94UnjHi3aO8fwO1V5rK5I2NZvxNF1lFstSU9JD3J18JbxqUAWd0MXB1YjE4cTJmZ3VnZ3F5Znp0YzBwNXJhenEwZnRwdXplNzJwOXRwN25lZ2plZTl6amtjaGx2MHFwNThyZTdyZGduajNqd2x3d3JscjN6Z2o3cmNkcWxnc2Z3Y2V2YWRqaGE0ZXZoOThkejdzN3pjYzh5MHZhZnY3amh1ajNobXR1M2ZtajM2eXdqZWNtbnF1OWgSEgoMCgZOQU5PR1QSAjExEMCaDBowMXrG9msevrtuVTHWuZdFIixl5hSO4tWOvIZV01T/p+Pbg1sPeBgWGHbKUcm1064KImkKJeHhoPogZ32xdJvDkmTqENs7tchCbHrQ1z1n7Eeh1/ud9weWADUSQJRr9hYE0jvDKTx9IsfYAh3myFPQaYV9pt+TEi+IKdFm2KOZGYckVEbFx9ydMn2F6UbhopD5Y5HbrKJzf0fF9woyBNcEqQY=
Raw Log: sync broadcast tx success //transaction is sent successfully
```

**Notes**
- The unit of token amount is onchain token symbol（AAA-94f）

### 6. Query Token Information

**Command**
```bash
gatecli token show [token symbol] --chain-id [chain ID]
```

**Parameters**

| Parameter | Type | Description |
|------|------|------|
| token symbol | String | Symbol of the token to query |
| chain-id | String | Chain ID of the network |

**Example**
```bash
gatecli token show ABC-94F --chain-id testnet
```

**Response Example**
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

### 7. Query All Tokens

**Command**
```bash
gatecli token list --chain-id [chain ID]
```

**Parameters**

| Parameter | Type | Description |
|------|------|------|
| chain-id | String | Chain ID of the network |

**Example**
```bash
gatecli token list --chain-id testnet
```

**Response Example**
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

## Transfer Management

### 1. Send Transaction

**Command**
```bash
gatecli tx send [recipient account] [amount] --from [sender account] --fees [tx fees] --chain-id [chain ID]
```

**Parameters**

| Parameter | Type | Description |
|------|------|------|
| recipient account | String | Account address of the recipient |
| amount | String | Amount of tokens to transfer |
| sender account | String | Account address of the sender |
| tx fees | String | Transaction fees (e.g., 100000NANOGT) |
| chain-id | String | Chain ID of the network |

**Example**
```bash
gatecli tx send gt11twm7dma44k7wg5jppeyphrct9nx2l4m8szy44h72qv9eatyla3hkaevg3vx99mlslwsnfq 100000NANOGT --from gt11twm7dma44k7wg5jppeyphrct9nx2l4m8szy44h72qv9eatyla3hkaevg3vx99mlslwsnfq --fees 100000NANOGT --chain-id testnet
```

**Response Example**
```
TxHash: TRANSFER-9F685A8362E6218E372CE60E306E8BC35B66006D82F9B3381A6AECE26FA6355CA38CD75AFFDF597794159D9356BE0376
Data: rQO5zc/tCu8BYPD/ggoo3TuBsrqFDCGccbnTERbiO5v48wnG3ahDuFWSzrdVLzU0iuZQw2rzDBIoHk1VTbZ0J94UnjHi3aO8fwO1V5rK5I2NZvxNF1lFstSU9JD3J18JbxqUAWd0MXB1YjE4cTJmZ3VnZ3F5Znp0YzBwNXJhenEwZnRwdXplNzJwOXRwN25lZ2plZTl6amtjaGx2MHFwNThyZTdyZGduajNqd2x3d3JscjN6Z2o3cmNkcWxnc2Z3Y2V2YWRqaGE0ZXZoOThkejdzN3pjYzh5MHZhZnY3amh1ajNobXR1M2ZtajM2eXdqZWNtbnF1OWgSEgoMCgZOQU5PR1QSAjExEMCaDBowMXrG9msevrtuVTHWuZdFIixl5hSO4tWOvIZV01T/p+Pbg1sPeBgWGHbKUcm1064KImkKJeHhoPogZ32xdJvDkmTqENs7tchCbHrQ1z1n7Eeh1/ud9weWADUSQJRr9hYE0jvDKTx9IsfYAh3myFPQaYV9pt+TEi+IKdFm2KOZGYckVEbFx9ydMn2F6UbhopD5Y5HbrKJzf0fF9woyBNcEqQY=
Raw Log: sync broadcast tx success
```

### 2. Query a Single Transaction

**Command**
```bash
gatecli tx show [transaction  Hash] --chain-id [chain ID]
```

**Parameters**

| Parameter | Type | Description |
|------|------|------|
| transaction hash | String | Transaction hash |
| chain-id | String | Chain ID of the network |

**Example**
```bash
gatecli tx show 03190D3F56D6B65CC11BFE3F9CD961729B134D224A35AC731728601C9DD3A3C7 --chain-id testnet
```

**Response Example**
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


### 3. Query Transaction List based on Criteria

**Command**
```bash
gatecli tx search 
--tags [<tag1>:<value1>&<tag2>:<value2>] 
--page [page number] 
--limit [entries per page] 
--chain-id [chain ID]
```

**Parameters**

| Parameter | Type | Description |
|------|------|------|
| tags | String | Transaction tags |
| page | String | Page number |
| limit | String | Entries per page |
| chain-id | String | Chain ID of the network |

**Example**
```bash
gatecli tx search --tags 'tx.height:771&message.sender:gt11dsck7f7txdc7hz5wdsspey0w5eyhw7yj4xrc3a6r4w4t39z9k45r86htlzxjf6z57an2r7' --page 1 --limit 30 --chain-id testnet
```

**Response Example**
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
### 4. Single Signature

**Command**
```bash
gatecli tx sign [tx file] --from [sender account] --chain-id [chain ID] --generate-only
```

**Parameters**

| Parameter | Type | Description |
|------|------|------|
| tx file | String | Path to the transaction file |
| sender account | String | Account address of the sender |
| chain-id | String | Chain ID of the network |

**Example**
```bash
gatecli tx sign unsignedTx.json --from gt11twm7dma44k7wg5jppeyphrct9nx2l4m8szy44h72qv9eatyla3hkaevg3vx99mlslwsnfq --chain-id testnet --generate-only > signedTx.json
```

**Notes**
- The signed transaction will be saved to the specified output file

### 5. Multi-signature signatures

**Command**
```bash
gatecli tx multisign [tx file] [multisig account name] [signer-1 account] [signer-2 account] ... [signer-n account] --chain-id [chain ID]
```

**Parameters**

| Parameter | Type | Description |
|------|------|------|
| tx file | String | Path to the transaction file |
| multisig account name | String | Name of the multisig account |
| signer account | String | Account address of each signer |
| chain-id | String | Chain ID of the network |

**Example**
```bash
gatecli tx multisign unsignedTx.json gt11twm7dma44k7wg5jppeyphrct9nx2l4m8szy44h72qv9eatyla3hkaevg3vx99mlslwsnfq gt11dsck7f7txdc7hz5wdsspey0w5eyhw7yj4xrc3a6r4w4t39z9k45r86htlzxjf6z57an2r7 --chain-id testnet
```

**Response Example**
```
TxHash: MULTISIGN-9F685A8362E6218E372CE60E306E8BC35B66006D82F9B3381A6AECE26FA6355CA38CD75AFFDF597794159D9356BE0376
Data: rQO5zc/tCu8BYPD/ggoo3TuBsrqFDCGccbnTERbiO5v48wnG3ahDuFWSzrdVLzU0iuZQw2rzDBIoHk1VTbZ0J94UnjHi3aO8fwO1V5rK5I2NZvxNF1lFstSU9JD3J18JbxqUAWd0MXB1YjE4cTJmZ3VnZ3F5Znp0YzBwNXJhenEwZnRwdXplNzJwOXRwN25lZ2plZTl6amtjaGx2MHFwNThyZTdyZGduajNqd2x3d3JscjN6Z2o3cmNkcWxnc2Z3Y2V2YWRqaGE0ZXZoOThkejdzN3pjYzh5MHZhZnY3amh1ajNobXR1M2ZtajM2eXdqZWNtbnF1OWgSEgoMCgZOQU5PR1QSAjExEMCaDBowMXrG9msevrtuVTHWuZdFIixl5hSO4tWOvIZV01T/p+Pbg1sPeBgWGHbKUcm1064KImkKJeHhoPogZ32xdJvDkmTqENs7tchCbHrQ1z1n7Eeh1/ud9weWADUSQJRr9hYE0jvDKTx9IsfYAh3myFPQaYV9pt+TEi+IKdFm2KOZGYckVEbFx9ydMn2F6UbhopD5Y5HbrKJzf0fF9woyBNcEqQY=
Raw Log: sync broadcast tx success
```



### 6. Broadcast Transaction

**Command**
```bash
gatecli tx broadcast [tx file] --chain-id [chain ID]
```

**Parameters**

| Parameter | Type | Description |
|------|------|------|
| tx file | String | Path to the signed transaction file |
| chain-id | String | Chain ID of the network |

**Example**
```bash
gatecli tx broadcast signedTx.json --chain-id testnet
```

**Response Example**
```
TxHash: BROADCAST-9F685A8362E6218E372CE60E306E8BC35B66006D82F9B3381A6AECE26FA6355CA38CD75AFFDF597794159D9356BE0376
Data: rQO5zc/tCu8BYPD/ggoo3TuBsrqFDCGccbnTERbiO5v48wnG3ahDuFWSzrdVLzU0iuZQw2rzDBIoHk1VTbZ0J94UnjHi3aO8fwO1V5rK5I2NZvxNF1lFstSU9JD3J18JbxqUAWd0MXB1YjE4cTJmZ3VnZ3F5Znp0YzBwNXJhenEwZnRwdXplNzJwOXRwN25lZ2plZTl6amtjaGx2MHFwNThyZTdyZGduajNqd2x3d3JscjN6Z2o3cmNkcWxnc2Z3Y2V2YWRqaGE0ZXZoOThkejdzN3pjYzh5MHZhZnY3amh1ajNobXR1M2ZtajM2eXdqZWNtbnF1OWgSEgoMCgZOQU5PR1QSAjExEMCaDBowMXrG9msevrtuVTHWuZdFIixl5hSO4tWOvIZV01T/p+Pbg1sPeBgWGHbKUcm1064KImkKJeHhoPogZ32xdJvDkmTqENs7tchCbHrQ1z1n7Eeh1/ud9weWADUSQJRr9hYE0jvDKTx9IsfYAh3myFPQaYV9pt+TEi+IKdFm2KOZGYckVEbFx9ydMn2F6UbhopD5Y5HbrKJzf0fF9woyBNcEqQY=
Raw Log: sync broadcast tx success
```

### 7.Encoding

**Command**
```bash
gatecli tx encode [file path]
```

**Parameters**

| Parameter | Type | Description |
|------|------|------|
| file path | String | Path to the transaction file |

**Example**
```bash
gatecli tx encode tx_sign.json
```

**Response Example**
```
vAG5zc/tCmrcKqCFCihsMW8nyzNx64qObCAcke6mSXd4kqmHiPdDq6q4lEW1aDPq6/iNJOhUEihVZVapclvx5gwUyOs1WgQNIlRZij2GOBM0JtRFbTTNmIVD08kb3a7iGhAKBk5BTk9HVBIGMTAwMDAwEhIKDAoGTkFOT0dUEgIxMRDAmgwaMHZI3vYAiiIVpsyR/xtwvbu9zTfr0vxZUmT7lhu3unO0lOsHGyPZ3GqlJ9La3c5L7DIEoAboBw==
```

### 8. Gas Estimation

**Command**
```bash
gatecli tx send [recipient account] [amount] --from [sender account]  --chain-id [chain ID] ----dry-run --gas-adjustment [value]
```

**Parameters**

| Parameter | Type | Description |
|------|------|------|
| recipient account | String | Recipient account address |
| amount | String | Transfer amount |
| sender account | String | Sender account address |
| chain-id | String | Chain ID |
| gas-adjustment  | float | Estimation coefficient |

**Example**
```bash
gatecli tx send gt11twm7dma44k7wg5jppeyphrct9nx2l4m8szy44h72qv9eatyla3hkaevg3vx99mlslwsnfq 100000NANOGT --from gt11twm7dma44k7wg5jppeyphrct9nx2l4m8szy44h72qv9eatyla3hkaevg3vx99mlslwsnfq  --chain-id testnet --dry-run --gas-adjustment 1.4
```

**Response Example**
```
gas estimate: 71349
```
### 9. Send Transaction with Estimated Gas

**Command**
```bash
gatecli tx send [recipient account] [amount] --from [sender account] --fees [tx fees] --gas [gaslimit] --chain-id [chain ID]
```

**Parameters**

| Parameter | Type | Description |
|------|------|------|
| recipient account | String | Recipient account address |
| amount | String | Transfer amount |
| sender account | String | Sender account address |
| tx fees | String | Transaction fees (e.g., 100000NANOGT) |
| gas| String | Gas limit |
| chain-id | String | Chain ID |

**Example**
```bash
gatecli tx send gt11twm7dma44k7wg5jppeyphrct9nx2l4m8szy44h72qv9eatyla3hkaevg3vx99mlslwsnfq 100000NANOGT --from gt11twm7dma44k7wg5jppeyphrct9nx2l4m8szy44h72qv9eatyla3hkaevg3vx99mlslwsnfq --fees 713490NANOGT --gas 71349 --chain-id testnet 
```

**Response Example**
```
Response:
  TxHash: IRREVOCABLEPAY-2C8F3C621A31CC84D11FAE2F3CA3F2A9315F0BBCC6EC4D17FA76920E0957B43ECC488C51952BDB592249D64E575AB02E
  Data: ngK5zc/tCl7cKqCFCiiJ3719mjDKkN+wByfBhTyFH31nlWIIfDLjF0nFlNa8uKExO8jRNFKBEhQ/qxhGItwZthCTSblIEUk78qRTYhoYCgZOQU5PR1QSDjEwMDAwMDAwMDAwMDAwEhYKEAoGTkFOT0dUEgY3MTM0OTAQta0EGjAAFFGQ8fF+ak+aFnmJafU2/01YaB5viwFYTYsrDuDBfau8+rUyHdtO9D4tLA/4e84iaQol4eGg+iBx0YPKICxemJ0xaakoVkgfJ8HG4FOYs1WKjCM5QPA1aBJA9I57PDH02c8vd1VpQvfiEqNkCXH5crGvPMos/zzj/SfNJTlcqJjl/Hu0Wawh65A/yUCHqo9fvZBbQDA3foGPCzIDFOYB
  Raw Log: sync broadcast tx success
```
**Notes**
- gasprice=fees/gas, gasprice must be greater than basefee, default gas value is 200000
- Basefee can be queried through the evm rpc eth_getBlockByNumber method




## Vault Account
### 1.Create a Vault Account
**Command**
```bash
gatecli vault-account create [base account] [recovery account] [delay effect height] [clearing height] [token amount to transfer] --from [sender account] --fees [tx fees] --chain-id [chain ID]
```

**Parameters**

| Parameter | Type | Description |
|------|------|------|
| base account | String | Base account address |
| recovery account | String | Recovery account address |
| delay effect height | Int | Delay effect height for the vault account |
| clearing height | Int | Clearing height for the vault account |
| token amount to transfer | String | Amount of tokens to transfer to the vault account |
| sender account | String | Account address of the sender |
| tx fees | String | Transaction fees |
| chain ID | String | Chain ID of the network |

**Example**
```bash
gatecli vault-account create gt11dsck7f7txdc7hz5wdsspey0w5eyhw7yj4xrc3a6r4w4t39z9k45r86htlzxjf6z57an2r7 gt11dsck7f7txdc7hz5wdsspey0w5eyhw7yj4xrc3a6r4w4t39z9k45r86htlzxjf6z57a3457 38 300 30NANOGT --from gt11dsck7f7txdc7hz5wdsspey0w5eyhw7yj4xrc3a6r4w4t39z9k45r86htlzxjf6z57a3457 --fees 10000000NANOGT --chain-id testnet
```

**Response Example**
```
TxHash: BASIC-9F685A8362E6218E372CE60E306E8BC35B66006D82F9B3381A6AECE26FA6355CA38CD75AFFDF597794159D9356BE0376 
//transaction hash, use gatecli tx show {hash} to query details of this transaction
Data: rQO5zc/tCu8BYPD/ggoo3TuBsrqFDCGccbnTERbiO5v48wnG3ahDuFWSzrdVLzU0iuZQw2rzDBIoHk1VTbZ0J94UnjHi3aO8fwO1V5rK5I2NZvxNF1lFstSU9JD3J18JbxqUAWd0MXB1YjE4cTJmZ3VnZ3F5Znp0YzBwNXJhenEwZnRwdXplNzJwOXRwN25lZ2plZTl6amtjaGx2MHFwNThyZTdyZGduajNqd2x3d3JscjN6Z2o3cmNkcWxnc2Z3Y2V2YWRqaGE0ZXZoOThkejdzN3pjYzh5MHZhZnY3amh1ajNobXR1M2ZtajM2eXdqZWNtbnF1OWgSEgoMCgZOQU5PR1QSAjExEMCaDBowMXrG9msevrtuVTHWuZdFIixl5hSO4tWOvIZV01T/p+Pbg1sPeBgWGHbKUcm1064KImkKJeHhoPogZ32xdJvDkmTqENs7tchCbHrQ1z1n7Eeh1/ud9weWADUSQJRr9hYE0jvDKTx9IsfYAh3myFPQaYV9pt+TEi+IKdFm2KOZGYckVEbFx9ydMn2F6UbhopD5Y5HbrKJzf0fF9woyBNcEqQY=
Raw Log: sync broadcast tx success //transaction sent successfully
```

**Notes**
- Must use a newly generated account as the base account to send the create vault account transaction
- If a regular transaction has been sent to this account previously, the account becomes a regular account and cannot be used to create a vault account
- The clearing height must be greater than (current height + delay effect height)

### 2. Update Clearing Height

**Command**
```bash
gatecli vault-account update-clearing-height [new clearing height] --from [sender account] --fees [tx fees] --chain-id [chain ID]
```

**Parameters**

| Parameter | Type | Description |
|------|------|------|
| new clearing height | Int | New clearing height to set |
| sender account | String | Account address of the sender |
| tx fees | String | Transaction fees |
| chain ID | String | Chain ID of the network |

**Example**
```bash
gatecli vault-account update-clearing-height 100000 --from vault11fg056uaatk9s3k6l34eqkc7qchjup8e39afauf9naz4jmpklsze3uwx6405vll88l5lvww --fees 10000000NANOGT --chain-id testnet
```


**Response Example**
```
TxHash: ACCOUNTSET-9F685A8362E6218E372CE60E306E8BC35B66006D82F9B3381A6AECE26FA6355CA38CD75AFFDF597794159D9356BE0376 
//transaction hash, use gatecli tx show {hash} to query details of this transaction
Data: rQO5zc/tCu8BYPD/ggoo3TuBsrqFDCGccbnTERbiO5v48wnG3ahDuFWSzrdVLzU0iuZQw2rzDBIoHk1VTbZ0J94UnjHi3aO8fwO1V5rK5I2NZvxNF1lFstSU9JD3J18JbxqUAWd0MXB1YjE4cTJmZ3VnZ3F5Znp0YzBwNXJhenEwZnRwdXplNzJwOXRwN25lZ2plZTl6amtjaGx2MHFwNThyZTdyZGduajNqd2x3d3JscjN6Z2o3cmNkcWxnc2Z3Y2V2YWRqaGE0ZXZoOThkejdzN3pjYzh5MHZhZnY3amh1ajNobXR1M2ZtajM2eXdqZWNtbnF1OWgSEgoMCgZOQU5PR1QSAjExEMCaDBowMXrG9msevrtuVTHWuZdFIixl5hSO4tWOvIZV01T/p+Pbg1sPeBgWGHbKUcm1064KImkKJeHhoPogZ32xdJvDkmTqENs7tchCbHrQ1z1n7Eeh1/ud9weWADUSQJRr9hYE0jvDKTx9IsfYAh3myFPQaYV9pt+TEi+IKdFm2KOZGYckVEbFx9ydMn2F6UbhopD5Y5HbrKJzf0fF9woyBNcEqQY=
Raw Log: sync broadcast tx success //transaction sent successfully
```

**Notes**
- The new clearing height must be greater than the current blockchain height + vault account delay effect height



### 3. Account Clearing Transaction

**Command**
```bash
gatecli vault-account clear [vault account1]([vault account2] [vault account3]...) --from [sender account] --fees [tx fees] --chain-id [chain ID]
```

**Parameters**

| Parameter | Type | Description |
|------|------|------|
| vault account | String | List of vault account addresses to clear |
| sender account | String | Account address of the sender |
| tx fees | String | Transaction fees |
| chain ID | String | Chain ID of the network |

**Example**
```bash
gatecli vault-account clear vault11d9t6... vault11w8c3v... vault11v5s2... --from gt11dsck7f7txdc7hz5wdsspey0w5eyhw7yj4xrc3a6r4w4t39z9k45r86htlzxjf6z57an2r7 --fees 10000000NANOGT --chain-id testnet
```

**Response Example**
```
TxHash: VAULTCLEAR-9F685A8362E6218E372CE60E306E8BC35B66006D82F9B3381A6AECE26FA6355CA38CD75AFFDF597794159D9356BE0376 
//transaction hash, use gatecli tx show {hash} to query details of this transaction
Data: rQO5zc/tCu8BYPD/ggoo3TuBsrqFDCGccbnTERbiO5v48wnG3ahDuFWSzrdVLzU0iuZQw2rzDBIoHk1VTbZ0J94UnjHi3aO8fwO1V5rK5I2NZvxNF1lFstSU9JD3J18JbxqUAWd0MXB1YjE4cTJmZ3VnZ3F5Znp0YzBwNXJhenEwZnRwdXplNzJwOXRwN25lZ2plZTl6amtjaGx2MHFwNThyZTdyZGduajNqd2x3d3JscjN6Z2o3cmNkcWxnc2Z3Y2V2YWRqaGE0ZXZoOThkejdzN3pjYzh5MHZhZnY3amh1ajNobXR1M2ZtajM2eXdqZWNtbnF1OWgSEgoMCgZOQU5PR1QSAjExEMCaDBowMXrG9msevrtuVTHWuZdFIixl5hSO4tWOvIZV01T/p+Pbg1sPeBgWGHbKUcm1064KImkKJeHhoPogZ32xdJvDkmTqENs7tchCbHrQ1z1n7Eeh1/ud9weWADUSQJRr9hYE0jvDKTx9IsfYAh3myFPQaYV9pt+TEi+IKdFm2KOZGYckVEbFx9ydMn2F6UbhopD5Y5HbrKJzf0fF9woyBNcEqQY=
Raw Log: sync broadcast tx success //transaction sent successfully
```

**Notes**
- The sender (--from) must be the security account for this vault account
- All vault accounts must have reached their clearing heights



### 4. Query Vault Account Information

**Command**
```bash
gatecli vault-account show [vault account] --chain-id [chain ID]
```

**Parameters**

| Parameter | Type | Description |
|------|------|------|
| vault account | String | Vault account address |
| chain ID | String | Chain ID of the network |

**Example**
```bash
gatecli vault-account show vault11fg056uaatk9s3k6l34eqkc7qchjup8e39afauf9naz4jmpklsze3uwx6405vll88l5lvww --chain-id testnet
```


**Response Example**
```
Account:
  Address:            vault1124j4d2tjt0c7vrq5er4n2ksyp539gkv28krrsye5ym2y2mf5ekvg2s7neydamthzwag272 //account address
  Pubkey: //account public key
  Tokens:        100000000NANOGT //account balance
  AccountNumber: 9 //account number
  Sequence:      0
  AccountType:        1 //account type: 0.single-signature normal account, 1.single-signature vault account, 2.multi-signature normal account, 3.multi-signature vault account
  DelayHeight:        100 //transaction delay effect height for vault account
  SecurityAddress:    gt11dsck7f7txdc7hz5wdsspey0w5eyhw7yj4xrc3a6r4w4t39z9k45r86htlzxjf6z57an2r7 //recovery account for vault account
  LastClearingHeight: 0 //last set clearing height
  LastClearingEffectHeight:   0 //transaction height when last clearing height was set
  NextClearingHeight:      700 //newest set clearing height
  NextClearingEffectHeight:   257 //transaction height when newest clearing height was set
  ReceivedRevocableTokens: //tokens that can still be revoked
  SentRevocableTokens: //revocable tokens that have been sent
```



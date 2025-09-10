# 如何成为 GateChain 验证人

验证人节点负责参与网络共识和区块生产。通过成为验证人，您可以为网络安全做出贡献，并通过 PoS 挖矿获得奖励。

要将您的全节点升级为验证人节点并参与 PoS 挖矿，请按照以下步骤操作：

1. 创建普通账户：
```bash
gatecli account create
```

2. 基于普通账户创建共识账户：
```bash
gatecli con-account create [account address] --chain-id mainnet
```

3. 向共识账户发送 GT：
```bash
gatecli tx send [account address] [GT amount] --from [sender account] --fees [fee amount] --chain-id [chain ID] -y
```

4. 发起"共识账户上线"交易：
```bash
gatecli con-account online \
--from [sender account] \
--pubkey [sender account public key] \
--moniker [name] \
--commission-max-change-rate [maximum daily commission rate change] \
--commission-max-rate [maximum commission rate] \
--commission-rate [commission rate] \
--chain-id mainnet
```

完成这些步骤后，您的节点将开始参与 GateChain 的共识过程。请确保您的机器正常运行并保持网络连接稳定。


# GateChain 系统合约

## 概述
系统合约是 GateChain 区块链网络中的核心组件，负责管理网络的基本功能和规则，包括用户质押、奖励分配以及网络参数调整等。本文档将详细介绍系统合约的地址、主要方法以及质押操作的特殊说明。

## 合约地址
系统合约的地址在 GateChain 网络中是固定的，通常在网络初始化时设定。GateChain 的系统合约地址为：`0x0000000000000000000000000000000000001000`。

## 主要方法

### 委托操作

#### Delegate(string valAddr, uint256 amount)
- **功能**：允许用户将指定金额的主币委托给某个验证人（validator），以参与网络共识或获得奖励。
- **参数**：
  - `valAddr`：验证人地址（字符串格式）。
  - `amount`：委托金额（无符号整数）。
- **说明**：在 GateChain 中，质押主币时，用户需要在交易的 `data` 字段中编码 `valAddr` 和 `amount` 参数，而不是通过 `value` 字段指定金额。这是 GateChain 的设计特点，旨在通过显式参数传递确保质押操作的准确性。


#### UnDelegate(string valAddr, uint256 amount)
- **功能**：允许用户取消对某个验证人的指定金额委托，并取回主币。
- **参数**：
  - `valAddr`：验证人地址（字符串格式）。
  - `amount`：取消委托的金额（无符号整数）。
- **说明**：此方法通过 `data` 字段传递参数，执行取消质押操作。

#### Redelegate(string valSrcAddr, string valDstAddr, uint256 amount)
- **功能**：允许用户将已委托给某个验证人的主币重新委托给另一个验证人。
- **参数**：
  - `valSrcAddr`：源验证人地址（字符串格式）。
  - `valDstAddr`：目标验证人地址（字符串格式）。
  - `amount`：重新委托的金额（无符号整数）。

### 奖励操作

#### WithdrawDelegatorRewards(string valSrcAddr)
- **功能**：允许用户提取从某个验证人处获得的委托奖励。
- **参数**：
  - `valSrcAddr`：验证人地址（字符串格式）。


#### WithdrawAllRewards(string delSrcAddr)
- **功能**：允许用户提取所有委托奖励。
- **参数**：
  - `delSrcAddr`：委托人地址（字符串格式）。


#### RewardReinvestment(string valSrcAddr)
- **功能**：允许用户将从某个验证人处获得的奖励重新投资（即重新委托）。
- **参数**：
  - `valSrcAddr`：验证人地址（字符串格式）。


### 查询操作

#### DelegatorRewardsSize(string delAddr) returns (uint8)
- **功能**：查询指定委托人的奖励条目数量。
- **参数**：
  - `delAddr`：委托人地址（字符串格式）。
- **返回值**：奖励条目数量（无符号 8 位整数）。


#### DelegatorReward(string delAddr, uint256 delIndex) returns (string, uint64, uint64)
- **功能**：查询指定委托人的某个奖励条目详细信息。
- **参数**：
  - `delAddr`：委托人地址（字符串格式）。
  - `delIndex`：奖励条目索引（无符号整数）。
- **返回值**：
  - 验证人地址（字符串格式）。
  - 奖励金额（无符号 64 位整数）。
  - 另一个相关数值（无符号 64 位整数）。

#### DelegatorRewardByValAddr(string delAddr, string valAddr) returns (uint64)
- **功能**：查询指定委托人从某个验证人处获得的奖励金额。
- **参数**：
  - `delAddr`：委托人地址（字符串格式）。
  - `valAddr`：验证人地址（字符串格式）。
- **返回值**：奖励金额（无符号 64 位整数）。


#### UndelegationsSize(string delAddr) returns (uint8)
- **功能**：查询指定委托人的取消委托条目数量。
- **参数**：
  - `delAddr`：委托人地址（字符串格式）。
- **返回值**：取消委托条目数量（无符号 8 位整数）。


#### Undelegations(string delAddr, uint256 delIndex) returns (string, uint64, uint64, uint64)
- **功能**：查询指定委托人的某个取消委托条目详细信息。
- **参数**：
  - `delAddr`：委托人地址（字符串格式）。
  - `delIndex`：取消委托条目索引（无符号整数）。
- **返回值**：
  - 验证人地址（字符串格式）。
  - 取消委托金额（无符号 64 位整数）。
  - 另一个相关数值（无符号 64 位整数）。
  - 另一个相关数值（无符号 64 位整数）。


## 总结
GateChain 的系统合约提供了丰富的功能，涵盖了委托、取消委托、重新委托、奖励提取以及奖励查询等操作。所有方法均通过交易的 `data` 字段传递参数，确保操作的明确性和可追溯性。用户和开发者可以通过与系统合约交互，参与 GateChain 网络的共识机制和奖励分配。
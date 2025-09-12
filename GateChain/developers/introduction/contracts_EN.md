# GateChain System Contracts

## Overview
System contracts are core components of the GateChain blockchain network, responsible for managing the network's basic functions and rules, including user staking, reward distribution, and network parameter adjustments. This document provides a detailed introduction to the system contract addresses, main methods, and special instructions for staking operations.

## Contract Address
The system contract address is fixed in the GateChain network and is typically set during network initialization. The system contract address for GateChain is: `0x0000000000000000000000000000000000001000`.

## Main Methods

### Delegation Operations

#### Delegate(string valAddr, uint256 amount)
- **Function**: Allows users to delegate a specified amount of main coins to a validator to participate in network consensus or earn rewards.
- **Parameters**:
  - `valAddr`: Validator address (string format).
  - `amount`: Delegation amount (unsigned integer).
- **Note**: In GateChain, when staking main coins, users need to encode the `valAddr` and `amount` parameters in the transaction's `data` field, rather than specifying the amount via the `value` field. This is a design feature of GateChain to ensure the accuracy of staking operations through explicit parameter passing.

#### UnDelegate(string valAddr, uint256 amount)
- **Function**: Allows users to undelegate a specified amount of main coins from a validator and retrieve them.
- **Parameters**:
  - `valAddr`: Validator address (string format).
  - `amount`: Undelegation amount (unsigned integer).
- **Note**: This method passes parameters through the `data` field to execute the undelegation operation.

#### Redelegate(string valSrcAddr, string valDstAddr, uint256 amount)
- **Function**: Allows users to redelegate main coins already delegated to one validator to another validator.
- **Parameters**:
  - `valSrcAddr`: Source validator address (string format).
  - `valDstAddr`: Destination validator address (string format).
  - `amount`: Redelegation amount (unsigned integer).

### Reward Operations

#### WithdrawDelegatorRewards(string valSrcAddr)
- **Function**: Allows users to withdraw delegation rewards earned from a specific validator.
- **Parameters**:
  - `valSrcAddr`: Validator address (string format).

#### WithdrawAllRewards(string delSrcAddr)
- **Function**: Allows users to withdraw all delegation rewards.
- **Parameters**:
  - `delSrcAddr`: Delegator address (string format).

#### RewardReinvestment(string valSrcAddr)
- **Function**: Allows users to reinvest (i.e., redelegate) rewards earned from a specific validator.
- **Parameters**:
  - `valSrcAddr`: Validator address (string format).

### Query Operations

#### DelegatorRewardsSize(string delAddr) returns (uint8)
- **Function**: Queries the number of reward entries for a specified delegator.
- **Parameters**:
  - `delAddr`: Delegator address (string format).
- **Returns**: Number of reward entries (unsigned 8-bit integer).

#### DelegatorReward(string delAddr, uint256 delIndex) returns (string, uint64, uint64)
- **Function**: Queries detailed information about a specific reward entry for a specified delegator.
- **Parameters**:
  - `delAddr`: Delegator address (string format).
  - `delIndex`: Reward entry index (unsigned integer).
- **Returns**:
  - Validator address (string format).
  - Reward amount (unsigned 64-bit integer).
  - Another related value (unsigned 64-bit integer).

#### DelegatorRewardByValAddr(string delAddr, string valAddr) returns (uint64)
- **Function**: Queries the reward amount earned by a specified delegator from a specific validator.
- **Parameters**:
  - `delAddr`: Delegator address (string format).
  - `valAddr`: Validator address (string format).
- **Returns**: Reward amount (unsigned 64-bit integer).

#### UndelegationsSize(string delAddr) returns (uint8)
- **Function**: Queries the number of undelegation entries for a specified delegator.
- **Parameters**:
  - `delAddr`: Delegator address (string format).
- **Returns**: Number of undelegation entries (unsigned 8-bit integer).

#### Undelegations(string delAddr, uint256 delIndex) returns (string, uint64, uint64, uint64)
- **Function**: Queries detailed information about a specific undelegation entry for a specified delegator.
- **Parameters**:
  - `delAddr`: Delegator address (string format).
  - `delIndex`: Undelegation entry index (unsigned integer).
- **Returns**:
  - Validator address (string format).
  - Undelegation amount (unsigned 64-bit integer).
  - Another related value (unsigned 64-bit integer).
  - Another related value (unsigned 64-bit integer).

## Summary
GateChain's system contracts provide a wide range of functionalities, including delegation, undelegation, redelegation, reward withdrawal, and reward querying operations. All methods pass parameters through the transaction's `data` field, ensuring clarity and traceability of operations. Users and developers can interact with system contracts to participate in the consensus mechanism and reward distribution of the GateChain network. 
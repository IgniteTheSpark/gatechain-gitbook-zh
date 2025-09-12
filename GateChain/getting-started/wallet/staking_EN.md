# Staking

## Overview

GateChain fundamentally abandons the traditional staking approach. Any account can become a consensus account and participate in block consensus to receive rewards by simply paying a regular transaction fee.

### Key Advantages

1. **No Minimum Asset Requirement**: The probability of being selected for the committee only relates to consensus weight and loyalty coefficient. A properly configured node can participate in consensus and earn rewards without asset delegation.

2. **Enhanced Security**: The committee is randomly elected during each consensus round (guaranteed by VRF algorithm), making it difficult for hackers to launch attacks. Node failures only affect individual consensus participation without significantly impacting network operation.

3. **Scalability**: Since the committee size is part of the mainnet consensus, the network can maintain efficient consensus achievement even as it expands.

## Why GateChain's Approach is Different

Most PoS models require accounts to stake a certain amount of assets for consensus participation. Traditional staking models face several challenges:

1. **High Entry Barriers**: Consensus participation becomes a bidding target, excluding accounts with fewer assets from becoming independent validators.

2. **Centralization Risk**: Fixed validator sets based on staked asset rankings can compromise network security and consensus achievement.

3. **Limited Scalability**: Linear validator growth can impede block consensus speed as the network expands.

## Key Concepts

### Loyalty Coefficient (LoyaltyParam)

- Initial value: 1
- Maximum value: 2
- Purpose: Acts as an "amplification factor" for user weight
- Benefits: Rewards consistent consensus participation with additional voting power
- Penalties: Decreases due to malicious voting, extended inactivity, or power reduction

### Slashing Mechanism

Malicious behavior includes:
- Voting for multiple proposals in the same phase
- Consequences:
  - Reduced loyalty coefficient
  - Decreased consensus weight
  - PoSsible consensus participation ban
  - Affected delegation rewards

## Participation Methods

### 1. Running a Full Node

Benefits:
- Direct consensus participation
- Earning delegation commission
- Full mining revenue

### 2. GT Delegation

Features:
- No minimum delegation amount
- Available to all account types
- Immediate effect
- 0-day freeze period for redelegation/undelegation
- Recommended for vault accounts

### Reward Distribution

#### Block Rewards
- Additional: Transaction fees
- Weight calculation: (Currency holdings + delegation GT quantity) * Loyalty coefficient

#### Committee Distribution Rules

1. Three consensus accounts:
   - Normal range: 27-40% based on weight
   - Outside range: Fixed 40%-33%-27% split

2. Two consensus accounts:
   - Normal range: 40-60% based on weight
   - Outside range: Fixed 60%-40% split

3. Single consensus account:
   - Receives 100% of rewards


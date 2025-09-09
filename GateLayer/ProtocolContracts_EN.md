# Protocol Contracts

This document lists the core protocol smart contract addresses deployed on GateChain (L1) for the Gate Layer system. These contracts collectively form the foundation of the Gate Layer network.

---

## Core L1 Contracts

These are the primary contracts directly related to the functionality of Gate Layer.

| Contract Name                  | Address                                    | Description                                                                 |
| ------------------------------ | ------------------------------------------ | --------------------------------------------------------------------------- |
| **L1StandardBridge**           | `0x75ded3be4d69e8403c9079ae5cf5e059a23f6a67` | Handles deposits and withdrawals of ETH and standard ERC20 tokens between L1 and L2. |
| **L1CrossDomainMessenger**     | `0x60cad8d7d622be3d62786b2e7a6617765d0f4d94` | Passes arbitrary messages between L1 and L2, forming the basis for cross-domain communication. |
| **OptimismPortal**             | `0x0d69cdd07cf93075fe53769945575274fc563ba9` | The entry point for L2 transaction data submission to L1 and handles withdrawal proofs. A core part of the system. |
| **DisputeGameFactory**         | `0x6f419bb5d754033e39f758d20133d7fea8e2bb55` | The core of the fault-proof system, used to create and manage challenges against L2 output roots. |
| **SystemConfig**               | `0xaeb4b93732c30c5d74ab3220cd73aa0fcdd275f8` | Stores L2 gas-related parameters, such as `overhead` and `scalar`.          |
| **L1ERC721Bridge**             | `0xa19cbb87428a6b943912f2f44241ca1cb3a8cec1` | Handles the bridging of ERC721 NFT tokens between L1 and L2.                |
| **OptimismMintableERC20Factory** | `0xca1bbd1187ccbf2297c828c48f5c0896d9985636` | Creates "mintable" versions of ERC20 tokens on L2 that correspond to deposited L1 tokens. |
| **ProxyAdmin**                 | `0x1be6f451a7578cb48ddbd8fea8d2fd25462e5d16` | Manages the upgrade permissions for the core contracts listed above.        |
| **AddressManager**             | `0x6cbd08ae08136ab974ae121a0895d3836c8a00ce` | (Legacy) A registry used to resolve and manage system contract addresses.     |

---

## Implementation Contracts

The following are the logic implementation addresses that the proxy contracts listed above currently point to. These addresses may change with protocol upgrades.

| Contract Name                       | Implementation Address                             |
| ----------------------------------- | -------------------------------------------------- |
| **L1StandardBridge Impl**           | `0x0b09ba359a106c9ea3b181cbc5f394570c7d2a7a`     |
| **L1CrossDomainMessenger Impl**     | `0x5d5a095665886119693f0b41d8dfee78da033e8b`     |
| **OptimismPortal Impl**             | `0xb1dfde4e7c3018b97fa68b12f7d5648c96e4674e`     |
| **DisputeGameFactory Impl**         | `0x4bba758f006ef09402ef31724203f316ab74e4a0`     |
| **SystemConfig Impl**               | `0xec6c6d47ec88f474bffa4defd38930fb2e79084c`     |
| **L1ERC721Bridge Impl**             | `0x7ae1d3bd877a4c5ca257404ce26be93a02c98013`     |
| **OptimismMintableERC20Factory Impl** | `0x5493f4677a186f64805fe7317d6993ba4863988f`     |

# Token Standards

In this section, we delve into the various token standards supported on GateChain. Understanding these standards is crucial for developers as they form the foundation of many decentralized applications.

GateChain offers support for three main token types:

- [GateChain Token (GT)](#gt)
- [Fungible Tokens](#fungible-tokens)
- [NFTs](#nfts)


## GT

The GT is the native token of the GateChain, serving multiple roles within the ecosystem:

- Fee Token: Used to pay transaction fees on the GateChain network.
- Governance Token: Used to participate in governance decisions affecting the network.

### EVM

```
1GT = 10**18 wei = 1000000000000000000wei
1GT = 10**9 wei = 1000000000gwei (giga-wei)
```

### Native

```
1GT = 10**9 NANOGT = 1000000000nGT (nano-GT)
```

## Fungible Tokens

Fungible tokens are digital assets that are interchangeable with one another and are not unique. GT supports both ERC20 and fungible token standards, so developers have the option to create tokens using the native TokenFactory or via smart contract standards.

### TokenFactory

TokenFactory allows for the creation of tokens that are natively integrated into the base chain modules. This integration means bank balances are available through native bank queries, unlike  ERC20 tokens which require querying the smart contract directly.

- **Native Integration**: Tokens created via TokenFactory are integrated into the chain allowing the core modules to efficiently interact with these tokens and RPC clients to return their balances in native bank and account queries.
- **Efficient Queries**: Tokens are available through native queries, making it more efficient than querying smart contracts directly.
- **Recommended Use**: Ideal for scenarios where performance and ease of access are priorities or for native applications.

### Smart Contract Tokens

- **ERC20**: The ERC20 standard defines a common set of rules for fungible tokens on EVM-based blockchains. These tokens can be transferred, approved, and queried using standard functions.

- **Standard Compatibility**: Supporting  ERC20 ensure compatibility with existing dApps and ecosystems while enabling onboarding of EVM-centric projects.
- **Interoperability**: Both standards support pointer contracts, enabling seamless interaction between native and EVM environments.

### Interoperability and Pointer Contracts

Pointer contracts enable interoperability between ERC20  tokens, allowing for seamless interaction between the two standards. A registry is kept to track pointer contracts and facilitate interoperability.

**Recommended Use**: Suitable for applications needing standard token interactions for use with existing dApps and maximum compatibility.

## Interoperability

Regardless of the choice, both TokenFactory and ERC20 tokens can have pointer contracts deployed, facilitating seamless cross-vm interoperability.

## NFTs

Non-fungible tokens (NFTs) represent unique digital assets. GT supports both ERC721 and CW721 standards as well as their counterparts with royalties (2981).

- **ERC721**: The ERC721 standard defines the structure for non-fungible tokens on EVM-based blockchains. Each token is unique and cannot be exchanged on a one-to-one basis like fungible tokens.
- **Interoperability**: Similar to fungible tokens, NFTs can interact across different standards using pointer contracts.
- **Pointer Contract Registry**: A registry for tracking pointer contracts specific to NFTs.


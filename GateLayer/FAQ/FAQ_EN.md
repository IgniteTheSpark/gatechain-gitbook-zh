# Frequently Asked Questions (FAQ)

### What is Gate Layer?
Gate Layer is a high-performance Layer 2 network built on the Optimism OP Stack. It is fully EVM compatible, secured by GateChain as the settlement layer, and optimized for low-cost, high-speed transactions.

### How is Gate Layer different from other OP Stack-based L2s?
Gate Layer differentiates itself from other OP Stack-based L2s by leveraging GateChain’s native Blob storage to store rollup state roots, reducing costs and improving scalability. Backed by GT staking, it ensures strong settlement security while delivering 5,700+ TPS and 1s block time.

### How to estimate the TPS for Gate Layer?
TPS can be estimated using a native transfer as an example. Each transfer typically consumes 21k gas, while Gate Layer’s block gas limit is 120M. This means each block can handle 120M ÷ 21k ≈ 5,714 transactions, resulting in a TPS of over 5,700.

### How can developers and users get started with Gate Layer?
Developers can deploy applications permissionlessly using existing Ethereum tools, while users will benefit from low-cost, fast transactions. Documentation, explorer, and bridges will be provided as core entry points once the network is live.

### Why are transaction fees on Gate Layer so low?
Transaction fees on Gate Layer are composed of two parts: an L2 execution fee and an L1 data fee. The key to its low cost is that the L1 data fee is paid by posting transaction data as `Blobs` to GateChain. Blobs are a highly cost-effective data storage solution designed specifically for rollups, making them significantly cheaper than using traditional `calldata` on a mainnet.

### What does "transaction finality" mean on Gate Layer?
A transaction on Gate Layer goes through two stages of confirmation:
1.  **Fast Confirmation (unsafe)**: Transactions are confirmed almost instantly (in about 1 second) by the L2 Sequencer, providing a smooth user experience.
2.  **Final Confirmation (finalized)**: The transaction becomes irreversible only after its data has been published to GateChain (L1) and an additional 10-block security buffer has passed. This L2-enforced waiting period ensures the highest level of security.

### How is Gate Layer secured?
Gate Layer's security is multi-layered:
- **Execution**: As an OP Stack-based rollup, it relies on a fault-proof mechanism to ensure the correctness of L2 state transitions.
- **Data Availability & Settlement**: It inherits security from GateChain, which uses GT staking and a validator network to secure all L2 transaction data and settlement operations.
- **Interoperability**: Cross-chain operations are secured by protocols like LayerZero, with GateChain serving as the ultimate security anchor for finality.

### What is the native gas token on Gate Layer?
The native gas token on Gate Layer is **GT (GateToken)**, which is used to pay for all transaction fees on the network.

### Is Gate Layer EVM compatible? What does that mean for developers?
Yes, Gate Layer is fully **EVM compatible**. This means any smart contract, DApp, or tool that works on Ethereum can run on Gate Layer with virtually no modifications. Developers can leverage the entire Ethereum toolchain (Hardhat, Foundry, Remix, Ethers.js, etc.) and their existing Solidity codebase to build and deploy applications seamlessly.

### How do I migrate my existing DApp to Gate Layer?
Migrating an EVM-based DApp is a simple process:
1.  **Configure Network:** Add the Gate Layer network details (RPC URL, Chain ID) to your development framework like Hardhat or Foundry.
2.  **Deploy Contracts:** Re-deploy your existing, unchanged smart contracts to Gate Layer.
3.  **Update Frontend:** Update your DApp's frontend to point to the new contract addresses and connect to the Gate Layer RPC endpoint.
Since no code changes are typically required, the migration process is fast and efficient.

### Why should I build my DApp on Gate Layer?
Gate Layer offers several key advantages for developers:
- **Ultra-Low Costs:** By using GateChain's Blobs for data availability, transaction fees are a fraction of those on Ethereum or even other L2s, making your DApp more accessible to users.
- **High Performance:** With over 5,700 TPS and 1-second block times, your DApp can offer a highly responsive, near-instant user experience.
- **Familiar Tooling:** There is no need to learn new languages or tools. You can build with the same EVM ecosystem you already know.
- **Interoperability:** Native integration with protocols like LayerZero allows your DApp to easily connect with a multi-chain ecosystem.

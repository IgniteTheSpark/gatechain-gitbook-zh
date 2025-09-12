### Frequently Asked Questions (FAQ)

#### Q: What does GateChain's EVM compatibility mean?

EVM (Ethereum Virtual Machine) is a component specifically designed to execute smart contracts on the Ethereum blockchain. Traditional first-generation blockchains like BTC, LTC, and Doge cannot execute smart contracts and only support transfers between accounts.

Ethereum, as a representative of second-generation blockchain, provides the EVM smart contract runtime environment in addition to account transfer functionality. Each node joining Ethereum runs EVM to process smart contract transactions.

For example: Early phones (blockchain) only had calling and texting functions. Later, Microsoft, Apple, and Google launched their own smartphone operating systems (like EVM) installed on better-performing phones, allowing each phone to run third-party developed apps (smart contracts).

Therefore, EVM compatibility means that GateChain not only supports regular account transfers but also provides a smart contract execution environment. Developers can deploy their smart contract code from Ethereum, BSC, and other chains directly on GateChain without any modifications.

Examples:
- Ethereum regular transfer transaction: [Example](https://etherscan.io/tx/0x46eefc3ab029c8e4737f0deba186b3c0e3282fbfdfc515849f9ef9093563f690)
- Ethereum smart contract transaction: [Example](https://etherscan.io/tx/0xd0cc5983fd933f6e3468b731476a55957721fe7534b76f6cebd8f2b5a8df2d65)

#### Q: What is ERC-20?

[ERC-20 Standard](https://eips.ethereum.org/EIPS/eip-20)

ERC stands for Ethereum Request for Comment, used to propose improvements for tokens and the Ethereum ecosystem. After an ERC is submitted, the Ethereum community evaluates it and decides whether to accept or reject the proposal. Once approved, the ERC becomes an official Ethereum Improvement Proposal (EIP).

Simply put, token standards define the data and functionality that each token must implement. On Ethereum, besides the native ETH token, all other tokens are ERC tokens conforming to various standards.

ERC-20 was introduced in November 2015 and officially standardized in September 2017. This protocol specifies a set of basic interfaces for fungible tokens, including token symbols, supply, transfer, and authorization. Tokens complying with this standard are fully compatible with Ethereum wallets.

For more information about ERC-20, please visit [gate.io](https://www.gate.io/bitwiki/detail/211).

#### Q: What is ERC-721?

[ERC-721 Standard](https://eips.ethereum.org/EIPS/eip-721)

ERC-721 was proposed in January 2018 and is the underlying standard for the NFT boom. The main difference from ERC-20 is that each token has a unique identifier. With numbered identifiers, different token IDs can represent different values, similar to artwork.

For example: If ERC-20 is like regular currency where each unit has the same value and purchasing power, then ERC-721 is like commemorative coins issued by banks, each with a unique number, belonging to only one account, and varying significantly in price.

#### Q: What is ERC-1155?

[ERC-1155 Standard](https://eips.ethereum.org/EIPS/eip-1155)

ERC-1155, proposed in June 2018, is called the Multi Token Standard. It means that when issuing an ERC-1155 token, it can contain multiple types of ERC tokens, including ERC-20 and ERC-721. It's essentially a token collection concept.

#### Q: Do I need to create new accounts on different machines when running multiple virtual machines? Can these VMs share one account?

If you need multiple virtual machines to participate in consensus, you must create accounts on new machines and bring consensus accounts online.

#### Q: When running multiple nodes, does each node need to synchronize blockchain information separately?

You can synchronize separately or package information from other synchronized nodes to new nodes. Specific method: Package and copy the root directory where gated runs (default ~/.gated folder). However, if your multiple nodes want to create new consensus accounts to participate in consensus, you need to delete the copied partkey (located in ~/.gated/mainnet/ with .0.0.partkey extension) and create new consensus accounts.

#### Q: After successfully setting up a node, using gatecli command returns error: open /Users/XXX/.gatecli/api.token: no such file or directory

This occurs because there's no api.token in the .gatecli directory. You need to copy it from the .gated directory using the command: `cp ~/.gated/api.token ~/.gatecli/`

#### Q: Using gatecli account show [address] command returns error: account XXXX does not exist, why?

This is because the account you're querying isn't on-chain. You need to send a transaction to this account, and after the transaction succeeds, you can query it.

#### Q: How to recover an account using mnemonic phrases after creating it?

Use the command `gatecli account create --recover` to recover your account.

#### Q: If I transferred money to a locally created account but haven't synchronized to that transaction block height, how can I quickly transfer out from this account?

You can construct the transaction body via RPC, sign it locally, and then broadcast via RPC. Specific steps:

1. Construct transaction body, modify valid_height parameter, suggested to modify to 300 blocks after GateChain's current latest height. [Guide](../../developers/api/tx/index.md#普通交易)
2. Save the returned transaction body to your desktop, then execute locally: `gatecli tx sign [transaction body file] --from [sender account] --chain-id mainnet --offline --account-number 1`
3. Copy the signed information and broadcast the transaction. [Guide](../../developers/api/tx/index.md#发送交易)
4. GateChain API node list. [Enter](../../integration/rpc-node-list/index.md)

#### Q: Already transferred money to the consensus account preparing to go online, but still getting error when bringing consensus account online: account XXXX does not exist, why?

Please confirm that your local sync node has reached ≥ the transaction block height of the consensus account transfer. If it's <, wait for synchronization to the transfer transaction before bringing the consensus account online.

#### Q: What affects the loyalty coefficient of consensus accounts?

The loyalty coefficient increases with the number of times selected for the committee. If your node hasn't participated in consensus for a long time, GateChain will automatically process the consensus account as offline, and the loyalty coefficient will drop to 1.

#### Q: What should I do if I delegated to a consensus account but found it hasn't participated in consensus for a long time?

There is indeed a risk of revenue loss in consensus account delegation. For example, if the delegated node can't participate in mainnet consensus due to factors like network outage or power failure, it won't generate revenue. It's recommended that users set up their own nodes to ensure stable network connectivity. For users who find their consensus account hasn't participated in consensus for a long time and are unable to set up their own nodes, they can transfer their delegation to other consensus accounts.

#### Q: Which block did GateChain start supporting smart contracts?

Block 731444

#### Q: EVM interface, getting BlockNumber returns 404 error.

Check if your port 8080 is occupied by gated. Port 8080 must be free when gated starts. Otherwise, you need to modify the config.json file, and EVM RPC startup needs additional parameters. It's recommended to keep port 8080 free for gated use.

#### Q: EVM interface, getting BlockNumber returns error: HTTP 401 Unauthorized: Invalid API Token

Execute this command: `cp ~/.gated/api.token ~/.gatecli/`

> FAQ continues to be updated...

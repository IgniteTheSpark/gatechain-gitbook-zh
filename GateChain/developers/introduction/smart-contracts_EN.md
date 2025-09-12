# EVM (general)

## Overview

The Ethereum Virtual Machine (EVM) is the runtime environment for smart contracts, enabling compatibility with Ethereum-based dApps. GateChain is an EVM compatible blockchain. GateChain's parallelized EVM ensures high performance and efficiency.

Here are some key points about the EVM:

- **Turing Completeness**: The EVM is Turing complete, meaning it can execute any computable function. This allows developers to write complex smart contracts.
- **Gas**: Transactions and contract executions on the EVM compatible network consume gas. Gas is a measure of computational work, and users pay for it in GateChain on GateChain networks. Gas ensures that malicious or inefficient code doesn't overload the network.
- **Bytecode Execution**: Smart contracts are compiled into bytecode (low-level machine-readable instructions) and deployed to the EVM compatible network. The EVM executes this bytecode.

## Smart Contract Languages

The two most popular languages for developing smart contracts on the EVM are Solidity and Vyper.

### Solidity

- Object-oriented, high-level language for implementing smart contracts.
- Curly-bracket language that has been most profoundly influenced by C++.
- Statically typed (the type of a variable is known at compile time).
- Supports:
  - Inheritance (you can extend other contracts)
  - Libraries (you can create reusable code that you can call from different contracts – like static functions in a static class in other object oriented programming languages)
  - Complex user-defined types

### Example Solidity Contract

```solidity
// SPDX-License-Identifier: GPL-3.0
pragma solidity >= 0.7.0;
 
contract Coin {
    // The keyword "public" makes variables
    // accessible from other contracts
    address public minter;
    mapping (address => uint) public balances;
 
    // Events allow clients to react to specific
    // contract changes you declare
    event Sent(address from, address to, uint amount);
 
    // Constructor code is only run when the contract
    // is created
    constructor() {
        minter = msg.sender;
    }
 
    // Sends an amount of newly created coins to an address
    // Can only be called by the contract creator
    function mint(address receiver, uint amount) public {
        require(msg.sender == minter);
        require(amount < 1e60);
        balances[receiver] += amount;
    }
 
    // Sends an amount of existing coins
    // from any caller to an address
    function send(address receiver, uint amount) public {
        require(amount <= balances[msg.sender], "Insufficient balance.");
        balances[msg.sender] -= amount;
        balances[receiver] += amount;
        emit Sent(msg.sender, receiver, amount);
    }
}
```

## Deploying EVM Contract on GateChain

Since GateChain is an EVM compatible chain, existing EVM tooling like hardhat, foundry forge or other could be re-used.

In this example we will be using foundry tooling.

1. Install the foundry tooling by following this [Installation guide](https://book.getfoundry.sh/getting-started/installation).
2. Create a new project following the [Creating New Project Guide](https://book.getfoundry.sh/projects/creating-a-new-project).
3. Make sure you have a wallet on GateChain network.

Once project is created, tweak the contract code to the following, by adding a getCounter function:

```solidity
// SPDX-License-Identifier: UNLICENSED
pragma solidity ^0.8.13;
 
contract Counter {
    uint256 public number;
 
    function setNumber(uint256 newNumber) public {
        number = newNumber;
    }
 
    function increment() public {
        number++;
    }
 
    function getCount() public view returns (uint256) {
        return number;
    }
}
```

And the test code to the following:

```solidity
// SPDX-License-Identifier: UNLICENSED
pragma solidity ^0.8.13;
 
import {Test, console} from "forge-std/Test.sol";
import {Counter} from "../src/Counter.sol";
 
contract CounterTest is Test {
    Counter public counter;
 
    function setUp() public {
        counter = new Counter();
        counter.setNumber(0);
    }
 
    function test_Increment() public {
        counter.increment();
        assertEq(counter.number(), 1);
    }
 
    function testFuzz_SetNumber(uint256 x) public {
        counter.setNumber(x);
        assertEq(counter.number(), x);
    }
 
    function test_GetCount() public {
        uint256 initialCount = counter.getCount();
        counter.increment();
        assertEq(counter.getCount(), initialCount + 1);
    }
}
```

Run the tests with the following command:

```bash
$ forge test
```

If tests pass, deploy the contract to the GateChain chain with the following command:

```bash
$ forge create --rpc-url $GateChain_NODE_URI --mnemonic $MNEMONIC src/Counter.sol:Counter
```

Where `$GateChain_NODE_URI` is the URI of the GateChain node and `$MNEMONIC` is the mnemonic of the account that will deploy the contract. If you run local GateChain node, the address will be `http://localhost:8545`, otherwise you could grab a evm_rpc url from the registry. If deployment is successful, you will get the EVM contract address in the output:

```
[⠒] Compiling...
No files changed, compilation skipped
Deployer: $0X_DEPLOYER_ADDRESS
Deployed to: $0X_CONTRACT_ADDRESS
Transaction hash: $0X_TX_HASH
```

Let's use the cast command to query the contract:

```bash
$ cast call $0X_CONTRACT_ADDRESS "getCount()(uint256)" --rpc-url $GateChain_NODE_URI
```

The command should return 0 as the initial value of the counter.

Now we can use the cast command to call the increment function:

```bash
$ cast send $0X_CONTRACT_ADDRESS "increment()" --mnemonic $MNEMONIC --rpc-url $GateChain_NODE_URI
```

If command is successful, you will get the transaction hash and other info back.

Now let's call the getCount function again and this case it should return 1.

## Calling Contract from JS Client

To call contract from frontend, you could use ethers like:

```javascript
import {ethers} from "ethers";
 
const privateKey = <Your Private Key>;
const evmRpcEndpoint = <Your Evm Rpc Endpoint>
const provider = new ethers.JsonRpcProvider(evmRpcEndpoint);
const signer = new ethers.Wallet(privateKey, provider);
 
if (!signer) {
    console.log('No signer found');
    return;
}

const abi = [
 {
      "type": "function",
      "name": "setNumber",
      "inputs": [
        {
          "name": "newNumber",
          "type": "uint256",
          "internalType": "uint256"
        }
      ],
      "outputs": [],
      "stateMutability": "nonpayable"
    },
    {
        "type": "function",
        "name": "getCount",
        "inputs": [],
        "outputs": [
            {
                "name": "",
                "type": "int256",
                "internalType": "int256"
            }
        ],
        "stateMutability": "view"
    },
    {
        "type": "function",
        "name": "increment",
        "inputs": [],
        "outputs": [],
        "stateMutability": "nonpayable"
    }
];
 
// Define the address of the deployed contract
const contractAddress = 0X_CONTRACT_ADDRESS;
 
// Create a new instance of the ethers.js Contract object
const contract = new ethers.Contract(contractAddress, abi, signer);
 
// Call the contract's functions
async function getCount() {
    const count = await contract.getCount();
    console.log(count.toString());
}
 
async function increment() {
    const txResponse = await contract.increment();
    const mintedTx = await txResponse.wait();
    console.log(mintedTx);
}
 
await increment();
await getCount();
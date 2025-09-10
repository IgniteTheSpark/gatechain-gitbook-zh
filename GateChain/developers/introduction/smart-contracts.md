# EVM (概述)

## 概述

以太坊虚拟机（EVM）是智能合约的运行时环境，可实现与基于以太坊的 dApp 的兼容性。GateChain 是一个 EVM 兼容的区块链。GateChain 的并行化 EVM 确保了高性能和高效率。

以下是关于 EVM 的一些关键点：

- **图灵完备性**: EVM 是图灵完备的，这意味着它可以执行任何可计算的函数。这使开发者能够编写复杂的智能合约。
- **Gas**: 在 EVM 兼容网络上的交易和合约执行都需要消耗 gas。Gas 是计算工作量的度量单位，在 GateChain 网络上用户需要支付 gas 费用。Gas 确保恶意或低效的代码不会使网络过载。
- **字节码执行**: 智能合约被编译成字节码（低级机器可读指令）并部署到 EVM 兼容网络。EVM 执行这些字节码。

## 智能合约语言

在 EVM 上开发智能合约最流行的两种语言是 Solidity 和 Vyper。

### Solidity

- 面向对象的高级语言，用于实现智能合约。
- 大括号语言，主要受 C++ 影响。
- 静态类型（变量类型在编译时确定）。
- 支持：
  - 继承（你可以扩展其他合约）
  - 库（你可以创建可重用的代码，可以从不同的合约中调用 - 类似于其他面向对象编程语言中静态类中的静态函数）
  - 复杂的用户自定义类型

### Solidity 合约示例

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

## 在 GateChain 上部署 EVM 合约

由于 GateChain 是一个 EVM 兼容链，现有的 EVM 工具如 hardhat、foundry forge 等都可以重复使用。

在这个例子中，我们将使用 foundry 工具。

1. 按照这个[安装指南](https://book.getfoundry.sh/getting-started/installation)安装 foundry 工具。
2. 按照[创建新项目指南](https://book.getfoundry.sh/projects/creating-a-new-project)创建新项目。
3. 确保你在 GateChain 网络上有一个钱包。

项目创建后，通过添加 getCounter 函数来修改合约代码：

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

运行以下命令进行测试：

```bash
$ forge test
```

如果测试通过，使用以下命令将合约部署到 GateChain 链上：

```bash
$ forge create --rpc-url $GateChain_NODE_URI --mnemonic $MNEMONIC src/Counter.sol:Counter
```

其中 `$GateChain_NODE_URI` 是 GateChain 节点的 URI，`$MNEMONIC` 是将部署合约的账户的助记词。如果你运行本地 GateChain 节点，地址将是 `http://localhost:8545`，否则你可以从注册表中获取 evm_rpc url。如果部署成功，你将在输出中获得 EVM 合约地址：

```
[⠒] Compiling...
No files changed, compilation skipped
Deployer: $0X_DEPLOYER_ADDRESS
Deployed to: $0X_CONTRACT_ADDRESS
Transaction hash: $0X_TX_HASH
```

让我们使用 cast 命令查询合约：

```bash
$ cast call $0X_CONTRACT_ADDRESS "getCount()(uint256)" --rpc-url $GateChain_NODE_URI
```

该命令应返回 0 作为计数器的初始值。

现在我们可以使用 cast 命令调用 increment 函数：

```bash
$ cast send $0X_CONTRACT_ADDRESS "increment()" --mnemonic $MNEMONIC --rpc-url $GateChain_NODE_URI
```

如果命令成功，你将收到交易哈希和其他信息。

现在让我们再次调用 getCount 函数，这次应该返回 1。

## 从 JS 客户端调用合约

要从前端调用合约，你可以使用 ethers，如下所示：

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
```

测试代码如下：

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
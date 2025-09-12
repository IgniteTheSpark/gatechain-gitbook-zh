# Deploy Smart Contract to GateChain Mainnet using Hardhat

This guide provides a step-by-step tutorial on how to deploy smart contracts on GateChain Mainnet using Hardhat - a popular Ethereum development environment. You'll learn how to set up a Hardhat project, configure it for GateChain, write a simple smart contract, and deploy it to the GateChain Mainnet. This tutorial is perfect for developers who are familiar with Ethereum development and want to start building on GateChain.
## 1. Create Hardhat Project

### 1.1 Create Project Directory
```bash
mkdir hardhat && cd hardhat
```

### 1.2 Initialize Project (Create package.json)
```bash
npm init -y
```
![console output](../../.gitbook/assets/images/init_package.png)


### 1.3 Install Hardhat
```bash
npm install hardhat
```

### 1.4 Create Hardhat Project
```bash
npx hardhat init
```
![console output](../../.gitbook/assets/images/init_hardhat.png)

### 1.5 Select Project Type
Choose `Create an empty hardhat.config.js` option, which will create a basic Hardhat configuration file for your project.

## 2. Modify Hardhat Configuration File

### 2.1 Configure hardhat.config.js

Main configuration contents:
- Solidity compiler version: 0.8.18
- GateChain Mainnet EVM RPC address options:
  - https://evm.nodeinfo.cc (recommended)
  - https://evm-1.nodeinfo.cc
  - https://evm.gatenode.cc
  - https://evm-hk.gatenode.cc (Hong Kong node)
- chainId: 86 (GateChain Mainnet Chain ID)

```javascript
require("@nomicfoundation/hardhat-toolbox");
require("@nomiclabs/hardhat-web3");

/** @type import('hardhat/config').HardhatUserConfig */

const privateKey = 'INSERT_PRIVATE_KEY';

module.exports = {
  solidity: '0.8.18',
  networks: {
    Mainnet: {
      url: 'https://evm.nodeinfo.cc', 
      chainId: 86, 
      accounts: [privateKey],
      gasPrice: 10000000000, // 10 gwei
    },
  },
};
```

## 3. Write Contract File

### 3.1 Create Contracts Directory
```bash
mkdir contracts
```

### 3.2 Create Contract File
```bash
touch contracts/Storage.sol
```

### 3.3 Contract Content
```solidity
pragma solidity ^0.8.0;

contract Storage {
    uint storedData;

    function set(uint x) public {
        storedData = x;
    }

    function get() public view returns (uint) {
        return storedData;
    }
}
```

## 4. Compile Solidity

### 4.1 Install Dependencies
```bash
npm install @nomicfoundation/hardhat-toolbox@^2.0.2 @nomiclabs/hardhat-web3@^2.0.0
```

### 4.2 Compile Contract
```bash
npx hardhat compile
```
![compile result](../../.gitbook/assets/images/compile_contract.png)


After compilation, an `artifacts` directory will be created in the project, containing the contract bytecode and metadata (.json files). It is recommended to add this directory to .gitignore.

## 5. Deploy Contract

### 5.1 Create Deployment Script
```bash
mkdir scripts && touch scripts/deploy.js
```

### 5.2 Deployment Script Content
```javascript
const { ethers } = require("hardhat");
async function deploy(deployer) {
    const Contract = await ethers.getContractFactory("Storage", deployer);
    const contract = await Contract.deploy();
    await contract.deployed();
    console.log("Contract deployed to address:", contract.address);
    return contract.address;
}

async function call(contractAddress, deployer) {
    const Contract = await ethers.getContractFactory("Storage",deployer);
    const contract = await Contract.attach(contractAddress);
    const number = 22;
    console.log("Write to contract:", number);
    const tx = await contract.set(number);

    receipt = await tx.wait();
    console.log(receipt);

    const counter = await contract.get();
    console.log("Read from contract:", counter);
}

async function main(){
    const [deployer] = await ethers.getSigners();
    console.log(
        "Deploying contracts with the account:",
        deployer.address
    );

    const address = await deploy(deployer);
    await call(address,deployer);
}

main() .then(() => process.exit(0))
    .catch(error => {
        console.error(error);
        process.exit(1);
    });
```

### 5.3 Run Deployment Script
```bash
npx hardhat run scripts/deploy.js --network Mainnet
```
![deploy result](../../.gitbook/assets/images/deploy_contract.png)

## 6. Query Transactions

After deployment, you can view the contract interaction details on the GateChain block explorer.

Important Notes:
- Make sure to replace the `privateKey` in the configuration file with your own private key
- Ensure you have enough GT in your account to pay for gas fees before deployment
- It is recommended to test on the testnet before deploying to mainnet 
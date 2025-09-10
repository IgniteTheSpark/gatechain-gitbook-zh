# Deploying a Smart Contract

Because Gate Layer is fully EVM-equivalent, developers can continue to use the entire suite of Ethereum tools they are familiar with (such as Hardhat, Foundry, Remix, Ethers.js, Web3.js, etc.) to develop and deploy smart contracts with almost no code changes.

This guide will use **Hardhat**, one of the most popular development frameworks, to walk you through the entire process of deploying a simple `Greeter` contract, from project initialization to final deployment.

---

### 1. Prerequisites

Before you begin, please ensure you have the following ready:

*   **Node.js**: `v18` or later.
*   **Wallet Account**: A personal wallet account with its private key.
*   **Testnet Tokens**: Make sure your wallet has some GT tokens on the Gate Layer Testnet to pay for gas. You can get them from the [Faucet]().
*   **Gate Layer Network Information**:
    *   **Testnet**:
        *   **RPC URL**: `http://gatelayer-testnet.gatenode.cc:8545`
        *   **Chain ID**: `10087`
    *   **Mainnet**:
        *   **RPC URL**: `(TBD)`
        *   **Chain ID**: `(TBD)`

---

### 2. Initialize a Hardhat Project

First, let's create a new project directory and initialize Hardhat.

```bash
# 1. Create a project folder and navigate into it
mkdir gatelayer-greeter && cd gatelayer-greeter

# 2. Initialize an npm project
npm init -y

# 3. Install Hardhat
npm install --save-dev hardhat

# 4. Run the Hardhat wizard
npx hardhat
```

In the Hardhat wizard, select `Create a JavaScript project` and accept all the default settings.

Next, install the Hardhat toolbox and `dotenv` for managing environment variables.
```bash
npm install --save-dev @nomicfoundation/hardhat-toolbox dotenv
```

---

### 3. Write the Smart Contract

Hardhat automatically creates a `contracts/Lock.sol` file during initialization. You can delete it and then create a new file, `contracts/Greeter.sol`, with the following code:

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.9;

/**
 * @title Greeter
 * @dev A simple smart contract for storing and retrieving a greeting.
 */
contract Greeter {
    string private greeting;

    constructor(string memory _greeting) {
        greeting = _greeting;
    }

    function greet() public view returns (string memory) {
        return greeting;
    }

    function setGreeting(string memory _greeting) public {
        greeting = _greeting;
    }
}
```

---

### 4. Configure the Gate Layer Network

To protect your private key and RPC URL, we'll use a `.env` file to manage them.

1.  Create a file named `.env` in the root of your project and add the following content (replace the private key with your own):

    ```
    GATELAYER_TESTNET_RPC_URL="http://gatelayer-testnet.gatenode.cc:8545"
    PRIVATE_KEY="YOUR_WALLET_PRIVATE_KEY"

    # Mainnet RPC URL (update once available)
    # GATELAYER_MAINNET_RPC_URL=""
    ```
    > **Security Tip**: The `.env` file should not be committed to any public repository (like GitHub). Make sure it's added to your `.gitignore` file.

2.  Modify the `hardhat.config.js` file to import the environment variables and configure the Gate Layer testnet:

    ```javascript
    require("@nomicfoundation/hardhat-toolbox");
    require("dotenv").config();

    /** @type import('hardhat/config').HardhatUserConfig */
    module.exports = {
      solidity: "0.8.9",
      networks: {
        gatelayer_testnet: {
          url: process.env.GATELAYER_TESTNET_RPC_URL || "",
          accounts:
            process.env.PRIVATE_KEY !== undefined ? [process.env.PRIVATE_KEY] : [],
          chainId: 10087,
        },
        // Mainnet Configuration (once available)
        /*
        gatelayer_mainnet: {
          url: process.env.GATELAYER_MAINNET_RPC_URL || "",
          accounts:
            process.env.PRIVATE_KEY !== undefined ? [process.env.PRIVATE_KEY] : [],
          chainId: <MAINNET_CHAIN_ID>, // TBD
        },
        */
      },
    };
    ```

---

### 5. Write the Deployment Script

Hardhat also generates a sample deployment script at `scripts/deploy.js`. Replace its contents with the following code to deploy our `Greeter` contract:

```javascript
const hre = require("hardhat");

async function main() {
  const initialGreeting = "Hello, Gate Layer!";
  
  // Deploy the contract
  const greeter = await hre.ethers.deployContract("Greeter", [initialGreeting]);
  await greeter.waitForDeployment();

  // Log the deployment information
  console.log(
    `Greeter with greeting "${initialGreeting}" deployed to ${greeter.target}`
  );
}

// Execute the main function and handle errors
main().catch((error) => {
  console.error(error);
  process.exitCode = 1;
});
```

---

### 6. Compile and Deploy

Everything is now ready.

1.  **Compile the contract**:
    ```bash
    npx hardhat compile
    ```
    If everything is correct, you'll see a success message.

2.  **Deploy to the Gate Layer Testnet**:
    ```bash
    npx hardhat run scripts/deploy.js --network gatelayer_testnet
    ```

Upon successful deployment, you will see output in your terminal similar to this, which includes the address where your contract was deployed:

```
Greeter with greeting "Hello, Gate Layer!" deployed to 0x5FbDB2315678afecb367f032d93F642f64180aa3
```

Congratulations! You have successfully deployed a smart contract to Gate Layer. This process is identical to deploying to Ethereum or any other EVM chain, highlighting the EVM-equivalence of Gate Layer.

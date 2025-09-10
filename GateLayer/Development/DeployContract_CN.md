# 部署智能合约 (Deploying a Smart Contract)

由于 Gate Layer 是完全 EVM 等效的，开发者可以继续使用他们所熟悉的以太坊全套工具链（如 Hardhat, Foundry, Remix, Ethers.js, Web3.js 等）来开发和部署智能合约，几乎无需任何代码修改。

本指南将以最流行的开发框架 **Hardhat** 为例，引导您完成从项目初始化到最终部署一个 `Greeter` 合约的全过程。

---

### 1. 环境准备 (Prerequisites)

在开始之前，请确保您已准备好以下环境：

*   **Node.js**: `v18` 或更高版本。
*   **钱包账户**: 一个拥有私钥的个人钱包账户。
*   **测试网代币**: 确保您的钱包在 Gate Layer 测试网上有一些 GT 代币用于支付 Gas。您可以从 [水龙头]() 获取。
*   **Gate Layer 网络信息**:
    *   **Testnet**:
        *   **RPC URL**: `http://gatelayer-testnet.gatenode.cc:8545`
        *   **Chain ID**: `10087`
    *   **Mainnet**:
        *   **RPC URL**: `(待上线)`
        *   **Chain ID**: `(待上线)`

---

### 2. 初始化 Hardhat 项目

首先，创建一个新的项目目录并初始化 Hardhat。

```bash
# 1. 创建项目文件夹并进入
mkdir gatelayer-greeter && cd gatelayer-greeter

# 2. 初始化 npm 项目
npm init -y

# 3. 安装 Hardhat
npm install --save-dev hardhat

# 4. 运行 Hardhat 向导
npx hardhat
```

在 Hardhat 向导中，选择 `Create a JavaScript project` 并接受所有默认设置。

接着，安装 Hardhat 工具箱和 `dotenv` 用于管理环境变量。
```bash
npm install --save-dev @nomicfoundation/hardhat-toolbox dotenv
```

---

### 3. 编写智能合约

Hardhat 初始化时会自动创建一个 `contracts/Lock.sol` 文件。您可以将其删除，然后创建一个新的文件 `contracts/Greeter.sol`，并填入以下代码：

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.9;

/**
 * @title Greeter
 * @dev 一个简单的智能合约，用于存储和返回一个问候语。
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

### 4. 配置 Gate Layer 网络

为了保护您的私钥和 RPC URL，我们使用 `.env` 文件来管理它们。

1.  在项目根目录下创建一个名为 `.env` 的文件，并填入以下内容（请将私钥替换为您自己的）：

    ```
    GATELAYER_TESTNET_RPC_URL="http://gatelayer-testnet.gatenode.cc:8545"
    PRIVATE_KEY="YOUR_WALLET_PRIVATE_KEY"
    
    # 主网 RPC URL (待上线后更新)
    # GATELAYER_MAINNET_RPC_URL=""
    ```
    > **安全提示**: `.env` 文件不应提交到任何公共代码库（如 GitHub）。请确保已将其添加到 `.gitignore` 文件中。

2.  修改 `hardhat.config.js` 文件，引入环境变量并配置 Gate Layer 测试网：

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
        // 主网配置 (待上线)
        /*
        gatelayer_mainnet: {
          url: process.env.GATELAYER_MAINNET_RPC_URL || "",
          accounts:
            process.env.PRIVATE_KEY !== undefined ? [process.env.PRIVATE_KEY] : [],
          chainId: <MAINNET_CHAIN_ID>, // 待定
        },
        */
      },
    };
    ```

---

### 5. 编写部署脚本

Hardhat 同样会自动生成一个示例部署脚本 `scripts/deploy.js`。将其内容替换为以下代码，用于部署我们的 `Greeter` 合约：

```javascript
const hre = require("hardhat");

async function main() {
  const initialGreeting = "Hello, Gate Layer!";
  
  // 部署合约
  const greeter = await hre.ethers.deployContract("Greeter", [initialGreeting]);
  await greeter.waitForDeployment();

  // 打印部署信息
  console.log(
    `Greeter with greeting "${initialGreeting}" deployed to ${greeter.target}`
  );
}

// 执行主函数并处理错误
main().catch((error) => {
  console.error(error);
  process.exitCode = 1;
});
```

---

### 6. 编译和部署

现在，一切准备就绪。

1.  **编译合约**:
    ```bash
    npx hardhat compile
    ```
    如果一切顺利，您会看到编译成功的提示。

2.  **部署到 Gate Layer 测试网**:
    ```bash
    npx hardhat run scripts/deploy.js --network gatelayer_testnet
    ```

部署成功后，您将在终端看到类似以下的输出，其中包含了您的合约被部署到的地址：

```
Greeter with greeting "Hello, Gate Layer!" deployed to 0x5FbDB2315678afecb367f032d93F642f64180aa3
```

恭喜！您已成功将一个智能合约部署到了 Gate Layer。这个流程与部署到以太坊或其他 EVM 链完全一致，充分体现了 Gate Layer 的 EVM 等效性。

# 验证智能合约

在成功部署您的智能合约之后，强烈建议您在 Gate Layer 的区块浏览器 (GateScan) 上验证您的合约源代码。完成验证不仅可以增强项目的透明度和可信度，还能让用户和开发者直接在浏览器上与您的合约进行交互。

由于 GateScan 是基于 Blockscout 构建的，它与 Etherscan 的合约验证 API 兼容，因此您可以使用 Hardhat 或 Foundry 等主流开发框架轻松地自动化验证过程。

---

## 1. 通过开发框架验证

### 使用 Hardhat 验证

要通过 Hardhat 自动验证合约，您需要安装 `hardhat-etherscan` 插件，并在 `hardhat.config.js` 文件中添加 `etherscan` 的相关配置。

1.  **安装插件**（如果您在初始化项目时使用的是 `@nomicfoundation/hardhat-toolbox`，则该插件已包含在内，无需重复安装）：
    ```bash
    npm install --save-dev @nomiclabs/hardhat-etherscan
    ```

2.  **配置 `hardhat.config.js`**：
    在 `networks` 配置的同级添加 `etherscan` 字段。

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
      etherscan: {
        // apiKey 字段是必需的，但对于 GateScan 可以填写任意字符串
        apiKey: {
          gatelayer_testnet: 'any-api-key-string',
          // gatelayer_mainnet: 'any-api-key-string' // 主网上线后取消注释
        },
        customChains: [
          {
            network: "gatelayer_testnet",
            chainId: 10087,
            urls: {
              // GateScan 的 API 端点
              apiURL: "http://gatescan.org/gatelayer-testnet/api",
              // GateScan 的浏览器 URL
              browserURL: "http://gatescan.org/gatelayer-testnet/"
            }
          },
          /*
          {
            network: "gatelayer_mainnet",
            chainId: <MAINNET_CHAIN_ID>, // 待定
            urls: {
              apiURL: "<MAINNET_API_URL>", // 待定
              browserURL: "<MAINNET_BROWSER_URL>" // 待定
            }
          }
          */
        ]
      },
    };
    ```

3.  **运行验证命令**：
    在部署合约后，使用以下命令进行验证。请将 `CONTRACT_ADDRESS` 替换为您部署的实际合约地址，并将构造函数参数（如果有的话）附加在后面。

    ```bash
    # 示例：验证 Greeter 合约
    npx hardhat verify --network gatelayer_testnet CONTRACT_ADDRESS "Hello, Gate Layer!"
    ```

    成功后，您将在终端看到验证成功的链接。

### 使用 Foundry 验证

如果您使用 Foundry，可以通过 `forge verify-contract` 命令进行验证。

```bash
forge verify-contract <CONTRACT_ADDRESS> <CONTRACT_NAME> \
  --chain-id 10087 \
  --verifier-url http://gatescan.org/gatelayer-testnet/api \
  --verifier blockscout
```
*   将 `<CONTRACT_ADDRESS>` 替换为您的合约地址。
*   将 `<CONTRACT_NAME>` 替换为您的合约名称（例如 `Greeter`）。

---

## 2. 通过 GateScan 前端手动验证

如果您倾向于手动操作，也可以直接访问 GateScan 浏览器来验证您的合约。

1.  访问 [GateScan](http://gatescan.org/gatelayer-testnet/) 并导航到您的合约地址页面。
2.  找到并点击 `Contract` 标签页下的 `Verify & Publish` 按钮。

GateScan 支持多种源代码格式，最常用的包括：

*   **Solidity (Flattened Source Code)**: 将所有 Solidity 源文件和依赖项合并（"flatten"）成一个单一的文件进行上传。您可以使用 `npx hardhat flatten` 或 `forge flatten` 命令来生成此文件。
*   **Solidity (Standard JSON Input)**: 上传由 `solc` 编译器生成的标准 JSON 输入文件。Remix IDE 或 Hardhat 在编译时也会在 `artifacts/build-info/` 目录下生成这些文件。
*   **Solidity (Multi-part files)**: 分别上传多个 `.sol` 源文件，并确保导入路径正确。

在上传时，请确保您选择的**编译器版本 (Compiler)**和**优化设置 (Optimization)**与您部署时使用的设置完全一致。

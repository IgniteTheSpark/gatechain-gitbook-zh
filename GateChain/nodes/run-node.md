# GateChain 全节点设置指南


## 支持的平台
GateChain 全节点目前支持 Unix 环境（macOS、Ubuntu、CentOS）。

## 硬件要求
硬件必须满足以下要求才能运行全节点：

- 操作系统：Mac OS 10.14.6 或更高版本，CentOS Linux release 7.7.1908 或更高版本，或 Ubuntu 18.04.2 或更高版本
- 4 核 CPU，8GB+ 内存，以及 100GB+ 磁盘空间
- 稳定的网络连接，带宽至少 1MB/s

## 安装步骤

### 方法一：自动安装脚本
注意：请确保您的环境中已安装 "wget"

我们在 GitHub 上维护了一个安装脚本（"install.sh"），用于处理链可执行文件的设置。该脚本使用以下默认设置：

可执行文件放置在 "/usr/local/bin"（即 "gated" "gatecli"）

```bash
# 一键安装（需要代理）
bash <(wget -qO- https://raw.githubusercontent.com/gatechain/node-binary/master/node/install.sh)

# 中国用户一键安装（无需代理）
bash <(wget -qO- https://gatechainbucket.oss-cn-beijing.aliyuncs.com/auto-install.sh)
```

注意：如果您是第二次或多次安装，需要先手动删除本地 gated/gatecli 文件。

### 方法二：自定义安装 1
我们目前使用此仓库存储已编译节点二进制文件的历史版本。

1. 安装 Git LFS
Git LFS 是用于管理大文件的 Git 扩展。请访问 https://git-lfs.github.com/ 进行安装。

2. 克隆仓库：
```bash
git lfs clone https://github.com/gatechain/node-binary.git
```

3. 根据更新日志选择相应版本的二进制文件：
```bash
cd node-binary/node/{network}/{version}/{os_type}
```

4. 将二进制文件复制到 "/usr/local/bin"：
```bash
cp gated gatecli /usr/local/bin
```

5. 创建 GateChain 节点根目录 $GATEHOME（即 ~/.gated）：
```bash
mkdir ~/.gated
```

6. 从 "node-binary/node/{network}/{version}/config/" 复制 "config.json" 和 "genesis.json" 到 "$GATEHOME/"

### 方法三：自定义安装 2
用户可以根据系统要求选择下载链接。

注意：请确保您的环境中已安装 "wget"

#### macOS 系统：
```bash
# 下载 gated
wget https://gatechainbucket.oss-cn-beijing.aliyuncs.com/binary/mac/gated

# 下载 gatecli
wget https://gatechainbucket.oss-cn-beijing.aliyuncs.com/binary/mac/gatecli
```

#### Linux 系统：
```bash
# 下载 gated
wget https://gatechainbucket.oss-cn-beijing.aliyuncs.com/binary/linux/gated

# 下载 gatecli
wget https://gatechainbucket.oss-cn-beijing.aliyuncs.com/binary/linux/gatecli
```

#### 下载配置文件：
```bash
# 下载 config.json
wget https://gatechainbucket.oss-cn-beijing.aliyuncs.com/config/config.json

# 下载 genesis.json
wget https://gatechainbucket.oss-cn-beijing.aliyuncs.com/config/genesis.json
```

将下载的二进制文件复制到 "/usr/local/bin"：
```bash
cp gated gatecli /usr/local/bin
```

创建节点根目录：
```bash
mkdir ~/.gated
```

将配置文件复制到 "$GATEHOME/"

### 方法四：Docker 安装
Docker 提供了一种在容器中运行 GateChain 节点的便捷方式。官方 Docker 镜像可在 [GateChain Node Binary Repository](https://github.com/gatechain/node-binary) 获取。

#### 前置要求
- 系统已安装 Docker
- 系统已安装 Docker Compose
- 至少 100GB 可用磁盘空间

#### 设置步骤

1. 创建 Docker 卷目录：
```bash
mkdir -p ~/.gatechain_docker/{data,logs}
```

2. 创建 `docker-compose.yml` 文件，内容如下：
```yaml
version: '3'

services:
  gated:
    image: gatechain:latest
    container_name: gatechain-node
    volumes:
      - ~/.gatechain_docker/data:/root/.gated
      - ~/.gatechain_docker/logs:/root/log
    command: sh -c "mkdir -p /root/log && gated start > /root/log/node.log 2>&1"
    restart: always
    ports:
      - "8080:8080"
      - "8081:8081"

  evm-rest:
    image: gatechain:latest
    container_name: gatechain-evm-rest
    volumes:
      - ~/.gatechain_docker/data:/root/.gated
      - ~/.gatechain_docker/data:/root/.gatecli
      - ~/.gatechain_docker/logs:/root/log
    depends_on:
      - gated
    command: sh -c "sleep 10 && gatecli evm rest-server --gm-websocket-port http://gatechain-node:8081 --chain-id gate-66 --laddr tcp://0.0.0.0:9545 --node http://gatechain-node:8080  --rpc-api web3,eth,personal,net,debug > /root/log/evm_rpc.log 2>&1"
    restart: always
    ports:
      - "9545:9545"

  rest-server:
    image: gatechain:latest
    container_name: gatechain-rest
    volumes:
      - ~/.gatechain_docker/data:/root/.gated
      - ~/.gatechain_docker/data:/root/.gatecli
      - ~/.gatechain_docker/logs:/root/log
    depends_on:
      - gated
      - evm-rest
    command: sh -c "gatecli rest-server --chain-id gate-66 --node http://gatechain-node:8080 --gm-websocket-port  http://gatechain-node:8081 --laddr tcp://0.0.0.0:1317 > /root/log/rest.log 2>&1"
    restart: always
    ports:
      - "1317:1317"
```

3. 启动服务：
```bash
docker-compose up -d
```

4. 查看日志：
```bash
# 查看节点日志
tail -f ~/.gatechain_docker/logs/node.log
# 查看 EVM RPC 日志
tail -f ~/.gatechain_docker/logs/evm_rpc.log
# 查看 REST 服务器日志
tail -f ~/.gatechain_docker/logs/rest.log
```

该设置包含三个服务：
- `gated`：主要的 GateChain 节点
- `evm-rest`：用于以太坊兼容性的 EVM RPC 服务
- `rest-server`：GateChain 的 REST API 服务

#### 开放端口
- 8080：节点 P2P 通信
- 8081：WebSocket 端口
- 9545：EVM RPC 端点
- 1317：REST API 端点

#### 数据持久化
所有数据都持久化存储在 `~/.gatechain_docker/data` 中，日志存储在 `~/.gatechain_docker/logs` 中。

## 启动节点

### 基本启动
```bash
gated start
```

### GC 模式启动
要在 GC 模式下启动，修改启动命令：
```bash
gated start --pruning nothing
```

### 启动 EVM RPC
```bash
gatecli evm rest-server --gm-websocket-port http://127.0.0.1:8085 --chain-id mainnet --laddr tcp://0.0.0.0:6060 --rpc-api web3,eth,personal,net,debug
```

对于 EVM RPC 支持，修改 config.json 中的以下属性：
```json
"WsPort": "tcp://0.0.0.0:8085"
"IsWebSocketServerActive": true
```
保存更改并重启 gated。

### 与节点交互
您可以使用 [gatecli 命令行界面](../api/http.md) 或 [RPC API](../api/http.md) 与您的本地节点进行交互。
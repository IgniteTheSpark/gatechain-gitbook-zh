# GateChain Full Node Setup Guide


## Supported Platforms
GateChain full node currently supports Unix environments (macOS, Ubuntu, CentOS).

## Hardware Requirements
Hardware must meet certain requirements to run a full node:

- Operating System: Mac OS 10.14.6 or above, CentOS Linux release 7.7.1908 or above, or Ubuntu 18.04.2 or above
- 4 CPU cores, 8GB+ RAM, and 100GB+ disk space
- Stable internet connection with at least 1MB/s bandwidth

## Installation Steps

### Method 1: Automated Installation Script
Note: Please ensure "wget" is installed in your environment

We maintain an installation script ("install.sh") on GitHub that handles the setup of chain executables. This uses the following defaults:

Executables are placed in "/usr/local/bin" (i.e., "gated" "gatecli")

```bash
# One-click installation (requires proxy)
bash <(wget -qO- https://raw.githubusercontent.com/gatechain/node-binary/master/node/install.sh)

# One-click installation for users in China (no proxy needed)
bash <(wget -qO- https://gatechainbucket.oss-cn-beijing.aliyuncs.com/auto-install.sh)
```

Note: If you're installing for the second time or more, you need to manually delete the local gated/gatecli files first.

### Method 2: Custom Installation 1
We currently use this repository to store historical versions of compiled node binaries.

1. Install Git LFS
Git LFS is a Git extension for managing large files. Please visit https://git-lfs.github.com/ for installation.

2. Clone the repository:
```bash
git lfs clone https://github.com/gatechain/node-binary.git
```

3. Select the corresponding version binary according to the changelog:
```bash
cd node-binary/node/{network}/{version}/{os_type}
```

4. Copy binaries to "/usr/local/bin":
```bash
cp gated gatecli /usr/local/bin
```

5. Create root directory for GateChain node $GATEHOME (i.e., ~/.gated):
```bash
mkdir ~/.gated
```

6. Copy "config.json" and "genesis.json" from "node-binary/node/{network}/{version}/config/" to "$GATEHOME/"

### Method 3: Custom Installation 2
Users can choose download links based on their system requirements.

Note: Please ensure "wget" is installed in your environment

#### For macOS:
```bash
# Download gated
wget https://gatechainbucket.oss-cn-beijing.aliyuncs.com/binary/mac/gated

# Download gatecli
wget https://gatechainbucket.oss-cn-beijing.aliyuncs.com/binary/mac/gatecli
```

#### For Linux:
```bash
# Download gated
wget https://gatechainbucket.oss-cn-beijing.aliyuncs.com/binary/linux/gated

# Download gatecli
wget https://gatechainbucket.oss-cn-beijing.aliyuncs.com/binary/linux/gatecli
```

#### Download Configuration Files:
```bash
# Download config.json
wget https://gatechainbucket.oss-cn-beijing.aliyuncs.com/config/config.json

# Download genesis.json
wget https://gatechainbucket.oss-cn-beijing.aliyuncs.com/config/genesis.json
```

Copy downloaded binaries to "/usr/local/bin":
```bash
cp gated gatecli /usr/local/bin
```

Create node root directory:
```bash
mkdir ~/.gated
```

Copy configuration files to "$GATEHOME/"

### Method 4: Docker Installation
Docker provides a convenient way to run GateChain nodes in containers. The official Docker image is available at [GateChain Node Binary Repository](https://github.com/gatechain/node-binary).

#### Prerequisites
- Docker installed on your system
- Docker Compose installed on your system
- At least 100GB of free disk space

#### Setup Steps

1. Create a directory for Docker volumes:
```bash
mkdir -p ~/.gatechain_docker/{data,logs}
```

2. Create a `docker-compose.yml` file with the following content:
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

3. Start the services:
```bash
docker-compose up -d
```

4. Check the logs:
```bash
# View node logs
tail -f ~/.gatechain_docker/logs/node.log
# View EVM RPC logs
tail -f ~/.gatechain_docker/logs/evm_rpc.log
# View REST server logs
tail -f ~/.gatechain_docker/logs/rest.log
```

The setup includes three services:
- `gated`: The main GateChain node
- `evm-rest`: EVM RPC service for Ethereum compatibility
- `rest-server`: REST API service for GateChain

#### Exposed Ports
- 8080: Node P2P communication
- 8081: WebSocket port
- 9545: EVM RPC endpoint
- 1317: REST API endpoint

#### Data Persistence
All data is persisted in `~/.gatechain_docker/data` and logs in `~/.gatechain_docker/logs`.

## Starting the Node

### Basic Start
```bash
gated start
```

### Start in Archive Mode (Disable Pruning)
To start in archive mode, which preserves all historical states and disables pruning, modify the start command:
```bash
gated start --pruning nothing
```

### Start EVM RPC
```bash
gatecli evm rest-server --gm-websocket-port http://127.0.0.1:8085 --chain-id mainnet --laddr tcp://0.0.0.0:6060 --rpc-api web3,eth,personal,net,debug
```
Note: To support pre-EIP1599 transactions, add the `allow-unprotected-txs` parameter when starting the EVM RPC Server.

For EVM RPC support, modify the following properties in config.json:
```json
"WsPort": "tcp://0.0.0.0:8085"
"IsWebSocketServerActive": true
```
Save changes and restart gated.

### Interact with a node
You can interact with your local node using either the [gatecli command line interface](../api/http_EN.md) or the [RPC API](../api/http_EN.md).
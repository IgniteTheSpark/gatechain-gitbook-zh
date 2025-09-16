# Gate Layer RPC 节点部署

> 注意：本文档基于当前“测试网”参数示例编写；生产部署请以实际网络发布说明与参数为准。

本文档描述如何部署 Gate Layer L2 RPC 节点，包括依赖安装、配置文件准备、节点启动与 Blob 参数说明。

---

## 1. 环境准备

依赖项：golang 1.22+ · make · git · gcc · libc-dev

组件：
- `gatelayer-geth`：L2 执行客户端（op-geth）
- `gatelayer-node`：L2 RPC/服务节点（op-node）

设置工作目录：

```bash
export GATELAYER_WORKSPACE=/tmp/gatelayer
mkdir -p "$GATELAYER_WORKSPACE"
cd "$GATELAYER_WORKSPACE"
```

---

## 2. 配置文件准备

### 2.1 下载/准备主节点配置文件

- `rollup.json`
- `genesis.json`

将上述文件保存到 `$GATELAYER_WORKSPACE` 根目录。

### 2.2 生成 JWT 密钥

```bash
openssl rand -hex 32 > jwt.txt
```

### 2.3 拷贝到对应目录

- `genesis.json` 与 `jwt.txt` 放到 `$GATELAYER_WORKSPACE` 根目录
- `rollup.json` 与 `jwt.txt` 放到 `$GATELAYER_WORKSPACE` 根目录

> 说明：`jwt.txt` 将被 op-geth 与 op-node 共同使用（Engine API 鉴权），两端需保持完全一致。

---

## 3. 启动 L2 节点

### 3.1 初始化 op-geth（gatelayer-geth）

如 `genesis.json` 变化需重新执行：

```bash
mkdir -p datadir

gatelayer-geth init \
  --state.scheme=hash \
  --datadir=datadir \
  genesis.json
```

### 3.2 启动 gatelayer-geth（执行层）

```bash
gatelayer-geth \
  --datadir ./datadir \
  --http \
  --http.corsdomain="*" \
  --http.vhosts="*" \
  --http.addr=0.0.0.0 \
  --http.api=web3,debug,eth,txpool,net,engine,miner \
  --ws \
  --ws.addr=0.0.0.0 \
  --ws.port=8546 \
  --ws.origins="*" \
  --ws.api=debug,eth,txpool,net,engine,miner \
  --syncmode=full \
  --gcmode=archive \
  --rollup.sequencer=http://gatelayer-testnet.gatenode.cc \
  --nodiscover \
  --maxpeers=0 \
  --networkid=42069 \
  --authrpc.vhosts="*" \
  --authrpc.addr=0.0.0.0 \
  --authrpc.port=8551 \
  --authrpc.jwtsecret=./jwt.txt
```

### 3.3 启动 gatelayer-node（共识/协调 + RPC）

```bash
gatelayer-node \
  --l2=http://localhost:8551 \
  --l2.jwt-secret=./jwt.txt \
  --sequencer.l1-confs=5 \
  --verifier.l1-confs=4 \
  --rollup.config=./rollup.json \
  --p2p.addr=0.0.0.0 \
  --p2p.static=/ip4/10.0.4.8/tcp/9222/p2p/16Uiu2HAkzFMUK9pFMBbYwD5htccC3fBvmmY9Jd1B72f3xH7YNaG \
  --p2p.enable-admin \
  --l1=http://gatelayer-testnet.gatenode.cc \
  --l1.beacon.ignore=true \
  --l1.trustrpc=true \
  --l1.rpc.kind=basic
```

注意：
- `p2p.static` 可以替换为种子/发现方式（按实际运维策略选择）。
- 确保 `--l2.jwt-secret` 与 op-geth 的 `--authrpc.jwtsecret` 指向同一文件（内容一致）。

---

## 4. Blob（EIP-4844）相关参数

如需启用/接入 L1 Beacon（便于 Blob 处理），对 `gatelayer-node` 增加：

```bash
--l1.beacon.ignore=false \
--l1.beacon=https://api.nodeinfo.cc \
--l1.beacon.fetch-all-sidecars=true
```

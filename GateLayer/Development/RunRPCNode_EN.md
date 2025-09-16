# Gate Layer L2 RPC Node Deployment (Developers)

> Note: This guide is based on the current testnet setup. For production, follow the actual network release notes and parameters.

This guide explains how to deploy a Gate Layer L2 RPC node, including environment setup, configuration files, node startup, and Blob-related parameters.

---

## 1. Environment Setup

Dependencies: golang 1.22+ · make · git · gcc · libc-dev

Components:
- `gatelayer-geth`: L2 execution client (op-geth)
- `gatelayer-node`: L2 RPC/service node (op-node)

Prepare a workspace:

```bash
export GATELAYER_WORKSPACE=/tmp/gatelayer
mkdir -p "$GATELAYER_WORKSPACE"
cd "$GATELAYER_WORKSPACE"
```

---

## 2. Prepare Configuration Files

### 2.1 Download/prepare main configuration files

- `rollup.json`
- `genesis.json`

Save both files to the `$GATELAYER_WORKSPACE` root directory.

### 2.2 Generate JWT secret

```bash
openssl rand -hex 32 > jwt.txt
```

### 2.3 Copy to the corresponding locations

- Put `genesis.json` and `jwt.txt` in the `$GATELAYER_WORKSPACE` root directory
- Put `rollup.json` and `jwt.txt` in the `$GATELAYER_WORKSPACE` root directory

> Note: `jwt.txt` is used by both op-geth and op-node (Engine API auth); the contents must match on both sides.

---

## 3. Start the L2 Node

### 3.1 Initialize op-geth (gatelayer-geth)

Re-run if `genesis.json` changes:

```bash
mkdir -p datadir

gatelayer-geth init \
  --state.scheme=hash \
  --datadir=datadir \
  genesis.json
```

### 3.2 Start gatelayer-geth (execution layer)

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

### 3.3 Start gatelayer-node (coordination + RPC)

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

Notes:
- `p2p.static` can be replaced with your own seed/discovery approach per ops policy.
- Ensure `--l2.jwt-secret` matches op-geth's `--authrpc.jwtsecret` (same file & contents).

---

## 4. Blob (EIP-4844) Parameters

If you need to enable/connect to an L1 Beacon (to handle Blobs), add to `gatelayer-node`:

```bash
--l1.beacon.ignore=false \
--l1.beacon=https://api.nodeinfo.cc \
--l1.beacon.fetch-all-sidecars=true
```

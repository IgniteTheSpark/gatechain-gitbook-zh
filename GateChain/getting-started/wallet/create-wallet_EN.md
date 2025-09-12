# Wallet

The GateChain Wallet allows you to monitor your assets on GateChain. Assets can be native tokens on GateChain, as well as bridged assets from Ethereum, Solana, Polygon and various IBC-enabled chains.

There are a variety of different wallets that are supported on GateChain. Users can choose to submit transactions on GateChain using either their Ethereum or Cosmos-native wallets.

## Overview

GateChain's Account type uses Ethereum's ECDSA secp256k1 curve for keys. Simply put, GateChain's Account is native (compatible) with Ethereum accounts, allowing Ethereum-native wallets, such as MetaMask, to interact with GateChain. Popular Cosmos wallets like Keplr and Leap have also integrated with GateChain. 

## Ethereum-Based Wallets

As explained above, Ethereum-based wallets can be used to interact with GateChain. Right now, the most popular Ethereum-based wallets are supported on GateChain. These include:

* [MetaMask](https://metamask.io/zh-CN/)
* [Ledger](https://www.ledger.com/)
* [Trezor](https://www.trezor.io/)
* [Torus](https://tor.us/)

The process of signing transactions on GateChain using an Ethereum-native wallet consists of:

1. Converting the transaction into EIP712 TypedData
2. Signing the EIP712 TypedData using an Ethereum-native wallet
3. Packing the transaction into a native Cosmos transaction (including the signature), and broadcasting the transaction to the chain

This process is abstracted away from the end-user. If you've previously used an Ethereum-native wallet, the user experience will be the same.

## GateChain Web Wallet

Visit [GateChain Web Wallet](https://www.gate.io/zh/web3)

## GateChain CLI Wallet


```bash
gatecli account create
```

response:

```bash

- name: "1739429752"
  type: local
  address: gt11p2ddfwz82ezguvwhjued5y6lw4wgk89hqylcxtevkfcf45u9pdk04u2nn99cezv9e9a0zl
  pubkey: gt1pub1u8s6p73qrernvfy4drmnwhtuc3qd5t2sfu8l2lmww6l2hjjmpc2cutqd8gxsn6l9xj
  mnemonic: ""
  threshold: 0
  pubkeys: []


**Important** write this mnemonic phrase in a safe place.
It is the only way to recover your account if you ever forget your password.

theme ghost symbol also quit vessel antenna token close copper plastic patch wolf payment cluster nasty elbow that theory slogan hybrid furnace anger always
```
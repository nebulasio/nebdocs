# How to Join Nebulas Testnet

## Introduction

  We are glad to release the Nebulas Testnet. It simulates the Nebulas network and NVM, and allows developers to interact with Nebulas without paying the cost of gas.

> [https://github.com/nebulasio/go-nebulas/tree/testnet](https://github.com/nebulasio/go-nebulas/tree/testnet)

### Build

The Nebulas Testnet's executable file and dependant libraries need to be built first. Several important modules are highlighted below:

* **NBRE:** The Nebulas Blockchain Runtime Environment is the platform for running Nebulas Protocol Representation, such as the DIP, the NR, etcetera. 
* **NEB:** The main process of the Nebulas Testnet. `NEB` and `NBRE` run in standalone processes, and communicate through IPC.

Details of building the modules can be found in [tutorials](http://wiki.nebulas.io/en/latest/go-nebulas/tutorials/01-installation.html#compile-nebulas).

### Configuration

The testnet configuration files are in the folder [`testnet/conf`](https://github.com/nebulasio/go-nebulas/tree/testnet/testnet/conf) under `testnet` branch, including

#### genesis.conf

All configurable information about the genesis block is defined in genesis.conf, including

* **meta.chain\_id:** chain identity
* **consensus.dpos.dynasty:** the initial dynasty of validators
* **token\_distribution:** the initial allocation of tokens

> _Attention_: DO NOT change the genesis.conf.

#### config.conf

All configurable information about runtime is defined in config.conf.

Please check the [`template.conf`](https://github.com/smalloranges/wiki/tree/887270957eb99d971309610bc1fdafb6a2d9d552/resources/conf/template.conf) to find more details about the runtime configuration.

> _Tips_: the official seed node info is as below,

```javascript
seed:["/ip4/47.92.203.173/tcp/8680/ipfs/QmfSJ7JUnCEDP6LFyKkBUbpuDMETPbqMVZvPQy4keeyBDP","/ip4/47.89.180.5/tcp/8680/ipfs/QmTmnd5KXm4UFUquAJEGdrwj1cbJCHsTfPWAp5aKrKoRJK"]
```

#### API List

Test Endpoint:

| API | URL | Protocol |
| --- | :---: | :---: |
| RESTful | [https://testnet.nebulas.io/](https://testnet.nebulas.io/) | HTTP |

* [GetNebState](https://github.com/nebulasio/wiki/blob/master/rpc.md#getnebstate) : returns nebulas client info.
* [GetAccountState](https://github.com/nebulasio/wiki/blob/master/rpc.md#getaccountstate): returns the account balance and nonce.
* [LatestIrreversibleBlock](https://github.com/nebulasio/wiki/blob/master/rpc.md#latestirreversibleblock): returns the latest irreversible block.
* [Call](https://github.com/nebulasio/wiki/blob/master/rpc.md#call): execute smart contract locally. The tx won't be submitted on chain.
* [SendRawTransaction](https://github.com/nebulasio/wiki/blob/master/rpc.md#sendrawtransaction): submit signed transaction. The transaction must be signed before sending.
* [GetTransactionReceipt](https://github.com/nebulasio/wiki/blob/master/rpc.md#gettransactionreceipt): get transaction receipt info from the transaction hash.

More Nebulas APIs at [RPC](https://github.com/nebulasio/wiki/blob/master/rpc.md).

#### Claim Tokens

Each email can claim tokens every day [here](https://testnet.nebulas.io/claim).

## Tutorials

#### English

1. [Installation](tutorials/01-installation.md) \(thanks [Victor](https://github.com/victorychain)\)
2. [Sending a Transaction](tutorials/02-transaction.md) \(thanks [Victor](https://github.com/victorychain)\)
3. [Writing Smart Contract in JavaScript](tutorials/03-smart-contracts-javascript.md) \(thanks [otto](https://github.com/ottokafka)\)
4. [Introducing Smart Contract Storage](tutorials/04-smart-contract-storage.md) \(thanks [Victor](https://github.com/victorychain)\)
5. [Interacting with Nebulas by RPC API](tutorials/05-interacting-with-nebulas-by-rpc-api.md) \(thanks [Victor](https://github.com/victorychain)\)


## Contributing

Feel free to join Nebulas Testnet. If you did find something wrong, please [submit an issue](https://github.com/nebulasio/go-nebulas/issues/new) or [submit a pull request](https://github.com/nebulasio/go-nebulas/pulls) to let us know, we will add your name and url to this page as soon as possible.


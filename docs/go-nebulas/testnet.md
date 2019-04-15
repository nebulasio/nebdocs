# How to Join Nebulas Testnet

## Introduction

  We are glad to release the Nebulas Testnet. It simulates the Nebulas network and NVM, and allows developers to interact with Nebulas without paying the cost of gas.

```text
https://github.com/nebulasio/go-nebulas/tree/testnet
```

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
seed:["/ip4/52.60.150.236/tcp/8680/ipfs/QmVJikqWQst13QsgdCLBjgcSWwpAAdZjoExGdvK3r2CNhv"]
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

1. [Installation](https://github.com/nebulasio/wiki/blob/master/tutorials/[English]%20Nebulas%20101%20-%2001%20Installation.md) \(thanks [Victor](https://github.com/victorychain)\)
2. [Sending a Transaction](https://github.com/nebulasio/wiki/blob/master/tutorials/[English]%20Nebulas%20101%20-%2002%20Transaction.md) \(thanks [Victor](https://github.com/victorychain)\)
3. [Writing Smart Contract in JavaScript](https://github.com/nebulasio/wiki/blob/master/tutorials/[English]%20Nebulas%20101%20-%2003%20Smart%20Contracts%20JavaScript.md) \(thanks [otto](https://github.com/ottokafka)\)
4. [Introducing Smart Contract Storage](https://github.com/nebulasio/wiki/blob/master/tutorials/[English]%20Nebulas%20101%20-%2004%20Smart%20Contract%20Storage.md) \(thanks [Victor](https://github.com/victorychain)\)
5. [Interacting with Nebulas by RPC API](https://github.com/nebulasio/wiki/blob/master/tutorials/[English]%20Nebulas%20101%20-%2005%20Interacting%20with%20Nebulas%20by%20RPC%20API.md) \(thanks [Victor](https://github.com/victorychain)\)


## Contributing

Feel free to join Nebulas Testnet. If you did find something wrong, please [submit an issue](https://github.com/nebulasio/go-nebulas/issues/new) or [submit a pull request](https://github.com/nebulasio/go-nebulas/pulls) to let us know, we will add your name and url to this page as soon as possible.


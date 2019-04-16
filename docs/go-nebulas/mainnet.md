# How to Join Nebulas Mainnet

## Introduction

The Nebulas Mainnet 2.0 (Nebulas Nova) was released, this tutorial will guide you to join and enjoy Nebulas Mainnet.

> [https://github.com/nebulasio/go-nebulas/tree/master](https://github.com/nebulasio/go-nebulas/tree/master)

### Build

The Nebulas Mainnet executable file and dependant libraries need to be built firstly. Several important modules are highlighted below:

* **NBRE:** The Nebulas Blockchain Runtime Environment, which is the platform for running Nebulas Protocol Representation, such as: DIP, NR etc. 
* **NEB:** The main process of Nebulas Mainnet, `NEB` and `NBRE` are running in standalone processes, while communicating through IPC

Details of building the modules can be found in [tutorials](http://wiki.nebulas.io/en/latest/go-nebulas/tutorials/01-installation.html#compile-nebulas).


### Configuration

The Mainnet configuration files are in folder [`mainnet/conf`](https://github.com/nebulasio/go-nebulas/tree/master/mainnet/conf), including

### genesis.conf

All configurable information about genesis block is defined in genesis.conf, including

* **meta.chain\_id:** chain identity
* **consensus.dpos.dynasty:** the initial dynasty of validators
* **token\_distribution:** the initial allocation of tokens

> _Attention_: DO NOT change the genesis.conf.

### config.conf

All configurable information about runtime is defined in config.conf.

Please check the [`template.conf`](https://github.com/nebulasio/nebdocs/blob/master/docs/resources/conf/template.conf) to find more details about the runtime configuration.

> _Tips_: the official seed node info is as below,

```javascript
seed:["/ip4/52.2.205.12/tcp/8680/ipfs/QmQK7W8wrByJ6So7rf84sZzKBxMYmc1i4a7JZsne93ysz5","/ip4/52.56.55.238/tcp/8680/ipfs/QmVy9AHxBpd1iTvECDR7fvdZnqXeDhnxkZJrKsyuHNYKAh","/ip4/13.251.33.39/tcp/8680/ipfs/QmVm5CECJdPAHmzJWN2X7tP335L5LguGb9QLQ78riA9gw3"]
```

### API List

Main Endpoint:

| API | URL | Protocol |
| --- | :---: | :---: |
| RESTful | [https://mainnet.nebulas.io/](https://mainnet.nebulas.io/) | HTTP |

* [GetNebState](https://github.com/nebulasio/wiki/blob/master/rpc.md#getnebstate) : returns nebulas client info.
* [GetAccountState](https://github.com/nebulasio/wiki/blob/master/rpc.md#getaccountstate): returns the account balance and nonce.
* [Call](https://github.com/nebulasio/wiki/blob/master/rpc.md#call): execute smart contract local, don't submit on chain.
* [SendRawTransaction](https://github.com/nebulasio/wiki/blob/master/rpc.md#sendrawtransaction): submit the signed transaction.
* [GetTransactionReceipt](https://github.com/nebulasio/wiki/blob/master/rpc.md#gettransactionreceipt): get transaction receipt info by tansaction hash.

More Nebulas APIs at [RPC](https://github.com/nebulasio/wiki/blob/master/rpc.md).

## Tutorials

### English

1. [Installation](tutorials/01-installation.md) \(thanks [Victor](https://github.com/victorychain)\)
2. [Sending a Transaction](tutorials/02-transaction.md) \(thanks [Victor](https://github.com/victorychain)\)
3. [Writing Smart Contract in JavaScript](tutorials/03-smart-contracts-javascript.md) \(thanks [otto](https://github.com/ottokafka)\)
4. [Introducing Smart Contract Storage](tutorials/04-smart-contract-storage.md) \(thanks [Victor](https://github.com/victorychain)\)
5. [Interacting with Nebulas by RPC API](tutorials/05-interacting-with-nebulas-by-rpc-api.md) \(thanks [Victor](https://github.com/victorychain)\)


## Contribution

Feel free to join the Nebulas Mainnet. If you have found something wrong, please [submit an issue](https://github.com/nebulasio/go-nebulas/issues/new) or [submit a pull request](https://github.com/nebulasio/go-nebulas/pulls) to let us know, and we will add your name and url to this page as soon as possible.


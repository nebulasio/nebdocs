# 加入星云测试网

> _Tips_: 以下是星云鹰星云版本（Eagle Nebula）的加入测试网教程，当前Nebulas NOVA版测试网相关教程即将更新，敬请期待。

## Introduction

We are glad to release Nebulas Testnet here. It simulate the Nebulas network and NVM, and allow developers to interact with Nebulas without paying the cost of gas.

```text
https://github.com/nebulasio/go-nebulas/tree/testnet
```

### Configuration

The testnet configuration files are in folder [`testnet/conf`](https://github.com/nebulasio/go-nebulas/tree/testnet/testnet/conf) under `testnet` branch, including

#### genesis.conf

All configurable information about genesis block is defined in genesis.conf, including

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
* [LatestIrreversibleBlock](https://github.com/nebulasio/wiki/blob/master/rpc.md#latestirreversibleblock): returns the lastest irreversible block.
* [Call](https://github.com/nebulasio/wiki/blob/master/rpc.md#call): execute smart contract locally. the tx won't be submitted on chain.
* [SendRawTransaction](https://github.com/nebulasio/wiki/blob/master/rpc.md#sendrawtransaction): submit signed transaction. The transaction must be signed before send.
* [GetTransactionReceipt](https://github.com/nebulasio/wiki/blob/master/rpc.md#gettransactionreceipt): get transaction receipt info by tansaction hash.

More Nebulas APIs at [RPC](https://github.com/nebulasio/wiki/blob/master/rpc.md).

#### Token Claim

Every email can claim some tokens every day at [here](https://testnet.nebulas.io/claim).

## Tutorials


1. [编译安装及运行neb](tutorials/01-instaallation.md)
2. [在星云链上发送交易](tutorials/02-transaction.md)
3. [使用JavaScript编写智能合约](tutorials/03-smart-contracts-javascript.md)
4. [智能合约存储区介绍](tutorials/04-smart-contract-storage.md)
5. [通过RPC接口与星云链交互](tutorials/05-interacting-with-nebulas-by-rpc-api.md)

## Contribution

Feel free to join Nebulas Testnet. If you did find something wrong, please [submit a issue](https://github.com/nebulasio/go-nebulas/issues/new) or [submit a pull request](https://github.com/nebulasio/go-nebulas/pulls) to let us know, we will add your name and url to this page soon.


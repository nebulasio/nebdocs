# 加入星云测试网

## Introduction

我们发布了Nebulas Testnet。它模拟了星云网络和NVM，并允许开发人员与星云交互而无需支付gas费用。

> [https://github.com/nebulasio/go-nebulas/tree/testnet](https://github.com/nebulasio/go-nebulas/tree/testnet)

### 编译

需要首先构建Nebulas Mainnet的可执行文件和依赖库。 以下重点介绍几个重要模块：

* **NBRE:** 星云区块链运行时环境，运行Nebulas Protocol Representation的平台，例如DIP，NR等。
* **NEB:** 星云主网的主进程。 `NEB`和`NBRE`运行于独立进程中，两者通过IPC进行通信。

有关编译的详细信息，请参阅 [tutorials](tutorials/01-installation.html#compile-nebulas).

### 配置文件

testnet 配置文件在 [`testnet/conf`](https://github.com/nebulasio/go-nebulas/tree/testnet/testnet/conf) `testnet` 分支下, 包括

#### genesis.conf

关于genesis块的所有可配置信息都在genesis.conf中定义，包括

* **meta.chain\_id:** chain identity
* **consensus.dpos.dynasty:** the initial dynasty of validators
* **token\_distribution:** the initial allocation of tokens

> _注意_: 不要修改 genesis.conf.

#### config.conf

有关运行时的所有可配置信息都在config.conf中定义。

请查看 [`template.conf`](../resources/conf/template.conf) 查找有关运行时配置的更多详细信息。

> _Tips_: 官方节点信息如下,

```javascript
seed:["/ip4/52.60.150.236/tcp/8680/ipfs/QmVJikqWQst13QsgdCLBjgcSWwpAAdZjoExGdvK3r2CNhv"]
```

#### API 列表

Test Endpoint:

| API | URL | Protocol |
| --- | :---: | :---: |
| RESTful | [https://testnet.nebulas.io/](https://testnet.nebulas.io/) | HTTP |

* [GetNebState](dapp-development/rpc/README.html#getnebstate) : returns nebulas client info.
* [GetAccountState](dapp-development/rpc/README.html#getaccountstate): returns the account balance and nonce.
* [LatestIrreversibleBlock](dapp-development/rpc/README.html#latestirreversibleblock): returns the lastest irreversible block.
* [Call](dapp-development/rpc/README.rst#call): execute smart contract locally. the tx won't be submitted on chain.
* [SendRawTransaction](dapp-development/rpc/README.html#sendrawtransaction): submit signed transaction. The transaction must be signed before send.
* [GetTransactionReceipt](dapp-development/rpc/README.html#gettransactionreceipt): get transaction receipt info by tansaction hash.

更多的星云 APIs ： [RPC](dapp-development/rpc/README.html).

#### 申领测试NAS币

每封电子邮件每天都可以获得一些NAS币 [点击这里](https://testnet.nebulas.io/claim)。

## 教程


1. [编译安装及运行neb](tutorials/01-installation.md)
2. [在星云链上发送交易](tutorials/02-transaction.md)
3. [使用JavaScript编写智能合约](tutorials/03-smart-contracts-javascript.md)
4. [智能合约存储区介绍](tutorials/04-smart-contract-storage.md)
5. [通过RPC接口与星云链交互](tutorials/05-interacting-with-nebulas-by-rpc-api.md)

## 作出贡献

可随意加入Nebulas Testnet。 如果你确实发现了什么问题，请 [submit a issue](https://github.com/nebulasio/go-nebulas/issues/new) 或者 [submit a pull request](https://github.com/nebulasio/go-nebulas/pulls) 告诉我们，我们会尽快将您的姓名和网址添加到此页面。


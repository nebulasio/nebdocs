# 加入星云主网

## 介绍

Nebulas Mainnet 2.0（Nebulas Nova）已经发布。 本教程将教您如何加入和使用星云主网。

> [https://github.com/nebulasio/go-nebulas/tree/master](https://github.com/nebulasio/go-nebulas/tree/master)

### 编译

需要首先构建Nebulas Mainnet的可执行文件和依赖库。 以下重点介绍几个重要模块：

* **NBRE:** 星云区块链运行时环境，运行Nebulas Protocol Representation的平台，例如DIP，NR等。
* **NEB:** 星云主网的主进程。 `NEB`和`NBRE`运行于独立进程中，两者通过IPC进行通信。

有关编译的详细信息，请参阅 [tutorials](http://wiki.nebulas.io/en/latest/go-nebulas/tutorials/01-installation.html#compile-nebulas).


### 配置文件

Mainnet配置文件位于文件夹 [`mainnet/conf`](https://github.com/nebulasio/go-nebulas/tree/master/mainnet/conf),
包括：

### genesis.conf

关于创世块的所有可配置信息都在genesis.conf中定义，包括：

* **meta.chain\_id:** chain identity
* **consensus.dpos.dynasty:** the initial dynasty of validators
* **token\_distribution:** the initial allocation of tokens

> _注意_: 不要修改 genesis.conf.

### config.conf

有关运行时的所有可配置信息都在config.conf中定义。

请查看 [`template.conf`](https://github.com/smalloranges/wiki/tree/887270957eb99d971309610bc1fdafb6a2d9d552/resources/conf/template.conf) 查找有关运行时配置的更多详细信息。

> _Tips_: 官方种子节点信息如下,

```javascript
seed:["/ip4/52.2.205.12/tcp/8680/ipfs/QmQK7W8wrByJ6So7rf84sZzKBxMYmc1i4a7JZsne93ysz5","/ip4/52.56.55.238/tcp/8680/ipfs/QmVy9AHxBpd1iTvECDR7fvdZnqXeDhnxkZJrKsyuHNYKAh","/ip4/13.251.33.39/tcp/8680/ipfs/QmVm5CECJdPAHmzJWN2X7tP335L5LguGb9QLQ78riA9gw3"]
```

### API 列表

Main Endpoint:

| API | URL | Protocol |
| --- | :---: | :---: |
| RESTful | [https://mainnet.nebulas.io/](https://mainnet.nebulas.io/) | HTTP |

* [GetNebState](https://github.com/nebulasio/wiki/blob/master/rpc.md#getnebstate) : returns nebulas client info.
* [GetAccountState](https://github.com/nebulasio/wiki/blob/master/rpc.md#getaccountstate): returns the account balance and nonce.
* [Call](https://github.com/nebulasio/wiki/blob/master/rpc.md#call): execute smart contract local, don't submit on chain.
* [SendRawTransaction](https://github.com/nebulasio/wiki/blob/master/rpc.md#sendrawtransaction): submit the signed transaction.
* [GetTransactionReceipt](https://github.com/nebulasio/wiki/blob/master/rpc.md#gettransactionreceipt): get transaction receipt info by tansaction hash.

更多的星云 APIs ： [RPC](https://github.com/nebulasio/wiki/blob/master/rpc.md).

## 教程

1. [编译安装及运行neb](tutorials/01-installation.md)
2. [在星云链上发送交易](tutorials/02-transaction.md)
3. [使用JavaScript编写智能合约](tutorials/03-smart-contracts-javascript.md)
4. [智能合约存储区介绍](tutorials/04-smart-contract-storage.md)
5. [通过RPC接口与星云链交互](tutorials/05-interacting-with-nebulas-by-rpc-api.md)

## 作出贡献

可随意加入Nebulas Mainnet。 如果你确实发现了什么问题，请 [submit a issue](https://github.com/nebulasio/go-nebulas/issues/new) or [submit a pull request](https://github.com/nebulasio/go-nebulas/pulls) 来让我们知道, 我们会尽快将您的姓名和网址添加到此页面。


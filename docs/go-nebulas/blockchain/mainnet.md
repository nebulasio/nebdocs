# 加入星云主网

## 介绍

Nebulas Mainnet 2.0（Nebulas Nova）已经发布。 本教程将教您如何加入和使用星云主网。

> [https://github.com/nebulasio/go-nebulas/tree/master](https://github.com/nebulasio/go-nebulas/tree/master)

### 编译

需要首先构建Nebulas Mainnet的可执行文件和依赖库。 以下重点介绍几个重要模块：

* **NBRE:** 星云区块链运行时环境，运行Nebulas Protocol Representation的平台，例如DIP，NR等。
* **NEB:** 星云主网的主进程。 `NEB`和`NBRE`运行于独立进程中，两者通过IPC进行通信。

有关编译的详细信息，请参阅 [教程](../tutorials/01-installation.html#compile-nebulas).


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

请查看 [`template.conf`](https://github.com/nebulasio/nebdocs/blob/zh-CN/docs/resources/conf/template.conf) 查找有关运行时配置的更多详细信息。

> _Tips_: 官方种子节点信息如下,

```javascript
seed:["/ip4/52.2.205.12/tcp/8680/ipfs/QmQK7W8wrByJ6So7rf84sZzKBxMYmc1i4a7JZsne93ysz5","/ip4/52.56.55.238/tcp/8680/ipfs/QmVy9AHxBpd1iTvECDR7fvdZnqXeDhnxkZJrKsyuHNYKAh","/ip4/13.251.33.39/tcp/8680/ipfs/QmVm5CECJdPAHmzJWN2X7tP335L5LguGb9QLQ78riA9gw3"]
```

#### Miner config 
Nodes can participate in mining and share rewards after signing up for mining. The miner node needs to turn on the mine switch and configure both the miner address and reward address(coinbase).

miner config example:

```
chain {
  # mainnet chainID
  chain_id: 1
  # mainnet datadir, should be different with private chain
  datadir: "mainnet/data.db"
  keydir: "keydir"
  # mainnet genesis.conf
  genesis: "mainnet/conf/genesis.conf"
  # mainnet dynasty.conf
  dynasty: "mainnet/conf/dynasty.conf"
  
  # start mine
  start_mine: true
  # receive the mining award, must change to your address
  coinbase: "n1XkoVVjswb5Gek3rRufqjKNpwrDdsnQ7Hq"
  # block signature address, needs to be placed in the node's configuration `keydir`. Also make sure that the address is the node address at the time of registration
  miner: "n1FF1nz6tarkDVwWQkMnnwFPuPKUaQTdptE"
  # 
  passphrase: "passphrase"

  signature_ciphers: ["ECC_SECP256K1"]
}
```

### Synchronization

Since Nebulas mainnet is running there for certain period of time, it will take quite some time to sync all the mainnet data from scratch. 

For developers' convenience, we provided a offline data package, which already includes the data of more than 1 million blocks, you can download the package directly by following either link below (choose whichever is faster for you):
- [`download from AWS s3`](https://s3-us-west-1.amazonaws.com/develop-center/data/mainnet/data.db.tar)
- [`download from Aliyun oss`](http://develop-center.oss-cn-zhangjiakou.aliyuncs.com/data/mainnet/data.db.tar.gz)

> Please note that, the data package should be put under the same path of "datadir" as specified in your `config.conf` file.



### API 列表

Main Endpoint:

| API | URL | Protocol |
| --- | :---: | :---: |
| RESTful | [https://mainnet.nebulas.io/](https://mainnet.nebulas.io/) | HTTP |

* [GetNebState](../dapp-development/rpc/README.html#getnebstate) : returns nebulas client info.
* [GetAccountState](../dapp-development/rpc/README.html#getaccountstate): returns the account balance and nonce.
* [Call](../dapp-development/rpc/README.html#call): execute smart contract local, don't submit on chain.
* [SendRawTransaction](../dapp-development/rpc/README.html#sendrawtransaction): submit the signed transaction.
* [GetTransactionReceipt](../dapp-development/rpc/README.html#gettransactionreceipt): get transaction receipt info by tansaction hash.

更多星云 APIs ： [RPC](../dapp-development/rpc/README).

## 教程

1. [编译安装及运行neb](../tutorials/01-installation.md)
2. [在星云链上发送交易](../tutorials/02-transaction.md)
3. [使用JavaScript编写智能合约](../tutorials/03-smart-contracts-javascript.md)
4. [智能合约存储区介绍](../tutorials/04-smart-contract-storage.md)
5. [通过RPC接口与星云链交互](../tutorials/05-interacting-with-nebulas-by-rpc-api.md)

## 作出贡献

可随意加入星云主网。如果你发现了问题，请 [提交Bug](https://github.com/nebulasio/go-nebulas/issues/new) 或者 [提交改动](https://github.com/nebulasio/go-nebulas/pulls) 来帮助社区。


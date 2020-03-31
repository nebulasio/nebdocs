# 加入星云主网


星云主网 3.0（Nebulas Voyager）已经发布。 本教程将教您如何加入和使用星云主网。

> [https://github.com/nebulasio/go-nebulas/tree/v3.0.0](https://github.com/nebulasio/go-nebulas/tree/v3.0.0)

* [编译](#id1)
* [配置文件](#id2)
* [数据同步](#id3)
* [教程](#id4)
* [API列表](#id5)
* [作出贡献](#id6)


## 编译

需要首先构建星云主网的可执行文件和依赖库：

* **NEB:** 星云主网的主进程

有关编译的详细信息，请参阅 [教程](../tutorials/01-installation.html#id2) 。


## 配置文件

主网配置文件位于文件夹 [`mainnet/conf`](https://github.com/nebulasio/go-nebulas/tree/master/mainnet/conf)，
包括：

- genesis.conf
- config.conf
- Miner.conf

### 1 genesis.conf

关于创世块的所有可配置信息都在genesis.conf中定义，包括：

* **meta.chain\_id:** chain identity
* **consensus.dpos.dynasty:** the initial dynasty of validators
* **token\_distribution:** the initial allocation of tokens

**_注意_**: 不要修改 genesis.conf。

### 2 config.conf

有关运行时的所有可配置信息都在config.conf中定义。

请查看 [`template.conf`](https://github.com/nebulasio/nebdocs/blob/zh-CN/docs/resources/conf/template.conf) 查找有关运行时配置的更多详细信息。

**_注意_**: 官方种子节点信息如下：

```javascript
seed:["/ip4/52.76.103.107/tcp/8680/ipfs/Qmbi1NVTYHkeuST2wS3B3aHiTLHDajHZpoZk5EDpAXt9H2","/ip4/52.56.55.238/tcp/8680/ipfs/QmVy9AHxBpd1iTvECDR7fvdZnqXeDhnxkZJrKsyuHNYKAh","/ip4/34.198.52.191/tcp/8680/ipfs/QmQK7W8wrByJ6So7rf84sZzKBxMYmc1i4a7JZsne93ysz5"]
```

### 3 miner.conf 

节点可以参与挖矿并获得奖励。挖矿节点需要允许访问出块地址和激励地址（coinbase）。

出块配置文件示例：

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

## 数据同步

因为星云主网2018年3月上线，已经运行了一段时间，需要一定时间来同步主网历史数据。为了方便开发者，我们提供离线数据包方便下载。该数据包已经包含了超过一百万的区块，你可以直接点击以下链接快速下载（选择对你来说最快的方法）：

- [`从亚马逊云服务 AWS s3 下载（位于美国）`](https://s3-us-west-1.amazonaws.com/develop-center/data/mainnet/data.db.tar)
- [`从阿里云OSS下载（位于中国）`](http://develop-center.oss-cn-zhangjiakou.aliyuncs.com/data/mainnet/data.db.tar.gz)

**_注意_**：数据包必须放在你的config.conf文件里指定的datadir目录下。数据包更新于3.0发布前夕，缺失的块会自动追块。




## API 列表

Main Endpoint:

| API | URL | Protocol |
| --- | :---: | :---: |
| RESTful | [https://mainnet.nebulas.io/](https://mainnet.nebulas.io/) | HTTP |

* [GetNebState](../dapp-development/rpc/README.html#getnebstate) : 返回星云客户端信息。
* [GetAccountState](../dapp-development/rpc/README.html#getaccountstate): 返回账户余额和随机数。
* [Call](../dapp-development/rpc/README.html#call): 执行职能合约，不提交上链。
* [SendRawTransaction](../dapp-development/rpc/README.html#sendrawtransaction): 提交已签名的转账。
* [GetTransactionReceipt](../dapp-development/rpc/README.html#gettransactionreceipt): 通过hash获得转账返回信息。

更多星云 APIs ： [RPC](../dapp-development/rpc/README) 。

## 教程

1. [编译安装及运行neb](../tutorials/01-installation.md)
2. [在星云链上发送交易](../tutorials/02-transaction.md)
3. [使用JavaScript编写智能合约](../tutorials/03-smart-contracts-javascript.md)
4. [智能合约存储区介绍](../tutorials/04-smart-contract-storage.md)
5. [通过RPC接口与星云链交互](../tutorials/05-interacting-with-nebulas-by-rpc-api.md)

## 作出贡献

可随意加入星云主网。如果你发现了问题，请 [提交Bug](https://github.com/nebulasio/go-nebulas/issues/new) 或者 [提交改动](https://github.com/nebulasio/go-nebulas/pulls) 来帮助社区。


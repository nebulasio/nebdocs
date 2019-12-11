# How to Join the Nebulas Mainnet

## Introduction

The Nebulas Mainnet 2.0 (Nebulas Nova) has been released. This tutorial will teach you how to join and work with the Nebulas Mainnet.

> [https://github.com/nebulasio/go-nebulas/tree/master](https://github.com/nebulasio/go-nebulas/tree/master)

### Build

The Nebulas Mainnet's executable file and dependant libraries need to be built first. Several important modules are highlighted below:

* **NBRE:** The Nebulas Blockchain Runtime Environment is the platform for running Nebulas Protocol Representation, such as the DIP, the NR, etcetera. 
* **NEB:** The main process of the Nebulas Mainnet. `NEB` and `NBRE` run in standalone processes, and communicate through IPC.

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

> _Tips_: the official seed node info is as follows,

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
- [`download from AWS s3`](https://s3-us-west-1.amazonaws.com/develop-center/data/mainnet/data.db.tar.gz)
- [`download from Aliyun oss`](http://develop-center.oss-cn-zhangjiakou.aliyuncs.com/data/mainnet/data.db.tar.gz)

> Please note that, the data package should be put under the same path of "datadir" as specified in your `config.conf` file.


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

Feel free to join the Nebulas Mainnet. If you have found something wrong, please [submit an issue](https://github.com/nebulasio/go-nebulas/issues/new) or [submit a pull request](https://github.com/nebulasio/go-nebulas/pulls) to let us know, and we will add your name and URL to this page as soon as possible.


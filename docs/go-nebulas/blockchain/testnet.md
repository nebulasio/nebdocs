# How to Join the Nebulas Testnet
## Introduction

We are glad to release the Nebulas Testnet. It simulates the Nebulas network and NVM, and allows developers to interact with Nebulas without paying the cost of gas.

> [https://github.com/nebulasio/go-nebulas/tree/testnet](https://github.com/nebulasio/go-nebulas/tree/testnet)

### Build
The Nebulas Testnet's executable file and dependant libraries need to be built first. Several important modules are highlighted below:

* **NEB:** The main process of the Nebulas Testnet. `NEB` and `NBRE` run in standalone processes, and communicate through IPC.

Details of building the modules can be found in [tutorials](../tutorials/01-installation.html#compile-nebulas).

### Configuration
The testnet configuration files are in the folder [`testnet/conf`](https://github.com/nebulasio/go-nebulas/tree/testnet/testnet/conf) under `testnet` branch, including:

#### genesis.conf
All configurable information about the genesis block is defined in genesis.conf, such as:

* **meta.chain\_id:** chain identity.
* **consensus.dpos.dynasty:** the initial dynasty of validators.
* **token\_distribution:** the initial allocation of tokens.

> _Attention_: DO NOT change the genesis.conf.

#### config.conf
All configurable information about the runtime is defined in config.conf.

Please check the [`template.conf`](https://github.com/nebulasio/nebdocs/blob/master/docs/resources/conf/template.conf) to find more details about the runtime configuration.

> _Tips_: the official seed node info is as below,

```javascript
seed:["/ip4/47.92.203.173/tcp/8680/ipfs/QmfSJ7JUnCEDP6LFyKkBUbpuDMETPbqMVZvPQy4keeyBDP","/ip4/47.89.180.5/tcp/8680/ipfs/QmTmnd5KXm4UFUquAJEGdrwj1cbJCHsTfPWAp5aKrKoRJK"]
```

#### Miner config 
Nodes can participate in mining and share rewards after signing up for mining. The miner node needs to turn on the mine switch and configure both the miner address and reward address(coinbase).

miner config example:

```
chain {
  # testnet chainID
  chain_id: 1001
  # testnet datadir, should be different with private chain
  datadir: "testnet/data.db"
  keydir: "keydir"
  # testnet genesis.conf
  genesis: "testnet/conf/genesis.conf"
  # testnet dynasty.conf
  dynasty: "testnet/conf/dynasty.conf"
  
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

#### Synchronization

Since Nebulas testnet is running there for certain period of time, it will take quite some time to sync all the testnet data from scratch. 

For developers' convenience, we provided a offline data package, which already includes the data of more than 1.2 million blocks, you can download the package directly by following either link below (choose whichever is faster for you):
- [`download from AWS s3`](https://develop-center.s3-us-west-1.amazonaws.com/data/testnet/data.db.tar)
- [`download from Aliyun oss`](http://develop-center.oss-cn-zhangjiakou.aliyuncs.com/data/testnet/data.db.tar)

> Please note that, the data package should be put under the same path of "datadir" as specified in your `config.conf` file.

#### API List
Test Endpoint:

| API | URL | Protocol |
| --- | :---: | :---: |
| RESTful | [https://testnet.nebulas.io/](https://testnet.nebulas.io/) | HTTP |

* [GetNebState](../dapp-development/rpc/README.html#getnebstate) : returns nebulas client info.
* [GetAccountState](../dapp-development/rpc/README.html#getaccountstate): returns the account balance and nonce.
* [Call](../dapp-development/rpc/README.html#call): execute smart contract local, don't submit on chain.
* [SendRawTransaction](../dapp-development/rpc/README.html#sendrawtransaction): submit the signed transaction.
* [GetTransactionReceipt](../dapp-development/rpc/README.html#gettransactionreceipt): get transaction receipt info by tansaction hash.

More Nebulas APIs at [RPC](../dapp-development/rpc/README).

#### Claim Tokens
[Each email can claim tokens every day here](https://testnet.nebulas.io/claim).

## Tutorials
1. [Installation](../tutorials/01-installation.md) \(thanks [Victor](https://github.com/victorychain)\)
2. [Sending a Transaction](../tutorials/02-transaction.md) \(thanks [Victor](https://github.com/victorychain)\)
3. [Writing Smart Contract in JavaScript](../tutorials/03-smart-contracts-javascript.md) \(thanks [otto](https://github.com/ottokafka)\)
4. [Introducing Smart Contract Storage](../tutorials/04-smart-contract-storage.md) \(thanks [Victor](https://github.com/victorychain)\)
5. [Interacting with Nebulas by RPC API](../tutorials/05-interacting-with-nebulas-by-rpc-api.md) \(thanks [Victor](https://github.com/victorychain)\)


## Contributing
Feel free to join Nebulas Testnet. If you did find something wrong, please [submit an issue](https://github.com/nebulasio/go-nebulas/issues/new) or [submit a pull request](https://github.com/nebulasio/go-nebulas/pulls) to let us know, we will add your name and url to this page as soon as possible.


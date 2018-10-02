# Como se Juntar à Mainnet da Nebulas

## Introdução

Estamos contentes por termos lançado a Mainnet da Nebulas. Junte-se à Mainnet e desfrute!

```text
https://github.com/nebulasio/go-nebulas/tree/master
```

### Configuração

Os ficheiros de configuração da Mainnet estão no directório [`mainnet/conf`](https://github.com/nebulasio/go-nebulas/tree/master/mainnet/conf), incluíndo

### genesis.conf

Toda a informação da configuração do bloco genesis está definida em genesis.conf, incluíndo

* **meta.chain\_id:** chain identity
* **consensus.dpos.dynasty:** the initial dynasty of validators
* **token\_distribution:** the initial allocation of tokens

> _Atenção_: NÃO altere o genesis.conf.

### config.conf

Toda a informação da configuração sobre o runtime está definida em config.conf.

Por favor leia o [`template.conf`](https://github.com/smalloranges/wiki/tree/887270957eb99d971309610bc1fdafb6a2d9d552/resources/conf/template.conf) para aprender como configurar o runtime.

> _Conselhos_: a informação do nó raiz oficial é como abaixo descrito,

```javascript
seed:["/ip4/52.2.205.12/tcp/8680/ipfs/QmQK7W8wrByJ6So7rf84sZzKBxMYmc1i4a7JZsne93ysz5","/ip4/52.56.55.238/tcp/8680/ipfs/QmVy9AHxBpd1iTvECDR7fvdZnqXeDhnxkZJrKsyuHNYKAh","/ip4/13.251.33.39/tcp/8680/ipfs/QmVm5CECJdPAHmzJWN2X7tP335L5LguGb9QLQ78riA9gw3"]
```

### Lista do API

Endpoint principal:

| API | URL | Protocol |
| --- | :---: | :---: |
| RESTful | [https://mainnet.nebulas.io/](https://mainnet.nebulas.io/) | HTTP |

* [GetNebState](https://github.com/nebulasio/wiki/blob/master/rpc.md#getnebstate) : retorna a informação do cliente Nebulas.
* [GetAccountState](https://github.com/nebulasio/wiki/blob/master/rpc.md#getaccountstate): retorna o saldo da conta e o nonce.
* [Call](https://github.com/nebulasio/wiki/blob/master/rpc.md#call): executa o smart contract local, sem o submeter na chain.
* [SendRawTransaction](https://github.com/nebulasio/wiki/blob/master/rpc.md#sendrawtransaction): submete a transação assinada.
* [GetTransactionReceipt](https://github.com/nebulasio/wiki/blob/master/rpc.md#gettransactionreceipt): obtém a factura da transacção através da sua hash.

Mais APIs Nebulas em [RPC](https://github.com/nebulasio/wiki/blob/master/rpc.md).

## Tutoriais

### Português

1. [Installation](https://github.com/nebulasio/wiki/blob/master/tutorials/[English]%20Nebulas%20101%20-%2001%20Installation.md) \(thanks [Victor](https://github.com/victorychain)\)
2. [Sending a Transaction](https://github.com/nebulasio/wiki/blob/master/tutorials/[English]%20Nebulas%20101%20-%2002%20Transaction.md) \(thanks [Victor](https://github.com/victorychain)\)
3. [Writing Smart Contract in JavaScript](https://github.com/nebulasio/wiki/blob/master/tutorials/[English]%20Nebulas%20101%20-%2003%20Smart%20Contracts%20JavaScript.md) \(thanks [otto](https://github.com/ottokafka)\)
4. [Introducing Smart Contract Storage](https://github.com/nebulasio/wiki/blob/master/tutorials/[English]%20Nebulas%20101%20-%2004%20Smart%20Contract%20Storage.md) \(thanks [Victor](https://github.com/victorychain)\)
5. [Interacting with Nebulas by RPC API](https://github.com/nebulasio/wiki/blob/master/tutorials/[English]%20Nebulas%20101%20-%2005%20Interacting%20with%20Nebulas%20by%20RPC%20API.md) \(thanks [Victor](https://github.com/victorychain)\)

### 中文

1. [编译安装及运行neb](https://github.com/nebulasio/wiki/blob/master/tutorials/[中文]%20Nebulas%20101%20-%2001%20编译安装.md)
2. [在星云链上发送交易](https://github.com/nebulasio/wiki/blob/master/tutorials/[中文]%20Nebulas%20101%20-%2002%20发送交易.md)
3. [使用JavaScript编写智能合约](https://github.com/nebulasio/wiki/blob/master/tutorials/[中文]%20Nebulas%20101%20-%2003%20编写智能合约.md)
4. [智能合约存储区介绍](https://github.com/nebulasio/wiki/blob/master/tutorials/[中文]%20Nebulas%20101%20-%2004%20智能合约存储区.md)
5. [通过RPC接口与星云链交互](https://github.com/nebulasio/wiki/blob/master/tutorials/[中文]%20Nebulas%20101%20-%2005%20通过RPC接口与星云链交互.md)

## Contribution

Feel free to join the Nebulas Mainnet. If you have found something wrong, please [submit an issue](https://github.com/nebulasio/go-nebulas/issues/new) or [submit a pull request](https://github.com/nebulasio/go-nebulas/pulls) to let us know, and we will add your name and url to this page as soon as possible.


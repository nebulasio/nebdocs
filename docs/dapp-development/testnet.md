# Como se Juntar à Testnet da Nebulas

## Introducção

Estamos felizes por lançar a Testnet da Nebulas. Ela simula a rede Nebulas e a NVM, e permite a interacção com a Nebulas sem pagamento de custo de gás.

```text
https://github.com/nebulasio/go-nebulas/tree/testnet
```

### Configuração

Os ficheiros de configuração estão no directório [`testnet/conf`](https://github.com/nebulasio/go-nebulas/tree/testnet/testnet/conf) na branch `testnet`, inclusive

#### genesis.conf

Toda a informação configurável sobre o genesis block está definida em genesis.conf, inclusive

* **meta.chain\_id:** identidade da chain
* **consensus.dpos.dynasty:** dinastia inicial dos validadores
* **token\_distribution:** alocação inicial de tokens

> _Atenção_: NÃO modifique o ficheiro genesis.conf.

#### config.conf

Toda a informação configurável sobre o runtime está definida em config.conf

Por favor veja o [`template.conf`](https://github.com/smalloranges/wiki/tree/887270957eb99d971309610bc1fdafb6a2d9d552/resources/conf/template.conf) para conhecer mais detalhes sobre a configuração do runtime.

> _Dica_: a informação do nó-raíz oficial está descrita abaixo,

```javascript
seed:["/ip4/52.60.150.236/tcp/8680/ipfs/QmVJikqWQst13QsgdCLBjgcSWwpAAdZjoExGdvK3r2CNhv"]
```

#### Lista do API

Test Endpoint:

| API | URL | Protocol |
| --- | :---: | :---: |
| RESTful | [https://testnet.nebulas.io/](https://testnet.nebulas.io/) | HTTP |

* [GetNebState](https://github.com/nebulasio/wiki/blob/master/rpc.md#getnebstate) : retorna informação do cliente de Nebulas.
* [GetAccountState](https://github.com/nebulasio/wiki/blob/master/rpc.md#getaccountstate): retorna o balança e nonce da conta.
* [LatestIrreversibleBlock](https://github.com/nebulasio/wiki/blob/master/rpc.md#latestirreversibleblock): retorna o último bloco irreverssível.
* [Call](https://github.com/nebulasio/wiki/blob/master/rpc.md#call): executa o smart contract localmente. A transacção não será submetida na chain.
* [SendRawTransaction](https://github.com/nebulasio/wiki/blob/master/rpc.md#sendrawtransaction): submete transacção assinada. A transacção tem de ser assinada antes de ser enviada.
* [GetTransactionReceipt](https://github.com/nebulasio/wiki/blob/master/rpc.md#gettransactionreceipt): receba a factura da transacção através da hash da mesma.

Mais APIs Nebulas em [RPC](https://github.com/nebulasio/wiki/blob/master/rpc.md).

#### Obtenção de Tokens

Cada email pode pedir tokens uma vez por dia [aqui](https://testnet.nebulas.io/claim).

## Tutoriais

#### English

1. [Installation](https://github.com/nebulasio/wiki/blob/master/tutorials/[English]%20Nebulas%20101%20-%2001%20Installation.md) \(thanks [Victor](https://github.com/victorychain)\)
2. [Sending a Transaction](https://github.com/nebulasio/wiki/blob/master/tutorials/[English]%20Nebulas%20101%20-%2002%20Transaction.md) \(thanks [Victor](https://github.com/victorychain)\)
3. [Writing Smart Contract in JavaScript](https://github.com/nebulasio/wiki/blob/master/tutorials/[English]%20Nebulas%20101%20-%2003%20Smart%20Contracts%20JavaScript.md) \(thanks [otto](https://github.com/ottokafka)\)
4. [Introducing Smart Contract Storage](https://github.com/nebulasio/wiki/blob/master/tutorials/[English]%20Nebulas%20101%20-%2004%20Smart%20Contract%20Storage.md) \(thanks [Victor](https://github.com/victorychain)\)
5. [Interacting with Nebulas by RPC API](https://github.com/nebulasio/wiki/blob/master/tutorials/[English]%20Nebulas%20101%20-%2005%20Interacting%20with%20Nebulas%20by%20RPC%20API.md) \(thanks [Victor](https://github.com/victorychain)\)

#### 中文

1. [编译安装及运行neb](https://github.com/nebulasio/wiki/blob/master/tutorials/[中文]%20Nebulas%20101%20-%2001%20编译安装.md)
2. [在星云链上发送交易](https://github.com/nebulasio/wiki/blob/master/tutorials/[中文]%20Nebulas%20101%20-%2002%20发送交易.md)
3. [使用JavaScript编写智能合约](https://github.com/nebulasio/wiki/blob/master/tutorials/[中文]%20Nebulas%20101%20-%2003%20编写智能合约.md)
4. [智能合约存储区介绍](https://github.com/nebulasio/wiki/blob/master/tutorials/[中文]%20Nebulas%20101%20-%2004%20智能合约存储区.md)
5. [通过RPC接口与星云链交互](https://github.com/nebulasio/wiki/blob/master/tutorials/[中文]%20Nebulas%20101%20-%2005%20通过RPC接口与星云链交互.md)

## Contribuições

Feel free to join Nebulas Testnet. If you did find something wrong, please [submit an issue](https://github.com/nebulasio/go-nebulas/issues/new) or [submit a pull request](https://github.com/nebulasio/go-nebulas/pulls) to let us know, we will add your name and url to this page as soon as possible.


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

1. [Instalação](https://github.com/nebulasio/wiki/blob/master/tutorials/%5BPortugues%5D%20Nebulas%20101%20-%2001%20Instalacao.md) \(obrigado [Cristiano](https://github.com/crisbrm)\)
2. [Envio de Transacções](https://github.com/nebulasio/wiki/blob/master/tutorials/%5BPortugues%5D%20Nebulas%20101%20-%2002%20Transacao.md) \(obrigado [Cristiano](https://github.com/crisbrm)\)
3. [Criar Smart Contract em JavaScript](https://github.com/nebulasio/wiki/blob/master/tutorials/%5BPortugues%5D%20Nebulas%20101%20-%2003%20Smart%20Contracts%20JavaScript.md) \(obrigado [Cristiano](https://github.com/crisbrm)\)
4. [Introdução de Armazenamento em Smart Contracts](https://github.com/nebulasio/wiki/blob/master/tutorials/%5BPortugues%5D%20Nebulas%20101%20-%2004%20Armazenamento%20Smart%20Contract.md) \(obrigado [Cristiano](https://github.com/crisbrm)\)
5. [Interacção com Nebulas através do API RPC](https://github.com/nebulasio/wiki/blob/master/tutorials/%5BPortugues%5D%20Nebulas%20101%20-%2005%20Interacao%20com%20Nebulas%20por%20API%20RPC.md) \(obrigado [Cristiano](https://github.com/crisbrm)\)

### 中文

1. [编译安装及运行neb](https://github.com/nebulasio/wiki/blob/master/tutorials/[中文]%20Nebulas%20101%20-%2001%20编译安装.md)
2. [在星云链上发送交易](https://github.com/nebulasio/wiki/blob/master/tutorials/[中文]%20Nebulas%20101%20-%2002%20发送交易.md)
3. [使用JavaScript编写智能合约](https://github.com/nebulasio/wiki/blob/master/tutorials/[中文]%20Nebulas%20101%20-%2003%20编写智能合约.md)
4. [智能合约存储区介绍](https://github.com/nebulasio/wiki/blob/master/tutorials/[中文]%20Nebulas%20101%20-%2004%20智能合约存储区.md)
5. [通过RPC接口与星云链交互](https://github.com/nebulasio/wiki/blob/master/tutorials/[中文]%20Nebulas%20101%20-%2005%20通过RPC接口与星云链交互.md)

## Contribuir

Não se iniba e junte-se à Mainnet da Nebulas. Se encontrou algo de errado, por favor submeta [submeta um pedido](https://github.com/nebulasio/go-nebulas/issues/new) ou [uma pull request](https://github.com/nebulasio/go-nebulas/pulls) para estarmos a par, e adicionaremos o seu nome e url a esta página o mais rápido possível.

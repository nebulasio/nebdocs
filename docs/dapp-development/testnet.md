# Como se Juntar à Testnet da Nebulas

## Introducção

Estamos felizes por lançar a Testnet da Nebulas. Ela simula a rede Nebulas e a NVM, e permite a interacção com a Nebulas sem pagamento de custo de gás.

> [https://github.com/nebulasio/go-nebulas/tree/testnet](https://github.com/nebulasio/go-nebulas/tree/testnet)

## Configuração

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

### Lista do API

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

### Português

1. [Instalação](https://github.com/nebulasio/wiki/blob/master/tutorials/%5BPortugues%5D%20Nebulas%20101%20-%2001%20Instalacao.md) \(obrigado [Cristiano](https://github.com/crisbrm)\)
2. [Envio de Transacções](https://github.com/nebulasio/wiki/blob/master/tutorials/%5BPortugues%5D%20Nebulas%20101%20-%2002%20Transacao.md) \(obrigado [Cristiano](https://github.com/crisbrm)\)
3. [Criar Smart Contract em JavaScript](https://github.com/nebulasio/wiki/blob/master/tutorials/%5BPortugues%5D%20Nebulas%20101%20-%2003%20Smart%20Contracts%20JavaScript.md) \(obrigado [Cristiano](https://github.com/crisbrm)\)
4. [Introdução de Armazenamento em Smart Contracts](https://github.com/nebulasio/wiki/blob/master/tutorials/%5BPortugues%5D%20Nebulas%20101%20-%2004%20Armazenamento%20Smart%20Contract.md) \(obrigado [Cristiano](https://github.com/crisbrm)\)
5. [Interacção com Nebulas através do API RPC](https://github.com/nebulasio/wiki/blob/master/tutorials/%5BPortugues%5D%20Nebulas%20101%20-%2005%20Interacao%20com%20Nebulas%20por%20API%20RPC.md) \(obrigado [Cristiano](https://github.com/crisbrm)\)


## Contribuir

Não se iniba e junte-se à Testnet da Nebulas. Se encontrou algo de errado, por favor submeta [submeta um pedido](https://github.com/nebulasio/go-nebulas/issues/new) ou [uma pull request](https://github.com/nebulasio/go-nebulas/pulls) para estarmos a par, e adicionaremos o seu nome e url a esta página o mais rápido possível.

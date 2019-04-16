# Como se Juntar à Mainnet da Nebulas

## Introdução

A Mainnet 2.0 da Nebulas (Nebulas Nova) foi lançada. Este tutorial visa explicar como utilizá-la. Junte-se à Mainnet e desfrute!

> [https://github.com/nebulasio/go-nebulas/tree/master](https://github.com/nebulasio/go-nebulas/tree/master)

### Compilação

O ficheiro executável da Mainnet da Nebulas e as suas bibliotecas têm de ser compilados primeiro. Modulos de alta importância estão listados abaixo:

* **NBRE:** O Ambiente de Execução da Nebulas é a plataforma que corre o Protocolo de Representação da Nebulas, como o DIP, o NR, etcetera.
* **NEB:** O processo principal da Mainnet da Nebulas, o `NEB` e o `NBRE` correm em processos individuais, e comunicam através de IPC

Detalhes sobre a compilação dos modulos pode ser encontrada em [tutoriais](http://wiki.nebulas.io/en/latest/go-nebulas/tutorials/01-installation.html#compile-nebulas).
  
## Configuração

Os ficheiros de configuração da Mainnet estão no directório [`mainnet/conf`](https://github.com/nebulasio/go-nebulas/tree/master/mainnet/conf), incluíndo

### genesis.conf

Toda a informação da configuração do bloco genesis está definida em genesis.conf, incluíndo

* **meta.chain\_id:** cadeia de identidade
* **consensus.dpos.dynasty:** a dinastia inicial dos validadores
* **token\_distribution:** a alocação inicial de tokens

> _Atenção_: NÃO altere o genesis.conf.

### config.conf

Toda a informação da configuração sobre o runtime está definida em config.conf.

Por favor, verifique o [`template.conf`](https://github.com/smalloranges/wiki/tree/887270957eb99d971309610bc1fdafb6a2d9d552/resources/conf/template.conf) para encontrar mais detalhes sobre a configuração do tempo de execução.

> _Nota_: a informação do nó da raíz oficial está descrita em baixo,

```javascript
seed:["/ip4/52.2.205.12/tcp/8680/ipfs/QmQK7W8wrByJ6So7rf84sZzKBxMYmc1i4a7JZsne93ysz5","/ip4/52.56.55.238/tcp/8680/ipfs/QmVy9AHxBpd1iTvECDR7fvdZnqXeDhnxkZJrKsyuHNYKAh","/ip4/13.251.33.39/tcp/8680/ipfs/QmVm5CECJdPAHmzJWN2X7tP335L5LguGb9QLQ78riA9gw3"]
```

## Lista do API

Ponto final principal:

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


## Contribuição

Sinta-se livre de se juntar à Mainnet Nebulas. Se encontrar algo errado, [submeta um relatório](https://github.com/nebulasio/go-nebulas/issues/new) ou [envie uma solicitação de recebimento](https://github.com/nebulasio/go-nebulas/pulls) para nos informar, e adicionaremos seu nome e seu URL a esta página o mais rápido possível.



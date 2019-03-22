# Cómo unirse a la mainnet de Nebulas

## Código fuente

Puedes encontrar el código fuente de la mainnet de Nebulas en [nuestro repositorio github](https://github.com/nebulasio/go-nebulas/tree/master).

## Configuración

Podrás encontrar los archivos de configuración de la mainnet en [`mainnet/conf`](https://github.com/nebulasio/go-nebulas/tree/master/mainnet/conf).

Esa carpeta contiene los siguientes archivos:

### genesis.conf

Permite configurar los parámetros del bloque inicial (_genesis block_), incluyendo:

* **meta.chain\_id**: Identidad de la cadena.
* **consensus.dpos.dynasty**: Dinastía inicial de validadores.
* **token\_distribution**: Asignación inicial de tokens.

> **IMPORTANTE**: No se deben realizar cambios sobre el archivo genesis.conf.

### config.conf

Permite configurar los parámetros del runtime.

Para más información sobre este archivo, véase [`template.conf`](https://github.com/smalloranges/wiki/tree/887270957eb99d971309610bc1fdafb6a2d9d552/resources/conf/template.conf).

> **Nota**: la información del nodo semilla oficial debe verse tal como se muestra aquí debajo:

```javascript
seed:["/ip4/52.2.205.12/tcp/8680/ipfs/QmQK7W8wrByJ6So7rf84sZzKBxMYmc1i4a7JZsne93ysz5","/ip4/52.56.55.238/tcp/8680/ipfs/QmVy9AHxBpd1iTvECDR7fvdZnqXeDhnxkZJrKsyuHNYKAh","/ip4/13.251.33.39/tcp/8680/ipfs/QmVm5CECJdPAHmzJWN2X7tP335L5LguGb9QLQ78riA9gw3"]
```

## Resumen de la API

Endpoint principal:

| API | URL | Protocolo |
| --- | :---: | :---: |
| RESTful | [https://mainnet.nebulas.io/](https://mainnet.nebulas.io/) | HTTP |

* [GetNebState](./rpc/README.md#getnebstate): Devuelve información sobre el cliente.
* [GetAccountState](./rpc/README.md#getaccountstate): Devuelve el balance y el _nonce_ de la cuenta.
* [Call](./rpc/README.md#call): Ejecuta un contrato inteligente de forma local, sin enviar datos al chain.
* [SendRawTransaction](./rpc/README.md#sendrawtransaction): Permite enviar una transacción firmada.
* [GetTransactionReceipt](./rpc/README.md#gettransactionreceipt): Obtiene información del recibo de una transacción mediante su hash.

Léase más sobre estos métodos [aquí](./rpc/README.md).

## Tutoriales

### En inglés

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
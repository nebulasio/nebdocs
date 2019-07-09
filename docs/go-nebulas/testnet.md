# Cómo unirse a la testnet de Nebulas
## Introducción
Estamos encantados de lanzar la Testnet de Nebulas. Simula la red de Nebulas y NVM, y permite a los desarrolladores interactuar con Nebulas sin pagar el costo del gas.

> [https://github.com/nebulasio/go-nebulas/tree/testnet](https://github.com/nebulasio/go-nebulas/tree/testnet)

### Compilacion
El archivo ejecutable de la mainnet de Nebulas y las bibliotecas dependientes se deben compilar primero. Varios módulos importantes se enumeran a continuación:

* **NBRE:** el Nebulas Blockchain Runtime Environment es la plataforma para ejecutar la Representación del Protocolo de Nebulas, como el DIP, el NR, etc.
* **NEB:** el proceso principal de la mainnet de Nebulas. NEB y NBRE se ejecutan en procesos independientes y se comunican a través de IPC. 

Las instrucciones sobre cómo compilar los módulos se pueden encontrar en los [tutoriales](http://wiki.nebulas.io/en/latest/go-nebulas/tutorials/01-installation.html#compile-nebulas).

### Configuración
Podrás encontrar los archivos de configuración de la testnet en [`testnet/conf`](https://github.com/nebulasio/go-nebulas/tree/testnet/testnet/conf).

Esa carpeta contiene los siguientes archivos:

#### genesis.conf

Permite configurar los parámetros del bloque inicial (_genesis block_), incluyendo:

* **meta.chain\_id:** identidad de la cadena.
* **consensus.dpos.dynasty:** dinastía inicial de validadores.
* **token\_distribution:** asignación inicial de tokens.

> **IMPORTANTE**: No se deben realizar cambios sobre el archivo genesis.conf.

#### config.conf

Permite configurar los parámetros del runtime.

Para más información sobre este archivo, véase [`template.conf`](https://github.com/nebulasio/nebdocs/blob/master/docs/resources/conf/template.conf).

> **Nota**: la información del nodo semilla oficial debe verse tal como se muestra aquí debajo:

```javascript
seed:["/ip4/47.92.203.173/tcp/8680/ipfs/QmfSJ7JUnCEDP6LFyKkBUbpuDMETPbqMVZvPQy4keeyBDP","/ip4/47.89.180.5/tcp/8680/ipfs/QmTmnd5KXm4UFUquAJEGdrwj1cbJCHsTfPWAp5aKrKoRJK"]
```

#### Resumen de la API
Test Endpoint:

| API | URL | Protocol |
| --- | :---: | :---: |
| RESTful | [https://testnet.nebulas.io/](https://testnet.nebulas.io/) | HTTP |

* [GetNebState](https://github.com/nebulasio/wiki/blob/master/rpc.md#getnebstate): returns nebulas client info.
* [GetAccountState](https://github.com/nebulasio/wiki/blob/master/rpc.md#getaccountstate): devuelve el balance y el _nonce_ de la cuenta.
* [LatestIrreversibleBlock](https://github.com/nebulasio/wiki/blob/master/rpc.md#latestirreversibleblock): devuelve el último bloque irreversible.
* [Call](https://github.com/nebulasio/wiki/blob/master/rpc.md#call): ejecuta un contrato inteligente de forma local, sin enviar datos al chain.
* [SendRawTransaction](https://github.com/nebulasio/wiki/blob/master/rpc.md#sendrawtransaction): permite enviar una transacción firmada.
* [GetTransactionReceipt](https://github.com/nebulasio/wiki/blob/master/rpc.md#gettransactionreceipt): obtiene información del recibo de una transacción mediante su hash.

Léase más sobre estos APIs [aquí](https://github.com/nebulasio/wiki/blob/master/rpc.md).

#### Pedir Tokens
Cada email puede reclamar tokens todos los días [aquí](https://testnet.nebulas.io/claim).

## Tutoriales
1. [Installation](tutorials/espanol-nebulas-101-01-instalacion.md) \(thanks [Ariel](https://github.com/arielsbecker)\)
2. [Sending a Transaction](tutorials/english-nebulas-101-02-transaction.md) \(thanks [Victor](https://github.com/victorychain)\)
3. [Writing Smart Contract in JavaScript](tutorials/english-nebulas-101-03-smart-contracts-javascript.md) \(thanks [otto](https://github.com/ottokafka)\)
4. [Introducing Smart Contract Storage](tutorials/english-nebulas-101-04-smart-contract-storage.md) \(thanks [Victor](https://github.com/victorychain)\)
5. [Interacting with Nebulas by RPC API](tutorials/english-nebulas-101-05-interacting-with-nebulas-by-rpc-api.md) \(thanks [Victor](https://github.com/victorychain)\)


## Contributing
¡Siéntete libre de unirte a la Mainnet de Nebulas! Si has encontrado un error, por favor [envía un aviso](https://github.com/nebulasio/go-nebulas/issues/new), o si eres desarrollador, [crea un pull request](https://github.com/nebulasio/go-nebulas/pulls); de ese modo podremos corregir los errores o añadir tu contribución a esta página lo antes posible.


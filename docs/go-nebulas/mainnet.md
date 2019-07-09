# Cómo unirse a la mainnet de Nebulas
## Introducción
The Nebulas Mainnet 2.0 (Nebulas Nova) ha sido recientemente lanzada. Este tutorial le enseñará cómo unirse y cómo trabajar con la  Mainnet de Nebulas.

> [https://github.com/nebulasio/go-nebulas/tree/master](https://github.com/nebulasio/go-nebulas/tree/master)

## Compilacion

El archivo ejecutable de la mainnet de Nebulas y las bibliotecas dependientes se deben compilar primero. Varios módulos importantes se enumeran a continuación:

* **NBRE:** el Nebulas Blockchain Runtime Environment es la plataforma para ejecutar la Representación del Protocolo de Nebulas, como el DIP, el NR, etc.
* **NEB:** el proceso principal de la mainnet de Nebulas. NEB y NBRE se ejecutan en procesos independientes y se comunican a través de IPC. 

Las instrucciones sobre cómo compilar los módulos se pueden encontrar en los tutoriales.

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

Para más información sobre este archivo, véase [`template.conf`](https://github.com/nebulasio/nebdocs/blob/master/docs/resources/conf/template.conf).

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

Léase más sobre estos métodos [aquí](https://github.com/nebulasio/wiki/blob/master/rpc.md).

## Tutoriales
### En inglés
1. [Instalación](https://github.com/nebulasio/wiki/blob/master/tutorials/%5BEnglish%5D%20Nebulas%20101%20-%2001%20Installation.md), (contribución de [Victor](https://github.com/victorychain)).
2. [Cómo enviar una transacción](https://github.com/nebulasio/wiki/blob/master/tutorials/%5BEnglish%5D%20Nebulas%20101%20-%2002%20Transaction.md), (contribución de [Victor](https://github.com/victorychain)).
3. [Cómo escribir un contrato inteligente en Javascript](https://github.com/nebulasio/wiki/blob/master/tutorials/%5BEnglish%5D%20Nebulas%20101%20-%2003%20Smart%20Contracts%20JavaScript.md), (contribución de [otto](https://github.com/ottokafka)).
4. [Introducción al almacenamiento de contratos inteligentes](https://github.com/nebulasio/wiki/blob/master/tutorials/%5BEnglish%5D%20Nebulas%20101%20-%2004%20Smart%20Contract%20Storage.md), (contribución de [Victor](https://github.com/victorychain)).
5. [Interacción con Nebulas por medio de la API RPC](https://github.com/nebulasio/wiki/blob/master/tutorials/%5BEnglish%5D%20Nebulas%20101%20-%2005%20Interacting%20with%20Nebulas%20by%20RPC%20API.md), (contribución de [Victor](https://github.com/victorychain)).

## Cómo contribuir
¡Siéntete libre de unirte a la Mainnet de Nebulas! Si has encontrado un error, por favor [envía un aviso](https://github.com/nebulasio/go-nebulas/issues/new), o si eres desarrollador, [crea un pull request](https://github.com/nebulasio/go-nebulas/pulls); de ese modo podremos corregir los errores o añadir tu contribución a esta página lo antes posible.

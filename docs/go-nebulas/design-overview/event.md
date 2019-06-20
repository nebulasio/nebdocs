# Funcionalidad de los eventos

La funcionalidad `Event` se utiliza para que los desarrolladores y los usuarios puedan suscribirse a eventos de su interés. Estos eventos se generan durante la ejecución del _blockchain_, y almacenan los pasos de la ejecución y sus resultados en el _chain_. Para consultar y verificar los resultados de la ejecución de transacciones y contratos inteligentes, se almacenan esos dos tipos de eventos en un _trie_ en el _chain_.

Estructura **evento**:

```go
type Event struct {
    Topic string // event topic, subscribe keyword
    Data  string // event content, a json string
}
```

Al generarse un evento, se debe procesar con [eventEmitter](https://github.com/nebulasio/go-nebulas/blob/master/core/event.go). Los usuarios pueden suscribirse al emisor del evento.

Si no hay suscripción al evento, este se descartará. Para todo nueva suscripción, si el canal no se bloquea a tiempo, se descartará el evento debido al mecanismo _non-blocking_.

## Lista de eventos:

* [TopicNewTailBlock](event.md#topicnewtailblock)
* [TopicRevertBlock](event.md#topicrevertblock)
* [TopicLibBlock](event.md#topiclibblock)
* [TopicPendingTransaction](event.md#topicpendingtransaction)
* [TopicTransactionExecutionResult](event.md#topictransactionexecutionresult)
* [EventNameSpaceContract](event.md#eventnamespacecontract)

### Referencia

#### TopicNewTailBlock

Este evento se dispara cuando el último bloque de la cadena se actualiza.

* Tópico:`chain.newTailBlock`
* Datos:
  * `height`: altura del bloque
  * `hash`: hash del bloque
  * `parent_hash`: hash del bloque padre
  * `acc_root`: hash raíz del estado de la cuenta
  * `timestamp`: timestamp del bloque
  * `tx`: hash raíz del estado de la transacción
  * `miner`: minero del bloque

#### TopicRevertBlock

Este evento ocurre cuando se revierte un bloque en la cadena.

* Tópico:`chain.revertBlock`
* Datos: El contenido del tópico es similar a los datos de [TopicNewTailBlock](event.md#topicnewtailblock).

#### TopicLibBlock

Este evento se dispara cuando el último bloque irreversible sufre un cambio.

* Tópico:`chain.latestIrreversibleBlock`
* Dato: El contenido del tópico es similar a los datos de [TopicNewTailBlock](event.md#topicnewtailblock).

#### TopicPendingTransaction

Este evento se dispara cuando se introduce una transacción en el pozo de transacciones (_transaction pool_).

* Tópico:`chain.pendingTransaction`
* Datos:
  * `chainID`: id de cadena
  * `hash`: hash de la transacción
  * `from`: cadena que indica la dirección origen de la transacción
  * `to`: cadena que indica la dirección destino de la transacción
  * `nonce`: nonce de la transacción
  * `value`: valor de la transacción
  * `timestamp`: timestamp de la transacción
  * `gasprice`: precio del gas para la transacción
  * `gaslimit`: límite de gas para la transacción
  * `type`: tipo de transacción

#### TopicTransactionExecutionResult

Este evento ocurre cuando se ejecuta el final de una transacción. This event will be recorded on the chain, and users can query with RPC interface [GetEventsByHash](https://github.com/nebulasio/wiki/blob/master/rpc.md#geteventsbyhash).

Este evento registra los resultados de la ejecución de la transacción, y es sumamente importante.

* Tópico:`chain.transactionResult`
* Datos:
  * `hash`: hash de la transacción
  * `status`: estado de la transacción; 0: falló, 1: sin errores, 2: pendiente
  * `gasUsed`: gas usado para la transacción
  * `error`: error de ejecución de la transacción. Si no hubo errores, este campo estará vacío.

#### EventNameSpaceContract

Este evento ocurre cuando se ejecuta un contrato inteligente. Si la ejecución ocurre exitosamente, los eventos quedarán registrados en el chain y pueden ser suscritos; por el contrario, si ocurre un error en la ejecución, el evento no se registrará.

Este evento también se registrará en el chain, donde los usuarios lo pueden consultar mediante la interfaz RPC[GetEventsByHash](https://github.com/nebulasio/wiki/blob/master/rpc.md#geteventsbyhash).

* Tópico: `chain.contract.[topic]` El tópico del evento del contrato tiene el prefijo `chain.contract.`, El contenido es definido por quien escribe el contrato.
* Datos: El contenido es definido por quien escribe el contrato.

### Suscripción

Es posible suscribirse a todos los eventos, y el _cloud chain_ expone una interfaz RPC de suscripción llamada [Subscribe](https://github.com/nebulasio/wiki/blob/master/rpc.md#subscribe). Es importante notar que la suscripción a los eventos es un mecanismo ajeno al blockchain. Los nuevos eventos se descartarán si la interfaz RPC no se maneja a tiempo.

### Consultas

Sólo es posible consultar los eventos registrados en el chain, mediante el uso de la interfaz RPC [GetEventsByHash](https://github.com/nebulasio/wiki/blob/master/rpc.md#geteventsbyhash).

Al momento, es posible consultar los siguientes eventos:

* [TopicTransactionExecutionResult](event.md#topictransactionexecutionresult)
* [EventNameSpaceContract](event.md#eventnamespacecontract)
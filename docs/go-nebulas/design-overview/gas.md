# _Gas_ para transacciones (_Transaction Gas_)

En Nebulas, tanto una transacción normal en la que se transfiere balance, o un contrato inteligente, utilizan _gas_, cuyo costo se deduce del balance de la dirección que origina la transacción o que hace uso del contrato.

Una transacción contiene dos parámetros: `gasPrice` y `gasLimit`.

* `gasPrice`: el valor de la unidad de _gas_.
* `gasLimit`: el límite máximo de uso de _gas_.

El valor del consumo de _gas_ en una transacción está dado por la fórmula:

`gasPrice` * `gasUsed`

que será la recompensa otorgada a los mineros. El valor `gasUsed` debe ser igual o menor a `gasLimit`. El posible valor de `gasUsed` se p
uede estimar a través de la interfaz RPC [_estimategas_](https://github.com/nebulasio/wiki/blob/master/rpc.md#estimategas); el mismo se almacenará en el evento de resultado de la ejecución de la transacción.

## Razones para este diseño

Tal como Bitcoin y Ethereum, el _gas_ en Nebulas se utiliza para pagar el costo de las transacciones. Tiene dos propósitos fundamentales:

* Como recompensa para mineros, para incentivarlos a empaquetar transacciones. El empaquetamiento de esas transacciones conlleva un costo computacional —en especial la ejecución de contratos inteligentes— por lo que los usuarios deben pagar por ese servicio.
* Como deterrente costoso para los atacantes. El ataque _DDoS_ (Distributed Denial of Service) es bastante económico en internet, y cualquier hacker inescrupuloso podría enviar volúmenes muy elevados de transacciones al servidor objeto de sus ataques. En las redes Bitcoin y Ethereum, cada transacción tiene un costo económico, lo que incrementa significativamente el costo de un ataque de este tipo, haciéndolos imprácticos.

## Constitución del _gas_

Cuando un usuario emite una transacción, el _gas_ se utilizará del siguiente modo:

* `Emisión de la transacción`
* `Almacenamiento de los datos de la transacción`
* `Inserción del _payload_`
* `Ejecución del _payload_ (contrato inteligente)`

En todos estos puntos se consumen recursos de la red Nebulas, y por ende es necesario pagar por ellos.

### Envío de transacciones

Al enviar una transacción esta se añadirá al final del bloque al que fue asignada. Los mineros se encargarán de realizar el proceso, por el cual deben recibir una recompensa; para ello, se usará una cantidad determinada de _gas_, que se calcula del siguiente modo:

```text
// TransactionGas: gas consumido en una transacción normal
TransactionGas = 20000
```

Si la verificación de la transacción falla, el gas se devolverá al emisor.

### Almacenamiento de datos de transacción

Al implementar un contrato —o una llamada al mismo—, los datos de su ejecución se almacenan en el blockchain, lo que también tiene un costo. La fórmula del costo asociado de _gas_ es la siguiente:

```text
TransactionDataGas = 1

len(data) * TransactionDataGas
```8

El campo `TransactionDataGas` es un número fijo definido en el código.

Para cada tipo de _payload_ en una transacción existen diferentes consumos de gas al momento de su ejecución. Los tipos de transacción soportados por Nebulas en este momento son los siguientes:

* `binary`: permite a los usuarios adjuntar datos binarios durante la ejecución del contrato inteligente. Estos datos no se procesan durante la ejecución. En este caso `TransactionDataGas` es 0.
* `deploy` y `call`: Los tipos `deploy` y `call` les permiten a los usuarios implementar contratos inteligentes en Nebulas. Nebulas debe correr su máquina virtual `nvm` para ejecutar el contrato, por lo que se debe pagar por el derecho de uso de ese recurso. En este caso `TransactionDataGas` es 60.

### Ejecución de los _payloads_ (Smart contract deploy & call)

Como el tipo de transacción `binary` no realiza ningún tipo de procesamiento al momento de ejecutar la transacción, la ejecución no requiere _gas_. Por el contrario, cuando un contrato inteligente implementa una llamada al momento de la emisión de la transacción, la ejecución consumirá recursos de la computadora de los mineros, y probablemente se haga uso también del almacenamiento de datos en el blockchain.

#### Ejecución de instrucciones

Cada ejecución de un contrato utiliza recursos de los mineros, que se deben pagar. El contador de instrucciones V8 realiza el cálculo del número de instrucciones a procesar. El límite de número de instrucciones a ejecutar impide el uso excesivo de recursos y la generación de _deadlocks_.

#### Almacenamiento de contratos

La característica `LocalContractStorage`, que permite almacenar objetos de los contratos inteligentes, utiliza una unidad de _gas_ cada 32 bytes que se almacenan. La lectura de esos objetos no consume gas.

El límite para la ejecución de contratos está dada por:

```text
    gasLimit - TransactionGas - len(data) * TransactionDataGas - TransactionPayloadGasCount[type]
```

## Matriz de conteo de _gas_

La matriz de conteo de _gas_ para la ejecución de los contratos inteligentes está dada por:

| Operador | Conteo de gas/operador | Descripción |
| --- | ---: | :--- |
| Binario | 1 | Operador binario y lógico |
| Carga | 2 | Carga desde memoria |
| Almacenamiento | 2 | Almacenamiento a memoria |
| Retorno | 2 | Devolución de valor, almacenamiento a memoria |
| Llamada (interna) | 4 | Llamada a funciones del mismo contrato inteligente |
| Llamada (externa) | 100 | Llamada a funciones de otro contrato inteligente |

| Expresión | Código de ejemplo | Op. binario | Op. de carga | Op. de almacenamiento | Op. de devolución | Op. llamada interna | Conteo de gas |  |  |
| --- | :--- | ---: | ---: | ---: | ---: | ---: | ---: |
| CallExpression | a\(x, y\) | 0 | 0 | 1 | 1 | 1 | 8 |  |  |
| AssignmentExpression | x&=y | 1 | 0 | 1 | 0 | 0 | 3 |  |  |
| BinaryExpression | x==y | 1 | 0 | 0 | 1 | 0 | 3 |  |  |
| UpdateExpression | x++ | 1 | 0 | 1 | 0 | 0 | 3 |  |  |
| UnaryExpression | x+y | 1 | 0 | 0 | 1 | 0 | 3 |  |  |
| LogicalExpression | x |  | y | 1 | 0 | 0 | 1 | 0 | 3 |
| MemberExpression | x.y | 0 | 1 | 0 | 1 | 0 | 4 |  |  |
| NewExpression | new X\(\) | 0 | 0 | 1 | 1 | 1 | 8 |  |  |
| ThrowStatement | throw x | 0 | 0 | 0 | 1 | 1 | 6 |  |  |
| MetaProperty | new.target | 0 | 1 | 0 | 1 | 0 | 4 |  |  |
| ConditionalExpression | x?y:z | 1 | 0 | 0 | 1 | 0 | 3 |  |  |
| YieldExpression | yield x | 0 | 0 | 0 | 1 | 1 | 6 |  |  |
| Event |  | 0 | 0 | 0 | 0 | 0 | 20 |  |  |
| Storage |  | 0 | 0 | 0 | 0 | 0 | 1 gas/bit |  |  |

## Consejos

En Nebulas, el _pozo_ de transacciones de cada nodo tiene un valor mínimo y máximo para `gasPrice` y un valor máximo para `gasLimit`. Si el valor de `gasPrice` en la transacción no está en el rango del parámetro `gasPrice` definido en el pozo de transacciones, o el valor para `gasLimit` es mayor que el definido en ese pozo, la transacción será rechazada.

Configuración de los parámetros gasPrice y gasLimit:

### `gasPrice`
* mínimo: el valor mínimo de `gasPrice` se puede establecer en el archivo de configuración. Si no está configurado, el valor por defecto es `1000000` (10^6).
* máximo: el valor máximo para `gasPrice` es de `1000000000000` (10^12).

No se debe superar los valores máximos y mínimos en la configuración del pozo de transacciones.

### `gasLimit`
* mínimo: debe ser mayor a cero.
* máximo: su valor máximo es de `50000000000` (50 * 10^9).

No se debe superar los valores máximos y mínimos en la configuración del pozo de transacciones.
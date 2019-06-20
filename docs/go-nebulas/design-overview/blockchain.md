# Blockchain

## Modelo

Nebulas utiliza un modelo de cuentas en vez del modelo UTXO (_[Unspent Transaction Output](https://en.wikipedia.org/wiki/Unspent_transaction_output)_). La ejecución de transacciones requiere el consumo de _gas_.

## Estructura de datos

### Estructura de los bloques

```text
+---------------+----------------+--------------+
|  blockHeader  |  transacciones | dependencias |
+---------------+----------------+--------------+
```

* blockHeader: información de encabezado
* transactions: matriz de transacciones
* dependency: relación de dependencias entre transacciones

### Estructura del encabezado de los bloques

```text
+-----------+--------+--------------+------------+-------------+-------+--------+
|  chainid  |  hash  |  parentHash  |  coinbase  |  timestamp  |  alg  |  sign  |
+-----------+--------+--------------+------------+-------------+-------+--------+
+-------------+-----------+--------------+-----------------+
|  stateRoot  |  txsRoot  |  eventsRoot  |  consensusRoot  |
+-------------+-----------+--------------+-----------------+
```

* chainid: identificador de la cadena a la que pertenece el bloque
* hash: hash del bloque
* parentHash: hash del bloque padre
* coinbase: cuenta que recibirá la _recompensa de acuñación_ (minting reward)
* timestamp: el número de nanosegundos transcurridos desde el 1 de enero de 1970, UTC
* alg: algoritmo de firma (_signature_) utilizado
* sign: firma del hash del bloque
* stateRoot: hash raíz del estado de cuenta
* txsRoot: hash raíz del estado de la transacción
* eventsRoot: hash raíz del estado de los eventos
* consensusRoot: estado del consenso, incluyendo proponente y dinastía de validadores

### Estructura de las transacciones

```text
+-----------+--------+--------+------+---------+---------+-------------+
|  chainid  |  hash  |  from  |  to  |  value  |  nonce  |  timestamp  |
+-----------+--------+--------+------+---------+---------+-------------+
+--------+------------+------------+
|  data  |  gasPrice  |  gasLimit  |
+--------+------------+------------+
```

* chainid: identificador de la cadena a la que pertenece el bloque
* hash: hash de la transacción
* from: dirección de la cartera del emisor
* to: dirección de la cartera del receptor
* value: monto transferido
* nonce: _nonce_ de la transacción
* timestamp: el número de nanosegundos transcurridos desde el 1 de enero de 1970, UTC
* alg: algoritmo de firma (_signature_) utilizado
* sign: firma del hash del bloque
* data: datos de la transacción, incluyendo el tipo de transacción (binaria, implementación de contrato inteligente, llamada a contrato inteligente) y su _payload_.
* gasPrice: el precio unitario del gas consumido por la transacción
* gasLimit: la cantidad máxima permitida de gas a consumir en la transacción

## Actualización del blockchain

En nuestra opinión, un blockchain debería ocuparse únicamente de procesar nuevos bloques para lograr un crecimiento seguro y eficiente. Aun más: un blockchain sólo puede obtener nuevos bloques a través de dos canales, que detallamos a continuación.

### Desde la red

A causa de la inestabilidad en la latencia de la red, es imposible asegurar directamente que los nuevos bloques recibidos se puedan vincular a la cadena actual. Así, se hace necesaria la implementación de _pozos de bloque_ (_blocks pool_) para almacenar los nuevos bloques.

### Desde un minero local

En principio, necesitamos que el **pozo de transacciones** almacene las transacciones recibidas desde la red. A partir de ello, es necesario esperar a que se genere un nuevo bloque a través de un componente local de consenso, tal como _DPoS_.

Sin importar la procedencia del nuevo bloque, Nebulas utiliza los mismos pasos para procesarlo; veamos esos pasos en detalle.

![](../../resources/blockpool.png)

## Estado global

Cada bloque contiene el estado global, que se compone del estado de cuatro componentes que detallamos abajo. Todos ellos son mantenidos mediante [Merkle Trees](merkle_trie.md).

### Estado de cuentas

Todas las cuentas del bloque actual se almacenan en Estados de Cuentas.

Las cuentas se dividen en dos tipos: normales, y contratos inteligentes.

#### Normales

Incluye:

* **Direcciones de carteras**
* **Balance**
* **_nonce_**: el _nonce_ de cada cuenta, cuyo valor se incrementa de 1 en 1.

#### Contratos Inteligentes

Incluye:

* **Direcciones de contratos inteligentes**
* **Balance**
* **Lugar de nacimiento**: el _hash_ de la transacción en la que tuvo lugar la implementación del contrato.
* **Variables**: Contiene todos los valores de las variables del contrato.

### Estados de transacciones

Todas las transacciones enviadas al blockchain se almacenan en Estados de Transacciones.

### Estados de eventos

Mientras se ejecutan las transacciones, es posible que se disparen múltiples eventos. Todos los eventos disparados por transacciones en el blockchain se almacenan en Estados de Eventos.

### Estado del Consenso

El contexto del algoritmo de consenso se almacena en Estado de Consenso.

Con respecto a DPoS, el estado del consenso incluye:

* **timestamp**: el timestamp actual.
* **proposer**: propositor actual.
* **dynasty**: dinastía de validadores actual.

### Serialización

Elegimos utilizar Protocol Buffers para la serialización general, en vista de los siguientes beneficios que otorga:

* Solidez comprobada a gran escala.
* Eficiencia. Omite _key literals_ y en su lugar utiliza codificación _varint_.
* Brinda soporte a multi-tipos.
* Brinda soporte a clientes multilenguaje.
* API fácil de utilizar.
* Su _schema_ es ideal para las comunicaciones.
* Su _schema_ es ideal para hacer versionado y extensiones; por ejemplo, para añadir nuevos campos de mensaje o para dejar otros en desuso.

En especial y para mejorar la legibilidad del código de los contratos inteligentes, utilizamos JSON en vez de _protobuf_ para su codificación.

## Sincronización

En ocasiones recibiremos un bloque cuya altura es mucho mayor que la del último bloque de la cadena. Cuando esto ocurre, necesitamos sincronizar los bloques desde los nodos de pares para estar a tono con ellos.

Nebulas provee dos métodos para la sincronización de bloques con nodos de pares: _Chunks Downloader_ y _Block Downloader_. Si la diferencia es mayor a 32 bloques, debemos elegir Chunk Downloader para descargar gran cantidad de bloques en trozos. Si no, elegiremos _Block Downloader_ para descargar los bloques uno a la vez.

### _Chunks Downloader_

El _chunk_ es una colección de 32 bloques sucesivos. _Chunks Downloader_ nos permite descargar, cada vez, un máximo de 10 chunks que suceden a nuestro último bloque. Este mecanismo nos ayuda a minimizar el número de paquetes de red y a lograr una mayor seguridad.

#### Procedimiento

```text
1. **A** envía su último bloque a una cantidad _n_ de pares remotos.
2. Los pares remotos localizan el _chunk_ **C** que contiene el último bloque de **A**.
3. Los pares remotos le envían a **A** el encabezado de **C**, los encabezados de los siguientes 9 _chunks_ después de **C**, y los hashes de todos esos encabezados.
4. Si **A** recibe más de la mitad de los mismos encabezados **H**, **A** intentará sincronizar los _chunks_ representados por **H**.
5. Si **A** recibió todos los chunks representados por **H** y los enlazó correctamente a su cadena, se repite todo el proceso desde el punto 1.
```

En los pasos 1 a 3, utilizamos la decisión mayoritaria para confirmar los _chunks_ en la cadena canónica. Luego, descargamos los bloques en los _chunks_ del paso 4.

##### Nota

* `ChunkHeader` contiene una matriz de 32 bloques hash más el hash de la matriz.
* `ChunkHeaders` contiene una matriz de 10 `ChunkHeaders` más el hash de la matriz.

Aquí podemos observar un diagrama de este procedimiento de sincronización:

![](../../resources/the-diagram-of-sync-process.png)

### _Block Downloader_

Cuando la diferencia de longitud entre nuestra cadena local y la cadena canónica es menor a 32, utilizaremos _Block Downloader_ para descargar los bloques faltantes, uno a la vez.

#### Procedimiento

```text
1. **C** transfiere el nuevo bloque **B** a **A** y **A** encuentra que la altura de **B** es mayor que la del último bloque actual.
2. **A** reenvía el hash del bloque **B** a **C** para descargar el bloque padre de **B**.
3. Si **A** recibe el bloque padre de **B**, **A** intentará enlazar el bloque **B** con el último bloque de la cadena de **A**.
4. Si falla, **A** volverá al paso 2 y continuará descargando el bloque padre de **B**. Si no falla, el proceso termina.
```

El procedimiento se repite hasta que **A** queda totalmente sincronizado con la cadena canónica.

Aquí podemos ver un diagrama de este procedimiento:

![](../../resources/the-diagram-of-download-process.png)
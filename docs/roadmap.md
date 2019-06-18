# Hoja de ruta de la red autónoma Metanet

Para una versión más actualizada, visita [esta página](https://nebulas.io/roadmap.html).

## Hitos

* En diciembre de 2017 la testnet de Nebulas entrará en línea.
* El primer trimestre de 2018, la versión 1.0 de Nebulas será lanzada y la mainnet estará en línea.

### Versión 1.0 (primer trimestre de 2018)

* Blockchain completamente funcional, con JavaScript y TypeScript como lenguajes para los contratos inteligentes.
* Una cartera Nebulas amigable con el usuario, tanto para escritorio como para dispositivos móviles, que permita administrar los activos en Nebulas.
* Un explorador web de bloques, que le permita a los desarrolladores y al usuario en general buscar y visualizar todos los datos en Nebulas.

### Versión 2.0 (cuarto trimestre de 2018)

* Agregar _Nebulas Rank_ (NR) a cada dirección en Nebulas, ayudando así a los usuarios y desarrolladores a encontrar valor en los datos.
* Implementar el Protocolo de Incentivos para Desarrolladores (_Developer Incentive Protocol_, DIP) para alentar a que los desarrolladores construyan aplicaciones de valor en Nebulas.

### Versión 3.0 (cuarto trimestre de 2019)

* _Nebulas Force_ totalmente funcional.
* Implementación de _Proof of Devotion_.

## Metas de largo plazo

* Escalabilidad para altos volúmenes de transacciones.
* Soporte _subchain_.
* Integración con _Zero-knowledge Proof_.

## Versiones

### Versión 0.1.0

Hecha

#### Metas

* Implementar un kernel en Nebulas.
* Blockchain en memoria con implementación del consenso _PoW_.
* Soporte de red P2P completo.

Puedes descargarla [desde este enlace](https://github.com/nebulasio/go-nebulas/releases/tag/v0.1.0).

### v0.2.0

Hecha.

#### Metas

* Proveer llamadas RPC API para realizar consultas y transacciones de forma externa.
* Implementar _Sync Protocol_ para ayudar al lanzamiento de cualquier nodo que se una a la red de Nebulas en cualquier momento.

#### Núcleo

* Implementar un pool de transacciones.
* Impedir ataques de tipo _record-replay_ en las transacciones.
* Integrar un _Protocol Buffer_ para la serialización.

#### Red

* Refactorear el diseño de la red.
* Implementar _Sync Protocol_.
* Implementar una función de _Broadcast and Relay_.

#### API

* Añadir _Balance API_.
* Añadir _Transaction API_.
* Añadir una API para depuración; ejemplos: «Dump Chain» o «Dump Block».

#### Crypto

* Brindar soporte a los archivos Ethereum-keystore.
* Brindar soporte a los archivos _multi key management_ en KeyStore.

Puedes descargarla [desde este enlace](https://github.com/nebulasio/go-nebulas/releases/tag/v0.2.0).

### v0.3.0

Hecha.

#### Metas

* Soporte de almacenamiento en disco disk storage para todos los datos de blockchain.
* Añadir un motor de ejecución de contratos inteligentes basado en Chrome V8.

#### Núcleo

* Agregar almacenamiento en disco por medio de _middleware_.
* Implementar transacciones de contratos inteligentes.

#### NVM

* Integrar Chrome V8 como motor de ejecución de contratos inteligentes.

Puedes descargarla [desde este enlace](https://github.com/nebulasio/go-nebulas/releases/tag/v0.3.0).

### Versión 0.4.0

Hecha.

#### Metas

* Implementar cálculo de costos de gas en el motor de ejecución de contratos inteligentes.
* Brindar soporte a más APIs.
* Añadir _repl_ a las aplicaciones neb.
* Añadir métricas y capacidad de generar reportes.

#### Núcleo

* Añadir campos relacionados al gas en Transacciones.
* Implementar un mecanismo de cálculo de gas.

#### NVM

* Añadir límites de ejecución al motor V8.
* Implementar un mecanismo de cálculo de gas.

#### CMD

* Añadir _repl_ a las aplicaciones neb.

#### Misceláneos

* Crear más API.
* Añadir métricas y capacidad de generar reportes.

Puedes descargarla [desde este enlace](https://github.com/nebulasio/go-nebulas/releases/tag/v0.4.0).

### Versión 0.5.0

Hecha.

#### Metas

* Preparar todo para el lanzamiento de la _testnet_.
* Mejorar la estabilidad.

#### Núcleo

* Mejorar la estabilidad
* Añadir cualquier funcionalidad que falte.

#### Consenso

* Implementar el algoritmo de consenso _DPoS_.
* Continuar el desarrollo del algoritmo _PoD_.

#### NVM

* Completar la matriz de costos de gas.
* Brindar soporte a la funcionalidad _Event liked pubsub_.

#### Misceláneos

* Añadir más métricas para monitorizar la estabilidad de las aplicaciones neb.

Puedes descargarla [desde este enlace](https://github.com/nebulasio/go-nebulas/releases/tag/v0.5.0).

### Versión 0.6.0

Hecha.

#### Metas

* Mejoras en la estabilidad y la performance.
* Reconstruir la red P2P.
* Rediseñar la lógica _block sync_.

#### Testnet

* Corregir errores y mejorar la performance.

#### Red

* Añadir _Stream_ para la administración de conexiones simples.
* Añadir _StreamManager_ para la administración de conexiones.
* Implementar mensajes prioritarios _chan_.
* Añadir una estrategia de persistencia de tablas de enrutamiento.
* Mejorar la estrategia para procesar el empalme de paquetes TCP.

#### Registro

* Añadir funcionalidad de registro de consola (CLog), con salida a consola y archivo, para que los desarrolladores puedan saber qué sucede en Neb.
* Añadir un registro _verbose_ (VLog),con salida a consola y archivo, para brindar más detalles en la depuración de Neb.
* Ajuste de registros.

#### Sincronización

* Utilizar un _chunk header hash_ para incrementar la performance de sincronización.
* Ajustar la lógica de reintentos síncronos y la configuración para eventos _timeout_.
* Corregir errores en la sincronización y añadir más métricas de estadísticas.

Puedes descargarla [desde este enlace](https://github.com/nebulasio/go-nebulas/releases/tag/v0.6.0).

### Versión 0.6.1

Hecha.

#### Metas

* Mejorar la compatibilidad de la _testnet_.

#### Núcleo

* Actualizar la estructura de almacenamiento de los bloques.

Puedes descargarla [desde este enlace](https://github.com/nebulasio/go-nebulas/releases/tag/v0.6.1).

### Versión 0.8.0

Hecha.

#### Metas

* Implementar el nuevo explorador de bloques.
* Lanzar la nueva cartera.
* Implementar nuevas herramientas web _Playground_ para interactuar con Nebulas.

### Versión 1.0.0

Hecha.

#### Metas

* Preparar todo para lanzar la _mainnet_.
* Brindar soporte a JavaScript y TypeScript en los contratos inteligentes.
* Lograr un sistema de blockchain estable y de alta performance.
* Lanzar el nuevo explorador de bloques de Nebulas.
* Lanzar la nueva cartera de Nebulas, tanto para escritorio como para móviles.
* Crear herramientas de desarrollo basadas en web.

Descarga el [explorador de bloques](https://github.com/nebulasio/explorer/releases/tag/explorer-tag-1.0.0).

Descarga la [cartera](https://github.com/nebulasio/web-wallet).

Descarga la librería [neb.js](https://github.com/nebulasio/neb.js).

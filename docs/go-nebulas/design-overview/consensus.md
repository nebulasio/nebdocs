# Consenso

Cada algoritmo de consenso puede ser descripto como una combinación de _State Machine_ y _Fork Choice Rules_.

## DPoS (Delegate Proof-of-Stake)

DPoS puede ser descrita como una _state machine_ o máquina de estado finito.

### Advertencia

El consenso en Nebulas será _PoD_; la elección del algoritmo DPoS es sólo una solución temporaria. Luego de la verificación formal del algoritmo _PoD_, haremos la transición de la _mainnet_ a ese algoritmo definitivo. Todos los _testigos_ (contables y mineros) de DPoS son, por ahora, cuantas mantenidas oficialmente por Nebulas; nos encargaremos de realizar una transición suave de DPoS a PoD y crearemos un nuevo fondo para administrar todas las recompensas para los contables y para incentivar el crecimiento de nuestro ecosistema.

### _State Machine_

![](../../resources/dpos.png)

### _Fork Choice Rules_

1. Se debe elegir la ruta más larga como la ruta canónica.
1. Si las rutas A y B tienen la misma longitud, se debe elegir aquella con el menor _hash_.

## PoD (Proof-of-Devotion), o Prueba de Devoción

Actualmente este algoritmo se encuentra en desarrollo; puedes encontrar una versión [aquí](https://github.com/nebulasio/research/tree/master/pod).

### _State Machine_

![](../../resources/pod.png)

### _Fork Choice Rules_

1. Se debe elegir la cadena (chain) con la mayor cantidad de votos.
1. Si los _chains_ A y B tienen la misma cantidad de votos, se debe elegir aquel con el menor _hash_.
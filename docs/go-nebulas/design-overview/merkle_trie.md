# _Merkle Patricia Tree_

## Introducción: _Radix Tree_

> [Referencia](https://en.wikipedia.org/wiki/Radix_tree)

Un Radix Tree que utiliza una dirección a modo de clave tendrá las siguientes características:

* Las direcciones se representan mediante caracteres hexadecimales.
* Cada nodo del árbol es una matriz de 16 elementos (0123...def).
* Nodos _leaf_: su valor puede ser cualquier dato binario.
* non-leaf node: su valor es un hash calculado en base a los datos de sus nodos __children_.

Para una dirección de 160-bits, la altura máxima del árbol es 40.

![](../../resources/radix_tree.png)

**Problemas:** mucho espacio para una entrada sencilla; 40 pasos para cada _lookup_

## Avanzado: _Merkle Patricia Tree_

> [Referencia](https://github.com/ethereum/wiki/wiki/Patricia-Tree), [http://gavwood.com/Paper.pdf](http://gavwood.com/Paper.pdf)

Para reducir el espacio de almacenamiento de Radix Tree, los nodos en _Merkle Patricia Tree_ se dividen en tres tipos:

* nodo de extensión: comprime los nodos utilizando prefijos comunes.
* nodo hoja: comprime los nodos utilizando prefijos únicos
* nodo rama: ídem a los nodos en el Radix Tree.

![](../../resources/merkle_tree.png)

## Cómo almacenar _Merkle Patricia Tree_

### Almacenamiento de los pares clave/valor

hash(value) = sha3(serialize(value))

key = hash(value)

## Cómo actualizar _Merkle Patricia Tree_

### Consulta

DFS de arriba hacia abajo

#### Actualizar, Eliminar o Añadir

1. Realizar consultas en el nodo, desde arriba hacia abajo.
1. Actualizar el hash y el path desde abajo hacia arriba.

![](../../resources/merkle_tree_update.png)

**Performance** Cada operación cuesta O (log (n)).

## Cómo realizar verificaciones usando _Merkle Patricia Tree_

### Teoremas

1. Los _Merkle Trees_ iguales deben tener el mismo _root hash_.
1. Los _Merkle Trees_ distintos deben tener distintos _root hash_.

Usando los teoremas se puede verificar el resultado de la ejecución de las transacciones.

### Verificación rápida

Un cliente _lightweight_, sin necesidad de sincronizar enormes bloques de transacciones, puede determinar inmediatamente el estado y el balance exacto de cualquier cuenta simplemente consultando la red para buscar una ruta desde la raíz hasta la cuenta nodo.
# Smart Contract

## Lenguajes

Los contratos inteligentes en la plataforma Nebulas pueden ser escritos en dos lenguajes:

* [JavaScript](https://en.wikipedia.org/wiki/JavaScript)
* [TypeScript](https://en.wikipedia.org/wiki/TypeScript)

Ambos lenguajes están integrados mediante [Chrome V8](https://developers.google.com/v8/), un motor javascript desarrollado por el proyecto Chromium (Google Chrome y navegadores Chromium derivados).

## Modelo de ejecución

El diagrama de aquí abajo representa el modelo de ejecución de los contratos inteligentes:

![Smart Contract Execution Model](../resources/smart_contract_execution_model.png)

1. El código completo del contrato inteligente, junto a sus argumentos, son empaquetados en la transacción e implementados en Nebulas.
2. La ejecución de todo contrato inteligente se divide en dos fases:
   1. Pre-proceso: inyectar traceo de ejecuciones y otras medidas de seguridad similares.
   2. Ejecución: generar el código ejecutable y lanzarlo.

## Contratos

Los contratos en Nebulas son similares a las clases en el contexto de lenguajes de programación orientados a objetos. Contienen datos persistentes en variables de estado y funciones que pueden modificar dichas variables.

### Escribir un contrato

Un contrato, tanto en JavaScript como en TypeScript, debe ser siempre un prototipo de objeto o bien una clase.

Todo contrato debe incluir una función `init`, que se ejecutará por única vez durante la implementación en el blockchain.

Las funciones cuyo nombre lleva el prefijo `_` son de tipo `private` y no se pueden ejecutar directamente en una transacción (ya que están fuera de scope). Todas las demás se consideran `public` y se pueden ejecutar directamente en una transacción.

Debido a que los contratos se ejecutan en Chrome V8, las variables de instancia se cargan en memoria, por lo que no es recomendable guardarlas a todas en un [state trie](https://github.com/nebulasio/wiki/blob/master/merkle_trie.md). Con este fin, Nebulas expone los objetos `LocalContractStorage` y `GlobalContractStorage` que les permiten a los desarrolladores definir cuáles son los campos que se requiere almacenar en un _state trie_. Estos campos se deben definir en el `constructor` del contrato, antes de cualquier otra función.

El siguiente es un ejemplo de contrato inteligente:

```javascript

class Rectangle {
	constructor() {
		// define fields stored to state trie.
		LocalContractStorage.defineProperties(this, {
			height: null,
			width: null,
		});
	}

	// init function.
	init(height, width) {
		this.height = height;
		this.width = width;
	}

	// calc area function.
	calcArea() {
		return this.height * this.width;
	}

	// verify function.
	verify(expected) {
		let area = this.calcArea();
		if (expected != area) {
			throw new Error("Error: expected " + expected + ", actual is " + area + ".");
		}
	}
}

```

### Alcance

En Nebulas se definen dos tipos de alcance: `public` y `private`:

* `public`: toda función cuyo nombre coincide con la expresión regular `^[a-zA-Z$][A-Za-z0-9_$]*$` es pública —con excepción de `init`—. Las funciones públicas se pueden llamar directamente desde `Transaction`.
* `private`: toda función cuyo nombre coincide con el prefijo `_` es privada. Una función privada sólo puede ser llamada por una función pública, u otra función privada, pero nunca directamente desde `Transaction`.

## Objetos globales

### console

El módulo `console` brinda una consola de depuración similar a la de JavaScript que poseen los navegadores modernos.

La consola global se puede utilizar sin necesidad de realizar una llamada a `require('console')`.

#### console.info\(\[...args\]\)

* `...args <any>`

  La función console.info\(\) es un alias de `console.log()`.

#### console.log\(\[...args\]\)

* `...args <any>`

  Imprime los parámetros `args` en el registro de Nebulas, a nivel de `info`.

#### console.debug\(\[...args\]\)

* `...args <any>`

  Imprime los parámetros `args` en el registro de Nebulas, a nivel de `debug`.

#### console.warn\(\[...args\]\)

* `...args <any>`

  Imprime los parámetros `args` en el registro de Nebulas, a nivel de `warn`.

#### console.error\(\[...args\]\)

* `...args <any>`

  Imprime los parámetros `args` en el registro de Nebulas, a nivel de `error`.

### LocalContractStorage

El módulo `LocalContractStorage` provee un _state trie_ basado en la capacidad de almacenamiento. Acepta sólo pares de valores clave de tipo cadena.

Todos los datos se almacenan en un state trie privado, asociado a la dirección del contrato actual. Únicamente el contrato puede acceder a la misma.

```typescript

interface Descriptor {
	// serializar el valor a una cadena;
	stringify?(value: any): string;

	// de-serializar el valor desde una cadena;
	parse?(value: string): any;
}

interface DescriptorMap {
	[fieldName: string]: Descriptor;
}

interface ContractStorage {
	//obtener y devolver el valor de una clave desde Native Storage.
	rawGet(key: string): string;
	// establecer clave y par de valores en Native Storage,
	// devolver 0 si no hay errores.
	rawSet(key: string, value: string): number;

	// definir una propiedad llamada `fieldname` para el objeto `obj` con descriptor.
	// el descriptor por defecto es JSON.parse/JSON.stringify.
	// devolver lo siguiente:
	defineProperty(obj: any, fieldName: string, descriptor?: Descriptor): any;

	// definir las propiedades del objeto `obj` desde `props`.
	// el descriptor por defecto es JSON.parse/JSON.stringify.
	// devolver lo siguiente:
	defineProperties(obj: any, props: DescriptorMap): any;

	// define a StorageMap property named `fieldname` to `obj` with descriptor.
	// el descriptor por defecto es JSON.parse/JSON.stringify.
	// devolver lo siguiente:
	defineMapProperty(obj: any, fieldName: string, descriptor?: Descriptor): any;

	// define StorageMap properties to `obj` from `props`.
	// el descriptor por defecto es JSON.parse/JSON.stringify.
	// devolver lo siguiente:
	defineMapProperties(obj: any, props: DescriptorMap): any;

	// borrar una clave en Native Storage.
	// devolver 0 si no hay errores.
	del(key: string): number;

	// obtener un valor mediante una clave desde Native Storage,
	// de-serializar el valor llamando al método `descriptor.parse`.
	get(key: string): any;

	// establecer clave y par de valores en Native Storage,
	// el valor debe serializarse a una cadena mediante una llamada a `descriptor.stringify`.
	// devolver 0 si no hay errores.
	set(key: string, value: any): number;
}

interface StorageMap {
	// borrar clave desde Native Storage, devolver 0 si no hay errores.
	del(key: string): number;

	// obtener valor mediante clave desde Native Storage,
	// de-serializar el valor mediante una llamada a `descriptor.parse`.
	get(key: string): any;

	// establecer clave y par de valores en Native Storage,
	// el valor debe serializarse a una cadena mediante una llamada a `descriptor.stringify`.
	// devolver 0 si no hay errores.
	set(key: string, value: any): number;
}

```

### BigNumber

El módulo `BigNumber` hace uso de [bignumber.js](https://github.com/MikeMcl/bignumber.js), una librería JavaScript para realizar operaciones aritméticas sobre números decimales y no decimales de precisión arbitraria.

El contrato inteligente puede hacer uso de este módulo `BigNumber` para calcular directamente el valor de una transacción, o para cualquier otra operación que requiera el manejo de grandes números de precisión arbitraria.

```javascript

var value = new BigNumber(0);
value.plus(1);
// ...

```

### Blockchain

El módulo `Blockchain` expone un objeto que permite obtener las transacciones y los bloques ejecutados por el contrato inteligente que realiza la llamada. Also, the NAS can be transferred from the contract and the address check is provided.

Blockchain API:

```javascript

// current block
Blockchain.block;

// transacción actual; el valor de la transacción/gasPrice/gasLimit se convierte automáticamente en un objeto BigNumber.
Blockchain.transaction;

// transferir NAS desde un contrato inteligente a una dirección dada
Blockchain.transfer(address, value);

// verificar la dirección
Blockchain.verifyAddress(address);

```

#### Propiedades

* `block`: bloque actual para la ejecución del contrato
  * `timestamp`: timestamp del bloque
  * `seed`: semilla aleatoria
  * `height`: altura del bloque
* `transaction`: transacción actual para la ejecución del contrato
  * `hash`: hash de la transacción
  * `from`: dirección del emisor de la transacción
  * `to`: dirección del destinatario de la transacción
  * `value`: valor de la transacción (objeto BigNumber)
  * `nonce`: nonce de la transacción
  * `timestamp`: timestamp de la transacción
  * `gasPrice`: gasPrice de la transacción (objeto BigNumber)
  * `gasLimit`: gasLimit de la transacción, (objeto BigNumber)
* `transfer(address, value)`: transferir NAS desde un contrato inteligente a una dirección
  * parámetros:
    * `address`: dirección Nebulas en donde se recibirán los NAS
    * `value`: valor a transferir (objeto BigNumber)
  * valor devuelto:
    * `0`: transferencia exitosa
    * `1`: transferencia fallida
* `verifyAddress(address)`: verificar dirección
  * parámetros:
    * `address`: dirección que se desea chequear
  * return:
    * `1`: dirección válida
    * `0`: dirección inválida

Ejemplo de uso:

```javascript

"use strict";

var SampleContract = function () {
    LocalContractStorage.defineProperties(this, {
        name: null,
        count: null
    });
    LocalContractStorage.defineMapProperty(this, "allocation");
};

SampleContract.prototype = {
    init: function (name, count, allocation) {
        this.name = name;
        this.count = count;
        allocation.forEach(function (item) {
            this.allocation.put(item.name, item.count);
        }, this);
        console.log('init: Blockchain.block.coinbase = ' + Blockchain.block.coinbase);
        console.log('init: Blockchain.block.hash = ' + Blockchain.block.hash);
        console.log('init: Blockchain.block.height = ' + Blockchain.block.height);
        console.log('init: Blockchain.transaction.from = ' + Blockchain.transaction.from);
        console.log('init: Blockchain.transaction.to = ' + Blockchain.transaction.to);
        console.log('init: Blockchain.transaction.value = ' + Blockchain.transaction.value);
        console.log('init: Blockchain.transaction.nonce = ' + Blockchain.transaction.nonce);
        console.log('init: Blockchain.transaction.hash = ' + Blockchain.transaction.hash);
    },
    transfer: function (address, value) {
        var result = Blockchain.transfer(address, value);
        console.log("transfer result:", result);
        Event.Trigger("transfer", {
            Transfer: {
                from: Blockchain.transaction.to,
                to: address,
                value: value
            }
        });
    },
    verifyAddress: function (address) {
         var result = Blockchain.verifyAddress(address);
        console.log("verifyAddress result:", result);
    }
};

module.exports = SampleContract;
```

### Evento

El módulo `Event` almacena los eventos de la ejecución de un contrato inteligente dado. Los eventos registrados se almacenan en el _trie_ de eventos en la cadena, desde donde se pueden recuperar por medio del método `FetchEvents` utilizando para ello el hash de la transacción.

Todos los tópicos de evento de contrato llevan el prefijo `chain.contract.` antes del tópico asociado al contrato.

```javascript

Event.Trigger(topic, obj);

```

* `topic`: tópico definido por el usuario
* `obj`: objeto JSON

Véase el ejemplo en `SampleContract` (más arriba).

### Math.random

* `Math.random()` devuelve un número pseudoaleatorio de coma flotante en el intervalo (0, 1]. El uso típico es:

```javascript

"use strict";

var BankVaultContract = function () {};

BankVaultContract.prototype = {

    init: function () {},

    game: function(subscript){

        var arr =[1,2,3,4,5,6,7,8,9,10,11,12,13];

        for(var i = 0;i < arr.length; i++){
            var rand = parseInt(Math.random()*arr.length);
            var t = arr[rand];
            arr[rand] =arr[i];
            arr[i] = t;
        }

        return arr[parseInt(subscript)];
    },
};
module.exports = BankVaultContract;

```

* `Math.random.seed(myseed)`: si es necesario, se puede utilizar este método para reinicializar la semilla de números aleatorios. **El argumento `myseed` debe ser de tipo string**.

```js

"use strict";
var BankVaultContract = function \(\) {};

BankVaultContract.prototype = {
	init: function () {},

	game:function(subscript, myseed){

		 var arr =[1,2,3,4,5,6,7,8,9,10,11,12,13];

		 console.log(Math.random());

		 for(var i = 0;i < arr.length; i++){

			  if (i == 8) {
					// reinicializar la semilla de aleatorios con `myseed`
					Math.random.seed(myseed);
			  }

			  var rand = parseInt(Math.random()*arr.length);
			  var t = arr[rand];
			  arr[rand] =arr[i];
			  arr[i] = t;
		 }
		 return arr[parseInt(subscript)];
	},
};

module.exports = BankVaultContract;

```

### Date

```js

"use strict";

var BankVaultContract = function () {};

BankVaultContract.prototype = {
    init: function () {},

    test: function(){
        var d = new Date();
        return d.toString();
    }
};

module.exports = BankVaultContract;

```

#### Notas

* Métodos no soportados: `toDateString()`, `toTimeString()`, `getTimezoneOffset()`, `toLocaleXXX()`.
* `new Date()` y `Date.now()` devuelven el timestamp del bloque actual en milisegundos.
* `getXXX` devuelve el resultado de `getUTCXXX`.

### accept

Este método permite realizar una transferencia binaria hacia un contrato inteligente.

Siempre y cuando `to` sea una dirección de contrato inteligente que declara el método `accept()` y se ejecuta correctamente, la transferencia se realizará sin errores.

```javascript

"use strict";
var DepositeContent = function (text) {
    if(text){
            var o = JSON.parse(text);
            this.balance = new BigNumber(o.balance);// información de balance
            this.address = o.address;
    }else{
            this.balance = new BigNumber(0);
            this.address = "";
        }
};

DepositeContent.prototype = {
    toString: function () {
          return JSON.stringify(this);
    }
};

var BankVaultContract = function () {
    LocalContractStorage.defineMapProperty(this, "bankVault", {
         parse: function (text) {
              return new DepositeContent(text);
         },
         stringify: function (o) {
              return o.toString();
         }
    });
};

BankVaultContract.prototype = {
    init: function () {},

    save: function () {
          var from = Blockchain.transaction.from;
          var value = Blockchain.transaction.value;
          value = new BigNumber(value);
          var orig_deposit = this.bankVault.get(from);
          if (orig_deposit) {
                value = value.plus(orig_deposit.balance);
          }

          var deposit = new DepositeContent();
          deposit.balance = new BigNumber(value);
          deposit.address = from;
          this.bankVault.put(from, deposit);
    },

    accept:function(){
        this.save();
        Event.Trigger("transfer", {
            Transfer: {
                from: Blockchain.transaction.from,
                to: Blockchain.transaction.to,
                value: Blockchain.transaction.value,
            }
           });
    }

};
module.exports = BankVaultContract;

```
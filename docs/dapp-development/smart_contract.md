# Smart Contract

## Linguagens 

Na Nebulas suportamos duas linguagens de smart contract:

* [JavaScript](https://en.wikipedia.org/wiki/JavaScript)
* [TypeScript](https://en.wikipedia.org/wiki/TypeScript)

São suportadas pela integração do [Chrome V8](https://developers.google.com/v8/), um motor de JavaScript desenvolvido pelo The Chromium Project para o Google Chrome e navegadores baseados no Chromium.

## Modelo de Execução

O diagrama abaixo é o Modelo de Execução do Smart Contract:

![Modelo de Execução do Smart Contract](../resources/smart_contract_execution_model.png)

1. O código fonte do Smart Contract e argumentos serão guardados na transacção e implementados na Nebulas.
2. The execution of Smart Contract are divided into two phases:
   1. Preprocesso: injecção de instrução tracing, etcetera.
   2. Executa: gera src executável e executa-o.

## Contractos

Contractos são semelhantes a classes, em linguagens orientadas a objectos. Contêm dados persistentes em variáveis de estado e funções que podem modificar essas variáveis.

### Escrever um Contracto

Um contracto tem de ser um Objecto Protótipo ou Classe em JavaScript ou TypeScript.

Um contracto tem de incluir uma função `init`, que apenas é executada uma vez após a execução. Functions, named starting with `_` are `private`, can't be executed in Transaction. The others are all `public` and can be executed in Transaction.

Visto que o Contracto é executado no Chrome V8, todas as instâncias das variáveis estão na memória. Não é aconselhável guardá-las todas no [state trie](https://github.com/nebulasio/wiki/blob/master/merkle_trie.md) da Nebulas. Na Nebulas, fornecemos os objectos  `LocalContractStorage` e `GlobalContractStorage` de modo a permitir que desenvolvedores definam os campos que necessitam de ser guardados no state trie. Estes campos devem ser definidos no `constructor` do Contracto, antes de qualquer outra função.

O seguinte é um exemplo de um contracto:

```javascript
class Rectangle {
    constructor() {
        // define campos guardados no state trie.
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

    // calcula área
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

### Visibilidade

Em JavaScript, não há visibilidade de funções. Todas as funções definidas no objecto protótipo são públicas.

Na Nebulas, definimos dois tipos de visibilidade, `public` e `private`:

* `public` Todas as funções cujo regexp seja `^[a-zA-Z$][A-Za-z0-9_$]*$` são públicas, excepto o `init`. Funções públicas podem ser chamadas através de Transaction.
* `private` Todas as funções cujo nome comece por `_` são privadas. Uma função privada apenas pode ser invocada por um função pública.

## Objectos Globais

### linha de comandos

O módulo `console` fornece uma consola de depuração que é semelhante à do JavaScript fornecida por navegadores web.

A consola global pode ser usada sem usar `require('console')`.

#### console.info\(\[...args\]\)

* `...args <any>`

  A função console.info\(\) é um alias para `console.log()`.

#### console.log\(\[...args\]\)

* `...args <any>`

  Print `args` para o Logger da Nebulas, com o nível `info`.

#### console.debug\(\[...args\]\)

* `...args <any>`

  Print `args` para o Logger da Nebulas, com o nível `debug`.

#### console.warn\(\[...args\]\)

* `...args <any>`

  Print `args` para o Logger da Nebulas, com o nível `warn`.

#### console.error\(\[...args\]\)

* `...args <any>`

  Print `args` para o Logger da Nebulas, com o nível `error`.

### LocalContractStorage

O módulo `LocalContractStorage` fornece capacidade de armazenamento baseada no state trie. Aceita key value pairs de cadeias de caracteres apenas. Todos os dados são guardados num state trie privado, associado com o endereço do contracto, e apenas este os pode aceder.

```typescript
interface Descriptor {
    // serialize value to string;
    stringify?(value: any): string;

    // deserialize value from string;
    parse?(value: string): any;
}

interface DescriptorMap {
    [fieldName: string]: Descriptor;
}

interface ContractStorage {
    // get and return value by key from Native Storage.
    rawGet(key: string): string;
    // set key and value pair to Native Storage,
    // return 0 for success, otherwise failure.
    rawSet(key: string, value: string): number;

    // define a object property named `fieldname` to `obj` with descriptor.
    // default descriptor is JSON.parse/JSON.stringify descriptor.
    // return this.
    defineProperty(obj: any, fieldName: string, descriptor?: Descriptor): any;

    // define object properties to `obj` from `props`.
    // default descriptor is JSON.parse/JSON.stringify descriptor.
    // return this.
    defineProperties(obj: any, props: DescriptorMap): any;

    // define a StorageMap property named `fieldname` to `obj` with descriptor.
    // default descriptor is JSON.parse/JSON.stringify descriptor.
    // return this.
    defineMapProperty(obj: any, fieldName: string, descriptor?: Descriptor): any;

    // define StorageMap properties to `obj` from `props`.
    // default descriptor is JSON.parse/JSON.stringify descriptor.
    // return this.
    defineMapProperties(obj: any, props: DescriptorMap): any;

    // delete key from Native Storage.
    // return 0 for success, otherwise failure.
    del(key: string): number;

    // get value by key from Native Storage,
    // deserialize value by calling `descriptor.parse` and return.
    get(key: string): any;

    // set key and value pair to Native Storage,
    // the value will be serialized to string by calling `descriptor.stringify`.
    // return 0 for success, otherwise failure.
    set(key: string, value: any): number;
}

interface StorageMap {
    // delete key from Native Storage, return 0 for success, otherwise failure.
    del(key: string): number;

    // get value by key from Native Storage,
    // deserialize value by calling `descriptor.parse` and return.
    get(key: string): any;

    // set key and value pair to Native Storage,
    // the value will be serialized to string by calling `descriptor.stringify`.
    // return 0 for success, otherwise failure.
    set(key: string, value: any): number;
}
```

### BigNumber

O modulo `BigNumber` usa [bignumber.js](https://github.com/MikeMcl/bignumber.js), uma biblioteca JavaScript para precisão décimal arbitrária e aritmética não décimal. O contracto pode usar o `BigNumber` directamente para lidar com o valor da transacção e transferências de outros valores.

```javascript
var value = new BigNumber(0);
value.plus(1);
...
```

### Blockchain

The `Blockchain` module provides an object for contracts to obtain transactions and blocks executed by the current contract. Also, the NAS can be transferred from the contract and the address check is provided.

Blockchain API:

```javascript
// current block 
Blockchain.block;

// current transaction, transaction's value/gasPrice/gasLimit auto change to BigNumber object
Blockchain.transaction;

// transfer NAS from contract to address
Blockchain.transfer(address, value);

// verify address
Blockchain.verifyAddress(address);
```

properties:

* `block`: current block for contract execution
  * `timestamp`: block timestamp
  * `seed`: random seed
  * `height`: block height
* `transaction`: current transaction for contract execution
  * `hash`: transaction hash
  * `from`: transaction from address
  * `to`: transaction to address
  * `value`: transaction value, a BigNumber object for contract use
  * `nonce`: transaction nonce
  * `timestamp`: transaction timestamp
  * `gasPrice`: transaction gasPrice, a BigNumber object for contract use
  * `gasLimit`: transaction gasLimit, a BigNumber object for contract use
* `transfer(address, value)`: transfer NAS from contract to address
  * params:
    * `address`: nebulas address to receive NAS
    * `value`: transfer value, a BigNumber object
  * return:
    * `0`: transfer success
    * `1`: transfer failed   
* `verifyAddress(address)`: verify address
  * params:
    * `address`: address need to check
  * return:
    * `1`: address is valid
    * `0`: address is invalid 

Example to use:

```javascript
'use strict';

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

### Event

The `Event` module records execution events in the contract. The recorded events are stored in the event trie on the chain, which can be fetched by `FetchEvents` method in block with the execution transaction hash. All contract event topics have a `chain.contract.` prefix before the topic they set in contract.

```javascript
Event.Trigger(topic, obj);
```

* `topic`: user-defined topic
* `obj`: JSON object

You can see the example in `SampleContract` above.

### Math.random

* `Math.random()` returns a floating-point, pseudo-random number in the range from 0 inclusive, up to, but not including 1. The typical usage is:

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

* `Math.random.seed(myseed)` if needed, you can use this method to reset the random seed. The argument `myseed` must be a **string**.

  \`\`\`js

  "use strict";

var BankVaultContract = function \(\) {};

BankVaultContract.prototype = {

```text
init: function () {},

game:function(subscript, myseed){

    var arr =[1,2,3,4,5,6,7,8,9,10,11,12,13];

    console.log(Math.random());

    for(var i = 0;i < arr.length; i++){

        if (i == 8) {
            // reset random seed with `myseed`
            Math.random.seed(myseed);
        }

        var rand = parseInt(Math.random()*arr.length);
        var t = arr[rand];
        arr[rand] =arr[i];
        arr[i] = t;
    }
    return arr[parseInt(subscript)];
},
```

};

module.exports = BankVaultContract;

```text
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

Tips:

* Unsupported methods：`toDateString()`, `toTimeString()`, `getTimezoneOffset()`, `toLocaleXXX()`.
* `new Date()`/`Date.now()` returns the timestamp of current block in milliseconds.
* `getXXX` returns the result of `getUTCXXX`.

### accept

this method aims to make it possible to send a binary transfer to a contract account. As `to` is a smart contact address, which has declared the function `accept()` and it excuted correctly, the transfer will succeed. If the Tx is a non-binary Tx, it will be treated as a normal function.

```javascript
"use strict";
var DepositeContent = function (text) {
    if(text){
            var o = JSON.parse(text);
            this.balance = new BigNumber(o.balance);//余额信息
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


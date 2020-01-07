# NRC20

## Resumen

El siguiente estándar permite la implementación de una API estandarizada para la utilización de tokens dentro de contratos inteligentes, como así también su transferencia y su uso por terceros dentro del blockchain.

## Motivación

Una interfaz estándar permite crear tokens desde una aplicación de una forma rápida, para ser usada en distintos contextos: desde una cartera hasta una casa de cambios descentralizada.

## Métodos

#### name

Devuelve el nombre del token; por ejemplo: `"MyToken"`.

```javascript
// Devuelve una cadena con el nombre del token.
function name()
```

#### symbol

Devuelve el símbolo del token; por ejemplo: "TK".

```javascript
// Devuelve una cadena con el símbolo del token.
function symbol()
```

#### decimals

Devuelve el número de decimales que utiliza el token; por ejemplo: `8`.

Este número indica, además, la unidad indivisible y mínima, y el coeficiente a ser utilizado por otras funciones. Por ejemplo, si esta función devuelve `8`, será necesario multiplicar por `0.00000001` cualquier otro valor para obtener su representación coloquial, ya que las funciones devuelven normalmente los valores en unidades mínimas.

```javascript
// Devuelve un número con la cantidad de decimales que utiliza el token.
function decimals()
```

#### totalSupply

Devuelve el suministro total de tokens.

```javascript
// Devuelve una cadena que representa el suministro total de tokens, expresado mediante su unidad más pequeña (lo cual se puede consultar mediante el método decimals.
function totalSupply()
```

#### balanceOf

Devuelve el balance de una dirección, expresado en unidades mínimas (véase [decimals](#decimals)).

```javascript
// Devuelve una cadena que representa el balance de la dirección dada.
function balanceOf(address)
```

#### transfer

Transfiere la cantidad de tokens indicada por `value` (expresado en unidades mínimas; véase [decimals](#decimals) para más información) a la dirección `address`. Devuelve un boolean `true` si no hay errores; de lo contrario se lanza un `error`.

```javascript
// Devuelve `true` si la transferencia se realiza exitosamente; en otro caso, se lanza un error.
function transfer(address, value)
```

##### Nota

Las transferencias cuyo valor es 0 son transferencias válidas, que también disparan el evento `Transfer`.

#### transferFrom

Transfiere la cantidad de tokens indicada por `value` (expresado en unidades mínimas; véase [decimals](#decimals) para más información) desde la dirección `from` a la dirección `to`. Devuelve un boolean `true` si no hay errores; de lo contrario se lanza un `error`.

Este método tiene sentido en el contexto de un contrato inteligente, en el que es necesario delegar el envío de tokens.

```javascript
// Devuelve `true` si la transferencia se realiza exitosamente; de lo contrario devuelve error.
function transferFrom(from, to, value)
```

##### Nota

Las transferencias cuyo valor es 0 son transferencias válidas, que también disparan el evento `Transfer`.

#### approve

Otorga a `spender` el derecho de realizar múltiples extracciones sobre la cuenta actual, hasta alcanzar el valor especificado mediante el parámetro `value`.

El parámetro `currentValue` especifica el valor actual para `value`, y se establece en 0 antes de la primera llamada; el parámetro existe para impedir ataques.

Con cada nueva llamada, este método sobreescribe el valor permitido con el nuevo valor pasado mediante `value`.

```javascript
// Devuelve `true` si la transacción se ejecuta correctamente; si no, se devuelve un error.
function approve(spender, currentValue, value)
```

##### Nota

Para prevenir ataques, el valor por defecto para `value` y `currentValue` es 0.

#### allowance

Devuelve el valor remanente que `spender` puede todavía extraer de la cuenta `owner`.

```javascript
// Devuelve una cadena que representa el valor remanente que spender puede todavía extraer de la cuenta owner.
function allowance(owner, spender)
```

### Eventos

#### transferEvent

Debe dispararse cuando se transfieren tokens, incluyendo operaciones por un valor nulo (0).
-
Todo contrato inteligente, luego de generar nuevos tokens, **debe** lanzar un evento Transfer con el parámetro `from` indicando el valor `totalSupply`.

```javascript
function transferEvent: function(status, from, to, value)
```

#### approveEvent

Este evento se debe disparar luego de una llamada al método `approve(spender, currentValue, value)`.

```javascript
function approveEvent: function(status, from, spender, value)
```

## Implementación

Los ejemplos de implementaciones están disponibles en el archivo [NRC20.js](https://github.com/nebulasio/go-nebulas/blob/master/nf/nvm/test/NRC20.js).

```javascript
'use strict';

var Allowed = function (obj) {
    this.allowed = {};
    this.parse(obj);
}

Allowed.prototype = {
    toString: function () {
        return JSON.stringify(this.allowed);
    },

    parse: function (obj) {
        if (typeof obj != "undefined") {
            var data = JSON.parse(obj);
            for (var key in data) {
                this.allowed[key] = new BigNumber(data[key]);
            }
        }
    },

    get: function (key) {
        return this.allowed[key];
    },

    set: function (key, value) {
        this.allowed[key] = new BigNumber(value);
    }
}

var StandardToken = function () {
    LocalContractStorage.defineProperties(this, {
        _name: null,
        _symbol: null,
        _decimals: null,
        _totalSupply: {
            parse: function (value) {
                return new BigNumber(value);
            },
            stringify: function (o) {
                return o.toString(10);
            }
        }
    });

    LocalContractStorage.defineMapProperties(this, {
        "balances": {
            parse: function (value) {
                return new BigNumber(value);
            },
            stringify: function (o) {
                return o.toString(10);
            }
        },
        "allowed": {
            parse: function (value) {
                return new Allowed(value);
            },
            stringify: function (o) {
                return o.toString();
            }
        }
    });
};

StandardToken.prototype = {
    init: function (name, symbol, decimals, totalSupply) {
        this._name = name;
        this._symbol = symbol;
        this._decimals = decimals || 0;
        this._totalSupply = new BigNumber(totalSupply).mul(new BigNumber(10).pow(decimals));

        var from = Blockchain.transaction.from;
        this.balances.set(from, this._totalSupply);
        this.transferEvent(true, from, from, this._totalSupply);
    },

    // Returns the name of the token
    name: function () {
        return this._name;
    },

    // Returns the symbol of the token
    symbol: function () {
        return this._symbol;
    },

    // Returns the number of decimals the token uses
    decimals: function () {
        return this._decimals;
    },

    totalSupply: function () {
        return this._totalSupply.toString(10);
    },

    balanceOf: function (owner) {
        var balance = this.balances.get(owner);

        if (balance instanceof BigNumber) {
            return balance.toString(10);
        } else {
            return "0";
        }
    },

    transfer: function (to, value) {
        value = new BigNumber(value);
        if (value.lt(0)) {
            throw new Error("invalid value.");
        }

        var from = Blockchain.transaction.from;
        var balance = this.balances.get(from) || new BigNumber(0);

        if (balance.lt(value)) {
            throw new Error("transfer failed.");
        }

        this.balances.set(from, balance.sub(value));
        var toBalance = this.balances.get(to) || new BigNumber(0);
        this.balances.set(to, toBalance.add(value));

        this.transferEvent(true, from, to, value);
    },

    transferFrom: function (from, to, value) {
        var spender = Blockchain.transaction.from;
        var balance = this.balances.get(from) || new BigNumber(0);

        var allowed = this.allowed.get(from) || new Allowed();
        var allowedValue = allowed.get(spender) || new BigNumber(0);
        value = new BigNumber(value);

        if (value.gte(0) && balance.gte(value) && allowedValue.gte(value)) {

            this.balances.set(from, balance.sub(value));

            // update allowed value
            allowed.set(spender, allowedValue.sub(value));
            this.allowed.set(from, allowed);

            var toBalance = this.balances.get(to) || new BigNumber(0);
            this.balances.set(to, toBalance.add(value));

            this.transferEvent(true, from, to, value);
        } else {
            throw new Error("transfer failed.");
        }
    },

    transferEvent: function (status, from, to, value) {
        Event.Trigger(this.name(), {
            Status: status,
            Transfer: {
                from: from,
                to: to,
                value: value
            }
        });
    },

    approve: function (spender, currentValue, value) {
        var from = Blockchain.transaction.from;

        var oldValue = this.allowance(from, spender);
        if (oldValue != currentValue.toString()) {
            throw new Error("current approve value mistake.");
        }

        var balance = new BigNumber(this.balanceOf(from));
        var value = new BigNumber(value);

        if (value.lt(0) || balance.lt(value)) {
            throw new Error("invalid value.");
        }

        var owned = this.allowed.get(from) || new Allowed();
        owned.set(spender, value);

        this.allowed.set(from, owned);

        this.approveEvent(true, from, spender, value);
    },

    approveEvent: function (status, from, spender, value) {
        Event.Trigger(this.name(), {
            Status: status,
            Approve: {
                owner: from,
                spender: spender,
                value: value
            }
        });
    },

    allowance: function (owner, spender) {
        var owned = this.allowed.get(owner);

        if (owned instanceof Allowed) {
            var spender = owned.get(spender);
            if (typeof spender != "undefined") {
                return spender.toString(10);
            }
        }
        return "0";
    }
};

module.exports = StandardToken;
```
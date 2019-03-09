# NRC20

## Resumo 

O seguinte padrão permite a implementação de uma API padrão para tokens em contratos inteligentes. Este padrão fornece funcionalidade básica para transferir tokens, bem como permite que os tokens sejam aprovados para que possam ser gastos por outro terceiro na cadeia.

## Motivação 

Uma interface padrão permite que um novo token seja facilmente criado por qualquer aplicativo: desde carteiras até trocas descentralizadas.

## Métodos 

#### name

Nome. Retorna o nome do token - por exemplo `"MyToken"`.

```javascript
// returns string, the name of the token.
function name()
```

#### symbol

Símbolo. Retorna o símbolo do token. Por exemplo, `"TK"`.

```javascript
// returns string, the symbol of the token 
function symbol()
```

#### decimals

Decimais. Retorna o número de decimais que o token usa - por exemplo `8`, significa dividir o valor do token `100000000` para obter sua representação do utilizador.

```javascript
// returns number, the number of decimals the token uses
function decimals()
```

#### totalSupply

Retorna o fornecimento total de token.

```javascript
// returns string, the total token supply, the decimal value is decimals* total.
function totalSupply()
```

#### balanceOf

Retorna o saldo da conta de um endereço.

```javascript
// returns string, the account balance of another account with address
function balanceOf(address)
```

#### transfer

Transferência. Transfere a `value` quantidade de tokens para `address`e deve disparar o `Transfer`
evento. A função deve `throw`se o `from`saldo da conta não tiver tokens suficientes para gastar.

> _Nota:_ Transferências de 0 valores DEVEM ser tratadas como transferências normais e disparar o `Transfer` evento.

Transfers `value` amount of tokens from address `from` to address `to`, and MUST fire the `Transfer` event.

```javascript
// returns `true`, if transfer success, else throw error
function transfer(address, value)
```

#### transferFrom

Transfere a `value` quantidade de tokens do endereço `from` para o endereço `to`e DEVE disparar o `Transfer`evento.

O `transferFrom` método é usado para um fluxo de trabalho de retirada, permitindo que contratos transfiram tokens em seu nome. Isso pode ser usado, por exemplo, para permitir que um contrato transfira tokens em seu nome e / ou cobrar taxas em sub-moedas. A função deve `throw`, a menos que a `from` conta deliberadamente autorizou o remetente da mensagem através de algum mecanismo.

> _Nota:_ Transferências de 0 valores DEVEM ser tratadas como transferências normais e disparar o `Transfer` evento.


```javascript
// returns `true`, if transfer success, else throw error
function transferFrom(from, to, value)
```

#### approve

Aprovar. Permite `spender` retirar sua conta várias vezes, até `currentValue`o `value` montante. Se esta função for chamada novamente, ela sobrescreve a permissão atual com `value`.

> _Nota:_ Para evitar vetores de ataque, o utilizador precisa de fornecer um valor de aprovação anterior e o valor padrão de não aprovado é 0.

```javascript
// returns `true`, if approve success, else throw error
function approve(spender, currentValue, value)
```

#### allowance

Subsídio. Retorna o valor que `spender` ainda pode ser retirado `owner`.

```javascript
// returns string, the value allowed to withdraw from `owner`.
function allowance(owner, spender)
```

## Events

#### transferEvent

DEVE ser acionado quando tokens são transferidos, incluindo transferências de valor zero.

Um contrato de token que cria novos tokens DEVE acionar um evento de transferência com o `from` endereço definido `totalSupply` quando os tokens são criados.

```javascript
function transferEvent: function(status, from, to, value)
```

#### approveEvent

DEVE acionar em qualquer chamada para. `approve(spender, currentValue, value)`.

```javascript
function approveEvent: function(status, from, spender, value)
```

## Implementação 

Exemplos de implementações estão disponíveis em

* [NRC20.js](https://github.com/nebulasio/go-nebulas/blob/master/nf/nvm/test/NRC20.js)

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


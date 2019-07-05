# Overview

Chamadas de Procedimento Remoto (RPCs) criam um abstração útil para a construção de aplicação distribuídas e serviços.

Nebulas fornece um APi [gRPC](https://grpc.io) e RESTful para que os utilizadores interagirem com as Nebulas.

[gRPC](https://github.com/grpc/grpc-go) proporciona uma implementação concreta do protocolo gRPC, por cima de HTTP/2. Estas bibliotecas permitem comunicação entre clientes e servidores usando uma combinação das linguagens de programação suportadas.

[gRPC-gateway](https://github.com/grpc-ecosystem/grpc-gateway) é um plugin do protoc. Lê a definição do serviço gRPC, e gera um servidor reverse-proxy que traduz o API RESTful JSON para gRPC. É então usado para mapear gRPC para HTTP.

## Endpoint

Endpoints padrão:

| API | URL | Protocol |
|-------|:------------:|:------------:|
| gRPC |  http://localhost:8684 | Protobuf|
| RESTful |http://localhost:8685 | HTTP |

##### API gRPC

Podemos executar o exemplo gRPC: [testing client code](https://github.com/nebulasio/go-nebulas/blob/develop/rpc/testing/client/main.go):

```bash
go run main.go
```

O cliente obtem o estado da conta do endereço do remetente, faz transacção entre remetente e destinatário, e verifica o estado da conta do endereço do remetente.

Podemos ver o output do cliente da seguinte maneira:

```bash
GetAccountState n1QZMXSZtW7BUerroSms4axNfyBGyFGkrh5 nonce 4 value 3142831039999999999992
SendTransaction n1QZMXSZtW7BUerroSms4axNfyBGyFGkrh5 -> n1Zn6iyyQRhqthmCfqGBzWfip1Wx8wEvtrJ value 2 txhash:"2c2f5404a2e2edb651dff44a2d114a198c00614b20801e58d5b00899c8f512ae"
GetAccountState n1Zn6iyyQRhqthmCfqGBzWfip1Wx8wEvtrJ nonce 0 value 10
```

##### HTTP

Agora também fornecemos HTTP para aceder ao API RPC. O ficheiro que termina com **gw.go** é o ficheiro de mapeamento.
Podemos aceder ao API RPC directamente do nosso browser. Pode actualizar o **rpc_listen** e **http_listen** em **conf/default/config.conf** para mudar a porta do RPC/HTTP, respectivamente.

###### Exemplo:
```bash
curl -i -H 'Content-Type: application/json' -X GET http://localhost:8685/v1/user/nebstate
```
if success, response will be returned like this
```json
{
    "result":{
        "chain_id":100,
        "tail":"b10c1203d5ae6d4d069d5f520eb060f2f5fb74e942f391e7cadbc2b5148dfbcb",
        "lib":"da30b4ed14affb62b3719fb5e6952d3733e84e53fe6e955f8e46da503300c985",
        "height":"365",
        "protocol_version":"/neb/1.0.0",
        "synchronized":false,
        "version":"0.7.0"
    }
}
```

Or, there is error form grpc, repose will carry the error message

```json
{
    "error":"message..."
}
```

## RPC methods

* [GetNebState](#getnebstate)
* [GetAccountState](#getaccountstate)
* [LatestIrreversibleBlock](#latestirreversibleblock)
* [Call](#call)
* [SendRawTransaction](#sendrawtransaction)
* [GetBlockByHash](#getblockbyhash)
* [GetBlockByHeight](#getblockbyheight)
* [GetTransactionReceipt](#gettransactionreceipt)
* [GetTransactionByContract](#gettransactionbycontract)
* [GetGasPrice](#getgasprice)
* [EstimateGas](#estimategas)
* [GetEventsByHash](#geteventsbyhash)
* [Subscribe](#subscribe)
* [GetDynasty](#getdynasty)

## Referência do API RPC

#### GetNebState

Return o estado da neb.

| Protocol | Method | API |
|----------|--------|-----|
| gRpc |  |  GetNebState |
| HTTP | GET |  /v1/user/nebstate |

###### Parametros

none

###### Returns

`chain_id` Block chain id，
* `1` : mainnet

- `1001` : testnet

`tail` neb tail hash actual.

`lib` neb lib hash actual.

`height` neb tail block height actual.

`protocol_version` a versão actual do protocolo neb.

`synchronized` o estado do peer sync.

`version` versão neb.

###### Exemplo HTTP

```bash
// Request
curl -i -H 'Content-Type: application/json' -X GET http://localhost:8685/v1/user/nebstate

// Result
{
    "result":{
        "chain_id":100,
        "tail":"b10c1203d5ae6d4d069d5f520eb060f2f5fb74e942f391e7cadbc2b5148dfbcb",
        "lib":"da30b4ed14affb62b3719fb5e6952d3733e84e53fe6e955f8e46da503300c985",
        "height":"365",
        "protocol_version":"/neb/1.0.0",
        "synchronized":false,
        "version":"0.7.0"
    }
}
```
***

#### GetAccountState

Return o estado de uma conta. Balancete e nonce de um determinado endereço.

| Protocol | Method | API |
|----------|--------|-----|
| gRpc |  |  GetAccountState |
| HTTP | POST |  /v1/user/accountstate |

###### Parametros

`address` Hex string do endereço da conta.

`height` estado do block account com height. Se não for especificado, use 0 como tail height.

###### Returns

`balance` balanço actual em unidades 1/(10^18) de nas.

`nonce` número de transacções actuais.

`type` o tipo de endereço. 87 significa endereços normais, e 88 endereços de contractos.

`height` height actual da blockchain.

`pending` transacções pendentes na pool de transacções.

###### Exemplo HTTP

```bash
// Request
curl -i -H 'Content-Type: application/json' -X POST http://localhost:8685/v1/user/accountstate -d '{"address":"n1Z6SbjLuAEXfhX1UJvXT6BB5osWYxVg3F3"}'

// Result
{
    result {
        "balance":"9489999998980000000000"
        "nonce":51
        "type":87
	    "height":"100",
        "pending":"0"
    }
}
```

***

#### LatestIrreversibleBlock

Return o último bloco irreversível.

| Protocol | Method | API |
|----------|--------|-----|
| gRpc |  |  LatestIrreversibleBlock |
| HTTP | GET |  /v1/user/lib |

##### Parametros

none

###### Returns

`hash` Hex string do block hash.

`parent_hash` Hex string do block parent hash.

`height` block height.

`nonce` block nonce.

`coinbase` Hex string do endereço da coinbase.

`timestamp` block timestamp.

`chain_id` block chain id.

`state_root` Hex string do state root.

`txs_root` Hex string do txs root.

`events_root` Hex string do event root.

`consensus_root` 

-  `Timestamp` tempo do estado de consenso.

-  `Proposer`  proponente do estado de consenso actual.

-  `DynastyRoot` Hex string da dynasty root.

    `miner` o minerador deste bloco.

    `is_finality` block é finality

    `transactions` block transactions slice.

- `transaction ` [GetTransactionReceipt](#gettransactionreceipt) response info.

##### Exemplo HTTP

```bash
// Request
curl -i -H 'Content-Type: application/json' -X GET http://localhost:8685/v1/user/lib

// Result
{
    "result":{
        "hash":"c4a51d6241db372c1b8720e62c04426bd587e1f31054b7d04a3509f48ee58e9f",
        "parent_hash":"8f9f29028356d2fb2cf1291dcee85785e1c20a2145318f36c136978edb6097ce",
        "height":"407",
        "nonce":"0",
        "coinbase":"n1QZMXSZtW7BUerroSms4axNfyBGyFGkrh5",
        "timestamp":"1521963660",
        "chain_id":100,
        "state_root":"a77bbcd911e7ee9488b623ce4ccb8a38d9a83fc29eb5ad43009f3517f1d3e19a",
        "txs_root":"664671e2fda200bd93b00aaec4ab12db718212acd51b4624e8d4937003a2ab22",
        "events_root":"2607e32c166a3513f9effbd1dc7caa7869df5989398d0124987fa0e4d183bcaf",
        "consensus_root":{
            "timestamp":"1521963660",
            "proposer":"GVeOQnYf20Ppxa2cqTrPHdpr6QH4SKs4ZKs=",
            "dynasty_root":"IfTgx0o271Gg4N3cVKHe7dw3NREnlYCN8aIl8VvRXDY="
        },
        "miner": "n1WwqBXVMuYC3mFCEEuFFtAXad6yxqj4as4"
        "is_finality":false,
        "transactions":[] 
    }
}
```
***

#### Call

Chama uma função de um smart contract. O smart contract tem de ter sido submetido. Chamadas de métodos apenas podem ser executadas no nó actual.

| Protocol | Method | API |
|----------|--------|-----|
| gRpc |  |  Call |
| HTTP | POST |  /v1/user/call |

###### Parametros

Os parametros do método de `call` são os mesmo que os do [SendTransaction](rpc_admin.md#sendtransaction). Atenção especial:

`to` Hex string do endereço do destinatário. **O valor do `to` é o endereço de um contracto.**

`contract` objecto de transacção de contracto para chamada de smart contract.

* Sub properties(**`source` e `sourceType` não são precisos**):
* `function` a função de chamada do contracto.
* `args` os parametros do contracto. O conteúdo dos args é uma string JSON com um array de parametros.

###### Returns

`result` resultado do método da chamada do smart contract

`execute_err` erro de execução.

`estimate_gas` estimativa de gas utilizado.

###### Exemplo HTTP

```bash
// Request
curl -i -H 'Content-Type: application/json' -X POST http://localhost:8685/v1/user/call -d '{"from":"n1Z6SbjLuAEXfhX1UJvXT6BB5osWYxVg3F3","to":"n1mL2WCZyRi1oELEugfCZoNAW3dt8QpHtJw","value":"0","nonce":3,"gasPrice":"20000000000","gasLimit":"2000000","contract":{"function":"transferValue","args":"[500]"}}'

// Result
{
   "result": {
       "result": "0",
       "execute_err": "insufficient balance",
       "estimate_gas": "22208"
   }
}
```
***

#### SendRawTransaction

Envia a transacção assinada. O valor da transacção assinada deve ser obtido por [SignTransactionWithPassphrase](rpc_admin.md#signtransactionwithpassphrase).

| Protocol | Method | API |
|----------|--------|-----|
| gRpc |  | SendRawTransaction |
| HTTP | POST |  /v1/user/rawtransaction |

###### Parameters

`data` dados assinados da transacção.

###### Returns

`txhash` Hex string do hash da transacção.

`contract_address ` return apenas para transacção já lançada.

###### Exemplo HTTP

```bash
// Request
curl -i -H 'Content-Type: application/json' -X POST http://localhost:8685/v1/user/rawtransaction -d '{"data":"CiCrHtxyyIJks2/RErvBBA862D6iwAaGQ9OK1NisSGAuTBIYGiY1R9Fnx0z0uPkWbPokTeBIHFFKRaosGhgzPLPtjEF5cYRTgu3jz2egqWJwwF/i9wAiEAAAAAAAAAAADeC2s6dkAAAoAjDd/5jSBToICgZiaW5hcnlAZEoQAAAAAAAAAAAAAAAAAA9CQFIQAAAAAAAAAAAAAAAAAABOIFgBYkGLnnvGZEDSlocc202ZRWtUlbl2RHfGNdBY5eajFiHKThfgXIwGixh17LpnZGnYHlmfiGe2zqnFHdj7G8b2XIP2AQ=="}'

// Result
{
    "result":{
        "txhash": "f37acdf93004f7a3d72f1b7f6e56e70a066182d85c186777a2ad3746b01c3b52"
    }
}
```

###### Exemplo de Lançamento de Contracto

```bash
// Request
curl -i -H 'Content-Type: application/json' -X POST http://localhost:8685/v1/user/rawtransaction -d '{"data":"CiDam3G9Sy5fV6/ZcjasYPwSF39ZJDIHNB0Us94vn6p6ohIaGVfLzJ83pom1DO1gD307f1JdTVdDLzbMXO4aGhlXy8yfN6aJtQztYA99O39SXU1XQy82zFzuIhAAAAAAAAAAAAAAAAAAAAAAKBswwfTs1QU64AcKBmRlcGxveRLVB3siU291cmNlVHlwZSI6ImpzIiwiU291cmNlIjoiJ3VzZSBzdHJpY3QnXG5cbnZhciBUcmFuc2ZlclZhbHVlQ29udHJhY3QgPSBmdW5jdGlvbiAoKSB7XG4gICAgLy8gTG9jYWxDb250cmFjdFN0b3JnZS5kZWZpbmVQcm9wZXJ0aWVzKHRoaXMsIHtcbiAgICAvLyAgICAgdG90YWxCYWxhbmNlOiBudWxsXG4gICAgLy8gfSlcbn1cblxuXG5UcmFuc2ZlclZhbHVlQ29udHJhY3QucHJvdG90eXBlID0ge1xuICAgICBpbml0OiBmdW5jdGlvbigpIHtcbiAgICAvLyAgICAgdGhpcy50b3RhbEJhbGFuY2UgPSAwO1xuICAgICB9LFxuXG4gICAgdHJhbnNmZXI6IGZ1bmN0aW9uKHRvKSB7XG4gICAgICAgIHZhciByZXN1bHQgPSBCbG9ja2NoYWluLnRyYW5zZmVyKHRvLCBCbG9ja2NoYWluLnRyYW5zYWN0aW9uLnZhbHVlKTtcbiAgICAgICAgLy8gdmFyIHJlc3VsdCA9IEJsb2NrY2hhaW4udHJhbnNmZXIodG8sIDApO1xuICAgICAgICBpZiAoIXJlc3VsdCkge1xuXHQgICAgXHR0aHJvdyBuZXcgRXJyb3IoXCJ0cmFuc2ZlciBmYWlsZWQuXCIpO1xuICAgICAgICB9XG4gICAgICAgIHJldHVybiBCbG9ja2NoYWluLnRyYW5zYWN0aW9uLnZhbHVlO1xuICAgIH0sXG4gICAgdHJhbnNmZXJTcGVjaWFsVmFsdWU6IGZ1bmN0aW9uKHRvLCB2YWx1ZSkge1xuICAgICAgICB2YXIgYW1vdW50ID0gbmV3IEJpZ051bWJlcih2YWx1ZSk7XG4gICAgICAgIHZhciByZXN1bHQgPSBCbG9ja2NoYWluLnRyYW5zZmVyKHRvLCBhbW91bnQpO1xuICAgICAgICAvLyB2YXIgcmVzdWx0ID0gQmxvY2tjaGFpbi50cmFuc2Zlcih0bywgMCk7XG4gICAgICAgIGlmICghcmVzdWx0KSB7XG4gICAgICAgICAgICB0aHJvdyBuZXcgRXJyb3IoXCJ0cmFuc2ZlciBmYWlsZWQuXCIpO1xuICAgICAgICB9IGVsc2Uge1xuICAgICAgICAgICAgcmV0dXJuIDBcbiAgICAgICAgfVxuICAgIH0sXG4gICAgXG59XG5tb2R1bGUuZXhwb3J0cyA9IFRyYW5zZmVyVmFsdWVDb250cmFjdDsifUBkShAAAAAAAAAAAAAAAAAAD0JAUhAAAAAAAAAAAAAAAAABMS0AWAFiQcJUX32jGcduxnJCjvJ9kRcGXhSK2+h3Tb46ySjAToGAY11C7mysGEU11OE6YTd+WNAo/CEbThvI0iKcjHhgBZUB"}'

// Result
{
    "result":{
        "txhash": "f37acdf93004f7a3d72f1b7f6e56e70a066182d85c186777a2ad3746b01c3b52",
        "contract_address":"4702b597eebb7a368ac4adbb388e5084b508af582dadde47"
    }
}
```
***

#### GetBlockByHash

Obtem informação do block header através da hash to bloco.

| Protocol | Method | API |
|----------|--------|-----|
| gRpc |  | GetBlockByHash |
| HTTP | POST |  /v1/user/getBlockByHash |

###### Parametros

`hash` Hex string do block hash.

`full_fill_transaction` se 'true' return os objectos da transacção. Se 'false' apenas as hashes das transacções.

###### Returns

Ver [LatestIrreversibleBlock](#latestirreversibleblock) resposta.

###### Exemplo HTTP

```bash
// Request
curl -i -H 'Content-Type: application/json' -X POST http://localhost:8685/v1/user/getBlockByHash -d '{"hash":"c4a51d6241db372c1b8720e62c04426bd587e1f31054b7d04a3509f48ee58e9f", "full_fill_transaction":true}'

// Result
{
    "result":{
        "hash":"c4a51d6241db372c1b8720e62c04426bd587e1f31054b7d04a3509f48ee58e9f",
        "parent_hash":"8f9f29028356d2fb2cf1291dcee85785e1c20a2145318f36c136978edb6097ce",
        "height":"407",
        "nonce":"0",
        "coinbase":"n1QZMXSZtW7BUerroSms4axNfyBGyFGkrh5",
        "timestamp":"1521963660",
        "chain_id":100,
        "state_root":"a77bbcd911e7ee9488b623ce4ccb8a38d9a83fc29eb5ad43009f3517f1d3e19a",
        "txs_root":"664671e2fda200bd93b00aaec4ab12db718212acd51b4624e8d4937003a2ab22",
        "events_root":"2607e32c166a3513f9effbd1dc7caa7869df5989398d0124987fa0e4d183bcaf",
        "consensus_root":{
            "timestamp":"1521963660",
            "proposer":"GVeOQnYf20Ppxa2cqTrPHdpr6QH4SKs4ZKs=",
            "dynasty_root":"IfTgx0o271Gg4N3cVKHe7dw3NREnlYCN8aIl8VvRXDY="
        },
        "miner": "n1WwqBXVMuYC3mFCEEuFFtAXad6yxqj4as4"
        "is_finality":false,
        "transactions":[{
            "hash":"1e96493de6b5ebe686e461822ec22e73fcbfb41a6358aa58c375b935802e4145",
            "chainId":100,
            "from":"n1Z6SbjLuAEXfhX1UJvXT6BB5osWYxVg3F3",
            "to":"n1orSeSMj7nn8KHHN4JcQEw3r52TVExu63r",
            "value":"10000000000000000000","nonce":"34",
            "timestamp":"1522220087",
            "type":"binary",
            "data":null,
            "gas_price":"1000000",
            "gas_limit":"2000000",
            "contract_address":"",
            "status":1,
            "gas_used":"20000"
        }]
    }
}
```
***

#### GetBlockByHeight

Obtem a informação do block header através do block height.

| Protocol | Method | API |
|----------|--------|-----|
| gRpc |  | GetBlockByHeight |
| HTTP | POST |  /v1/user/getBlockByHeight |

###### Parametros

`height` height da hash da transacção.

`full_fill_transaction` se 'true' return os objectos da transacção, se 'false' apenas faz return das hashes das transacções.

###### Returns

Ver [LatestIrreversibleBlock](#latestirreversibleblock) resposta.

###### Exemplo HTTP

```bash
// Request
curl -i -H 'Content-Type: application/json' -X POST http://localhost:8685/v1/user/getBlockByHeight -d '{"height": 256, "full_fill_transaction": true}'

// Result
{
    "result":{
        "hash":"c4a51d6241db372c1b8720e62c04426bd587e1f31054b7d04a3509f48ee58e9f",
        "parent_hash":"8f9f29028356d2fb2cf1291dcee85785e1c20a2145318f36c136978edb6097ce",
        "height":"407",
        "nonce":"0",
        "coinbase":"n1QZMXSZtW7BUerroSms4axNfyBGyFGkrh5",
        "timestamp":"1521963660",
        "chain_id":100,
        "state_root":"a77bbcd911e7ee9488b623ce4ccb8a38d9a83fc29eb5ad43009f3517f1d3e19a",
        "txs_root":"664671e2fda200bd93b00aaec4ab12db718212acd51b4624e8d4937003a2ab22",
        "events_root":"2607e32c166a3513f9effbd1dc7caa7869df5989398d0124987fa0e4d183bcaf",
        "consensus_root":{
            "timestamp":"1521963660",
            "proposer":"GVeOQnYf20Ppxa2cqTrPHdpr6QH4SKs4ZKs=",
            "dynasty_root":"IfTgx0o271Gg4N3cVKHe7dw3NREnlYCN8aIl8VvRXDY="
        },
        "miner": "n1WwqBXVMuYC3mFCEEuFFtAXad6yxqj4as4"
        "is_finality":false,
        "transactions":[{
            "hash":"1e96493de6b5ebe686e461822ec22e73fcbfb41a6358aa58c375b935802e4145",
            "chainId":100,
            "from":"n1Z6SbjLuAEXfhX1UJvXT6BB5osWYxVg3F3",
            "to":"n1orSeSMj7nn8KHHN4JcQEw3r52TVExu63r",
            "value":"10000000000000000000","nonce":"34",
            "timestamp":"1522220087",
            "type":"binary",
            "data":null,
            "gas_price":"1000000",
            "gas_limit":"2000000",
            "contract_address":"",
            "status":1,
            "gas_used":"20000"
        }]
    }
}
```
***

#### GetTransactionReceipt

Get transactionReceipt info by tansaction hash. If the transaction not submit or only submit and not packaged on chain, it will reurn not found error.

| Protocol | Method | API |
|----------|--------|-----|
| gRpc |  | GetTransactionReceipt |
| HTTP | POST |  /v1/user/getTransactionReceipt |

###### Parameters

`hash` Hex string of transaction hash.

###### Returns

`hash` Hex string of tx hash.

`chainId` Transaction chain id.

`from` Hex string of the sender account addresss.

`to` Hex string of the receiver account addresss.

`value` Value of transaction.

`nonce` Transaction nonce.

`timestamp` Transaction timestamp.

`type` Transaction type.

`data` Transaction data, return the payload data.

`gas_price` Transaction gas price.

`gas_limit` Transaction gas limit.

`contract_address` Transaction contract address.

`status` Transaction status, 0 failed, 1 success, 2 pending.

`gas_used` transaction gas used

`execute_error` the execute error of this transaction

`execute_result` return value of the smart-contract function

**Note:** the data length of `execute_result` is limited to 255 Bytes, if you want to receive a large return value from you smart-contract, please use  api `call` instead.

###### HTTP Example

```bash
// Request
curl -i -H 'Content-Type: application/json' -X POST http://localhost:8685/v1/user/getTransactionReceipt -d '{"hash":"cda54445ffccf4ea17f043e86e54be11b002053f9edbe30ae1fbc0437c2b6a73"}'

// Result
{
    "result":{
        "hash":"cda54445ffccf4ea17f043e86e54be11b002053f9edbe30ae1fbc0437c2b6a73",
        "chainId":100,
        "from":"n1Z6SbjLuAEXfhX1UJvXT6BB5osWYxVg3F3",
        "to":"n1PxKRaJ5jZHXwTfgM9WqkZJJVXBxRcggEE",
        "value":"10000000000000000000",
        "nonce":"53",
        "timestamp":"1521964742",
        "type":"binary",
        "data":null,
        "gas_price":"1000000",
        "gas_limit":"20000",
        "contract_address":"",
        "status":1,
        "gas_used":"20000",
        "execute_error":"",
        "execute_result":"\"\""
    }
}
```
***

#### GetTransactionByContract

Get transactionReceipt info by contract address. If contract not exists or packaged on chain, a not found error will be returned.

| Protocol | Method | API |
|----------|--------|-----|
| gRpc |  | GetTransactionByContract |
| HTTP | POST |  /v1/user/getTransactionByContract |

##### Parameters

`address` Hex string of contract account address.

##### Returns

The result is the same as that of [GetTransactionReceipt](rpc.md/#gettransactionbycontract)

##### HTTP Example

```bash
// Request
curl -i -H 'Content-Type: application/json' -X POST http://localhost:8685/v1/user/getTransactionByContract -d '{"address":"n1sqDHGjYtX6rMqFoq5Tow3s3LqF4ZxBvE3"}'

// Result
{
    "result":{
        "hash":"c5a45a789278f5cce9e95e8f31c1962567f58844456fed7a6eb9afcb764ca6a3",
        "chainId":100,
        "from":"n1Z6SbjLuAEXfhX1UJvXT6BB5osWYxVg3F3",
        "to":"n1Z6SbjLuAEXfhX1UJvXT6BB5osWYxVg3F3",
        "value":"0",
        "nonce":"1",
        "timestamp":"1521964742",
        "type":"deploy",
        "data":"eyJTb3VyY2VUeXBlIjoianMiLCJTb3VyY2UiOiJcInVzZSBzdHJpY3RcIjtcblxudmFyIENvbnRyYWN0ID0gZnVuY3VuY3Rpb24oKSB7XG5cbiAgICAgICAgRXZlbnQuVHJpZ2dlcih......UmFuZG9tMlwiOiByMTIsXG4gImRlZmF1bHRTZWVkUmFuZG9tM1wiOiByMTMsXG4gICAgICAgICAgICBcInVzZXJTZWVkUmFuZG9tXCI6IHIyXG4gICAgICAgIH0pO1xuICAgIH1cbn07XG5cbm1vZHVsZS5leHBvcnRzID0gQ29udHJhY3Q7IiwiQXJncyI6IiJ9",
        "gas_price":"1000000",
        "gas_limit":"20000",
        "contract_address":"n1sqDHGjYtX6rMqFoq5Tow3s3LqF4ZxBvE3",
        "status":1,
        "gas_used":"20000",
        "execute_error":"",
        "execute_result":"\"\""
    }
}
```

***

#### Subscribe

Return  the subscribed events of transaction & block. The request is a keep-alive connection.

**Note** that `subscribe` doesn't guarantee all the new events will be received successfully, it depends on the network condition, please run a local node to use `subscribe` api. 

| Protocol | Method | API |
|----------|--------|-----|
| gRpc |  |  Subscribe |
| HTTP | POST |  /v1/user/subscribe |

##### Parameters

`topics` repeated event topic name, string array.

The topic name list:

- `chain.pendingTransaction` The topic of pending a transaction in transaction_pool.

- `chain.latestIrreversibleBlock` The topic of updating latest irreversible block.

- `chain.transactionResult` The topic of executing & submitting tx.

- `chain.newTailBlock` The topic of setting new tail block.

- `chain.revertBlock` The topic of reverting block.

##### Returns

`topic` subscribed event topic name.

`data` subscribed event data.

##### HTTP Example

```bash
// Request
curl -i -H 'Content-Type: application/json' -X POST http://localhost:8685/v1/user/subscribe -d '{"topics":["chain.linkBlock", "chain.pendingTransaction"]}'

// Result
{
    "result":{
        "topic":"chain.pendingTransaction",
        "data":"{
                \"chainID\":100,
                 \"hash\":\"b466c7a9b667db8d15f74863a4bc60bc989566b6c3766948b2cacb45a4fbda42\", 
                 \"from\":\"n1Z6SbjLuAEXfhX1UJvXT6BB5osWYxVg3F3\",
                 \"to\":\"n1Z6SbjLuAEXfhX1UJvXT6BB5osWYxVg3F3\", 
                 \"nonce\":6,
                 \"value\":\"0\",
                 \"timestamp\":1522215320,
                 \"gasprice\": \"20000000000\", 
                 \"gaslimit\":\"20000000\",
                 \"type\":\"deploy\"}"
    }
    "result":{
        "topic":"chain.pendingTransaction",
        "data": "..."
    }
    ...
}
```
***

#### GetGasPrice

Return current gasPrice.

| Protocol | Method | API |
|----------|--------|-----|
| gRpc |  |  GetGasPrice |
| HTTP | GET |  /v1/user/getGasPrice |

##### Parameters

none

##### Returns

`gas_price` gas price. The unit is 10^-18 NAS.

##### HTTP Example

```bash
// Request
curl -i -H 'Content-Type: application/json' -X GET http://localhost:8685/v1/user/getGasPrice

// Result
{
    "result":{
        "gas_price":"20000000000"
    }
}
```
***

#### EstimateGas

Return the estimate gas of transaction.

| Protocol | Method | API |
|----------|--------|-----|
| gRpc |  |  EstimateGas |
| HTTP | POST |  /v1/user/estimateGas |

##### Parameters

The parameters of the `EstimateGas` method is the same as the
[SendTransaction](rpc_admin.md/#sendtransaction)parameters.

##### Returns

`gas` the estimate gas.

`err` error message of the transaction executing

##### HTTP Example

``` bash
// Request
curl -i -H 'Content-Type: application/json' -X POST http://localhost:8685/v1/user/estimateGas -d '{"from":"n1QZMXSZtW7BUerroSms4axNfyBGyFGkrh5","to":"n1SAeQRVn33bamxN4ehWUT7JGdxipwn8b17", "value":"1000000000000000000","nonce":1,"gasPrice":"20000000000","gasLimit":"2000000"}'

// Result
{
    "result": {
        "gas":"20000",
        "err":""
    }
}
```
***

#### GetEventsByHash
Return the events list of transaction.

| Protocol | Method | API |
|----------|--------|-----|
| gRpc |  |  GetEventsByHash |
| HTTP | POST |  /v1/user/getEventsByHash |

##### Parameters
`hash` Hex string of transaction hash.

##### Returns
`events` the events list.
- `topic` event topic;
- `data` event data.

##### HTTP Example
``` bash
// Request
curl -i -H 'Content-Type: application/json' -X POST http://localhost:8685/v1/user/getEventsByHash -d '{"hash":"ec239d532249f84f158ef8ec9262e1d3d439709ebf4dd5f7c1036b26c6fe8073"}'

// Result
{
    "result":{
        "events":[{
            "topic":"chain.transactionResult",
            "data":"{
                \"hash\":\"d7977f96294cd232781d9c17f0f3212b48312d5ef0f556551c5cf48622759785\",
                \"status\":1,
                \"gas_used\":\"22208\",
                \"error\":\"\"
            }"
        }]
    }
}
```
***

#### GetDynasty

GetDynasty get dpos dynasty.

| Protocol | Method | API |
|----------|--------|-----|
| gRpc |  |  GetDynasty |
| HTTP | POST |  /v1/user/dynasty |

###### Parameters

`height` block height

###### Returns

`miners` repeated string of miner address.

###### HTTP Example

``` bash
// Request
curl -i -H 'Content-Type: application/json' -X POST http://localhost:8685/v1/user/dynasty -d '{"height": 1}'

// Result
{
    {
        "result":{
            "miners":[
                "n1FkntVUMPAsESuCAAPK711omQk19JotBjM",
                "n1JNHZJEUvfBYfjDRD14Q73FX62nJAzXkMR",
                "n1Kjom3J4KPsHKKzZ2xtt8Lc9W5pRDjeLcW",
                "n1TV3sU6jyzR4rJ1D7jCAmtVGSntJagXZHC",
                "n1WwqBXVMuYC3mFCEEuFFtAXad6yxqj4as4",
                "n1Zn6iyyQRhqthmCfqGBzWfip1Wx8wEvtrJ"
            ]
        }
    }
}
```

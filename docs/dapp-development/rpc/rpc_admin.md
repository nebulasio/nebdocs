# Management RPC

Además de la interfaz [Neb API RPC](rpc.md), Nebulas ofrece otras API de administración. La consola Neb da soporte tanto para API como para interfaces administrativas. _Management RPC_ hace uso de gRPC y HTTP, lo que permite vincular, además, las interfaces [Neb API RPC](rpc.md).

Nebulas brinda tanto [gRPC](https://grpc.io) como RESTful API para que sus usuarios interactuen con el sistema.

Nuestro [archivo proto de administración](https://github.com/nebulasio/go-nebulas/blob/develop/rpc/pb/rpc.proto) define todas las API administrativas. **Recomendamos usar las interfaces de acceso vía consola, o restringir la RPC administrativa sólo para acceso local**.

Endpoint administrativo por defecto del RPC:

| API | URL | Protocol |
| --- | :---: | :---: |
| gRPC | [http://localhost:8684](http://localhost:8684) | Protobuf |
| RESTful | [http://localhost:8685](http://localhost:8685) | HTTP |

## Referencia

### Índice

* [NodeInfo](#nodeinfo)
* [Accounts](#accounts)
* [NewAccount](#newaccount)
* [UnLockAccount](#unlockaccount)
* [LockAccount](#lockaccount)
* [SignTransactionWithPassphrase](#signtransactionwithpassphrase)
* [SendTransactionWithPassphrase](#sendtransactionwithpassphrase)
* [SendTransaction](#sendtransaction)
* [SignHash](#signhash)
* [StartPprof](#startpprof)
* [GetConfig](#getconfig)

### NodeInfo

Devuelve información del nodo p2p.

| Protocol | Method | API |
| --- | --- | --- |
| gRpc |  | NodeInfo |
| HTTP | GET | /v1/user/nodeinfo |

#### Parámetros

Ninguno.

#### Devuelve

* `id`: ID del nodo.
* `chain_id`: ID del blockchain.
* `coinbase`: Coinbase.
* `peer_count`: Número de pares conectados al momento de la consulta.
* `synchronized`: Estado de sincronización del nodo.
* `bucket_size`: El tamaño del _bucket_ de la tabla de rutas del nodo.
* `protocol_version`: Versión del protocolo de red.
* `RouteTable*[] route_table`: Matriz routeTable de la red.

```js
message RouteTable {
    string id = 1;
    repeated string address = 2;
}
```

#### Ejemplo HTTP

```bash

// Petición
curl -i -H 'Content-Type: application/json' -X GET http://localhost:8685/v1/admin/nodeinfo

// Resultado
{
    "result":{
        "id":"QmP7HDFcYmJL12Ez4ZNVCKjKedfE7f48f1LAkUc3Whz4jP",
        "chain_id":100,
        "coinbase":"n1QZMXSZtW7BUerroSms4axNfyBGyFGkrh5",
        "peer_count":4,
        "synchronized":false,
        "bucket_size":64,
        "protocol_version":"/neb/1.0.0",
        "route_table":[
            {
                "id":"QmP7HDFcYmJL12Ez4ZNVCKjKedfE7f48f1LAkUc3Whz4jP",
                "address":[
                    "/ip4/127.0.0.1/tcp/8680",
                    "/ip4/192.168.1.206/tcp/8680"
                ]
            },
            {
                "id":"QmUxw4PZ8kMEnHD8WaSVE92dtvdnwgufM6m5DrWemdk2M7",
                "address":[
                    "/ip4/192.168.1.206/tcp/10003","/ip4/127.0.0.1/tcp/10003"
                ]
            }
        ]
    }
}

```

### Accounts

Devuelve una lista de cuentas.

| Protocol | Method | API |
| --- | --- | --- |
| gRpc |  | Accounts |
| HTTP | GET | /v1/admin/accounts |

#### Parámetros

Ninguno.

#### Devuelve

* `addresses`: Lista de cuentas.

#### Ejemplo HTTP

```bash

// Petición
curl -i -H 'Content-Type: application/json' -X GET http://localhost:8685/v1/admin/accounts

// Resultado
{
    "result":{
        "addresses":[
            "n1FkntVUMPAsESuCAAPK711omQk19JotBjM",
            "n1JNHZJEUvfBYfjDRD14Q73FX62nJAzXkMR",
            "n1Kjom3J4KPsHKKzZ2xtt8Lc9W5pRDjeLcW",
            "n1NHcbEus81PJxybnyg4aJgHAaSLDx9Vtf8",
            "n1QZMXSZtW7BUerroSms4axNfyBGyFGkrh5",
            "n1TV3sU6jyzR4rJ1D7jCAmtVGSntJagXZHC",
            "n1WwqBXVMuYC3mFCEEuFFtAXad6yxqj4as4",
            "n1Z6SbjLuAEXfhX1UJvXT6BB5osWYxVg3F3",
            "n1Zn6iyyQRhqthmCfqGBzWfip1Wx8wEvtrJ"
        ]
    }
}

```

### NewAccount

Crea una nueva cuenta protegida con una _passphrase_.

| Protocol | Method | API |
| --- | --- | --- |
| gRpc |  | NewAccount |
| HTTP | POST | /v1/admin/account/new |

#### Parámetros

* `passphrase`: La _passphrase_ de la cuenta a crear.

#### Devuelve

* `address`: Dirección de la cuenta creada.

#### Ejemplo HTTP

```bash
// Petición
curl -i -H 'Content-Type: application/json' -X POST http://localhost:8685/v1/admin/account/new -d '{"passphrase":"passphrase"}'

// Resultado

{
    "result":{
        "address":"n1czGUvbQQton6KUWga4wKDLLKYDEn39mEk"
    }
}
```

### UnLockAccount

Desbloquea una cuenta protegida con _passphrase_. Transcurrido el tiempo de desbloqueo por defecto, la cuenta se bloqueará nuevamente.

| Protocol | Method | API |
| --- | --- | --- |
| gRpc |  | UnLockAccount |
| HTTP | POST | /v1/admin/account/unlock |

#### Parámetros

* `address`: Dirección de la cuenta a desbloquear.
* `passphrase`: _Passphrase_ de la cuenta a desbloquear.
* `duration`: Duración del desbloqueo, en nanosegundos.

#### Devuelve

* `result`: Boolean con el resultado del desbloqueo.

#### Ejemplo HTTP

```bash

// Petición
curl -i -H 'Content-Type: application/json' -X POST http://localhost:8685/v1/admin/account/unlock -d '{"address":"n1czGUvbQQton6KUWga4wKDLLKYDEn39mEk","passphrase":"passphrase","duration":"1000000000"}'

// Resultado
{
    "result":{
        "result":true
    }
}

```

### LockAccount

Bloquea una cuenta dada.

| Protocol | Method | API |
| --- | --- | --- |
| gRpc |  | LockAccount |
| HTTP | POST | /v1/admin/account/lock |

#### Parámetros

* `address`: Dirección de la cuenta a bloquear.

#### Devuelve

* `result`: Boolean que indica el resultado del bloqueo.

#### Ejemplo HTTP

```bash
// Petición
curl -i -H 'Content-Type: application/json' -X POST http://localhost:8685/v1/admin/account/lock -d '{"address":"n1czGUvbQQton6KUWga4wKDLLKYDEn39mEk"}'

// Resultado
{
    "result":{
        "result":true
    }
}
```

### SignTransactionWithPassphrase

Firma una transacción utilizando una _passphrase_. La cuenta de la dirección del emisor debe estar desbloqueada antes de realizar esta llamada.

| Protocol | Method | API |
| --- | --- | --- |
| gRpc |  | SignTransactionWithPassphrase |
| HTTP | POST | /v1/admin/sign |

#### Parámetros

* `passphrase`: _passphrase_ de la cuenta de origen.
* `transaction`: ídem a [SendTransaction](#sendtransaction).

#### Devuelve

* `data`: Datos de la transacción firmada.

#### Ejemplo HTTP

```bash
// Petición
curl -i -H 'Content-Type: application/json' -X POST http://localhost:8685/v1/admin/sign -d '{"transaction":{"from":"n1QZMXSZtW7BUerroSms4axNfyBGyFGkrh5","to":"n1QZMXSZtW7BUerroSms4axNfyBGyFGkrh5", "value":"1000000000000000000","nonce":1,"gasPrice":"1000000","gasLimit":"2000000"}, "passphrase":"passphrase"}'

// Resultado
{
    "result":{
        "data":"CiBOW15yoZ+XqQbMNr4bQdJCXrBTehJKukwjcfW5eASgtBIaGVduKnw+6lM3HBXhJEzzuvv3yNdYANelaeAaGhlXbip8PupTNxwV4SRM87r798jXWADXpWngIhAAAAAAAAAAAA3gtrOnZAAAKAEwucHt1QU6CAoGYmluYXJ5QGRKEAAAAAAAAAAAAAAAAAAPQkBSEAAAAAAAAAAAAAAAAAAehIBYAWJB/BwhwhqUkp/gEJtE4kndoc7NdSgqD26IQqa0Hjbtg1JaszAvHZiW+XH7C+Ky9XTKRJKuTOc446646d/Sbz/nxQE="
    }
}
```

### SendTransactionWithPassphrase

Envía una transacción ya firmada con el método [SignTransactionWithPassphrase](#signtransactionwithpassphrase).

| Protocol | Method | API |
| --- | --- | --- |
| gRpc |  | SendTransactionWithPassphrase |
| HTTP | POST | /v1/admin/transactionWithPassphrase |

#### Parámetros

* `passphrase`: _Passphrase_ de la cuenta origen.
* `transaction`: Ídem a [SendTransaction](#sendtransaction).

#### Devuelve

* `txhash`: Hash de la transacción.
* `contract_address`: Dirección del contrato inteligente.

#### Ejemplo HTTP

```bash
// Petición
curl -i -H 'Content-Type: application/json' -X POST http://localhost:8685/v1/admin/transactionWithPassphrase -d '{"transaction":{"from":"n1QZMXSZtW7BUerroSms4axNfyBGyFGkrh5","to":"n1QZMXSZtW7BUerroSms4axNfyBGyFGkrh5", "value":"1000000000000000000","nonce":1,"gasPrice":"1000000","gasLimit":"2000000"},"passphrase":"passphrase"}'

// Resultado
{
    "result":{
        "hash":"143eac221da8079f017bd6fd6b6a08ea0623114c93c638b94334d16aae109666",
        "contract_address":""
    }
}
```

### SendTransaction

Envía una transacción. Los parámetros `from`, `to`, `value`, `nonce`, `gasPrice` y `gasLimit` son obligatorios.

| Protocol | Method | API |
| --- | --- | --- |
| gRpc |  | SendTransaction |
| HTTP | POST | /v1/user/transaction |

#### Parámetros

* `from`: Requerido. Cadena hexadecimal que representa la dirección de la cuenta origen.
* `to`: Requerido. Cadena hexadecimal que representa la dirección de la cuenta destino.
* `value`: Requerido. Valor a enviar junto a esta transacción.
* `nonce`: Requerido. Nonce de la transacción.
* `gas_price`: Requerido. Precio del gas para esta transacción.
* `gas_limit`: Requerido. Límite de gas para esta transacción.
* `binary`: Opcional. Cualquier tipo de datos binarios con un tamaño máximo de 64 bytes.
* `type`: Opcional. Tipo de payload para la transacción. Si se especifica un tipo, el necesario pasar el parámetro correspondiente; si no se especifica, el tipo se determinará de acuerdo al contrato y a sus datos binarios. Enumeración de tipos:
    * `binary`: Transacción normal con datos binarios.
    * `deploy`: Implementación de contrato inteligente.
    * `call`: Llamada a función de contrato inteligente.
* `contract`: Opcional. Objeto que contiene el contrato a implementar o la función a llamar mediante esta transacción. Propiedades:
    * `source`: Código fuente del contrato a implementar.
    * `sourceType`: Abreviatura del lenguaje del código fuente a implementar. Acepta los siguientes valores:
        * `js`: Cóigo fuente en Javascript.
        * `ts`: Código fuente en Typescript.
    * `function`: La llamada a función del contrato inteligente.
    * `args`: Parámetros para el contrato inteligente. El contenido es una matriz de cadenas codificada en un objeto JSON.

##### Importante

* Al implementar un contrato inteligente, los parámetros `to` y `from` deben ser idénticos, y deben corresponder a la dirección del contrato inteligente.
* `nonce`: Para obtener el valor correcto, debe sumarse 1 al nonce actual de la dirección asignada al contrato inteligente. El valor actual del nonce se puede obtener mediante [GetAccountState](rpc.md/#getaccountstate).
* Los parámetros `gasPrice` y `gasLimit` son necesarios para cada transacción. Recomendamos hacer uso de los métodos [GetGasPrice](rpc.md/#getgasprice) y [EstimateGas](rpc.md/#estimategas) para obtener los valores actuales.
* El parámetro `contract` sólo se requiere para las implementaciones (o llamadas a funciones) de contratos inteligentes.
* Cuando se implementa un contrato inteligente, los parámetros `source` y `sourceType` deben especificarse; `args` en este contexto es opcional y se usa únicamente cuando la función de inicialización lo requiere.
* El campo `function` se usa para llamar a un método de un contrato inteligente.

#### Devuelve

* `txhash`: Hash de la transacción.
* `contract_address`: Valor devuelto únicamente al implementar un contrato inteligente.

#### Ejemplo para una transacción corriente

```bash

// Petición
curl -i -H 'Content-Type: application/json' -X POST http://localhost:8685/v1/admin/transaction -d '{"from":"n1QZMXSZtW7BUerroSms4axNfyBGyFGkrh5","to":"n1SAeQRVn33bamxN4ehWUT7JGdxipwn8b17", "value":"1000000000000000000","nonce":1000,"gasPrice":"1000000","gasLimit":"2000000"}'

// Resultado
{
    "result":{
      "txhash":"fb5204e106168549465ea38c040df0eacaa7cbd461454621867eb5abba92b4a5",
      "contract_address":""
    }
}

```

#### Ejemplo para una implementación de contrato inteligente

```bash

// Petición
curl -i -H 'Content-Type: application/json' -X POST http://localhost:8685/v1/admin/transaction -d '{"from":"n1QZMXSZtW7BUerroSms4axNfyBGyFGkrh5","to":"n1QZMXSZtW7BUerroSms4axNfyBGyFGkrh5", "value":"0","nonce":2,"gasPrice":"1000000","gasLimit":"2000000","contract":{
"source":"\"use strict\";var BankVaultContract=function(){LocalContractStorage.defineMapProperty(this,\"bankVault\")};BankVaultContract.prototype={init:function(){},save:function(height){var deposit=this.bankVault.get(Blockchain.transaction.from);var value=new BigNumber(Blockchain.transaction.value);if(deposit!=null&&deposit.balance.length>0){var balance=new BigNumber(deposit.balance);value=value.plus(balance)}var content={balance:value.toString(),height:Blockchain.block.height+height};this.bankVault.put(Blockchain.transaction.from,content)},takeout:function(amount){var deposit=this.bankVault.get(Blockchain.transaction.from);if(deposit==null){return 0}if(Blockchain.block.height<deposit.height){return 0}var balance=new BigNumber(deposit.balance);var value=new BigNumber(amount);if(balance.lessThan(value)){return 0}var result=Blockchain.transfer(Blockchain.transaction.from,value);if(result>0){deposit.balance=balance.dividedBy(value).toString();this.bankVault.put(Blockchain.transaction.from,deposit)}return result}};module.exports=BankVaultContract;","sourceType":"js", "args":""}}'

// Resultado
{
    "result":{
        "txhash":"3a69e23903a74a3a56dfc2bfbae1ed51f69debd487e2a8dea58ae9506f572f73",
        "contract_address":"n21Y7arNbUfLGL59xgnA4ouinNxyvz773NW"
    }
}

```

### SignHash

Permite firmar el hash de un mensaje.

| Protocol | Method | API |
| --- | --- | --- |
| gRpc |  | SignHash |
| HTTP | POST | /v1/admin/sign/hash |

#### Parámetros

* `address`: Dirección del emisor.
* `hash`: Hash del mensaje, utilizando el algoritmo sha3-256.
* `alg`: Algoritmo a emplear para la firma.

#### Devuelve

* `data`: Datos de la transacción firmada.

#### Ejemplo de una transacción corriente

```bash
// Petición
curl -i -H 'Content-Type: application/json' -X POST http://localhost:8685/v1/admin/sign/hash -d '{"address":"n1QZMXSZtW7BUerroSms4axNfyBGyFGkrh5","hash":"W+rOKNqs/tlvz02ez77yIYMCOr2EubpuNh5LvmwceI0=","alg":1}'

// Resultado
{
    "result":{
        "data":"a7HHsLRvKTNazD1QEogY+Fre8KmBIyK+lNa4zv0Z72puFVkY9uZD6nGixGx/6s1x6Baq7etGwlDNxVvHsoGWbAA="
    }
}
```

### StartPprof

Inicia pprof.

| Protocol | Method | API |
| --- | --- | --- |
| gRpc |  | Pprof |
| HTTP | POST | /v1/admin/pprof |

#### Parámetros

* `listen`: IP y puerto a monitorear.

#### Devuelve

* `result`: Boolean que indica si pprof se inició correctamente.

#### Ejemplo HTTP

```bash
// Petición
curl -i -H 'Content-Type: application/json' -X POST http://localhost:8685/v1/admin/pprof -d '{"listen":"0.0.0.0:1234"}'

// Resultado
{
    "result":{
        "result":true
    }
}
```

### GetConfig

Devuelve los parámetros de configuración en uso por la consola Neb.

| Protocol | Method | API |
| --- | --- | --- |
| gRpc |  | GetConfig |
| HTTP | GET | /v1/admin/getConfig |

#### Parámetros

Ninguno.

#### Devuelve

* `config`: Matriz con los parámetros de configuración.

#### Ejemplo HTTP

```bash
// Petición
curl -i -H 'Content-Type: application/json' -X GET http://localhost:8685/v1/admin/getConfig

// Resultado
{
    "result":{
        "config":{
            "network":{
                "seed":[],
                "listen":["0.0.0.0:8680"],
                "private_key":"conf/network/ed25519key",
                "network_id":1
            },
            "chain":{
                "chain_id":100,
                "genesis":"conf/default/genesis.conf",
                "datadir":"data.db",
                "keydir":"keydir",
                "start_mine":true,
                "coinbase":"n1QZMXSZtW7BUerroSms4axNfyBGyFGkrh5",
                "miner":"n1Zn6iyyQRhqthmCfqGBzWfip1Wx8wEvtrJ",
                "passphrase":"",
                "enable_remote_sign_server":false,
                "remote_sign_server":"",
                "gas_price":"",
                "gas_limit":"",
                "signature_ciphers":["ECC_SECP256K1"]
            },
            "rpc":{
                "rpc_listen":["127.0.0.1:8684"],
                "http_listen":["127.0.0.1:8685"],
                "http_module":["api","admin"],
                "connection_limits":0,
                "http_limits":0,
                "http_cors":[]
            },
            "stats":{
                "enable_metrics":false,
                "reporting_module":[],
                "influxdb":{
                    "host":"http://localhost:8086",
                    "port":0,
                    "db":"nebulas",
                    "user":"admin",
                    "password":"admin"
                },
                "metrics_tags":[]
            },
            "misc":null,
            "app":{
                "log_level":"debug",
                "log_file":"logs",
                "log_age":0,
                "enable_crash_report":true,
                "crash_report_url":"https://crashreport.nebulas.io",
                "pprof":{
                    "http_listen":"0.0.0.0:8888",
                    "cpuprofile":"",
                    "memprofile":""
                },
                "version":"0.7.0"
            }
        }
    }
}
```
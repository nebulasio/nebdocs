# RPC de Gestão

Além da interface do [NEB API RPC](https://github.com/nebulasio/wiki/blob/master/rpc.md), a Nebulas fornece APIs de gestão adicionais. A consola Neb suporta gestão de ambos APIs e interfaces de gestão. RPC de gestão usa a mesma porta para gRPC e HTTP, que também vincula as interfaces do [NEB API RPC](https://github.com/nebulasio/wiki/blob/master/rpc.md).

A Nebulas fornece ambos [gRPC](https://grpc.io) e APIs de gestão RESTful para permitir interacção dos utilizadores com a Nebulas. O nosso ficheiro administrativo [proto](https://github.com/nebulasio/go-nebulas/blob/develop/rpc/pb/rpc.proto) define todos os admin APIs. **Recomendamos o uso da consola para aceder a interfaces de administrador, ou restringir o RPC de admin ao acesso local.**

Default management RPC Endpoint:

| API | URL | Protocol |
| --- | :---: | :---: |
| gRPC | [http://localhost:8684](http://localhost:8684) | Protobuf |
| RESTful | [http://localhost:8685](http://localhost:8685) | HTTP |

## Métodos do Management RPC

* [NodeInfo](rpc_admin.md#nodeinfo)
* [Accounts](rpc_admin.md#accounts)
* [NewAccount](rpc_admin.md#newaccount)
* [UnLockAccount](rpc_admin.md#unlockaccount)
* [LockAccount](rpc_admin.md#lockaccount)
* [SignTransactionWithPassphrase](rpc_admin.md#signtransactionwithpassphrase)
* [SendTransactionWithPassphrase](rpc_admin.md#sendtransactionwithpassphrase)
* [SendTransaction](rpc_admin.md#sendtransaction)
* [SignHash](rpc_admin.md#signhash)
* [StartPprof](rpc_admin.md#startpprof)
* [GetConfig](rpc_admin.md#getconfig)

## Referência do Management RPC API

### NodeInfo

Return a informação do nó p2p.

| Protocol | Method | API |
| --- | --- | --- |
| gRpc |  | NodeInfo |
| HTTP | GET | /v1/user/nodeinfo |

**Parametros**

none

**Returns**

`id` o ID do nó.

`chain_id` o block chainID.

`coninbase` coinbase.

`peer_count` número de peers ligados.

`synchronized` estado de sincronização do nó.

`bucket_size` tamanho do bucket da tabela de roteamento. 

`protocol_version` versão do protocolo da rede.

`RouteTable*[] route_table` a routeTable da rede.

```js
message RouteTable {
    string id = 1;
    repeated string address = 2;
}
```

**Exemplo HTTP**

```bash
# Request
curl -i -H 'Content-Type: application/json' -X GET http://localhost:8685/v1/admin/nodeinfo

# Result
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

Return lista de contas.

| Protocol | Method | API |
| --- | --- | --- |
| gRpc |  | Accounts |
| HTTP | GET | /v1/admin/accounts |

#### Parametros

none

#### Returns

`addresses` account list

#### Exemplo HTTP

```bash
# Request
curl -i -H 'Content-Type: application/json' -X GET http://localhost:8685/v1/admin/accounts

# Result
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

NewAccount cria uma nova conta com uma password.

| Protocol | Method | API |
| --- | --- | --- |
| gRpc |  | NewAccount |
| HTTP | POST | /v1/admin/account/new |

**Parameters**

`passphrase` Password da nova conta.

**Returns**

`address` Endereço da nova conta.

**Exemplo HTTP**

```bash
# Request
curl -i -H 'Content-Type: application/json' -X POST http://localhost:8685/v1/admin/account/new -d '{"passphrase":"passphrase"}'

# Result

{
    "result":{
        "address":"n1czGUvbQQton6KUWga4wKDLLKYDEn39mEk"
    }
}
```

### UnLockAccount

UnlockAccount desbloqueia uma conta com a password. Depois do tempo de desbloqueio passar, a conta será bloqueada outra vez.

| Protocol | Method | API |
| --- | --- | --- |
| gRpc |  | UnLockAccount |
| HTTP | POST | /v1/admin/account/unlock |

**Parametros**

`address` Endereço da conta desbloqueada.

`passphrase` Password da conta desbloqueada.

`duration` Duração do desbloqueamento da conta.

**Returns**

`result` Resultado do desbloqueio da conta, a unidade é ns.

**Exemplo HTTP**

```bash
# Request
curl -i -H 'Content-Type: application/json' -X POST http://localhost:8685/v1/admin/account/unlock -d '{"address":"n1czGUvbQQton6KUWga4wKDLLKYDEn39mEk","passphrase":"passphrase","duration":"1000000000"}'

# Result
{
    "result":{
        "result":true
    }
}
```

### LockAccount

LockAccount bloqueia a conta.

| Protocol | Method | API |
| --- | --- | --- |
| gRpc |  | LockAccount |
| HTTP | POST | /v1/admin/account/lock |

**Parameters**

`address` Endereço da conta bloqueada.

**Returns**

`result` Resultado da conta bloqueada.

**Exemplo HTTP**

```bash
# Request
curl -i -H 'Content-Type: application/json' -X POST http://localhost:8685/v1/admin/account/lock -d '{"address":"n1czGUvbQQton6KUWga4wKDLLKYDEn39mEk"}'

# Result
{
    "result":{
        "result":true
    }
}
```

### SignTransactionWithPassphrase

SignTransactionWithPassphrase assina a transacção. O endereço `from` da conta tem de ser desbloqueado antes da chamada da assinatura.

| Protocol | Method | API |
| --- | --- | --- |
| gRpc |  | SignTransactionWithPassphrase |
| HTTP | POST | /v1/admin/sign |

**Parametros**

`transaction` usa os mesmos parametros que a [SendTransaction](https://github.com/nebulasio/wiki/blob/master/rpc_admin.md#sendtransaction).

`passphrase` password da conta `from`.

**Returns**

`data` Dados da transacção assinados.

**Exemplo de uma transacção assinada normal**

```bash
# Request
curl -i -H 'Content-Type: application/json' -X POST http://localhost:8685/v1/admin/sign -d '{"transaction":{"from":"n1QZMXSZtW7BUerroSms4axNfyBGyFGkrh5","to":"n1QZMXSZtW7BUerroSms4axNfyBGyFGkrh5", "value":"1000000000000000000","nonce":1,"gasPrice":"20000000000","gasLimit":"2000000"}, "passphrase":"passphrase"}'

# Result
{
    "result":{
        "data":"CiBOW15yoZ+XqQbMNr4bQdJCXrBTehJKukwjcfW5eASgtBIaGVduKnw+6lM3HBXhJEzzuvv3yNdYANelaeAaGhlXbip8PupTNxwV4SRM87r798jXWADXpWngIhAAAAAAAAAAAA3gtrOnZAAAKAEwucHt1QU6CAoGYmluYXJ5QGRKEAAAAAAAAAAAAAAAAAAPQkBSEAAAAAAAAAAAAAAAAAAehIBYAWJB/BwhwhqUkp/gEJtE4kndoc7NdSgqD26IQqa0Hjbtg1JaszAvHZiW+XH7C+Ky9XTKRJKuTOc446646d/Sbz/nxQE="
    }
}
```

### SendTransactionWithPassphrase

SendTransactionWithPassphrase envia transacção com password.

| Protocol | Method | API |
| --- | --- | --- |
| gRpc |  | SendTransactionWithPassphrase |
| HTTP | POST | /v1/admin/transactionWithPassphrase |

**Parametros**

`transaction` parametros da transacção, que são os mesmos da [SendTransaction](https://github.com/nebulasio/wiki/blob/master/rpc_admin.md#sendtransaction).

`passphrase` Password do endereço `from`.

**Returns**

`txhash` hash da transacção.

`contract_address` return apenas para transacções de contracto lançadas.

**Exemplo**

```bash
# Request
curl -i -H 'Content-Type: application/json' -X POST http://localhost:8685/v1/admin/transactionWithPassphrase -d '{"transaction":{"from":"n1QZMXSZtW7BUerroSms4axNfyBGyFGkrh5","to":"n1QZMXSZtW7BUerroSms4axNfyBGyFGkrh5", "value":"1000000000000000000","nonce":1,"gasPrice":"20000000000","gasLimit":"2000000"},"passphrase":"passphrase"}'

# Result
{
    "result":{
        "hash":"143eac221da8079f017bd6fd6b6a08ea0623114c93c638b94334d16aae109666",
        "contract_address":""
    }
}
```

### SendTransaction

Envia a transacção. Parametros `from`, `to`, `value`, `nonce`, `gasPrice` e `gasLimit` são necessários. Se a transacção for para enviar um contracto, tem de especificar o `contract`.

| Protocol | Method | API |
| --- | --- | --- |
| gRpc |  | SendTransaction |
| HTTP | POST | /v1/user/transaction |

**Parametros**

`from` Hex string do endereço da conta do remetente.

`to` Hex string do endereço da conta do destinatário.

`value` Valor da quantidade a ser enviada com nesta transacção.

`nonce` Nonce da transacção.

`gas_price` gasPrice a ser usado nesta transacção.

`gas_limit` gasLimit desta transacção.

`type` tipo do payload da transacção. Se o tipo for especificado, o tipo de transacção é determinado e o parametro correspondente tem de ser passado, ou então o tipo de transacção será determinado de acordo com o contracto e dados binários. \[optional\]

* type enum:
  * `binary`: transacção normal com binário 
  * `deploy`: lança smart contract
  * `call`: chama função de smart contract

`contract` objecto de contrato de transacção para o lançamento/chamada de smart contract. \[optional\]

* Sub properties:
  * `source` código do contracto para lançar do contracto.
  * `sourceType` tipo da fonte do contracto para lançar contracto. De momento suporta `js` e `ts`
    * `js` é a fonte do contracto programada em javascript.
    * `ts` é a fonte do contracto programada em typescript. 
  * `function` a função de chamada do contracto.
  * `args` os parametros do contracto. O conteúdo dos args é uma string JSON de parametros num array.

`binary` qualquer dado binário com um limite igual a 64bytes. \[optional\]

Note:

* `from = to` quando lançar um contracto, o endereço `to` tem de ser igual ao endereço `from`.
* `nonce` o valor é **plus one**\(+1\) do valor actual do nonce do endereço `from`. O nonce actual pode ser obtido com o [GetAccountState](https://github.com/nebulasio/wiki/blob/master/rpc.md/#getaccountstate).
* `gasPrice` and `gasLimit` need for every transaction. We recommend taking them use [GetGasPrice](https://github.com/nebulasio/wiki/blob/master/rpc.md/#getgasprice) and [EstimateGas](https://github.com/nebulasio/wiki/blob/master/rpc.md/#estimategas).
* parametro `contract` apenas é preciso para o lançamento ou chamada de smart contracts. Quando um smart contract é lançado, a `source` e `sourceType` tem de ser especificada. Os `args` são opcionais e passados quando a inicialização da função leva um parametro. A field da `function` é usado no método de chamada.

**Returns**

`txhash` hash da transacção.

`contract_address` return apenas para transacções de lançamento de contractos.

**Exemplo de uma Transacção Normal**

```bash
# Request
curl -i -H 'Content-Type: application/json' -X POST http://localhost:8685/v1/admin/transaction -d '{"from":"n1QZMXSZtW7BUerroSms4axNfyBGyFGkrh5","to":"n1SAeQRVn33bamxN4ehWUT7JGdxipwn8b17", "value":"1000000000000000000","nonce":1000,"gasPrice":"20000000000","gasLimit":"2000000"}'

# Result
{
    "result":{
      "txhash":"fb5204e106168549465ea38c040df0eacaa7cbd461454621867eb5abba92b4a5",
      "contract_address":""
    }
}
```

**Exemplo de Lançamento de um Smart Contract**

```bash
# Request
curl -i -H 'Content-Type: application/json' -X POST http://localhost:8685/v1/admin/transaction -d '{"from":"n1QZMXSZtW7BUerroSms4axNfyBGyFGkrh5","to":"n1QZMXSZtW7BUerroSms4axNfyBGyFGkrh5", "value":"0","nonce":2,"gasPrice":"20000000000","gasLimit":"2000000","contract":{
"source":"\"use strict\";var BankVaultContract=function(){LocalContractStorage.defineMapProperty(this,\"bankVault\")};BankVaultContract.prototype={init:function(){},save:function(height){var deposit=this.bankVault.get(Blockchain.transaction.from);var value=new BigNumber(Blockchain.transaction.value);if(deposit!=null&&deposit.balance.length>0){var balance=new BigNumber(deposit.balance);value=value.plus(balance)}var content={balance:value.toString(),height:Blockchain.block.height+height};this.bankVault.put(Blockchain.transaction.from,content)},takeout:function(amount){var deposit=this.bankVault.get(Blockchain.transaction.from);if(deposit==null){return 0}if(Blockchain.block.height<deposit.height){return 0}var balance=new BigNumber(deposit.balance);var value=new BigNumber(amount);if(balance.lessThan(value)){return 0}var result=Blockchain.transfer(Blockchain.transaction.from,value);if(result>0){deposit.balance=balance.dividedBy(value).toString();this.bankVault.put(Blockchain.transaction.from,deposit)}return result}};module.exports=BankVaultContract;","sourceType":"js", "args":""}}'

# Result
{
    "result":{
        "txhash":"3a69e23903a74a3a56dfc2bfbae1ed51f69debd487e2a8dea58ae9506f572f73",
        "contract_address":"n21Y7arNbUfLGL59xgnA4ouinNxyvz773NW"
    }
}
```

### SignHash

SignHash assina a hash de uma mensagem.

| Protocol | Method | API |
| --- | --- | --- |
| gRpc |  | SignHash |
| HTTP | POST | /v1/admin/sign/hash |

**Parameters**

`address` Endereço da assinatura.

`hash` Hash sha3256 da mensagem.

`alg` Algoritmo da assinatura.

**Returns**

`data` Dados da transacção assinados.

**Exemplo de uma Transacção Assinada Normal**

```bash
# Request
curl -i -H 'Content-Type: application/json' -X POST http://localhost:8685/v1/admin/sign/hash -d '{"address":"n1QZMXSZtW7BUerroSms4axNfyBGyFGkrh5","hash":"W+rOKNqs/tlvz02ez77yIYMCOr2EubpuNh5LvmwceI0=","alg":1}'

# Result
{
    "result":{
        "data":"a7HHsLRvKTNazD1QEogY+Fre8KmBIyK+lNa4zv0Z72puFVkY9uZD6nGixGx/6s1x6Baq7etGwlDNxVvHsoGWbAA="
    }
}
```

### StartPprof

StartPprof starts pprof

| Protocol | Method | API |
| --- | --- | --- |
| gRpc |  | Pprof |
| HTTP | POST | /v1/admin/pprof |

**Parametros**

`listen` o endereço a escutar

**Returns**

`result` resultado da inicialização do pprof

**Exemplo**

```bash
# Request
curl -i -H 'Content-Type: application/json' -X POST http://localhost:8685/v1/admin/pprof -d '{"listen":"0.0.0.0:1234"}'

# Result
{
    "result":{
        "result":true
    }
}
```

### GetConfig

GetConfig return a configuração actual que o neb está a utilizar.

| Protocol | Method | API |
| --- | --- | --- |
| gRpc |  | GetConfig |
| HTTP | GET | /v1/admin/getConfig |

**Parametros**

none

**Returns**

`config` configuração do neb.

**Exemplo**

```bash
# Request
curl -i -H 'Content-Type: application/json' -X GET http://localhost:8685/v1/admin/getConfig

# Result
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


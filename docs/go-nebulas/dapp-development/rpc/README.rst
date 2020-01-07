RPC
====

Las **Llamadas a Procedimiento Remoto** (Remote Procedure Calls, RPC) brindan una abstracción útil para crear servicios y aplicaciones distribuidas.

Nebulas brinda tanto `gRPC <https://grpc.io>`__ como RESTful API para que sus usuarios interactuen con el sistema.

-  `grpc <https://github.com/grpc/grpc-go>`__ expone una implementación concreta del protocolo gRPC, construido en capas sobre HTTP/2. Estas librerías permiten la comunicación entre clientes y servidores usando cualquier combinación de lenguajes soportados.

-  `grpc-gateway <https://github.com/grpc-ecosystem/grpc-gateway>`__ es un *plugin* de protoc. Lee las definiciones de servicio de gRPC, y genera un servidor reverse-proxy server que traduce una API RESTful JSON a gRPC. Se utiliza para mapear gRPC a HTTP.

Puntos finales (endpoints)
--------------------------

Puntos finales por defecto:

+-----------+-------------------------+------------+
| API       | URL                     | Protocol   |
+===========+=========================+============+
| gRPC      | http://localhost:8684   | Protobuf   |
+-----------+-------------------------+------------+
| RESTful   | http://localhost:8685   | HTTP       |
+-----------+-------------------------+------------+

API gRPC
''''''''

Podemos correr el ejemplo para gRPC, `el cliente de pruebas <https://github.com/nebulasio/go-nebulas/blob/develop/rpc/testing/client/main.go>`__:

.. code:: text

    go run main.go

El cliente de pruebas obtiene los estados de cuentas de la dirección del emisor, crea una transacción de emisor a receptor, y chequea el estado de cuentas de la dirección del receptor.

La salida del registro podría verse así:

.. code:: text

    GetAccountState n1QZMXSZtW7BUerroSms4axNfyBGyFGkrh5 nonce 4 value 3142831039999999999992
    SendTransaction n1QZMXSZtW7BUerroSms4axNfyBGyFGkrh5 -> n1Zn6iyyQRhqthmCfqGBzWfip1Wx8wEvtrJ value 2 txhash:"2c2f5404a2e2edb651dff44a2d114a198c00614b20801e58d5b00899c8f512ae"
    GetAccountState n1Zn6iyyQRhqthmCfqGBzWfip1Wx8wEvtrJ nonce 0 value 10

HTTP
''''

También ofrecemos HTTP para acceder a la API RPC desde el navegador. El archivo de mapeo tiene la extensión **gw.go**.

Es posible actualizar los métodos **rpc\_listen** y **http\_listen** en **conf/default/config.conf** para cambiar el puerto RPC/HTTP.

Ejemplo

.. code:: text

    curl -i -H 'Content-Type: application/json' -X GET http://localhost:8685/v1/user/nebstate

Si todo marcha bien, la respuesta debería verse así:

.. code:: text

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

Si ocurre un error de grpc, la respuesta se verá así:

.. code:: text

    {
        "error":"message..."
    }

RPC methods
-----------

-  `GetNebState <#getnebstate>`__
-  `GetAccountState <#getaccountstate>`__
-  `LatestIrreversibleBlock <#latestirreversibleblock>`__
-  `Call <#call>`__
-  `SendRawTransaction <#sendrawtransaction>`__
-  `GetBlockByHash <#getblockbyhash>`__
-  `GetBlockByHeight <#getblockbyheight>`__
-  `GetTransactionReceipt <#gettransactionreceipt>`__
-  `GetTransactionByContract <#gettransactionbycontract>`__
-  `GetGasPrice <#getgasprice>`__
-  `EstimateGas <#estimategas>`__
-  `GetEventsByHash <#geteventsbyhash>`__
-  `Subscribe <#subscribe>`__
-  `GetDynasty <#getdynasty>`__

RPC API Referencia
-------------------

GetNebState
~~~~~~~~~~~

Devuelve el estado del neb.

+------------+----------+---------------------+
| Protocol   | Method   | API                 |
+============+==========+=====================+
| gRpc       |          | GetNebState         |
+------------+----------+---------------------+
| HTTP       | GET      | /v1/user/nebstate   |
+------------+----------+---------------------+

Parámetros

ninguno

Devuelve

-  ``chain_id``: ID del blockchain
-  ``tail``: Hash de la cola del neb actual
-  ``lib``: Hash de la librería neb actual
-  ``height``: Altura del bloque de cola neb actual
-  ``protocol_version``: La versión del protocolo neb actual.
-  ``synchronized``: Estado de la sincronización del par.
-  ``version``: Versión neb.

Ejemplo para HTTP

.. code:: text

    // Petición
    curl -i -H 'Content-Type: application/json' -X GET http://localhost:8685/v1/user/nebstate

    // Resultado
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

--------------

GetAccountState
~~~~~~~~~~~~~~~

Devuelve el estado de la cuenta. Se devuelve el balance y el nonce de la dirección dada.

+------------+----------+-------------------------+
| Protocol   | Method   | API                     |
+============+==========+=========================+
| gRpc       |          | GetAccountState         |
+------------+----------+-------------------------+
| HTTP       | POST     | /v1/user/accountstate   |
+------------+----------+-------------------------+

Parámetros

-  ``address``: Cadena hexadecimal que representa la dirección de la cuenta.
-  ``height``: Estado de la cuenta bloque, con altura. Si no se especifica, utilice 0 como altura de cola.

Devuelve

-  ``balance``: Balance actual en unidades de 1/(10^18) NAS.
-  ``nonce``: Índice numérico de la transacción actual.
-  ``type``: Tipo de dirección; **87** indica direcciones normales y **88** indica direcciones de contratos inteligentes.

Ejemplo para HTTP

.. code:: text

    // Petición
    curl -i -H 'Content-Type: application/json' -X POST http://localhost:8685/v1/user/accountstate -d '{"address":"n1Z6SbjLuAEXfhX1UJvXT6BB5osWYxVg3F3"}'

    // Resultado
    {
        result {
            "balance":"9489999998980000000000"
            "nonce":51
            "type":87
        }
    }

----------------------------

LatestIrreversibleBlock
~~~~~~~~~~~~~~~~~~~~~~~

Devuelve el último bloque irreversible.

+------------+----------+---------------------------+
| Protocol   | Method   | API                       |
+============+==========+===========================+
| gRpc       |          | LatestIrreversibleBlock   |
+------------+----------+---------------------------+
| HTTP       | GET      | /v1/user/lib              |
+------------+----------+---------------------------+

Parámetros

none

Devuelve

-  ``hash``: Cadena hexadecimal que representa el hash del bloque.
-  ``parent_hash``: Cadena hexadecimal que representa el hash del bloque padre.
-  ``height``: Altura del bloque.
-  ``nonce``: Nonce del bloque.
-  ``coinbase``: Cadena hexadecimal que representa la dirección coinbase.
-  ``timestamp``: Timestamp del bloque.
-  ``chain_id``: ID del blockchain.
-  ``state_root``: Cadena hexadecimal que representa el estado raíz.
-  ``txs_root``: Cadena hexadecimal que representa la transacción raíz.
-  ``events_root``: Cadena hexadecimal que representa el evento raíz.
-  ``consensus_root``: Cadena hexadecimal que representa el consenso raíz.
-  ``Timestamp``: Timestamp del estado de consenso.
-  ``Proposer``: Proponente del estado de consenso actual.
-  ``DynastyRoot``: Cadena hexadecimal que representa la dinastía raíz.
-  ``miner``: Minero del bloque.
-  ``is_finality``: Indica si el bloque es *finality*.
-  ``transactions``: Slice de transacciones del bloque.
-  ``transaction``: Información de respuesta `GetTransactionReceipt <#gettransactionreceipt>`__.

Ejemplo para HTTP
'''''''''''''''''

.. code:: text

    // Petición
    curl -i -H 'Content-Type: application/json' -X GET http://localhost:8685/v1/user/lib

    // Resultado
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


--------------

Call
~~~~

Permite realizar una llamada a las funciones de un contrato inteligente dado. Es importante que el contrato inteligente ya esté implementado en el blockchain.

Las llamadas a métodos se realizan sobre el nodo actual, y no se transmiten al resto de la red.

+------------+----------+-----------------+
| Protocol   | Method   | API             |
+============+==========+=================+
| gRpc       |          | Call            |
+------------+----------+-----------------+
| HTTP       | POST     | /v1/user/call   |
+------------+----------+-----------------+

Parámetros

Son los mismos que se utilizan en `SendTransaction <#sendtransaction>`__, con atención especial a:

-  ``to``: Cadena hexadecimal que representa la dirección de la cuenta receptora. **El valor correcto para ``to`` es la dirección del contrato.**
-  ``contract``: Objeto ``transaction contract`` para la llamada al contrato inteligente.
-  Subpropiedades (no se requieren los parámetros ``source`` y ``sourceType``):

   -  ``function``: El nombre de la función a llamar.
   -  ``args``: Los parámetros del contrato. El contenido de ``args`` es una cadena JSON que contiene una matriz de parámetros.

Devuelve

-  ``result``: El resultado de la llamada a la función del contrato inteligente.
-  ``execute_err``: Error de ejecución (si ocurrió un error).
-  ``estimate_gas``: Estimación del gas utilizado.

Ejemplo para HTTP

.. code:: text

    // Petición
    curl -i -H 'Content-Type: application/json' -X POST http://localhost:8685/v1/user/call -d '{"from":"n1Z6SbjLuAEXfhX1UJvXT6BB5osWYxVg3F3","to":"n1mL2WCZyRi1oELEugfCZoNAW3dt8QpHtJw","value":"0","nonce":3,"gasPrice":"20000000000","gasLimit":"2000000","contract":{"function":"transferValue","args":"[500]"}}'

    // Resultado
    {
       "result": "0",
       "execute_err": "insufficient balance",
       estimate_gas: "22208"
    }

--------------

SendRawTransaction
~~~~~~~~~~~~~~~~~~

Permite enviar una transacción firmada. El valor de la transacción firmada se debe devolver por medio de `SignTransactionWithPassphrase <rpc_admin.md#signtransactionwithpassphrase>`__.

+------------+----------+---------------------------+
| Protocol   | Method   | API                       |
+============+==========+===========================+
| gRpc       |          | SendRawTransaction        |
+------------+----------+---------------------------+
| HTTP       | POST     | /v1/user/rawtransaction   |
+------------+----------+---------------------------+

Parámetros

-  ``data``: Datos de la transacción, firmados.

Devuelve

-  ``txhash``: Cadena hexadecimal que representa el hash de la transacción.
-  ``contract_address``: Se devueve únicamente cuando se utiliza una transacción de contrato inteligente.

Ejemplo para HTTP

.. code:: text

    // Petición
    curl -i -H 'Content-Type: application/json' -X POST http://localhost:8685/v1/user/rawtransaction -d '{"data":"CiCrHtxyyIJks2/RErvBBA862D6iwAaGQ9OK1NisSGAuTBIYGiY1R9Fnx0z0uPkWbPokTeBIHFFKRaosGhgzPLPtjEF5cYRTgu3jz2egqWJwwF/i9wAiEAAAAAAAAAAADeC2s6dkAAAoAjDd/5jSBToICgZiaW5hcnlAZEoQAAAAAAAAAAAAAAAAAA9CQFIQAAAAAAAAAAAAAAAAAABOIFgBYkGLnnvGZEDSlocc202ZRWtUlbl2RHfGNdBY5eajFiHKThfgXIwGixh17LpnZGnYHlmfiGe2zqnFHdj7G8b2XIP2AQ=="}'

    // Resultado
    {
        "result":{
            "txhash": "f37acdf93004f7a3d72f1b7f6e56e70a066182d85c186777a2ad3746b01c3b52"
        }
    }

Ejemplo para utilización de contrato

.. code:: text

    // Petición
    curl -i -H 'Content-Type: application/json' -X POST http://localhost:8685/v1/user/rawtransaction -d '{"data":"CiDam3G9Sy5fV6/ZcjasYPwSF39ZJDIHNB0Us94vn6p6ohIaGVfLzJ83pom1DO1gD307f1JdTVdDLzbMXO4aGhlXy8yfN6aJtQztYA99O39SXU1XQy82zFzuIhAAAAAAAAAAAAAAAAAAAAAAKBswwfTs1QU64AcKBmRlcGxveRLVB3siU291cmNlVHlwZSI6ImpzIiwiU291cmNlIjoiJ3VzZSBzdHJpY3QnXG5cbnZhciBUcmFuc2ZlclZhbHVlQ29udHJhY3QgPSBmdW5jdGlvbiAoKSB7XG4gICAgLy8gTG9jYWxDb250cmFjdFN0b3JnZS5kZWZpbmVQcm9wZXJ0aWVzKHRoaXMsIHtcbiAgICAvLyAgICAgdG90YWxCYWxhbmNlOiBudWxsXG4gICAgLy8gfSlcbn1cblxuXG5UcmFuc2ZlclZhbHVlQ29udHJhY3QucHJvdG90eXBlID0ge1xuICAgICBpbml0OiBmdW5jdGlvbigpIHtcbiAgICAvLyAgICAgdGhpcy50b3RhbEJhbGFuY2UgPSAwO1xuICAgICB9LFxuXG4gICAgdHJhbnNmZXI6IGZ1bmN0aW9uKHRvKSB7XG4gICAgICAgIHZhciByZXN1bHQgPSBCbG9ja2NoYWluLnRyYW5zZmVyKHRvLCBCbG9ja2NoYWluLnRyYW5zYWN0aW9uLnZhbHVlKTtcbiAgICAgICAgLy8gdmFyIHJlc3VsdCA9IEJsb2NrY2hhaW4udHJhbnNmZXIodG8sIDApO1xuICAgICAgICBpZiAoIXJlc3VsdCkge1xuXHQgICAgXHR0aHJvdyBuZXcgRXJyb3IoXCJ0cmFuc2ZlciBmYWlsZWQuXCIpO1xuICAgICAgICB9XG4gICAgICAgIHJldHVybiBCbG9ja2NoYWluLnRyYW5zYWN0aW9uLnZhbHVlO1xuICAgIH0sXG4gICAgdHJhbnNmZXJTcGVjaWFsVmFsdWU6IGZ1bmN0aW9uKHRvLCB2YWx1ZSkge1xuICAgICAgICB2YXIgYW1vdW50ID0gbmV3IEJpZ051bWJlcih2YWx1ZSk7XG4gICAgICAgIHZhciByZXN1bHQgPSBCbG9ja2NoYWluLnRyYW5zZmVyKHRvLCBhbW91bnQpO1xuICAgICAgICAvLyB2YXIgcmVzdWx0ID0gQmxvY2tjaGFpbi50cmFuc2Zlcih0bywgMCk7XG4gICAgICAgIGlmICghcmVzdWx0KSB7XG4gICAgICAgICAgICB0aHJvdyBuZXcgRXJyb3IoXCJ0cmFuc2ZlciBmYWlsZWQuXCIpO1xuICAgICAgICB9IGVsc2Uge1xuICAgICAgICAgICAgcmV0dXJuIDBcbiAgICAgICAgfVxuICAgIH0sXG4gICAgXG59XG5tb2R1bGUuZXhwb3J0cyA9IFRyYW5zZmVyVmFsdWVDb250cmFjdDsifUBkShAAAAAAAAAAAAAAAAAAD0JAUhAAAAAAAAAAAAAAAAABMS0AWAFiQcJUX32jGcduxnJCjvJ9kRcGXhSK2+h3Tb46ySjAToGAY11C7mysGEU11OE6YTd+WNAo/CEbThvI0iKcjHhgBZUB"}'

    // Resultado
    {
        "result":{
            "txhash": "f37acdf93004f7a3d72f1b7f6e56e70a066182d85c186777a2ad3746b01c3b52",
            "contract_address":"4702b597eebb7a368ac4adbb388e5084b508af582dadde47"
        }
    }

--------------

GetBlockByHash
~~~~~~~~~~~~~~

Obtiene información del encabezado de un bloque por medio de su hash.

+------------+----------+---------------------------+
| Protocol   | Method   | API                       |
+============+==========+===========================+
| gRpc       |          | GetBlockByHash            |
+------------+----------+---------------------------+
| HTTP       | POST     | /v1/user/getBlockByHash   |
+------------+----------+---------------------------+

Parámetros


-  ``hash``: Cadena hexadecimal que representa el hash de la transacción.
-  ``full_fill_transaction``: Si su valor es ``true``, devuelve un objeto de transacción completo; si es ``false``, sólo sus hashes.

Devuelve


Véase ```LatestIrreversibleBlock`` <./#latestirreversibleblock>`__.

Ejemplo para HTTP


.. code:: text

    // Petición
    curl -i -H 'Content-Type: application/json' -X POST http://localhost:8685/v1/user/getBlockByHash -d '{"hash":"00000658397a90df6459b8e7e63ad3f4ce8f0a40b8803ff2f29c611b2e0190b8", "full_fill_transaction":"true"}'

    // Resultado
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

--------------

GetBlockByHeight
~~~~~~~~~~~~~~~~

Permite obtener información del encabezado de un bloque dado mediante su altura (parámetro ``height``).

+------------+----------+-----------------------------+
| Protocol   | Method   | API                         |
+============+==========+=============================+
| gRpc       |          | GetBlockByHeight            |
+------------+----------+-----------------------------+
| HTTP       | POST     | /v1/user/getBlockByHeight   |
+------------+----------+-----------------------------+

Parámetros


-  ``height``: Altura del hash de la transacción.
-  ``full_fill_transaction``: Si su valor es ``true``, devuelve un objeto de transacción completo; si es ``false``, sólo sus hashes.

Devuelve


Véase `LatestIrreversibleBlock <./#latestirreversibleblock>`__.

Ejemplo para HTTP


.. code:: text

    // Petición
    curl -i -H 'Content-Type: application/json' -X POST http://localhost:8685/v1/user/getBlockByHeight -d '{"height": 256, "full_fill_transaction": true}'

    // Resultado
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

--------------

GetTransactionReceipt
~~~~~~~~~~~~~~~~~~~~~

Obtiene información del recibo de una transacción mediante su hash.

Si la transacción no existe o no se ha empaquetado todavía en el blockchain, devolverá un error ``not found``.

+------------+----------+----------------------------------+
| Protocol   | Method   | API                              |
+============+==========+==================================+
| gRpc       |          | GetTransactionReceipt            |
+------------+----------+----------------------------------+
| HTTP       | POST     | /v1/user/getTransactionReceipt   |
+------------+----------+----------------------------------+

Parámetros


``hash`` Hex string of transaction hash.

Devuelve


-  ``hash``: Cadena hexadecimal que representa el hash de la transacción.
-  ``chainId``: ID del *chain* de la transacción.
-  ``from``: Cadena hexadecimal que representa la dirección de la cuenta del emisor.
-  ``to``: Cadena hexadecimal que representa la dirección de la cuenta del receptor.
-  ``value``: Valor de la transacción.
-  ``nonce``: Nonce de la transacción.
-  ``timestamp``: Timestamp de la transacción.
-  ``type``: Tipo de transacción.
-  ``data``: Datos del *payload* de la transacción.
-  ``gas_price``: Precio del gas utilizado por la transacción.
-  ``gas_limit``: Límite del gas utilizado por la transacción.
-  ``gas_used``: Gas utilizado por la transacción.
-  ``contract_address``: Dirección del contrato de la transacción.
-  ``status``: Estado de la transacción:

   -  0: falló.
   -  1: se ejecutó sin errores.
   -  2: pendiente.

Ejemplo para HTTP


.. code:: text

    // Petición
    curl -i -H 'Content-Type: application/json' -X POST http://localhost:8685/v1/user/getTransactionReceipt -d '{"hash":"cda54445ffccf4ea17f043e86e54be11b002053f9edbe30ae1fbc0437c2b6a73"}'

    // Resultado
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
            "gas_used":"20000"
        }
    }

--------------

GetTransactionByContract
~~~~~~~~~~~~~~~~~~~~~~~~

Obtiene información de la transacción mediante una dirección de contrato. Si ese contrato no existe o no está empaquetado en el blockchain, se devuelve un error ``not found``.

+------------+----------+-------------------------------------+
| Protocol   | Method   | API                                 |
+============+==========+=====================================+
| gRpc       |          | GetTransactionByContract            |
+------------+----------+-------------------------------------+
| HTTP       | POST     | /v1/user/getTransactionByContract   |
+------------+----------+-------------------------------------+

Parámetros


-  ``address``: Cadena hexadecimal que representa la dirección de la cuenta del contrato.

Devuelve


-  El resultado es el mismo que se obtiene mediante el método `GetTransactionReceipt <#gettransactionreceipt>`__.

Ejemplo para HTTP


.. code:: text

    // Petición
    curl -i -H 'Content-Type: application/json' -X POST http://localhost:8685/v1/user/getTransactionByContract -d '{"address":"n1sqDHGjYtX6rMqFoq5Tow3s3LqF4ZxBvE3"}'

    // Resultado
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

--------------

Subscribe
~~~~~~~~~

Permite suscribirse a tópicos (eventos) generados por transacciones y bloques. La petición se hace mediante una conexión *keep-alive*.

+------------+----------+----------------------+
| Protocol   | Method   | API                  |
+============+==========+======================+
| gRpc       |          | Subscribe            |
+------------+----------+----------------------+
| HTTP       | POST     | /v1/user/subscribe   |
+------------+----------+----------------------+

Parámetros


-  ``topics``: Matriz de cadenas con los nombres de los tópicos (eventos) a los cuales nos suscribiremos. Estos pueden ser:

   -  ``chain.pendingTransaction``: Transacciones pendientes en ``transaction_pool``.
   -  ``chain.latestIrreversibleBlock``: Actualización del último bloque irreversible.
   -  ``chain.transactionResult``: Ejecución y envío de una transacción.
   -  ``chain.newTailBlock``: Definición de un nuevo ``tail block``.
   -  ``chain.revertBlock``: Reversión de un bloque.

Devuelve


-  ``topic``: Nombre del evento o eventos suscritos.
-  ``data``: Datos del evento o eventos suscritos.

Ejemplo para HTTP


.. code:: text

    // Petición
    curl -i -H 'Content-Type: application/json' -X POST http://localhost:8685/v1/user/subscribe -d '{"topics":["chain.linkBlock", "chain.pendingTransaction"]}'

    // Resultado
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

--------------

GetGasPrice
~~~~~~~~~~~

Obtiene el valor actual del gas.

+------------+----------+------------------------+
| Protocol   | Method   | API                    |
+============+==========+========================+
| gRpc       |          | GetGasPrice            |
+------------+----------+------------------------+
| HTTP       | GET      | /v1/user/getGasPrice   |
+------------+----------+------------------------+

Parámetros


ninguno.

Devuelve


-  ``gas_price``: valor del gas al momento de la petición. La unidad es 10^-18 NAS.

Ejemplo para HTTP


.. code:: javascript

    // Petición
    curl -i -H 'Content-Type: application/json' -X GET http://localhost:8685/v1/user/getGasPrice

    // Resultado
    {
        "result":{
            "gas_price":"1000000"
        }
    }

--------------

EstimateGas
~~~~~~~~~~~

Obtiene un estimado de la cantidad de gas necesario para una transacción dada.

+------------+----------+------------------------+
| Protocol   | Method   | API                    |
+============+==========+========================+
| gRpc       |          | EstimateGas            |
+------------+----------+------------------------+
| HTTP       | POST     | /v1/user/estimateGas   |
+------------+----------+------------------------+

Parámetros


-  Se usan los mismos parámetros que en el método `SendTransaction <#sendtransaction>`__.

Devuelve


-  ``gas``: estimado de la cantidad de gas necesario para la transacción.
-  ``err``: mensaje de error correspondiente a la transacción que se ejecutará.

Ejemplo para HTTP


.. code:: text

    // Petición
    curl -i -H 'Content-Type: application/json' -X POST http://localhost:8685/v1/user/estimateGas -d '{"from":"n1QZMXSZtW7BUerroSms4axNfyBGyFGkrh5","to":"n1SAeQRVn33bamxN4ehWUT7JGdxipwn8b17", "value":"1000000000000000000","nonce":1,"gasPrice":"20000000000","gasLimit":"2000000"}'

    // Resultado
    {
        "gas":"20000",
        "err":""
    }

--------------

GetEventsByHash
~~~~~~~~~~~~~~~

Devuelve la lista de eventos de una transacción identificada mediante su hash.

+------------+----------+----------------------------+
| Protocol   | Method   | API                        |
+============+==========+============================+
| gRpc       |          | GetEventsByHash            |
+------------+----------+----------------------------+
| HTTP       | POST     | /v1/user/getEventsByHash   |
+------------+----------+----------------------------+

Parámetros


-  ``hash``: Cadena hexadecimal que representa el hash de la transacción.

Devuelve


-  ``events``: Lista de eventos, conteniendo, cada uno de ellos:

   -  ``topic``: Tópico del evento.
   -  ``data``: Datos del evento.

Ejemplo para HTTP


.. code:: text

    // Petición
    curl -i -H 'Content-Type: application/json' -X POST http://localhost:8685/v1/user/getEventsByHash -d '{"hash":"ec239d532249f84f158ef8ec9262e1d3d439709ebf4dd5f7c1036b26c6fe8073"}'

    // Resultado
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

--------------

GetDynasty
~~~~~~~~~~

Obtiene una lista con las direcciones de los mineros de la dinastía actual.

+------------+----------+--------------------+
| Protocol   | Method   | API                |
+============+==========+====================+
| gRpc       |          | GetDynasty         |
+------------+----------+--------------------+
| HTTP       | POST     | /v1/user/dynasty   |
+------------+----------+--------------------+

Parámetros


-  ``height``: altura del bloque.

Devuelve


-  ``miners``: matriz que contiene las direcciones de los mineros.

Ejemplo para HTTP


.. code:: text

    // Petición
    curl -i -H 'Content-Type: application/json' -X POST http://localhost:8685/v1/user/dynasty -d '{"height": 1}'

    // Resultado
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


.. toctree::
    :titlesonly:

    rpc_admin.md

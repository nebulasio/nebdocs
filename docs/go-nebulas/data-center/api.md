# Data Center API

> The data center provides external price and on-chain data query interfaces, and provides underlying data support for Nebulas ecosystem applications such as browsers and wallets.

## Node
Mainnet Node:

| API | URL | Protocol |
|-------|:------------:|:------------:|
| RESTful |https://data.nebulas.io | HTTP |


## API List

* [NRC20 transaction list](#nrc20-transaction-list)
* [Transaction Details](#transaction-details)
* [Transaction List](#transaction-list)
* [Off-chain (Pending) Transaction List](#off-chain-pending-transaction-list)
* [History Transaction Statistics](#history-transaction-statistics)
* [Block List](#block-list)
* [Block Details](#block-details)
* [Highest Block](#highest-block)
* [Contract List](#contract-list)
* [Token List](#token-list)
* [Token Details](#token-details)
* [Token Transaction List](#token-transaction-list)
* [Token Holders List](#token-holders-list)
* [NAS Holders List](#has-holders-list)
* [NAX Distribution History](#nax-distribution-history)
* [NAX Statistics](#nax-statistics)
* [NAX Address Distribution History](#nax-address-distribution-history)
* [Address Information](#address-information)
* [Address Statistics](#address-statistics)
* [Public Chain Statistics Information](#public-chain-statistics-information)

## API Details

### NRC20 Transaction List
> Query by address to obtain the NRC20 transaction list [paged].

| Protocol | Method | API |
|----------|--------|-----|
| HTTP | GET |  tx/nrc20 |

#### Parameters

`address` address

`page`	page number; optional parameter, default 1

`page_size` page size; optional parameter, default 20

#### Return

`txnList` transaction list

`totalPage` total page number

`maxDisplayCnt` maximum number of displays

`currentPage` current page number

`txnCnt` total

#### Example
```
//Request
curl -X GET https://data.nebulas.io/tx/nrc20?address="n1zJjyWVKr8HiL1b6dN7JGBtCZFuKQHenLG"&page=1&page_size=2

//Result
{"code":0,"msg":"success","data":{"txnList":[{"hash":"eb3c8d848679ad1786d5986da48bcc5d5732141fd6fccde0a9a3bfd78ae1d970","block":{"hash":"e188cb8f384f010cc5cfd84ed6779a96f74f7cb363383024b26502c0e85e767d","height":4841297},"from":{"hash":"n1QUMEs7qkvCDuimT2zYsbf62z9pRtofQiP"},"to":{"hash":"n1zJjyWVKr8HiL1b6dN7JGBtCZFuKQHenLG"},"status":1,"value":"346000000000000000000","nonce":27,"timestamp":1595146350000,"currentTimestamp":1595149022681,"timeDiff":2672681,"type":"call","gasPrice":"20000000000","gasLimit":"830000","gasUsed":"63615","data":"{\"Function\":\"transfer\",\"Args\":\"[\\\"n1zJjyWVKr8HiL1b6dN7JGBtCZFuKQHenLG\\\",\\\"346000000000000000000\\\"]\"}","contractAddress":"","executeError":"","tokenName":"MRH2","decimal":18,"txFee":"1272300000000000"},{"hash":"fc148958467c69d9ad1c0a5038a41b096d3c2302a604e0edecb52c7a6e9e1675","block":{"hash":"f7b9ed2a907c39a75789246ce071213d0903027ee014cece16063b397bde8e1e","height":4841292},"from":{"hash":"n1N9dDUn3AcdrjLhfCSzNxBGzfxHYTgKH1W"},"to":{"hash":"n1zJjyWVKr8HiL1b6dN7JGBtCZFuKQHenLG"},"status":1,"value":"323000000000000000000","nonce":41,"timestamp":1595146275000,"currentTimestamp":1595149022681,"timeDiff":2747681,"type":"call","gasPrice":"20000000000","gasLimit":"830000","gasUsed":"63615","data":"{\"Function\":\"transfer\",\"Args\":\"[\\\"n1zJjyWVKr8HiL1b6dN7JGBtCZFuKQHenLG\\\",\\\"323000000000000000000\\\"]\"}","contractAddress":"","executeError":"","tokenName":"MRH2","decimal":18,"txFee":"1272300000000000"}],"totalPage":243,"maxDisplayCnt":500,"currentPage":1,"txnCnt":485}}

```
***

### Transaction Details
> Query transaction details by address.

| Protocol | Method | API |
|----------|--------|-----|
| HTTP | GET |  tx/detail |

#### Parameters

`hash` transaction hash

#### Return

`tx_hash` transaction hash

`address_main` transaction origination address

`address_supporting` transaction acceptance address

`direction` transaction acceptance address

`tx_type` transaction type

`timestamp` timestamp

`block_timestamp` transaction timestamp

`contract_address` contract address

`tx_value` transaction amount

`real_value` amount

`gas_price` transaction fee price

`gas_limit` transaction fee limit

`gas_used` transaction usage

`statue` transaction status: 0 failed, 1 successful, 2 pending

`block_height` transaction block height

`is_nrc20` if it's a NRC20 transaction

#### Example
```
//Request
curl -X GET https://data.nebulas.io/tx/detail?hash="eb3c8d848679ad1786d5986da48bcc5d5732141fd6fccde0a9a3bfd78ae1d970"

//Result
{"code":0,"msg":"success","data":{"ext_info":{"data":"eyJGdW5jdGlvbiI6InRyYW5zZmVyIiwiQXJncyI6IltcIm4xekpqeVdWS3I4SGlMMWI2ZE43SkdCdENaRnVLUUhlbkxHXCIsXCIzNDYwMDAwMDAwMDAwMDAwMDAwMDBcIl0ifQ==","execute_error":"","execute_result":"\"\""},"tx_hash":"eb3c8d848679ad1786d5986da48bcc5d5732141fd6fccde0a9a3bfd78ae1d970","address_main":"n1QUMEs7qkvCDuimT2zYsbf62z9pRtofQiP","address_supporting":"n1zJjyWVKr8HiL1b6dN7JGBtCZFuKQHenLG","direction":"send","tx_type":"call","timestamp":1595146335,"block_timestamp":1595146350,"contract_address":"n1pxpisjJMsnVLrqHBnS52o8wsuMUsycfDV","tx_value":"0","real_value":"346000000000000000000","gas_price":"20000000000","gas_limit":830000,"gas_used":63615,"status":1,"block_height":4841297,"is_nrc20":true}}
```
***

### Transaction List
> Query the transaction list.

| Protocol | Method | API |
|----------|--------|-----|
| HTTP | GET |  tx/list |

#### Parameters

`block_height` block height, optional parameter

`page`	page number; optional parameter, default 1

`page_size` page size; optional parameter, default 20

#### Return

`next` next page request URL

`count` transactiontotal

`total_page` total number of pages

`current_page` current page number

`list` transaction list

`server_timestamp` Timestamp

#### Example
```
//Request
curl -X GET https://data.nebulas.io/tx/list?page=1&page_size=2


//Result
{"code":0,"msg":"success","data":{"next":"http://data.nebulas.io/tx/list?page=2&page_size=2","previous":null,"count":13352569,"total_page":6676285,"current_page":1,"list":[{"tx_hash":"b3805aaf308fea5fc975e5b87632dbd27c7795c3b2bcc03b69263f8301bfcc6a","chainId":1,"address_from":"n1Tx1bxFsgyTcjQ82h59kMQ3XbZGXSiq8w2","address_to":"n1KxWR8ycXg7Kb9CPTtNjTTEpvka269PniB","value":"586003179640000000000","nonce":7181,"timestamp":1595152078,"block_timestamp":1595157150,"block_date":"20200719","tx_type":"binary","gas_price":"20000000000","gas_limit":200000,"gas_used":20000,"contract_address":"","status":1,"execute_error":"","execute_result":"","block_height":4842017},{"tx_hash":"a920a35b4450d1cb7395b96a6b4f3ae3a616a6f0a129f6586efb6f6b2f71c13f","chainId":1,"address_from":"n1EoNsJNXG1tN3z9rvjwPKoBXbJMqAjmESC","address_to":"n1gtEoDBNnPrAHzRY9tSnpSSg3QppLYXUdR","value":"0","nonce":4915,"timestamp":2147483647,"block_timestamp":1595157075,"block_date":"20200719","tx_type":"call","gas_price":"20000000000","gas_limit":9000000,"gas_used":989604,"contract_address":"","status":1,"execute_error":"","execute_result":"{\"hasNext\":false,\"period\":298,\"start\":4836000,\"end\":4842000,\"page\":4}","block_height":4842012}],"server_timestamp":1595157487445}}
```
***

### Off-chain (Pending) Transaction List
> Quary the off-chain transaction list.

| Protocol | Method | API |
|----------|--------|-----|
| HTTP | GET |  tx/list/pending |

#### Parameters

`page`	page number; optional parameter, default 1

`page_size` page size; optional parameter, default 20

#### Return

`next` next page requestURL

`count` transactiontotal

`total_page` total number of pages

`current_page` current page number

`list` transaction list

`server_timestamp` Timestamp

#### Example
```
//Request
curl -X GET https://data.nebulas.io/tx/list/pending?page=1&page_size=2


//Result
{"code":0,"msg":"success","data":{"next":"http://data.nebulas.io/tx/list/pending?page=2&page_size=2","previous":null,"count":156,"total_page":78,"current_page":1,"list":[{"tx_hash":"4138b43575e419a534a878be45dd8e43f4ddb61ac122ed82a8d5050fde156f7b","chainId":1,"address_from":"n1Gfg8uqtFsvGKZn6XtVmEb116ZtagRHDoe","address_to":"n22CMMXaxkAjjbsWtVXSmFJgEsnVZ3UwUWf","value":"0","nonce":2818,"timestamp":1595154990,"block_timestamp":null,"block_date":null,"tx_type":"pod","gas_price":"1000000000000","gas_limit":50000000000,"gas_used":null,"contract_address":null,"status":2,"execute_error":"","execute_result":"","block_height":null},{"tx_hash":"9a6a2fa7c02c18a0f61c96cf4b545da6e41bf5e64a8641062e66c81134b924f8","chainId":1,"address_from":"n1bc9szRj57bD4HqM5672v92gnBvPayacJE","address_to":"n22CMMXaxkAjjbsWtVXSmFJgEsnVZ3UwUWf","value":"0","nonce":2894,"timestamp":1595154990,"block_timestamp":null,"block_date":null,"tx_type":"pod","gas_price":"1000000000000","gas_limit":50000000000,"gas_used":null,"contract_address":null,"status":2,"execute_error":"","execute_result":"","block_height":null}],"server_timestamp":1595157812578}}
```
***

### Query the Transaction List by Addresses
> Query the transaction list by addresses

| Protocol | Method | API |
|----------|--------|-----|
| HTTP | GET |  tx/listByAddress |

#### Parameters

`address` address

`is_nrc20` If it's a NRC20 transaction

`page`	page number; optional parameter, default 1

`page_size` page size; optional parameter, default 20

#### Return

`next` next page requestURL

`count` transactiontotal

`total_page` total number of pages

`current_page` current page number

`list` transaction list

`server_timestamp` Timestamp

#### Example
```
//Request
curl -X GET https://data.nebulas.io/tx/listByAddress?address=n1Gfg8uqtFsvGKZn6XtVmEb116ZtagRHDoe&page=1&page_size=2


//Result
{"code":0,"msg":"success","data":{"next":"http://data.nebulas.io/tx/listByAddress?address=n1Gfg8uqtFsvGKZn6XtVmEb116ZtagRHDoe&page=2&page_size=2","previous":null,"count":2829,"total_page":1415,"current_page":1,"list":[{"tx_hash":"9e862c72614aba4138e8e1fcbf44ef0a8f2007520015e899bf75da3d9136049b","chainId":1,"address_from":"n1Gfg8uqtFsvGKZn6XtVmEb116ZtagRHDoe","address_to":"n22CMMXaxkAjjbsWtVXSmFJgEsnVZ3UwUWf","value":"0","nonce":2818,"timestamp":1595154930,"block_timestamp":1595154990,"block_date":"20200719","tx_type":"pod","gas_price":"1000000000000","gas_limit":50000000000,"gas_used":150885,"contract_address":"","status":1,"execute_error":"","execute_result":"\"\"","block_height":4841873},{"tx_hash":"4138b43575e419a534a878be45dd8e43f4ddb61ac122ed82a8d5050fde156f7b","chainId":1,"address_from":"n1Gfg8uqtFsvGKZn6XtVmEb116ZtagRHDoe","address_to":"n22CMMXaxkAjjbsWtVXSmFJgEsnVZ3UwUWf","value":"0","nonce":2818,"timestamp":1595154990,"block_timestamp":null,"block_date":null,"tx_type":"pod","gas_price":"1000000000000","gas_limit":50000000000,"gas_used":null,"contract_address":null,"status":2,"execute_error":"","execute_result":"","block_height":null}],"server_timestamp":1595158063474}}
```
***

### History Transaction Statics
> History transaction statics

| Protocol | Method | API |
|----------|--------|-----|
| HTTP | GET |  tx/count/history |

#### Parameters

`days` statistical days, optional parameter, default is 15

#### Return

`price` NAS price

`date` date

`transaction_count` transaction amount

#### Example
```
//Request
curl -X GET https://data.nebulas.io/tx/count/history?days=2

//Result
{"code":0,"msg":"success","data":[{"price":0.4775322101523956,"date":"20200718","transaction_count":2354},{"price":0,"date":"20200717","transaction_count":2231}]}
```
***

### Current Transaction Statics
> Current (24 hours) transaction statics

| Protocol | Method | API |
|----------|--------|-----|
| HTTP | GET |  tx/count/today |

#### Parameters

None

#### Return

`count` transaction数

#### Example
```
//Request
curl -X GET https://data.nebulas.io/tx/count/today

//Result
{"code":0,"msg":"success","data":{"count":1074}}
```
***

### Block List
> Block list.

| Protocol | Method | API |
|----------|--------|-----|
| HTTP | GET |  block/list |

#### Parameters

`page`	page number; optional parameter, default 1

`page_size` page size; optional parameter, default 20

#### Return

`next` next page requestURL

`count` transactiontotal

`total_page` total number of pages

`current_page` current page number

`list` block list

#### Example
```
//Request
curl -X GET https://data.nebulas.io/block/list?page=1&page_size=2

//Result
{"code":0,"msg":"success","data":{"next":"http://data.nebulas.io/block/list?page=2&page_size=2","previous":null,"count":4842132,"total_page":2421066,"current_page":1,"list":[{"block_hash":"c7e7862a725acb12c599cb1bbf5dd2e9781a4edffb4d850d2ad70776f0517dcc","parent_hash":"87f27c9e71d6797eb80fcb4f4ee39d87c7ea153f7124b9679851fb672e6631eb","height":4842132,"tx_count":1,"nonce":0,"coinbase":"n1HEitaphPaJG1iJHVU5AVtC7GvhHmKf1YV","timestamp":1595158875,"chain_id":1,"state_root":"5f173a48643475b17564114b761112e2c091f12916606a91f3a9d3dff9037532","txs_root":"70204ae37a62a318de69fe33505edb10a3983bc417394c97186c56d831c9a3ff","events_root":"442586640329dc93208dae2177d924bf0dc58663b12ec85caa7ef87c87012beb","miner":"n1RKTn2SXD67TvBaf55eHcFHa1AKzJcmrh2","randomSeed":"ac841e144b2cc1725bcd26f0dca14697ceca2e41c2021fd4a481a7c05fdb6890","randomProof":"e4e895173487a7e69fe917a7e987dc646736f5503f0f4432505ddf1755f3e3478c9f8a395cda5b273c5db8b5554c2b0c0079283d4b51c12ce925b4ae1d0a3cc1045a45d40d9e575f4f8acb20f276fec1023cc0989feabab87d76a7d52814dbced2bb5712630c0c43e4cacdba54bd3f6bd0414a15346b82ba59636c49f0cfece864","is_finality":false},{"block_hash":"87f27c9e71d6797eb80fcb4f4ee39d87c7ea153f7124b9679851fb672e6631eb","parent_hash":"d7e6b5705addf45c5e33f2953ebb894be22fe6b6f4412abfe5e548aa22c0ad8b","height":4842131,"tx_count":1,"nonce":0,"coinbase":"n1bSo5dMmVejyTnD9fBKsWmufrsTnoMUJVJ","timestamp":1595158860,"chain_id":1,"state_root":"bd9d19401e797fdfcff2cca354d7b7f1db0b4aad08407242dab9facdb1334aef","txs_root":"e120e7f50f08a2a1773e1782668c3d9f4312c5fffb1317f22fd6ec37b9a47161","events_root":"494985b7def3ac8c18f96ecf0464e966fe5a11f948be435a48870a5e0eb1d867","miner":"n1QyuEwxQED86Wq1ADMCTFjCay4BUnJDBGX","randomSeed":"a8866802b63d386edfc1ee009fa2d8e73163192382e27857cd087aa2afa59c46","randomProof":"2a2f3d701f0aa89695a7a69f9d48c7fc136cb8cf6cde2ab51187f8da8f72f90457ed8cc8530b040cdb4d79736f46dfdb4ce1a3a5e8fcbfe1df74c42e0b8d23b30474230844ec470f97014f8d0064fb5b077cab04afaeba5cb5fed42b3401fbe77abd395ac8489f128cf46bd96ffdc58e9f760effc6baadba0239bb7f0bebc9458c","is_finality":false}]}}
```
***

### Block Details
> Query the block details

| Protocol | Method | API |
|----------|--------|-----|
| HTTP | GET |  block/detail |

#### Parameters

`h`	block hash or height

#### Return

`gas_info` gas infomation

`dynasty` miner address list

`node` miner node information

`block_hash` block hash

`parent_hash` parent block hash

`height` block height

`tx_count` transaction amount

`nonce` nonce

`coinbase` coinbase address

`timestamp` Timestamp

`chain_id` Chain ID

`miner` miner address

`is_finality` If it's finality

#### Example
```
//Request
curl -X GET https://data.nebulas.io/block/detail?h=4820298

//Result
{"code":0,"msg":"success","data":{"gas_info":{"gas_limit":"0","gas_reward":"0","avg_gas_price":"0"},"dynasty":["n1FfapZbhjFb2Lt9vxuSW7odjEPh8DeYsFG","n1FoHjHE3rMGsYEEPgadvrHeuiVmvkL44p3","n1HpAFoBMnhMRSJ4jXECWbhAVbx1fzh1Pcj","n1Jkdiq1H1HSXYJXtvDDkYm84Tmapo4hhMv","n1NSK8TiFQRGXzxcc5jmTCuCLbnCejUcw8s","n1Qm4ZaRf7HnQNVhg13BxjprVW42vAi8dYY","n1QyuEwxQED86Wq1ADMCTFjCay4BUnJDBGX","n1S7CvsNWbPtaxH8pMqSby8hW6EkHExM59C","n1Sg1yFp4fGFLtvmxHXBrcATSBWwNJfUyBc","n1SqTLCGr56ZpT58Gj3wLfKVmQ8GXvr9ZYG","n1TURfNzQzT3aG4WDDebhrC1VJ6VbXvh2ZC","n1VwmLzc8VGbLa6o49WVA4fctJszVo3JPgQ","n1YRxpRaKikNPJRUScnWaqM9zKEPbsbGsgD","n1ZciM6PYAkEjsXUfHCwYwW9gNWqU9AnXax","n1bYLRcBtA5McKxbdqAXWMiDygjGnFpMrH2","n1baJw6b6LhoTz1ARaLBhoESuV5wEMuLmCf","n1bc9szRj57bD4HqM5672v92gnBvPayacJE","n1cbzD8Y1Pgw94eojoKd7nGQdA1JtML4daT","n1d2RCScDDxVA7PgJNT1N1kuue7raT9KMtG","n1dHKZTnMA2hmocLD35UfqG6kwDQ3Uq92nS","n1dXgssVuGTGPCnfG3eh2huEpmP542GybSd"],"node":{"period":23001,"info":{"avatar":"vko6lukmtl.png","email":"support@keysinteractive.com","id":"keys01","income-distribute":"We will distribute 80% of the block rewards back to our voters.\n\nPayout frequency is weekly (every 168 polling cycle).","name":"Keys | Share 80%","server":"Memory: 32GB of RAM\nStorage: 1TB SSD\nCPU: 64-bit\nProcessor: 8 cores (3.2 GHz each)\nNetwork: 1 Gbps","summary":"We provide staking/validation services for Proof of Stake networks for more than a year. \n\nOur primary goal is to ensure stable and secure block production by maintaining high-performance hardware and the optimal infrastructure.","telegram":null,"twitter":"keys_tech","website":null,"wechat":null}},"block_hash":"88131b5416e2b564fe478317bf842110f10f27ab654f03a33a6be2b222149afc","parent_hash":"b33862a67bfc69ec2ee790748d32104b7ac863a2e891659e3e8bf0e90b3700b3","height":4820298,"tx_count":0,"nonce":0,"coinbase":"n1VNg5f4E3KW9sjif2BeQ8dshdK9wgKwwQv","timestamp":1594831260,"chain_id":1,"miner":"n1Sg1yFp4fGFLtvmxHXBrcATSBWwNJfUyBc","is_finality":true,"server_timestamp":1595163814094}}
```
***

### Highest Block
> Query the highest block.

| Protocol | Method | API |
|----------|--------|-----|
| HTTP | GET |  block/max |

#### Parameters

None

#### Return

`height` height

`server_timestamp` Timestamp

#### Example
```
//Request
curl -X GET https://data.nebulas.io/block/max

//Result
{"code":0,"msg":"success","data":{"height":4842503.0,"server_timestamp":1595164447597}}
```
***

### Contract List
> Query the contract list.

| Protocol | Method | API |
|----------|--------|-----|
| HTTP | GET |  contracts |

#### Parameters

`page`	page number; optional parameter, default 1

`page_size` page size; optional parameter, default 20

#### Return

`next` next page requestURL

`count` total

`total_page` total number of pages

`current_page` current page number

`list` contract list

#### Example
```
//Request
curl -X GET https://data.nebulas.io/contracts?page=1&page_size=2

//Result
{"code":0,"msg":"success","data":{"next":"http://data.nebulas.io/contracts?page=2&page_size=2","previous":null,"count":15423,"total_page":7712,"current_page":1,"list":[{"block_timestamp":1593176850000,"type":"NORMAL","address":"n1orbrZ5r7wa6C4dBmCo46ZkPtMiMaS5GcP","creator_address":"n1SD9Bp3pEDy1g8acG7qmVJu5DPWreLeqsm","deploy_tx_hash":"bff2e9c73c1c3a85ac5911614fef1892f9f35cbd253aa6eef79e905ebca2b150","block_height":4710023},{"block_timestamp":1591266855000,"type":"NORMAL","address":"n1nvqH9aE57USRPfGm33sTRPac9kRjsXiKu","creator_address":"n1QaTZhe1TABmigiGsutCJR2kdrhHNXqNeZ","deploy_tx_hash":"ce06c9e6a5b5bd0ca985df3b837f39a6bd475e45247b392629badeb9b64d3450","block_height":4582759}]}}
```
***

### token list
> QUery the token list.

| Protocol | Method | API |
|----------|--------|-----|
| HTTP | GET |  token/list |

#### Parameters

None

#### Return list

`token_name` token name

`description` token description

`contract` contractaddress

`total` total

`token_decimals` token decimals

#### Example
```
//Request
curl -X GET https://data.nebulas.io/token/list

//Result
{"code":0,"msg":"success","data":[{"token_name":"ATP","description":"ATP-DESC","contract":"n1zUNqeBPvsyrw5zxp9mKcDdLTjuaEL7s39","total":"10000000000000000000000000000","token_decimals":18},{"token_name":"WITI","description":"WITI-DESC","contract":"n1hiWG7Ce8HhTaJGzSJoAaJ9w1CJd7Do2rm","total":"200000000000000000","token_decimals":8},{"token_name":"NAT","description":"NAT","contract":"n1mpgNi6KKdSzr7i5Ma7JsG5yPY9knf9He7","total":"100000000000000000000000000000","token_decimals":18},{"token_name":"WICM","description":"WITTICISM TOKEN","contract":"n1jJHWrXMysNm7odsUed3RGddiEH1jbJXtt","total":"200000000000000000000000000","token_decimals":18},{"token_name":"NAX","description":"NAX","contract":"n1etmdwczuAUCnMMvpGasfi8kwUbb2ddvRJ","total":"10000000000000000000","token_decimals":9},{"token_name":"MRH2","description":"MRH2","contract":"n1pxpisjJMsnVLrqHBnS52o8wsuMUsycfDV","total":"1000000000000000000000000000","token_decimals":18}]}
```
***

### Token Details
> Query the token details

| Protocol | Method | API |
|----------|--------|-----|
| HTTP | GET |  token/detail |

#### Parameters

`token` token name

`contract` select one between contract address and token

#### Return

`token_name` token name

`description` token description

`contract` contract address

`total` total

`token_decimals` token decimals

#### Example
```
//Request
curl -X GET https://data.nebulas.io/token/detail?token=NAX

//Result
{"code":0,"msg":"success","data":{"token_name":"NAX","description":"NAX","contract":"n1etmdwczuAUCnMMvpGasfi8kwUbb2ddvRJ","total":"10000000000000000000","token_decimals":9}}
```
***

### Token Transaction List
> Query token dettails

| Protocol | Method | API |
|----------|--------|-----|
| HTTP | GET |  token/tx/list |

#### Parameters

`token` token name

`contract` select one between the contract address and token

`page`	page number; optional parameter, default 1

`page_size` page size; optional parameter, default 20

#### Return

`next` next page request URL

`count` total

`total_page` total number of pages

`current_page` current page number

`list` transaction list

`token` token information

#### Example
```
//Request
curl -X GET https://data.nebulas.io/token/tx/list?token=NAX&page=1&page_size=2

//Result
{"code":0,"msg":"success","data":{"next":"http://data.nebulas.io/token/tx/list?page=2&page_size=2&token=NAX","previous":null,"count":8318,"total_page":4159,"current_page":1,"list":[{"token":{"token_name":"NAX","description":"NAX","contract":"n1etmdwczuAUCnMMvpGasfi8kwUbb2ddvRJ","total":"10000000000000000000","token_decimals":9},"tx_hash":"63d2395551bc1ad95138d2efeee941573a6eb7154c8db532988f64663e1ea2b3","address_main":"n1dTbtBu98qsxLNmmZKRWmh5qMS6L2pSzKy","address_supporting":"n1aM6Bag362YnAVtjLkRWzFVjQdrZpsNmwk","direction":"send","tx_type":"call","timestamp":1595164824,"block_timestamp":1595164845,"contract_address":"n1etmdwczuAUCnMMvpGasfi8kwUbb2ddvRJ","tx_value":"0","real_value":"2486000000000","gas_price":"20000000000","gas_limit":810000,"gas_used":21105,"status":1,"block_height":4842530,"is_nrc20":true},{"token":{"token_name":"NAX","description":"NAX","contract":"n1etmdwczuAUCnMMvpGasfi8kwUbb2ddvRJ","total":"10000000000000000000","token_decimals":9},"tx_hash":"7063f2234170f43747149711bf119a03cdd8b51405212114c7c4ae9021295100","address_main":"n1dTbtBu98qsxLNmmZKRWmh5qMS6L2pSzKy","address_supporting":"n1aM6Bag362YnAVtjLkRWzFVjQdrZpsNmwk","direction":"send","tx_type":"call","timestamp":1595164477,"block_timestamp":1595164500,"contract_address":"n1etmdwczuAUCnMMvpGasfi8kwUbb2ddvRJ","tx_value":"0","real_value":"3707000000000","gas_price":"20000000000","gas_limit":810000,"gas_used":20673,"status":0,"block_height":4842507,"is_nrc20":true}],"server_timestamp":1595165282662,"token":{"token_name":"NAX","description":"NAX","contract":"n1etmdwczuAUCnMMvpGasfi8kwUbb2ddvRJ","total":"10000000000000000000","token_decimals":9}}}
```
***

### Token Holders List
> Query token holders list.

| Protocol | Method | API |
|----------|--------|-----|
| HTTP | GET |  token/holders |

#### Parameters

`token` token name

`contract` contract address，和token二选一请求

`page`	page number; optional parameter, default 1

`page_size` page size; optional parameter, default 20

#### Return

`next` next page requestURL

`count` total

`total_page` total number of pages

`current_page` current page number

`list` address list


#### Example
```
//Request
curl -X GET https://data.nebulas.io/token/holders?token=NAX&page=1&page_size=2

//Result
{"code":0,"msg":"success","data":{"next":"http://data.nebulas.io/token/holders?page=2&page_size=2&token=NAX","previous":null,"count":1399,"total_page":700,"current_page":1,"list":[{"address":"n1QbEBKhsGxJUahRQXqV88T9oi4d9ywK453","contract":"n1etmdwczuAUCnMMvpGasfi8kwUbb2ddvRJ","balance":"390084128964550818"},{"address":"n214bLrE3nREcpRewHXF7qRDWCcaxRSiUdw","contract":"n1etmdwczuAUCnMMvpGasfi8kwUbb2ddvRJ","balance":"100395915000000000"}]}}
```
***

### NAS Holders List
> Query NAS holders list。

| Protocol | Method | API |
|----------|--------|-----|
| HTTP | GET |  holders |

#### Parameters

`page`	page number; optional parameter, default 1

`page_size` page size; optional parameter, default 20

#### Return

`next` next page requestURL

`count` total

`total_page` total number of pages

`current_page` current page number

`list` address list


#### Example
```
//Request
curl -X GET https://data.nebulas.io/holders?page=1&page_size=2

//Result
{"code":0,"msg":"success","data":{"next":"http://data.nebulas.io/holders?page=2&page_size=2","previous":null,"count":339598,"total_page":400,"current_page":1,"list":[{"tx_count":12,"percentage":59.493851,"address":"n1gczhpkT54RaT4PB55CNoYbqmEQcfo4hqq","nonce":0,"type":88,"balance":"35901357730000000000000000","date_of_found":"20191009","rank":1.0},{"tx_count":88344,"percentage":20.707855,"address":"n1KxWR8ycXg7Kb9CPTtNjTTEpvka269PniB","nonce":49290,"type":87,"balance":"12496083258556280663196250","date_of_found":"20180424","rank":2.0}]}}
```
***

### NAX Distribution History
> Query NAX distribution history.

| Protocol | Method | API |
|----------|--------|-----|
| HTTP | GET |  nax/history |

#### Parameters

`page`	page number; optional parameter, default 1

`page_size` page size; optional parameter, default 20

#### Return

`next` next page requestURL

`count` total

`total_page` total number of pages

`current_page` current page number

`list` history list

#### Example
```
//Request
curl -X GET https://data.nebulas.io/nax/history?page=1&page_size=2

//Result
{"code":0,"msg":"success","data":{"next":"http://data.nebulas.io/nax/history?page=2&page_size=2","previous":null,"count":299,"total_page":150,"current_page":1,"list":[{"nas_price":0.4775322101523956,"nax_price":0.002492307692307692,"start_timestamp":"2020-07-18T10:08:00","end_timestamp":"2020-07-19T11:08:15","total_distributed_nax":"1233673083506519749","total_destroyed_nax":"1351842110126118095","total_vote_nax":"514052192214285714.2857","avg_reward_rate":0.950916,"created_at":"2020-07-18T10:12:16","stage":298,"start":4836000,"end":4842000,"estimate_nax":"7421906713080442","distributed_nax":"3425198001737814","destroyed_nax":"3996708711342628","total_supplied_nax":"1233673083506411630","pledged_nas":"27848503662499186630500000","total_supplied_nas":"60343663688079320000000000","status":1},{"nas_price":0.4541347259763038,"nax_price":0.002418367088607595,"start_timestamp":"2020-07-17T09:07:00","end_timestamp":"2020-07-18T10:08:00","total_distributed_nax":"1230247885504781935","total_destroyed_nax":"1347845401414775467","total_vote_nax":"513406552357142857.1429","avg_reward_rate":0.932828,"created_at":"2020-07-17T09:12:05","stage":297,"start":4830000,"end":4836000,"estimate_nax":"7429336049129572","distributed_nax":"3429332827592696","destroyed_nax":"4000003221536876","total_supplied_nax":"1230247885504674237","pledged_nas":"27850287662499186630500000","total_supplied_nas":"58103599263060000000000000","status":1}]}}
```
***

### NAX Static
> Query NAX static.

| Protocol | Method | API |
|----------|--------|-----|
| HTTP | GET |  nax/summary |

#### Parameters

None

#### Return

`last_distributed_nax` last distributed NAX

`total_distributed_nax` total distributed NAX

`current_pledged_nas` current dstaking nas

`current_total_nas` corrent total NAS

`list` distribution history list

#### Example
```
//Request
curl -X GET https://data.nebulas.io/nax/summary

//Result
{"code":0,"msg":"success","data":{"last_distributed_nax":"3425198001737814","total_distributed_nax":"1233673083506519749","current_pledged_nas":"27856286662499186630500000","current_total_nas":"71910268469860000000000000","estimate_nax":"7414484806367362","end_height":4848000,"list":[{"nas_price":0.4775322101523956,"nax_price":0.002492307692307692,"start_timestamp":"2020-07-18T10:08:00","end_timestamp":"2020-07-19T11:08:15","total_distributed_nax":"1233673083506519749","total_destroyed_nax":"1351842110126118095","total_vote_nax":"514052192214285714.2857","avg_reward_rate":0.950916,"created_at":"2020-07-18T10:12:16","stage":298,"start":4836000,"end":4842000,"estimate_nax":"7421906713080442","distributed_nax":"3425198001737814","destroyed_nax":"3996708711342628","total_supplied_nax":"1233673083506411630","pledged_nas":"27848503662499186630500000","total_supplied_nas":"60343663688079320000000000","status":1},{"nas_price":0.4541347259763038,"nax_price":0.002418367088607595,"start_timestamp":"2020-07-17T09:07:00","end_timestamp":"2020-07-18T10:08:00","total_distributed_nax":"1230247885504781935","total_destroyed_nax":"1347845401414775467","total_vote_nax":"513406552357142857.1429","avg_reward_rate":0.932828,"created_at":"2020-07-17T09:12:05","stage":297,"start":4830000,"end":4836000,"estimate_nax":"7429336049129572","distributed_nax":"3429332827592696","destroyed_nax":"4000003221536876","total_supplied_nax":"1230247885504674237","pledged_nas":"27850287662499186630500000","total_supplied_nas":"58103599263060000000000000","status":1},{"nas_price":0.46206315262654346,"nax_price":0.0023882391304347827,"start_timestamp":"2020-07-16T08:06:30","end_timestamp":"2020-07-17T09:07:00","total_distributed_nax":"1226818552677189239","total_destroyed_nax":"1343845398193238591","total_vote_nax":"511429083862068965.5172","avg_reward_rate":0.999776,"created_at":"2020-07-16T08:09:31","stage":296,"start":4824000,"end":4830000,"estimate_nax":"7436772821951523","distributed_nax":"3435182528484704","destroyed_nax":"4001590293466819","total_supplied_nax":"1226818552677081958","pledged_nas":"27865941662499186630500000","total_supplied_nas":"60326540408079320000000000","status":1},{"nas_price":0.49049006922562666,"nax_price":0.0026480666666666665,"start_timestamp":"2020-07-15T07:06:30","end_timestamp":"2020-07-16T08:06:30","total_distributed_nax":"1223383370148704535","total_destroyed_nax":"1339843807899771772","total_vote_nax":"507907569000000000.0000","avg_reward_rate":0.930706,"created_at":"2020-07-15T07:10:30","stage":295,"start":4818000,"end":4824000,"estimate_nax":"7444217038990514","distributed_nax":"3475204652507346","destroyed_nax":"3969012386483168","total_supplied_nax":"1223383370148597666","pledged_nas":"28158410662499186630500000","total_supplied_nas":"58080768183060000000000000","status":1},{"nas_price":0.4789442682472688,"nax_price":0.002679741935483871,"start_timestamp":"2020-07-14T06:06:30","end_timestamp":"2020-07-15T07:06:30","total_distributed_nax":"1219908165496197189","total_destroyed_nax":"1335874795513288604","total_vote_nax":"505217907310344827.5862","avg_reward_rate":0.935081,"created_at":"2020-07-14T06:10:31","stage":294,"start":4812000,"end":4818000,"estimate_nax":"7451668707698212","distributed_nax":"3470160088219490","destroyed_nax":"3981508619478722","total_supplied_nax":"1219908165496090731","pledged_nas":"28085431662499186630500000","total_supplied_nas":"60309417128079320000000000","status":1},{"nas_price":0.48084590963152873,"nax_price":0.002551816091954023,"start_timestamp":"2020-07-13T05:06:30","end_timestamp":"2020-07-14T06:06:30","total_distributed_nax":"1216438005407977699","total_destroyed_nax":"1331893286893809882","total_vote_nax":"498465746892857142.8571","avg_reward_rate":0.964755,"created_at":"2020-07-13T05:11:31","stage":293,"start":4806000,"end":4812000,"estimate_nax":"7459127835533746","distributed_nax":"3485080668820116","destroyed_nax":"3974047166713630","total_supplied_nax":"1216438005407871672","pledged_nas":"28173983662499186630500000","total_supplied_nas":"60300855488079320000000000","status":1},{"nas_price":0.48637544312914666,"nax_price":0.00253512,"start_timestamp":"2020-07-12T04:04:15","end_timestamp":"2020-07-13T05:06:30","total_distributed_nax":"1212952924739157583","total_destroyed_nax":"1327919239727096252","total_vote_nax":"493814339137931034.4828","avg_reward_rate":1.025423,"created_at":"2020-07-12T04:08:16","stage":292,"start":4800000,"end":4806000,"estimate_nax":"7466594429963709","distributed_nax":"3477434441786175","destroyed_nax":"3989159988177534","total_supplied_nax":"1212952924739051971","pledged_nas":"28080070662499186630500000","total_supplied_nas":"60292293848079320000000000","status":1},{"nas_price":0.4700681555242065,"nax_price":0.002359413043478261,"start_timestamp":"2020-07-11T03:04:15","end_timestamp":"2020-07-12T04:04:15","total_distributed_nax":"1209475490297371408","total_destroyed_nax":"1323930079738918718","total_vote_nax":"490152026857142857.1429","avg_reward_rate":1.037342,"created_at":"2020-07-11T03:09:15","stage":291,"start":4794000,"end":4800000,"estimate_nax":"7474068498462171","distributed_nax":"3487928053271783","destroyed_nax":"3986140445190388","total_supplied_nax":"1209475490297266225","pledged_nas":"28132645662499186630500000","total_supplied_nas":"60283732208079320000000000","status":1},{"nas_price":0.4345778883513563,"nax_price":0.0022704367816091956,"start_timestamp":"2020-07-10T02:04:15","end_timestamp":"2020-07-11T03:04:15","total_distributed_nax":"1205987562244099625","total_destroyed_nax":"1319943939293728330","total_vote_nax":"487198695321428571.4286","avg_reward_rate":1.011173,"created_at":"2020-07-10T02:09:16","stage":290,"start":4788000,"end":4794000,"estimate_nax":"7481550048510682","distributed_nax":"3498863698327820","destroyed_nax":"3982686350182862","total_supplied_nax":"1205987562243994834","pledged_nas":"28188624662499186630500000","total_supplied_nas":"60275170568079320000000000","status":1},{"nas_price":0.42344173104069566,"nax_price":0.0022432717391304346,"start_timestamp":"2020-07-09T01:04:15","end_timestamp":"2020-07-10T02:04:15","total_distributed_nax":"1202488698545771805","total_destroyed_nax":"1315961252943545468","total_vote_nax":"486888939275862068.9655","avg_reward_rate":1.04999,"created_at":"2020-07-09T01:08:16","stage":289,"start":4782000,"end":4788000,"estimate_nax":"7489039087598280","distributed_nax":"3501787111156831","destroyed_nax":"3987251976441449","total_supplied_nax":"1202488698545667432","pledged_nas":"28179961662499186630500000","total_supplied_nas":"60266608928079320000000000","status":1},{"nas_price":0.4371542430544301,"nax_price":0.0022258924731182795,"start_timestamp":"2020-07-08T00:04:00","end_timestamp":"2020-07-09T01:04:15","total_distributed_nax":"1198986911434614974","total_destroyed_nax":"1311974000967104019","total_vote_nax":"484083464107142857.1429","avg_reward_rate":1.054589,"created_at":"2020-07-08T00:09:16","stage":288,"start":4776000,"end":4782000,"estimate_nax":"7496535623221502","distributed_nax":"3504034562399862","destroyed_nax":"3992501060821640","total_supplied_nax":"1198986911434511012","pledged_nas":"28165847662499186630500000","total_supplied_nas":"58000859403060000000000000","status":1},{"nas_price":0.4082020724468925,"nax_price":0.00200694623655914,"start_timestamp":"2020-07-06T23:04:00","end_timestamp":"2020-07-08T00:04:00","total_distributed_nax":"1195482876872215112","total_destroyed_nax":"1307981499906282379","total_vote_nax":"480995342107142857.1429","avg_reward_rate":1.09882,"created_at":"2020-07-06T23:08:05","stage":287,"start":4770000,"end":4776000,"estimate_nax":"7504039662884386","distributed_nax":"3204930295742230","destroyed_nax":"4299109367142156","total_supplied_nax":"1195482876872111553","pledged_nas":"25732193662499186630500000","total_supplied_nas":"60249485648079320000000000","status":1},{"nas_price":0.3761060705104889,"nax_price":0.0019144444444444445,"start_timestamp":"2020-07-05T22:03:15","end_timestamp":"2020-07-06T23:04:00","total_distributed_nax":"1192277946576472882","total_destroyed_nax":"1303682390539140223","total_vote_nax":"480877008448275862.0690","avg_reward_rate":1.102562,"created_at":"2020-07-05T22:08:16","stage":286,"start":4764000,"end":4770000,"estimate_nax":"7511551214098485","distributed_nax":"3327871474520931","destroyed_nax":"4183679739577554","total_supplied_nax":"1192277946576369732","pledged_nas":"26688768656599066630500000","total_supplied_nas":"60240924008079320000000000","status":1},{"nas_price":0.3670408923895109,"nax_price":0.0018900108695652175,"start_timestamp":"2020-07-04T21:02:45","end_timestamp":"2020-07-05T22:03:15","total_distributed_nax":"1188950075101951951","total_destroyed_nax":"1299498710799562669","total_vote_nax":"480231897821428571.4286","avg_reward_rate":1.049588,"created_at":"2020-07-04T21:06:46","stage":285,"start":4758000,"end":4764000,"estimate_nax":"7519070284382868","distributed_nax":"3318646097157277","destroyed_nax":"4200424187225591","total_supplied_nax":"1188950075101849212","pledged_nas":"26584389656599066630500000","total_supplied_nas":"60232362368079320000000000","status":1},{"nas_price":0.3664989544953158,"nax_price":0.001934,"start_timestamp":"2020-07-03T20:02:45","end_timestamp":"2020-07-04T21:02:45","total_distributed_nax":"1185631429004794674","total_destroyed_nax":"1295298286612337078","total_vote_nax":"478950430892857142.8571","avg_reward_rate":1.031921,"created_at":"2020-07-03T20:06:48","stage":284,"start":4752000,"end":4758000,"estimate_nax":"7526596881264132","distributed_nax":"3353989426569032","destroyed_nax":"4172607454695100","total_supplied_nax":"1185631429004692345","pledged_nas":"26836828656599066630500000","total_supplied_nas":"57955197243060000000000000","status":1}]}}

```
***

### NAX Address Distribution History
> Query NAX address distribution history

| Protocol | Method | API |
|----------|--------|-----|
| HTTP | GET |  nax/profits |

#### Parameters

`address` address

`page`	page number; optional parameter, default 1

`page_size` page size; optional parameter, default 20

#### Return

`next` next page requestURL

`count` total

`total_page` total number of pages

`current_page` current page number

`list` data list

#### Example
```
//Request
curl -X GET https://data.nebulas.io/nax/profits?address=n1d4wXxTVmK4rzyiN1vxhMuzxT441bxr8Fg&page=1&page_size=2

//Result
{"code":0,"msg":"success","data":{"next":"http://data.nebulas.io/nax/profits?address=n1d4wXxTVmK4rzyiN1vxhMuzxT441bxr8Fg&page=2&page_size=2","previous":null,"count":10,"total_page":5,"current_page":1,"list":[{"address":"n1d4wXxTVmK4rzyiN1vxhMuzxT441bxr8Fg","tx_hash":"ed36d5bc52f48c55a92551044b14fd1e35b9e9b3a57226f9e910b8e9709ea9c0","block_timestamp":1593710040,"profit":"-1000000000","stage":-1,"source":1,"block_height":4745566},{"address":"n1d4wXxTVmK4rzyiN1vxhMuzxT441bxr8Fg","tx_hash":"c27185fc66fd346fb53dde96cfa73fec957fc40e2e38c19cf5f051fd42bcc516","block_timestamp":1593710115,"profit":"-1000000000","stage":-1,"source":1,"block_height":4745571}]}}
```
***

### Address Information
> Query address information

| Protocol | Method | API |
|----------|--------|-----|
| HTTP | GET |  address/info |

#### Parameters

`address` address

#### Return

`address` address

`nonce` transaction nonce

`type` type: 87 normal address, 88 contract address

`balance` balance

`date_of_found ` created date

#### Example
```
//Request
curl -X GET https://data.nebulas.io/address/info?address=n1d4wXxTVmK4rzyiN1vxhMuzxT441bxr8Fg

//Result
{"code":0,"msg":"success","data":{"contract":{},"address":"n1d4wXxTVmK4rzyiN1vxhMuzxT441bxr8Fg","nonce":124,"type":87,"balance":"22386060161435630252100","date_of_found":"20200130"}}
```
***

### Address Statistics
> Query address statistics

| Protocol | Method | API |
|----------|--------|-----|
| HTTP | GET |  address/count/history |

#### Parameters

`days` days, optional parameter, default is 30

#### Return

`created_at` created date

`updated_at` updated date

`date` date

`all_address_count` address count

`contract_count` contract count

#### Example
```
//Request
curl -X GET https://data.nebulas.io/address/count/history?days=5

//Result
{"code":0,"msg":"success","data":[{"created_at":"2020-07-19T00:00:00","updated_at":"2020-07-19T00:00:00","date":"20200718","all_address_count":339596,"contract_count":15922},{"created_at":"2020-07-18T00:00:00","updated_at":"2020-07-18T00:00:00","date":"20200717","all_address_count":339587,"contract_count":15922},{"created_at":"2020-07-17T00:00:00","updated_at":"2020-07-17T00:00:00","date":"20200716","all_address_count":339581,"contract_count":15922},{"created_at":"2020-07-17T00:00:00","updated_at":"2020-07-17T00:00:00","date":"20200716","all_address_count":339581,"contract_count":15922},{"created_at":"2020-07-16T00:00:00","updated_at":"2020-07-16T00:00:00","date":"20200715","all_address_count":339577,"contract_count":15922}]}
```
***

### Public Chain Statistics Informations
> Query public chain statistics information.

| Protocol | Method | API |
|----------|--------|-----|
| HTTP | GET |  chain/summary |

#### Parameters

None

#### Return

`block_count` block count

`tx_count` transaction count

`contract_count` contract count

`normal_address_count` address count

#### Example
```
//Request
curl -X GET https://data.nebulas.io/chain/summary

//Result
{"code":0,"msg":"success","data":{"block_count":4842814,"tx_count":13075893,"contract_count":15922,"normal_address_count":323676}}
```
***
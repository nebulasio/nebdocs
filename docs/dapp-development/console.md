# Linha de Comandos REPL

Nebulas fornece uma linha de comandos javascript interactiva que invoca todos os métodos do API e gestão RPC. A linha de comandos liga-se ao nó local por padrão, sem ter que especificar o host manualmente..

## Executar a linha de comandos

Execute a linha de comandos usando o comando:

```bash
./neb console
```
No caso de não ter especificado o ficheiro de configuração, o terminal usa o ficheiro em `conf/default/config.conf` por padrão. Se o ficheiro de configuração local não estiver disponível, ou caso queiro especificá-lo manualmente, o terminal começa da seguinte maneira:

```bash
./neb -c <config file> console
```

### Interacção com a linha de comandos

A linha de comandos pode usar a interface `admin.setHost` para especificar que a que nós se liga. Quando a linha de comandos é executada, ou o host não é especificado, o terminal está a interagir com o nó local. **Logo, o nó local deve ser executado antes de abrir a linha de comandos.**

```javascript
> admin.setHost("https://testnet.nebulas.io")
```

_Conselhos: A Testnet apenas executa a interface RPC do API, portanto apenas o esquema do API está disponível._

## Uso da linha de comandos

Temos os esquemas API e admin para aceder aos comandos da linha de comandos. Utilizadores podem fácilmente executar instruções ao usar a tecla `TAB`.

```javascript
> api.
api.call                    api.getBlockByHash          api.getNebState             api.subscribe
api.estimateGas             api.getBlockByHeight        api.getTransactionReceipt
api.gasPrice                api.getDynasty              api.latestIrreversibleBlock
api.getAccountState         api.getEventsByHash         api.sendRawTransaction
```

```javascript
> admin.
admin.accounts                      admin.nodeInfo                      admin.signHash
admin.getConfig                     admin.sendTransaction               admin.signTransactionWithPassphrase
admin.lockAccount                   admin.sendTransactionWithPassphrase admin.startPprof
admin.newAccount                    admin.setHost                       admin.unlockAccount
```

Alguns métodos de gestão podem pedir palavra passe. O utilizador pode escrever a palavra passe ao invocar a interface, ou se a linha de comandos a pedir. **Nós recomendamos a utilização da linha de comandos para escrever a palavra pase pois esta não é visível.**

Escreva a palavra passe directamente:

```javascript
> admin.unlockAccount("n1UWZa8yuvRgePRPgp8a2jX4J9UwGXfHp6i", "passphrase")
{
    "result": {
        "result": true
    }
}
```

Use o terminal:

```javascript
> admin.unlockAccount("n1UWZa8yuvRgePRPgp8a2jX4J9UwGXfHp6i")
Unlock account n1UWZa8yuvRgePRPgp8a2jX4J9UwGXfHp6i
Passphrase:
{
    "result": {
        "result": true
    }
}
```

As interfaces com pedido de palavra passe:

```javascript
admin.newAccount
admin.unlockAccount
admin.signHash
admin.signTransactionWithPassphrase
admin.sendTransactionWithPassphrase
```

Os parâmetros da linha de comandos são consistentes com os da interface RPC. [NEB RPC](https://github.com/nebulasio/wiki/blob/master/rpc.md) e [NEB RPC\_Admin](https://github.com/nebulasio/wiki/blob/master/rpc_admin.md).

## Fechar a linha de comandos

A linha de comandos pode ser terminada ao pressionar `ctrl-C` ou executar `exit`.


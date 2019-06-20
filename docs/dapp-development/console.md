# Consola REPL

Nebulas brinda una consola Javascript interactiva, capaz de invocar todas las funciones de la API y de administrar métodos RPC. Se conecta al nodo local por defecto, sin que sea necesario especificar el host.

## Iniciar la consola

Utilice el comando:

```bash
./neb console
```

En caso de no especificar ningún archivo de configuración, el sistema de arranque leerá el archivo `conf/default/config.conf`. Si ese archivo no está disponible, o si es necesario especificar un archivo de configuración distinto, debe iniciar la terminal con este comando:

```bash
./neb -c <config file> console
```

### Interacción con la consola

La consola puede utilizar la interfaz `admin.setHost` para especificar los nodos a los que se conecta. Cuando recién se inicia la consola, o cuando no se especifica un host, la terminal interactúa por defecto con el nodo local. **De esa manera, es necesario iniciar un nodo local antes de iniciar la consola.**

```javascript
> admin.setHost("https://testnet.nebulas.io")
```

#### Consejos

La _testnet_ sólo iniciará la interfaz RPC de la API, de modo que sólo estarán disponibles los esquemas API.

## Uso de la consola

Tenemos dos esquemas, API y admin, con los cuales acceder a los comandos de la consola. Los usuarios pueden acelerar el ingreso de comandos usando la tecla `TAB`, tal como en una shell de linux.

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

Algunos métodos de carácter administrativo requieren el ingreso de una contraseña. El usuario puede ingresar la contraseña en la misma línea de comandos o bien aguardar a que el proceso lo requiera. **Se recomienda esperar a que el comando lo solicite, ya que de este modo no será visible en la línea de comandos.**

Ingresar la contraseña directamente:

```javascript
> admin.unlockAccount("n1UWZa8yuvRgePRPgp8a2jX4J9UwGXfHp6i", "passphrase")
{
    "result": {
        "result": true
    }
}
```

Esperar a que el proceso la solicite:

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

Interfaces que solicitan contraseña:

```javascript
admin.newAccount
admin.unlockAccount
admin.signHash
admin.signTransactionWithPassphrase
admin.sendTransactionWithPassphrase
```

Los parámetros de la línea de comandos son consistentes con los parámetros de la interfaz RPC.

Véase:

* [NEB RPC](https://github.com/nebulasio/wiki/blob/master/rpc.md)
* [NEB RPC\_Admin](https://github.com/nebulasio/wiki/blob/master/rpc_admin.md)

## Salir de la consola

Para salir de la consola, sólo basta con presionar `ctrl-C` o tipear el comando `exit`.
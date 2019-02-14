# Registros

## Introducción

Nebulas provee dos tipos de registro: registro de consola & registro _verbose_.

### Registro de consola

El **registro de consola** (Console Log o CLog) se usa para para ayudar a los desarrolladores a comprender qué tipo de trabajo **Neb** se está ejecutando, incluyendo inicios y paradas de componentes, recepción de nuevos bloques en el _blockchain_, sincronización, etc.

CLog escribirá todos sus registros en stdout y en archivos de registro al mismo tiempo. Puedes ver ese registro directamente por la salida estándar.

Instrucciones de la consola de registros Nebulas

```text
// el nivel de registro puede ser `Info`, `Warning` y `Error`
logging.CLog().Info("")
```

#### Especificaciones de inicio de servicios

Al iniciarse un servicio Nebulas se debería crear un registro de consola, que se debe emitir inmediatamente antes de iniciar el servicio. Para ello, basta con ejecutar el siguiente comando:

```text
logging.CLog().Info("Iniciando servicio xxxxxx...")
```

#### Especificaciones de parada de servicios

Al detenerse un servicio Nebulas se debería crear un registro de consola, que se debe emitir inmediatamente antes de detener el servicio. Para ello, basta con ejecutar el siguiente comando:

```text
logging.CLog().Info("Deteniendo servicio xxx...")
```

### Registro _verbose_

Este tipo de registro (Verbose Log, o VLog) es útil para depurar cualquier trabajo **Neb**, o para entender su funcionamiento; esto incluye la verificación de bloques, el descubrimiento de nuevos nodos, emisión de tokens, etc.

* VLog escribirá sus registros únicamente en archivos de registro. Puedes consultarlos en tu carpeta de registro si lo deseas.

Es posible filtrar el nivel de _verbosidad_ de este registro; los niveles son **Debug &lt; Info &lt; Warn &lt; Error &lt; Fatal.**

## _Hooking_

Por defecto, los _hooks_ Function y FileRotate se añaden a la salida de CLog y VLog.

### _FunctionNameHooker_

Este _hook_ escribirá en los registros el nombre de la función llamante y su correspondiente número de línea en el código fuente. El resultado se vería así:

```text
time="2018-01-03T20:20:52+08:00" level=info msg="node init success" file=net\_service.go **func=p2p.NewNetManager** **line=137** node.listen="\[0.0.0.0:10001\]"
```

### _FileRotateHooker_

Este _hook_ dividirá los registros en pequeñas secciones encabezadas por _fecha_. Por defecto, todos los registros se reemplazarán una vez por hora. La carpeta de registros debería verse de forma similar a esta:

```text
> neb-2018010415.log neb-2018010416.log neb.log -&gt; /path/to/neb-2018010415.log
```

Si tienes alguna sugerencia con respecto a los registros, siéntete libre de enviar un issue a nuestro [repositorio wiki](https://github.com/nebulasio/wiki). ¡Gracias!
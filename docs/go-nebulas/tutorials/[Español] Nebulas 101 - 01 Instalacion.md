# Nebulas 101 - 01 Compilar e instalar Nebulas

[Tutorial en Youtube](https://www.youtube.com/watch?v=qtjss2LzSI4&list=PLFipfN18ZQwsW1_dge4w7dfsVNdNZZ37R)

El código del proyecto [Nebulas](https://nebulas.io/) se ha lanzado en distintas versiones y está probado para uso local. Se puede descargar su código fuente para compilar el blockchain de forma local.

* Para saber más acerca de Nebulas, sírvase leer el [libro blanco no-técnico](https://nebulas.io/docs/NebulasWhitepaper.pdf).
* Para aprender más acerca de su tecnología, léase el [libro blanco técnico](https://nebulas.io/docs/NebulasTechnicalWhitepaper.pdf) y el [código en github](https://github.com/nebulasio/go-nebulas).

## Importante

Por el momento, Nebulas sólo puede correr en entornos Mac y Linux. Estamos trabajando para lanzar la versión de Windows.

## Entorno Golang

Actualmente, Nebulas está escrito en Golang.

| Componentes | Versión | Descriptión |
|----------|-------------|-------------|
|[Golang](https://golang.org) | >= 1.9.2| Lenguaje de programación Go |

### Mac OSX

Se recomienda [Homebrew](https://brew.sh/) para instalar Golang en entornos Mac:

```bash
# instalación
brew install go

# configuración de las variables de entorno
export GOPATH=/path/to/workspace
```

#### Importante

GOPATH es una variable de entorno que apunta al directorio de trabajo local de golang, y que es personalizable. Luego de configurar GOPATH, es necesario guardar los proyectos GO en ese directorio.

### Linux

```bash
# descarga
wget https://dl.google.com/go/go1.9.3.linux-amd64.tar.gz

# extracción
tar -C /usr/local -xzf go1.9.3.linux-amd64.tar.gz

# configuración de las variables de entorno
export PATH=$PATH:/usr/local/go/bin
export GOPATH=/path/to/workspace
```

## Compilar Nebulas

### Descarga

Es necesario clonar el código fuente mediante estos comandos de consola:

```bash
# ingresar al espacio de trabajo
mkdir -p $GOPATH/src/github.com/nebulasio
cd $GOPATH/src/github.com/nebulasio

# descargar
git clone https://github.com/nebulasio/go-nebulas.git

# ingresar al repositorio
cd go-nebulas

# la rama master es siempre la más estable
git checkout master
```

### Instalar RocksDB

#### Mac OSX

Instalar rocksdb mediante [Homebrew](https://brew.sh/)

```bash
brew install rocksdb
```

#### Ubuntu

Instalar dependencias necesarias:

```bash
apt-get update
apt-get -y install build-essential libgflags-dev libsnappy-dev zlib1g-dev libbz2-dev liblz4-dev libzstd-dev
```

Instalar rocksdb desde el código fuente

```bash
git clone https://github.com/facebook/rocksdb.git
cd rocksdb && make shared_lib && make install-shared
```

#### CentOS

Instalar dependencias necesarias:

```bash
yum -y install epel-release && yum -y update
yum -y install gflags-devel snappy-devel zlib-devel bzip2-devel gcc-c++  libstdc++-devel
```

Instalar rocksdb desde el código fuente

```bash
git clone https://github.com/facebook/rocksdb.git
cd rocksdb && make shared_lib && make install-shared
```

### Instalar las dependencias Go

Las dependencias Go, en Go-Nebulas, se administran mediante [Dep](https://github.com/golang/dep).

| Componentes | Versión | Descripción |
|----------|-------------|-------------|
[Dep](https://github.com/golang/dep) | >= 0.3.1 | Dep es una herramienta de administración de dependencias para Go. |

### Instalar Dep

#### Mac OSX

* Instalar Dep mediante [Homebrew](https://brew.sh/)

```bash
brew install dep
brew upgrade dep
```

#### Linux

Instalar dep

```bash
cd /usr/local/bin/
wget https://github.com/golang/dep/releases/download/v0.3.2/dep-linux-amd64
ln -s dep-linux-amd64 dep
```

### Descargar las dependencias

Es necesario subir hasta el directorio raíz del proyecto para, desde allí, descargar las dependencias necesarias para Go-Nebulas:

```bash
cd $GOPATH/src/github.com/nebulasio/go-nebulas
make dep
```

#### Nota importante
El comando `make dep` provocará la descarga de una gran cantidad de dependencias, lo que podría tomar un tiempo considerable. Algunas de esas dependencias podrían no descargarse correctamente. Si tras varios intentos prosiguen esos errores, es posible intentar la descarga de un archivo ZIP que contiene todas las dependencias generadas mediante dep: [vendor.tar.gz](https://s3-us-west-1.amazonaws.com/develop-center/setup/vendor/vendor.tar.gz) y extraer el contenido de dicho ZIP al directorio raíz del proyecto.

> ```bash
> vendor.tar.gz
> MD5: a8ff50c9c01c67e37300a062edf7949d
> ```

La máquina virtual de Nebulas (NVM) depende del motor V8 JavaScript. Para mayor comodidad, hemos procesado el _build_ de esas dependencias para MAC y linux. Ejecútese el siguiente comando para instalarlas:

```bash
cd $GOPATH/src/github.com/nebulasio/go-nebulas
make deploy-v8
```

### Construcción de Neb

Con todos los pasos previos correctamente realizados, es posible construir el ejecutable de Nebulas. Esto se debe hacer desde el directorio raíz del proyecto mediante la siguiente serie de comandos:

```bash
cd $GOPATH/src/github.com/nebulasio/go-nebulas
make build
```

Una vez que este proceso se completa, habrá un nuevo ejecutable, llamado `neb`, en el directorio raíz.

![make build](resources/101-01-make-build.png)

## Iniciar neb

### Bloque inicial (Genesis Block)

Antes de crear un nuevo _blockchain_ Nebulas, es necesario definir la configuración del bloque inicial, o génesis.

#### Configuración del bloque inicial

```protobuf
# Esquema definido en core/pb/genesis.proto.

meta {
# Chain identity
chain_id: 100
}

consensus {
dpos {
# Dinastía inicial, incluyendo los mineros iniciales
dynasty: [
[ miner address ],
...
]
}
}

# Pre-asignación inicial de tokens
token_distribution [
{
address: [ allocation address ]
value: [ amount of allocation tokens ]
},
...
]
```

Existe un archivo genesis.conf de ejemplo en `conf/default/genesis.conf`.

### Nodo

Antes de poder lanzar un nodo neb, es necesario definir su configuración.

#### Configuración del nodo Neb

```protobuf

# El esquema está definido en neblet/pb/config.proto:Config.

# Configuración de la red
network {
# Para el primer nodo en un blockchain Nebulas, no es necesario el parámetro `seed`.
# En otros casos, todo nodo requiere nodos seed que los “presenten” en el blockchain de Nebulas.
# seed: ["/ip4/127.0.0.1/tcp/8680/ipfs/QmP7HDFcYmJL12Ez4ZNVCKjKedfE7f48f1LAkUc3Whz4jP"]

# Servicio de alojamiento de la red p2p. Soporta múltiples IP y puertos.
listen: ["0.0.0.0:8680"]

# La clave privada se utiliza para generar el ID del nodo. Si no se utiliza una clave privada, el nodo generará un ID nuevo.
# private_key: "conf/network/id_ed25519"
}

# Configuración del blockchain
chain {
# ID de la red del chain (cadena)
chain_id: 100

# Ubicación del almacenamiento de la base de datos
datadir: "data.db"

# Ubicación de los archivos keystore de las cuentas
keydir: "keydir"

# Configuración del bloque inicial
genesis: "conf/default/genesis.conf"

# Algoritmo de firma (signature)
signature_ciphers: ["ECC_SECP256K1"]

# Dirección del minero
miner: "n1SAQy3ix1pZj8MPzNeVqpAmu1nCVqb5w8c"

# Dirección coinbase; todas las recompensas por minería se enviarán a esta dirección:
coinbase: "n1FF1nz6tarkDVwWQkMnnwFPuPKUaQTdptE"

# La palabra clave para acceder al archivo keystore del minero
passphrase: "passphrase"
}

# Configuración de la API
rpc {
# Puerto API GRPC
rpc_listen: ["127.0.0.1:8684"]

# Puerto API HTTP
http_listen: ["127.0.0.1:8685"]

# módulo http
http_module: ["api", "admin"]
}

# Configuración de registro
app {
# Log level: [debug, info, warn, error, fatal]
log_level: "info"

# Ubicación del registro
log_file: "logs"

# Habilitación del registro de errores; `false` para deshabilitarlo, `true` para habilitarlo
enable_crash_report: false
}

# Configuración de las métricas
stats {
# Habilitación de las métricas; `false` para deshabilitarlo, `true` para habilitarlo
enable_metrics: false

# Configuración de InfluxDB
influxdb: {
host: "http://localhost:8086"
db: "nebulas"
user: "admin"
password: "admin"
}
}

```

Existen distintos ejemplos que se pueden consultar en la carpeta `$GOPATH/src/github.com/nebulasio/go-nebulas/conf/`

## Correr nodos

**El blockchain Nebulas que se ha configurado hasta este momento es privado y difiere de las redes oficiales Testnet y Mainnet de Nebulas**.

Para crear un nuevo nodo Nebulas, es necesario ejecutar estos comandos:

```bash
cd $GOPATH/src/github.com/nebulasio/go-nebulas
./neb -c conf/default/config.conf
```

Luego de iniciado, se debería ver esta salida por terminal:
![seed node start](resources/101-01-seed-node-start.png)

Por defecto, el nodo utiliza el archivo `conf/default/config.conf` y no minará nuevos bloques.
Para iniciar un nodo de minado Nebulas, ejecútese lo siguiente:

```bash
cd $GOPATH/src/github.com/nebulasio/go-nebulas
./neb -c conf/example/miner.conf
```

Luego de iniciado el nodo, y si se conecta de forma satisfactoria con el nodo _seed_, se verá la siguiente salida en el registro `logs/miner/neb.log`:
![node start](resources/101-01-node-start.png)

### IMPORTANTE

Es posible iniciar una cantidad arbitraria de nodos de forma local. Es necesario asegurarse de que los puertos especificados en los archivos de configuración no entren en conflicto entre sí.

## Capítulo siguiente: parte 2 del tutorial:

[Enviar transacciones en Nebulas](https://github.com/nebulasio/wiki/blob/master/tutorials/%5BEspa%C3%B1ol%5D%20Nebulas%20101%20-%2002%20Transacciones.md)
# Sistema de direcciones Nebulas

El sistema de direcciones Nebulas fue diseñado cuidadosamente. Como puedes ver más abajo, las direcciones de cuentas y contratos inteligentes comienzan con una letra "n".

## Direcciones de cuentas

De una forma similar a Bitcoin y Ethereum, Nebulas adopta el algoritmo de curvas elípticas como su sistema básico de encriptación para las cuentas Nebulas. La dirección se deriva de una clave pública que a su vez deriva de una clave privada, encriptada con la contraseña del usuario. Además, nuestro diseño de _checksum_ apunta a prevenir que un usuario accidentalmente envíe _NAS_ a la cuenta incorrecta debido a un error de tipeo.

La fórmula específica de cálculo de direcciones es la siguiente:

```text
1.  contenido = ripemd160(sha3_256(clave pública))
    longitud: 20 bytes
                         +--------+--------+------------------+
2.  checksum = sha3_256( |  0x19  +  0x57  |      contenido   | )[:4]
                         +--------+--------+------------------+
    longitud: 4 bytes

                        +--------+---------+-----------------+------------+
3.  dirección = base58( |    0x19  |  0x57   |     contenido |  checksum  | ）
                        +--------+---------+-----------------+------------+
    longitud: 35 caracteres
```

**0x57** es un _type code_ de un byte que indica que la dirección es de una cuenta; **0x19** es un _relleno_ de un byte.

En esta etapa, Nebulas adopta el estándar de codificación [base58](https://en.wikipedia.org/wiki/Base58), similar al de Bitcoin.

Una dirección válida se ve así: _n1TV3sU6jyzR4rJ1D7jCAmtVGSntJagXZHC_.

## Direcciones de contratos inteligentes

El cálculo de estas direcciones varía ligeramente del cálculo anterior. Para más información, véase [contratos inteligentes](https://github.com/nebulasio/wiki/blob/master/tutorials/[English]%20Nebulas%20101%20-%2003%20Smart%20Contracts%20JavaScript.md) y [rpc.sendTransaction](https://github.com/nebulasio/wiki/blob/master/rpc.md#sendtransaction).

La fórmula específica de cálculo de contratos inteligentes es la siguiente:

```text
1.  content = ripemd160(sha3_256(tx.from, tx.nonce))
    length: 20 bytes
                         +--------+--------+------------------+
2.  checksum = sha3_256( |  0x19  |  0x58  +      contenido   | )[:4]
                         +--------+--------+------------------+
    longitud: 4 bytes

                         +--------+---------+-----------------+------------+
3.  dirección = base58 ( |  0x19  |  0x58   |     contenido   |  checksum  | ）
                         +--------+---------+-----------------+------------+
    longitud: 35 caracteres
```

**0x58** es un _type code_ de un byte que indica que la dirección es de un contrato inteligente; **0x19** es un _relleno_ de un byte.

Una dirección de contrato inteligente válida es, por ejemplo: _n1sLnoc7j57YfzAVP8tJ3yK5a2i56QrTDdK_
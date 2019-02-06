# Recompensas para _bug hunters_

## Programa de recompensas por errores de Nebulas

El programa **Nebulas Bug Bounty** apunta a consolidar en Nebulas un ecosistema saludable y seguro. Para ello, hemos puesto a disposición de los _buscadores de errores_ una serie de recompensas que premian cada error que se encuentre.

Este programa está implementado por el Comité Técnico de Nebulas (_Nebulas Technical Committee_, o NTC), en unión con el equipo técnico de Nebulas y los miembros de su comunidad.

NTC alienta a su comunidad a informar de cualquier tipo de vulnerabilidad a través del proceso que se describe más abajo; de ese modo, cada miembro de la comunidad tiene la chance de participar en la construcción del ecosistema de Nebulas y de recibir a cambio una o más recompensas.

## Categorías

El programa divide las recompensas en dos categorías: recompensas por errores comunes (_common bug bounty_) y recompensas por errores especiales (_special bug bounty_).

### Recompensas por errores comunes

Serán otorgadas a todas aquellas personas que encuentren vulnerabilidades en la mainnet de Nebulas, en la testnet de Nebulas, en nebPay, en la cartera web, en la librería neb.js y en otros componentes similares.

### Recompensas por errores especiales

Se otorgarán a quienes descubran vulnerabilidades en las llamadas a funciones inter-contratos (_inter-contract call functions_) y similares.

## Determinación de las recompensas

El Comité Técnico de Nebulas (_Nebulas Technical Committee_) determinará el monto de las recompensas de acuerdo a la gravedad de la vulnerabilidad descubierta, de acuerdo al método de evaluación de riesgos [OWASP](https://www.owasp.org/index.php/OWASP_Risk_Rating_Methodology), basándose dos criterios:  **impacto** y **probabilidad**. No obstante, el valor final de las recompensas estarán sujetas a la decisión del comité.

![1](https://cdn-images-1.medium.com/max/800/1*rR7P3JTHT2KFAYTDodsilw.jpeg)

### Criterios
#### Impacto

* Alto: errores que afectan la seguridad de los activos.
* Medio: errores que afectan la estabilidad del sistema.
* Bajo: otros errores que no afectan la seguridad de los activos ni la estabilidad del sistema.

#### Probabilidad

* Alta: el error podría ser descubierto por cualquier persona que realice una operación determinada, independientemente de si el error fue reportado o no.
* Media: sólo algunas personas podrían encontrar el error (tales como desarrolladores que analicen el código); los usuarios comunes no podrían desencadenar el problema.
* Baja: cubre sólo el 1% (o menos) de la base de usuarios de Nebulas —por ejemplo, propietarios de un modelo poco usual de dispositivo Android— o cualquier otro caso excepcional.

### Montos

Para asegurar que la persona que reporta el error obtenga una recompensa adecuada, estable en el tiempo y proporcional al error hallado, el valor en NAS se ajustará según la paridad con el dólar estadounidense.

Los montos de las recompensas se dividen en cinco categorías:

* Errores críticos: US$ 1000 o más (sin límite superior)
* Errores de probabilidad alta: US$ 500 o más
* Errores de probabilidad media: US$ 250 o más
* Errores de probabilidad baja: US$ 100 o más
* Mejoras: US$30 o más

#### Testnet

Las recompensas para aquellos errores especiales que se encuentren en la testnet (como por ejemplo los vinculados a las llamadas de funciones inter-contratos) se han incrementado de forma acorde, y los montos, en NAS, estarán expresados en dólares estadounidenses.

## Reporta un error

Por favor, envíanos tu reporte a través de [este enlace](https://goo.gl/forms/5ysl61Mjpn6yDEuN2).

## Notas

1. Por favor, revisa que tu reporte sea lo más claro y conciso posible, ya que la evaluación de la recompensa estará basada en el contenido del formulario.

1. En el caso de que varias personas descubran un error al mismo tiempo, se valorarán los reportes de acuerdo a su orden cronológico. Los usuarios de la comunidad son libres de discutir acerca de los errores, pero la discusión en sí misma no se considera como reporte; por ello, debe utilizarse el formulario mencionado anteriormente.

1. El programa de recompensas (_Nebulas Bug Bounty Program_) es de largo plazo. El Comité Técnico de Nebulas se reserva el derecho a la interpretación final de este programa, y el derecho de ajustar o cancelar el alcance de las recompensas, el monto y el criterio de selección.

1. El Comité Técnico de Nebulas evaluará y confirmará los reportes de errores en forma posterior a su envío. Los tiempos de evaluación dependerán de la severidad del problema y la dificultad resultante para solucionarlos. El resultado de la evaluación se enviará a quien lo reportó lo antes posible.

1. Para evitar que los errores reportados puedan ser explotados por personas inescrupulosas, quienes reporten un error deben hacerlo únicamente a través del [formulario mencionado más arriba](https://goo.gl/forms/5ysl61Mjpn6yDEuN2).

1. Quienes reporten errores deben mantener los mismos en total confidencialidad al menos hasta pasados 30 días luego de enviar el reporte a Nebulas, y no deben revelar su naturaleza a terceros. Tal periodo de confidencialidad podrá ser extendido de forma unilateral por el equipo de Nebulas. Si quien reporta el error lo revela a terceros, de tal suerte que Nebulas o sus usuarios se vean perjudicados, tal persona deberá costear los gastos derivados del perjuicio a cada una de las partes involucradas.

1. El Comité Técnico de Nebulas alienta a los miembros de la comunidad a discutir cualquier otro tópico en el grupo público de discusión de Nebulas; asimismo, alentamos a la comunidad a unirse a nuestro esfuerzo por solucionar los problemas que se presenten. Siéntete libre de unirte a nuestra [lista de correo de Nebulas](https://lists.nebulas.io/cgi-bin/mailman/listinfo).

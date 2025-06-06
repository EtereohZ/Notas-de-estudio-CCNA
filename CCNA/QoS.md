Las redes modernas  son redes convergidas de [[IP Phones|teléfonos IP]], video, tráfico regular, etc.
Todos estos distintos tipos de tráfico deben competir por la *bandwidth* disponible, ahí es donde entra QoS.

QoS es un grupo de herramientas usadas por los equipos de la red para aplicar distintos tratos a distintos paquetes. Maneja las siguientes características:
1.  **Bandwidth**
	1. QoS nos permite reservar una cierta cantidad de *bandwidth* para tráficos distintos
2. **Delay**
	1. **One-way delay** mide el tiempo que se demora en llegar a su destino
	2. **Two-way delay** mide el tiempo que se demora en ir y volver
3. **Jitter**
	1. Es la variación de la demora de "one-way delay" de varios paquetes enviados por la misma aplicación
4. **Loss**
	1. el % de paquetes que no llega a su destino


## Estándares recomendados para audio

1. **One-way delay**: <= 150ms
2. **Jitter**: <= 30ms
3. **Loss**: <= 1%
***

## Queuing

si un equipo recibe mensajes más rápido de lo que puede *forwardear*, los mensajes entrar en una lista de espera.
Los paquetes saldrán en el mismo orden en el que llegan. si la lista de espera se llena, paquetes nuevos que lleguen serán botados, esto se llama **"tail drop"**. "Trail drop" puede llevarnos a **"TCP global synchronization"**.

Cuando "trail drop" ocurra, los *hosts* [[TCP]] que se encuentren enviando tráfico bajaran el volumen que es enviado. Al estar la red siendo poco usada, ahora aumentarán el volumen de tráfico enviado, generando oleadas de de alto tráfico. Este proceso se repetirá de nuevo.

Una solución para prevenir esto es **"Random Early Detection"** (RED). Cuando la *queue* llegue a un cierto nivel, el equipo empezará a botar paquetes de flujos específicos. Estos paquetes que sean botados reducirán el volumen de tráfico que está siendo enviado, pero evitará la "global TCP synchronization".


### Congestion Management

Una parte esencial de QoS es que existen **multiples *queues***. El tráfico podrá entrará a estas distintas filas dependiendo de varios factores, como las marcas.
El equipo usa un "scheduler" para determinar que *queue* *forwardeará* tráfico de forma siguiente. Esto le permite asignarle diferente prioridad a las listas.

Un método común de *scheduling* es **"weighted round-robin"**. Funciona tomando paquetes de cada lista, de forma cíclica y, tomando más paquetes de las listas de alta prioridad, cuando es su turno.

**CBWFQ** es otro método que toma elementos del "weighted round-robin" y le asegura a cada 
*queue* un porcentaje del ancho de banda de la interfaz, durante congestión.

Ambos métodos no son ideales para voz o video, ya que como deben esperar su turno en la cola, puede agregar "delay" o "jitter".
Esto lo podemos resolver con **"Low Latency Queuing"** (LLQ). Funciona estableciendo "strict priority queues", estas colas siempre serán priorizadas sobre otras mientras existan paquetes dentro de ella.

#### Shaping and Policing

Son usadas para controlar el flujo del tráfico.

- "Shaping" funciona como amortiguador si es que la tasa de tráfico en una *queue* aumenta sobre lo configurado.
- "Policing" bota el tráfico si es que la tasa aumenta sobre lo configurado.
	- Policing nos permite aceptar tráfico que viene en *bursts*, sin tener que botarlo
	- También nos permite cambiar la marca del tráfico en vez de solo botarlo
****

## Classification

El propósito de QoS es darle a algunas redes prioridad, sobre otras, en cuanto al tráfico, durante periodos de congestión.
Hay varios métodos para clasificar trafico, entre los cuales se encuentran:
1. Una ACL
2. Network Based Application Recognition (NBAR)
3. Campos específicos dentro de los *header* de [[Ethernet Frame|L2]] y [[IPv4 Header|L3]]
	1. PCP en el campo de 802.1Q
	2. DSCP en el paquete L3

### PCP

Su largo es de 3 *bits*, dando 8 posibles valores, cada valor representa una distinta prioridad. 
Debido a que PCP solo se encuentra en el header 802.1Q, solo se puede usar en estas conexiones:
1. Interfaces *[[VLAN|trunk]]*
2. Interfaces *access* con una *[[IP Phones|voice VLAN]]*

 ![[PCP_values.png]]

### DSCP

Algunas de las marcas que son importante saber:
1. **Default Forwarding**
	1. "Best Effort Traffic"
	2. Significa que no hay garantía de que el tráfico llegará a destino. o que cumple algún estándar QoS
2. **Expedited Forwarding**
	1. Para tráfico de voz
3. **Assured Forwarding**
	1. No es una marca en si, sino una *set* de 12 valores distintos
	2. Incluye video interactivo, con marca AF4x y *streaming*, con marca AF3x
	3. Incluye info de alta prioridad, con marca AF2x
4. **Class Selector**
	1. Otro *set* de 8 valores, nos da compatibilidad con IPP


## Trust Boundaries

Definen en que lugar los equipos confiarán, o no, las marcas de QoS recibidas en los mensajes. Si las marcas son confiables, los equipos *forwardearán* el mensaje sin cambiar las marcas. Si no se confía en las marcas, el equipo las cambiará de acuerdo a las políticas configuradas.

En general, puede ser necesario cambiar el lugar de la *boundary* para evitar que la marca en el paquete sea cambiada, como puede pasar con teléfonos IP (queremos que su prioridad sea más alta).
Stands for "Open Shortest Path First". Uses the same name algorithm (aka Dijkstra's algorithm)

Es un protocolo de routeo dinámico de tipo *[[Dynamic Routing|link state]]*
Revisar [[Dynamic Routing|generalidades del *routeo* dinámico]]

- LSA: Link State Advertisement
	- Son organizados en una LSDB
	- **Aging timer** de 30 minutos
- LSDB: Link State Database
	- Todos los [[Router|routers]] en una misma área comparten LSDB
## Versiones

- OSPFv1
	- Publicada en 1989, muy viejo
	- OSPFv2
		- Publicada en 1998
		- Usada para [[IPv4 Addressing|IPv4]]
	- OSPFv3
		- Usada para [[IPv6 Addressing|IPv6]]


## Como compartir LSA's y determinar mejor ruta

1. [[Router]] debe convertirse en vecino de otros *routers* conectados al mismo segmento
2. Compartir LSA's con *routers* vecinos
3. Cada *router* calcula, independientemente, la mejor ruta para cada destino y la inserta en su *routing table*
No todos los *routers* comparten sus LSA's con todos, revisar [[OSPF Networks]].

### Tipos de LSA

- Tipo 1: "Router LSA"
- Tipo 2: "Network LSA"
- Tipo 5: "AS External LSA"
Podemos verlas con el comando "show ip ospf database"

#### Tipo 1 - Router LSA

Este es el tipo de LSA que cada *router* OSPF genera. Identifica al *router* usando su ID y lista las redes que están conectadas a las interfaces OSPF del *router*.

#### Tipo 2 - Network LSA

Generadas por el **DR** en en cada red "multi-access" (ej. la red tipo *broadcast*) y listan a los *routers* que están conectadas a la red *multi-access*.

#### Tipo 5 - AS-External LSA

Generadas por ASBR's para describir rutas a destinos fuera de la AS (el dominio OSPF).



## OSPF Areas

¿Qué es una área?
Una área es un conjunto de *routers* y *links* que comparten una misma LSDB

Existe un área llamada "backbone area" y es una área a la cual todas las otras áreas se deben conectar. Es la área 0.
*Routers* que se encuentren conectados al área 0 se conocen como **"backbone routers"**.

Los *routers* con todas sus interfaces en la misma área se conocen como **"internal routers"**.
*Routers* con interfaces en multiples áreas se conocen como **"area border routers"** (ABR's).
- ABR's mantienen una LSDB para cada área en la que estén conectados.
- Se recomienda que un ABR no se conecte a más de 2 áreas
	- De lo contrario uno arriesga sobrecargar el *router*
Un "Autonomous System Boundary Router" conecta a un *router* de la red OSPF a una red externa.

### Reglas

- Cada área debe ser contigua, no pueden haber rutas separadas o divididas.
- Todas las áreas OSPF debe tener **al menos un** ABR conectado a la "backbone area"
- Interfaces OSPF en la misma [[CCNA/Subnetting/Subnetting|subnet]] deben estar en la misma área
### Nombres de rutas

- Una **"intra-area router"** es una ruta a un destino dentro de la misma área OSPF
- Una **"interarea route"** es una ruta a un destino en una área OSPF distinta

### ¿Por qué dividir OSPF en áreas?
OSPF utiliza áreas para dividir la red, pero redes pequeñas pueden solo ser de una área sin comprometer el rendimiento.
Si utilizamos solo 1 área en redes grandes:
- El algoritmo tomará más tiempo en calcular rutas
- El algoritmo requerirá exponencialmente más poder de procesamiento en los *routers*
- Mientras mas grande sea el LSDB más memoria tomará en los *routers*
- Cualquier cambio en la red, por pequeño que sea, provocará que cada *router* haga un *flood* de LSA's y corra el algoritmo de nuevo
Al dividir una red OSPF en **varias áreas pequeñas**, podemos evitar estas consecuencias.



## OSPF Neighbours

Es esencial para el funcionamiento de OSPF. Una vez que los *routers* se convierten en vecinos, automáticamente realizan el trabajo de compartir información de red, calculo de rutas, etc.

Cuando OSPF se activa en una interfaz, el *router* envía mensajes "hello" fuera de la interfaz a intervalos regulares (10 segundos por defecto, configurable). Estos son usados para encontrar y establecer vecinos OSPF.
Los mensajes "hello" son *multicast* a **224.0.0.5**, y es la misma dirección para todos los *routers* OSPF.


### OSPF Neighbours Requirements

Requisitos para que 2 *routers* puedan convertirse en vecinos:
1. Deben estar en la misma área
2. Las interfaces debe estar en la misma *[[CCNA/Subnetting/Subnetting|subnet]]*
3. El OSPF *process* debe no estar "shutdown"
	1. Entramos a configuración OSPF y usamos el comando "shutdown"
4. LAs ID's OSPF entre *[[Router|routers]]* deben ser únicas
	1. Podemos eliminar un ID configurado manualmente con "no router-id"
5. los tiempos *hello* y *dead* deben calzar
	1. El *hello* se puede configurar con "ip ospf hello-interval </valor>"
	2. El *dead* se puede configurar con "ip ospf dead-interval </valor>"
6. Las configuraciones de *authentication* deben calzar, podemos elegir contraseñas para proteger el OSPF
	1. "ip ospf authentication-key </contraseña>"
		1. Para elegir la contraseña
	2. "ip ospf authentication"
		1. Para activar la autenticación
7. Configuración "IP MTU" deben calzar
	1. "IP MTU" es el tamaño máximo de un paquete IP que puede ser enviado fuera de la interfaz
	2. Configurable con "ip mtu </valor>"
8. El tipo de red OSPF debe calzar

### Hello Message

- Incluye el *Router* ID del *router* que envía el mensaje y quien lo recibe
- Como R1, que envía el mensaje, inicialmente no conoce al otro *router*, el ID del destinatario es 0.0.0.0
- Cuando el R2 reciba el mensaje, agregará a R1 a su tabla de vecinos OSPF
- R2 luego enviará un "hello" a R1, este contiene las ID's de R2 y R1
- Finalmente, R1 enviará otro "hello" a R2, está vez conteniendo el ID de R2
### Neighbor states

#### Down State

En este estado, los *routers* no poseen conocimiento del resto.
R1 enviará un "hello" multicast 224.0.0.5 el cual será recibido por R2.

#### Init State

R2, al recibir el *hello*, se actualizará a estado **"init"**. Ahora, R2 le enviará otro "hello" a R1, conteniendo los Id's de R2 y R1.
Mientras R1 no reciba ningún mensaje, seguirá en estado **"down"**.

#### 2-way State

R1 recibirá el "hello" proveniente de R2 y entrará en estado **"2-way"**. Un *router* entra a este estado cuando recibe un "hello" que contiene su propio ID.
Ahora, R1 le enviará otro "hello" a R2, está vez conteniendo tanto el ID de R1 como el de R2, causando que R2 también entre a estado **"2-way"**

- Las condiciones para convertirse en vecino se encontrarán cumplidas cuando ambos *routers* se encuentren en el estado **"2-way"**
- En este estado **se elige DR y BDR**
#### Exstart State

Ya estando los 2 *routers* en estado "2-way", deben determinar quién iniciará el intercambio de LSDB. Esto ocurrirá en el estado **"exstart"**.
El *router* que tenga el ID más alto se convertirá en el "master" y el otro será el "slave".
Puede considerarse una etapa de preparación.

Estos determinan que posición tomarán intercambiando paquetes "Database Description" (DBD).

#### Exchange State

En este estado intercambiarán DBD's, estos contendrán **una lista** de los LSA's que tiene cada uno. No intercambiarán sus LSA's en si.
Luego, deben comparar la info recibida con su propia LSDB para determinar que LSA's deben solicitar de su vecino.

#### Loading State

Ya sabiendo que LSA's necesitan, los [[Router|routers]] enviarán mensajes "link State Request" (LSR) a su vecino para solicitar que le envíen las LSA's que no poseen.
Las LSA's serán enviadas en mensajes "Link State Update" (LSU).
Finalmente, enviarán un LSAck para informar que recibieron las LSA's.

#### Full State

En este estado, ya son vecinos completos y tienen la misma LSDB. Pero continuarán enviando mensajes "hello" (10 segundos por defecto) para mantenerse actualizados.

Cada vez que se reciba un "hello", un temporizador llamado **"dead timer"** se reiniciará (40 segundos por defecto).
Si se dejan de recibir *hello's* y el temporizador llega a 0, se eliminará a ese vecino.

Resumen de los distintos tipos de mensaje:
![[OSPF_messages.png]]



## OSPF Metric

- La métrica de OSPF se llama **"cost"**
	- El costo de llegar a un destino es el costo total de los saltos en el camino (igual que STP)
	- El costo de llegar a una *loopback interface* es de 1
- Es calculado automáticamente basado en el *bandwidth* de la interfaz.
- Se calcula dividiendo el **"reference bandwidth"** por el *bandwidth* de la interfaz
	- RBw / IBw = cost
	- El "reference bandwidth" por defecto es de 100mbps
	- Cualquier mbps >= al del RBw tendrá un costo de 1
- Podemos cambiar el valor del RBw con el comando **"auto-cost reference-bandwidth </valor_en_mbps>"**

### Configuración de costo de una interfaz

Además de cambiar la métrica con el comando de arriba, podemos cambiar específicamente el costo de una interfaz.
- Ingresamos el comando "ip ospf cost </valor>"
	- Recordar elegir la interfaz a configurar
	- Tomará prioridad sobre el costo calculado automáticamente
- También podemos cambiar el *bandwidth* de la ecuación, sin cambiar la velocidad actual a la que trabajará el *router*
	- Con el comando "bandwidth </valor>"
	- **No se recomienda**



## Configuración OSPF

**Antes de entrar a configurar, revisamos comandos informativos**
1. "show ip protocols"
2. **show ip ospf neighbor**
3. show ip ospf interface </opcion>
		1. Tiene varias opciones, como "brief"

**Primero, entramos al modo de configuración de OSPF**
1. Con el comando **"router ospf </ID>"**
	1. **No es necesario que los ID's sean iguales** entre distintos *routers* para convertirse en vecinos, al contrario de [[EIGRP]].
	2. No tiene relación con la configuración de áreas
	3. Pueden haber variso ID's distintos en un mismo *router*

**Ahora configuramos las redes que queremos agregar a OSPF**
1. Usando **"network </red> </wildcard> area </area>"**
	1. Este comando le dice al *router* que busque interfaces en el rango de IP especificado
		1. Y activar OSPF en la interfaces que calcen, y en el área especificada
		2. El *router*, entonces, tratará de buscar vecinos OSPF en *routers* con OSPF activado
	2. Se debe especificar el área

**Si tenemos una red donde no habrán vecinos OSPF**
1. Y ahora el comando "passive-interface </interface>"
	1. Le dice al *router* que no envíe mensajes OSPF "hello" fuera de esa interfaz
	2. Usado si no hay *routers* a los que anunciar fuera de esa interfaz (no tiene vecinos)

**Ahora configuramos una "default gateway" para OSPF**
1. Configuramos una "default route" en el *router*
2. Ingresamos el comando "default-information originate"
	1. Le dice al ¨router que se anuncie a la red OSPF como el "default gateway"

						
3. Configurar métricas cambiando RBw o directamente el costo de una interfaz, si se require
4. Podemos usar el comando "distance </numero>" para cambiar la AD
5. Revisamos info con comandos


#### Método alternativo de activación de OSPF en una interfaz

1. Elegimos la interfaz
2. Ingresamos el comando "ip ospf </process_id>" area </area>
	1. Si ingresamos el comando "show ip protocols", en vez de ver las redes en las que activamos OSPF, se verán las interfaces donde lo activamos .

### Router ID

#### Orden de prioridad
Orden de prioridad al determinar el *router* ID OSPF
1. Configuración manual
2. IP más alto en la *loopback interface*
3. IP más alto en la interfaz física

#### Configuración router ID

1. Usamos el comando "router-id </ID>"
2. Ahora seguimos con el comando **"clear ip ospf process"**
	1. Para que tome efecto el cambio de ID
	2. El *router* perderá todas sus rutas OSPF
	3. No recomendado usar en una red real

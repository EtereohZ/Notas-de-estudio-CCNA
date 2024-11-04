
The OSPF "network type" refers to the type of connection between [[OSPF]] neighbours (ethernet, etc)


## DR/BDR

Es el rol que tomará cada *router* en al subnet. Su elección varía dependiendo del tipo de red.

Se elige 1 DR y 1 BDR por cada *subnet*
"DROther" solo pasaran al estado "Full" con DR's y BDR's. Y mantendrán estado "2-way" con otros *DRother's*. Esto significa que los *routers* solo intercambiarán LSA's con el DR y BDR y, DROther's no compartirán LSA's entre ellos.
DR y BDR formarán estado "Full" con **todos** los *routers* en la *[[CCNA/Subnetting/Subnetting|subnet]]*, mientras que DROther's solo tendrán estado "Full" con DR/BDR


Mensajes al DR/BDR son *multicast* a 224.0.0.6

### Elección de DR/BDR

Se eligen en el estado "2-way, el que quede en primer lugar se convertirá en DR y el que quede en segundo lugar será DBR
A continuación la lista de prioridades:
1. La prioridad de interfaz OSPF más alta
	1. Todas las interfaces tienen la misma prioridad, por defecto
2. ID OSPF de *router* más alta

La elección de DR/BDR es **"non-preemptive"**, esto significa que una vez los roles sean elegidos, **no podrán ser cambiados** a menos que OSPF sea reiniciado, la interfaz falle, etc.
Si el DR cae, **es el BDR quien se convertirá** en DR, independiente de la prioridad de los otros *routers*. Se hará una **elección** para determinar **al siguiente BDR**.
## Network Types

Podemos cambiar el tipo de red con el comando "**ip ospf network </red>**"
Podemos cambiar la configuración del tipo de red a la estándar (broadcast) con el comando "no ip ospf network </red>"

ELEGIR INTERFAZ PARA CAMBIAR RED

### Broadcast

Esta es la que hemos estado viendo en clases anteriores.

Enabled by default on **Ethernet** interfaces and **FDDI** (Fiber Distributed Data Interface).
*Routers* dynamically discover neighbours by sending/listening to OSPF Hello messages using multicast address 224.0.0.5

Un DR (designated router) y **BDR** (backup designated router) deben ser elegidos en cada *subnet* (solo DR si no hay vecinos OSPF fuera de determinada interfaz).
*Routers* que no son DR o BDR se convierten en **DROther**

#### Cambio de prioridad de interfaz OSPF

- **ip ospf priority </valor>**
- Si elegimos un valor de 0 el *router* nunca podrá ser DR o BDR


### Point-to-Point

Enabled by default on **PPP** (Point-to-Point Protocol) and HDLC (High Level Data Link).
Tanto **PPP** como **HDLC** son encapsulaciones de L2, similar a [[Ethernet]]. Solo que este tipo de red se una en conexiones ***serial***. 
"Point-to Point" no soporta *[[Ethernet Frame|frames]]*.

*Routers* dinámicamente descubren vecinos enviado *Hello's* a través de *multicast* 244.0.0.5.

Dado que es un tipo de conexión "point-to-point" entre 2 *routers* no es necesario elegir un DR/BDR. Ambos *routers* tendrán *full adjacencies* entre ellos.

#### Serial Interfaces

Un lado funciona como **DCE** (Data Comms Equipment) y el otro lado funciona como **DTE** (Data Terminal Equipment).
El lado DCE especifica el *clock rate*, velocidad de la conexión.

- Podemos cambiar la encapsulación con **encapsulation </encapsulacion>**
	- Usan HDCL por defecto
- Podemos averiguar quien es DCE/DTE con el comando **show controllers </interfaz>**
- Podemos cambiar el *clock rate* con el comando **clock rate </Bps>**


### Non-broadcast

Enabled by default on **Frame Relay** and **X.25** interfaces
No es necesario aprenderla.
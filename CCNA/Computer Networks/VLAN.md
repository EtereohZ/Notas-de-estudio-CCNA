- It stands for Virtual Local Area Network. They're configured on [[Switch|switches]] on a **per-interface** basis.
- A switch **will not** forward traffic between VLAN's, including broadcast/unknown unicast traffic.
- The switch does not perform **inter-VLAN** [[Routing|routing]]. It must send the traffic through the [[Router|router]].
- They **logically** separate end hosts/broadcast domains at **layer 2**
- VLAN's help reduce unnecessary broadcast traffic, which helps prevent network congestion. They also improve network security
- We want to make sure that network traffic isn't sent unnecessarily to other devices.
- Paquetes entre VLAN's se enviarán primero a un *router*
- Si la fuente y destino pertenecen al mismo VLAN, los *[[Ethernet Frame|frames]]* no necesitan ser enviados al *router*
***

## VLAN Ranges
- VLAN normales
	- Rango de 1-1005
	- VLAN extendidas
		- 1006-4094

## Access Port

Es un *switchport* que pertenece a una sola VLAN y conecta a *endhosts*, como un PC

## Trunk Port

¿Qué es un *trunk port*?
Un *trunk port* lleva trafico de varias VLAN's en una sola interface, al contrario de *access ports*

### ¿Qué pasa si tenemos un switch en access y el otro en trunk?

Habrá un error y no podrán comunicarse
##### ¿Cómo sabe un *switch* a que VLAN pertenece un *frame*?
Con VLAN *tagging*, *switches* le ponen un *tag* a todos los *frames* que se envíen por un *trunk link*.
*Access ports* no necesitan un *tag* porque la interface pertenece a solo un VLAN.

##### ¿Configuración de un *trunk port?
Primero debemos elegir la interface que queremos configurar.
Para configurar interface como *trunk port* debemos usar el comando "switchport mode trunk", ese comando determina las interfaces elegidas como *trunk*, para ser utilizadas por más de 1 VLAN.
Consultar la pestaña de configuración de más abajo.

Puede ser que tengamos que elegir el tipo de encapsulación (802.1Q o ISL) antes de configurar la interface como *trunk port*, pero es solo en *switches* viejos. Para eso es el comando "switchport trunk encapsulation </tipo_de_encapsulacion>"
Después, debemos elegir que VLAN's serán permitidas por el *trunk port*, para eso usamos el comando "switchport trunk allowed vlan".

Para propósitos de seguridad, es buena idea cambiar la *native* VLAN a una VLAN que no esté siendo usada (recordar cambiar en ambos switches).
Para esto usaremos el comando "switchport trunk native vlan </numero_vlan>".

Por último, debemos configurar ROAS. Ver al final.
## VLAN Tagging

Existen 2 protocolos para hacer *tag*
Se usa para identificar a que VLAN pertenece el *frame*
- **ISL**
	- Viejo y nadie lo usa
- **IEE 802.1q**
	- Es un estándar de la industria
	- "dot1q"
	- Se inserta entre el "Source" y el "Type/Length" de un [[Ethernet Frame]]
	- 4 Bytes de largo == 32 *bits*
	- Está compuesto de 2 campos
		- **TPID**
			- Tag Protocol Identifier
			- Tiene un valor de 0x8100
				- Esto indica que el *frame* tiene un tag 802.1Q
			- 2 Bytes de largo == 16 *bits*
		- **TCI**
			- Tag Control Information
			- 2 Bytes de largo == 16 *bits* de largo
			- Está compuesto de 3 subcampos
				- **PCP**
					- Priority Code Point
					- Usado para Class of Service (CoS)
						- Usado para priorizar tráfico importante
					- 3 *bits* de largo
				- **DEI**
					- Drop Eligible Indicator
					- Usado para indicar *frames* que pueden ser botados en caso de que la red esté congestionada
					- 1 *bit* de largo
				- **VID**
					- VLAN ID
					- Indica a que VLAN pertenece el *frame*
					- 12 *bits* de largo
						- 2<sup>12</sup> = 4096 VLAN's totales
					- IMPORTANTE
![[dot1q_structure.png]]
#### Native VLAN
- Es una característica del protocolo 802.1Q
- Su *default* es VLAN 1 en todos los *trunk ports*
- El *switch* no le agrega un *tag* 802.1Q a los frames en el *native* VLAN
- Cuando un *switch* recibe un *frame* sin *tag* en un *trunk port*, asume que ese *frame* pertenece a la *native* VLAN
- Es muy importante que la *native* VLAN calce entre *switches*
- EL numero de *native* VLAN debe calzar con el numero de VLAN. Si queremos que VLAN 10 envíe *frames* sin *tag*, la *native* VLAN debe ser 10
- Si no estamos usando *native vlan* es mejor asignarla a una VLAN que no estemos usando



2. Configurar la dirección [[IPv4 Addressing|IP]] para la VLAN nativa en la interfaz física del *router*, sin tener que usar el comando del método previo
	1. En este método no usamos subinterfaces para la VLAN nativa, usamos una interfaz física
	2. Al mostrar las interfaces, veremos que la nativa está usando la interfaz física, y las otras VLAN's usarán las subinterfaces
	3. Borramos la subinterfaz que estaba usando la VLAN que queremos determinar como nativa y la asignamos a una interfaz física

***

## Configuración

 **Primero elegiremos las interfaces que agregaremos a los puertos *access***
1. Elegimos interfaces **"interface range </interfaces>"**
2. Las convertimos en *access* con **"switchport mode access"**
3. Les asignamos una VLAN con **"switchport access vlan </numero_vlan>"**
	1. Asigna el puerto a la VLAN y, la crea si es que no existe
4. Repetimos por cada VLAN que vayamos a crear

**Ahora entramos a la interfaz y configuramos los puertos *trunk que necesitemos***
1. Primero elegimos encapsulación con **"switchport trunk encapsulation dot1q"**
	1. Necesario en equipos que soporten ISL
2. Configuramos con **"switchport mode trunk"**

**Dentro de la interfaz, procedemos con configurar que VLAN's serán permitidas en el *trunk***
2. . Con **"switchport trunk allowed vlan </vlan_id>"**
	1. Tiene varias opciones para agregar

**Si queremos cambiar la VLAN nativa**
1. Usamos **"switchport trunk native vlan </vlan_id>"**

**Para revisar y cambiar nombres de nuestras VLAN's**
1. **"show vlan brief"** nos muestra los puertos *access*
2. **"show interfaces trunk"** nos muestra los puertos *trunk*
	1. Recordar que "show vlan brief" no muestra los *trunk ports*
	2. **Mode**
		- **on** nos dice que fue configurado manualmente
	3. **Encapsulation**
		- Tipo de encapsulación
	4. **Status**
		- Que está *trunkeando*
	5. **VLAN's allowed and active in management domain**
		- Nos da las VLAN's que tenemos configuradas
3. Entramos al modo de configuración de una VLAN con **"vlan </numero_vlan>"**
	1. También sirve para crear una VLAN directamente
	2. Ingresamos el comando **"name </nombre>"** para cambiar nombre de VLAN


RECORDAR QUE DEBEN EXISTIR TODAS LAS VLANS EN TODOS LOS *SWITCHES*. DEBEN CREARSE SI NO SALEN EN LA SECCIÓN DE "MANAGEMENT" EN "SHOW VLAN BRIEF"
***

### Router on a Stick

Es el nombre usado para *inter VLAN routing*, solo tendremos una interfaz física conectando el [[Router|router]] y el [[Switch|switch]].
Es usado para *routear* entre multiples VLANS usando solo 1 interfaz entre el *router* y el *switch*.
Se convertirán en subinterfaces de una interfaz, Ej.  g0/0 se dividirá en g0/0.10, g0/0.20, g0/0.30. Y pueden operar como 3 interfaces separadas. Usar los mismos número que tengan las VLAN's

##### ¿Como configurar ROAS?

**Ingresamos a la interfaz deseada y luego entramos a cada subinterfaz (esto la crea)**
1. Con el comando **"interface </subinterface>"**
	1. Idealmente usar una que calce con el ID de la VLAN

**Ahora le asignamos cada subinterfaz a una VLAN**
1. Con el comando **"encapsulation dot1q </id_vlan>"**
	1. Cualquier *frame* que llegue con el id de la VLAN configurada será tratado por el *router* como si hubiese llegado por esta subinterfaz
	2. También *tageará* todos los *frames* que tengan como destino a esa VLAN con el id de la VLAN
2. Repetir para cada subinterfaz

**Para configurar VLAN nativa en la subinterfaz
1. **encapsulation dot1q </vlan_id> native**
	1. Configuramos esa subinterfaz como VLAN nativa
	2. Asumirá que *frames* que no tengan *tag* pertenecen a la VLAN nativa y que frames que se envíen a la VLAN nativa no serán *tageados*

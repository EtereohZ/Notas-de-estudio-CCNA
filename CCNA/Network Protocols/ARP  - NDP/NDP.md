Protocolo parecido a [[ARP]] usado con [[IPv6 Addressing|IPv6]], con la función de aprender la dirección [[MAC Address|MAC]] de otros *hosts*.
Usa ICMPv6 y "solicited-node multicast addresses" para aprender la MAC.


##  Solicited-node Multicast Addresses

Son mucho mas eficientes que los mensajes *broadcast* de ARP, debido a que son *addressed* a un *host* específico.

Tipos de mensajes:
- **Neighbour Solicitation** (NS) = ICMPv6 Type 135
- **Neighbour Advertisement** (NA) = ICMPv6 type 136


Pasos al enviar el NS
1. PC1 hace *ping* a PC2 con dirección 2001:db8:78:9abc
2. Como no tenemos la MAC, PC1 envía un mensaje **NS**
3. Se crea un paquete (NS) con dirección origen con IPv6 de PC y de dirección se destino se usa la  dirección "solicited-node multicast" de PC2
	1. ¿Cómo PC1 sabe esa dirección?
		1. A partir de la dirección *unicast* que nosotros ingresamos, PC1 automáticamente podrá calcularla
4. De dirección MAC, se usará la MAC de PC1 para origen y para destino se usará una dirección MAC *multicast* basada en la "solicited-node" de PC2 

Pasos al responder
1. PC2 recibe el mensaje y enviará su MAC en un mensaje **NA**


Para ver la tabla de MAC's usamos el comando **"show ipv6 neighbor"**

## Automatically Discover Neighbours

Otra función de NDP permite a que los equipos automáticamente descubran a otros *routers* en la red.
Tipos de mensajes:
- **Router Solicitation** (RS) = ICMPv6 Type 133
	- Se envía a dirección *multicast* FF02::2 (todos los *routers*)
	- Le pide a todos los *routers* que se identifiquen
	- Se envía cuando una interfaz se conecta a la red
- **Router Advertisement** (RA) = ICMPv6 Type 134
	- Se envían a dirección *multicast* FF02::1 (todos los nodos)
	- Estos mensajes se envían como respuesta a un **RS**
		- También se envían de forma periódica, con o sin **RS**



## Duplicate Address Detection

Duplicate Address Detection (DAD) permite a los *hosts* *chequear* si otros equipos en el *local link* están usando la misma dirección IPv6.

Cada vez que una interfaz IPv6 se habilita, o se configura una dirección IPv6, ejecuta **DAD**. DAD envía un mensaje **NS** a su propia IPv6 y, si no le llega una respuesta, sabrá que su dirección es única. Si le llega una respuesta, significa que otro equipo ya está usando esa dirección.
Este mensaje **NS** es enviado a su propia dirección "solicited-node multicast"
- It's capable of both switching and [[Routing|routing]]. It knows about layer 3, so we can assign [[IPv4 Addressing|IP addresses]] to its interfaces, like a [[Router|router]].
- You can create virtual interfaces for each [[VLAN]], and assign IP addresses to those interfaces.
- It can be used for inter-VLAN routing

## Switch Virtual Interfaces

-  Llamados **SVI's**, son las interfaces virtuales en las que son asignadas las direcciones IP en un *multilayer switch*. Le asignamos las direcciones IP de cada red de cada VLAN; igual como lo haríamos en un *router*.
	- El *multilayer switch* tendrá su propia *routing table*, si le llega un *frame* destinado a VLAN 10, sabrá por que interfaz enviarlo.
- Usaremos el SVI como [[Routing|gateway address]], en vez del *router*. Y configuraremos cada PC para usarlo.
- Para enviar el tráfico a cada subnet/VLAN, los PC's le enviarán el tráfico al *switch*, y el *switch routeará* el tráfico.
- Si algún *host* quiere contactar destinos fuera de la [[LAN]], como el *multilayer switch* está actuando como nuestro *default gateway*, todos los paquetes serán enviados a él
	- Tendremos que asignar una [[CCNA/Subnetting/Subnetting|subnet]] a nuestro *point-to-point* entre el *multilayer switch* y el *router*
	- Luego le configuramos una [[Routing|default route]] al *switch* con destino al *router*

## Configuración MS para routear

1. Esta parte debe realizarse primero que la configuración para *routeo* inter-VLAN

**Primero, eliminamos las subinterfaces en el *router* (si existiesen) y reiniciamos la interfaz
1. Con **"no interface </interfaz>"**
2. Y "default interface </interfaz>" 
	1. Devolvemos la configuración estándar

**Le asignamos una dirección IP a la interfaz que conectará con el MS**

**Ahora habilitamos la capacidad del *switch* para routear***
1. Con el comando **"ip routing"**
	1. Habilita *routeo* L3
	2. Permite al *switch* crear una tabla de *routeo*

**Seguido, entramos a, y configuramos, la interfaz que conectará con el router para poder asignarle un IP**
1. Con **"no switchport"**
	1. cambia la interfaz desde un *switchport* L2 a un L3 *routed port*

**Finalmente, le configuramos la *default route* al *MS***
1. Con el comando **"ip route 0.0.0.0 0.0.0.0 </ip_router>"**

**Si queremos ver la nueva interfaz del *switch***
1. Usar comando **"show interfaces status"**

### Configurar SVI para inter-VLAN routing

**Primero, creamos la SVI que será usada para *routear* VLAN's**
1.  Ingresar comando **"interface </numero_de_vlan>"**
	1. Ejemplo: **"interface vlan10"**
	2. Esto creará una SVI para esa VLAN
	3. La VLAN debe crearse por separado

**Ahora le asignamos una IP a esa SVI, como cualquier otra interfaz**
1. Con **"ip address </ip_address> </netmask>"**

**Prendemos la interfaz**
1. Usamos **"no shutdown"** 
	1. Están *shutdown* por *default*

**Si queremos que la SVI esté up/up**
1. La VLAN debe existir en el *switch*
2. el *switch* debe tener algún *access port* que esté up/up o algún *trunk port* que permita a la VLAN que esté up/up
3. LA VLAN debe estar prendida

- It's capable of both switching and [[Routing|routing]]. It knows about layer 3, so we can assign [[IPv4 Addressing|IP addresses]] to its interfaces, like a [[Router|router]].
- You can create virtual interfaces for each [[VLAN]], and assign IP addresses to those interfaces.
- It can be used for inter-VLAN routing

#### Switch Virtual Interfaces

-  Llamados **SVI's**, son las interfaces virtuales en las que son asignadas las direcciones IP en un *multilayer switch*. Le asignamos las direcciones IP de cada red de cada VLAN; igual como lo haríamos en un *router*.
	- El *multilayer switch* tendrá su propia *routing table*, si le llega un *frame* destinado a VLAN 10, sabrá por que interfaz enviarlo.
- Usaremos el SVI como [[Routing|gateway address]], en vez del *router*. Y configuraremos cada PC para usarlo.
- Para enviar el tráfico a cada subnet/VLAN, los PC's le enviarán el tráfico al *switch*, y el *switch routeará* el tráfico.
- Si algún *host* quiere contactar destinos fuera de la [[LAN]], como el *multilayer switch* está actuando como nuestro *default gateway*, todos los paquetes serán enviados a él
	- Tendremos que asignar una [[CCNA/Subnetting/Subnetting|subnet]] a nuestro *point-to-point* entre el *multilayer switch* y el *router*
	- Luego le configuramos una [[Routing|default route]] al *switch* con destino al *router*

##### Configuración MS para routear
1. Esta parte debe realizarse primero que la configuración de SVI que está mas abajo
2. Eliminamos las subinterfaces del [[Router|router]] que usamos para las VLAN
3. Usamos el comando "default interface </interfaz>" para devolver la configuración estándar
4. Elegimos la interface que conectará *point-to-point* con el MS y le configuramos su dirección IP
5. En el MS, ingresamos el comando "ip routing"
	1. Habilita *routing* L3
6. Ingresar el comando "no switchport", cambia la interfaz desde un *switchport* L2 a un L3 *routed port*
	1. Acordarse de elegir la interfaz primero
	2. Nos permite asignarle IP a la interfaz
7. Le asignamos el IP a la interfaz
8. Asignar la *default route* al *multilayer switch* con el comando "ip route 0.0.0.0 0.0.0.0 </ip_router>"
9. Usar comando "show interfaces status" para ver las interfaces
##### Configurar SVI para inter-VLAN routing
1.  Ingresar comando "interface </numero_de_vlan>"
	1. Esto creará una SVI para esa VLAN
	2. La VLAN debe crearse por separado
2. Le asignamos [[IPv4 Addressing|dirección IP]] a la SVI
3. Usamos "no shutdown" para prenderla, están *shutdown* por *default*
4. Si queremos que la SVI esté up/up:
	1. La VLAN debe existir
	2. el *switch * debe tener algún *access port* que esté up/up o algún *trunk port* que permita a la VLAN que esté up/up
	3. LA VLAN debe estar prendida

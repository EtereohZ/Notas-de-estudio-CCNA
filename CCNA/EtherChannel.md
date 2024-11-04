
If you connect two switches together with multiple links, all except one will be disabled by [[STP]].
EtherChannel groups multiple interfaces together to act as a **single interface**. It's represented by drawing a circle around the interfaces that are grouped together.
El tráfico  que pase por un "EtherChannel" va a ser *load balanced* entre las interfaces físicas en el grupo. Un algoritmo será usado para determinar que tráfico usará que interfaz física. STP tratará a este grupo como una sola interfaz.

When the bandwidth of the interfaces connected to end hosts is greater than the bandwidth of the connection to the distribution switches, this is called **oversubscription**.


ASW: Access layer switch. A switch that end hosts connect to
DSW: Distribution layer switch. a Switch that access layer switches connect to

### EtherChannel load-balancing

- El balanceo de carga de "EtherChannel" está basado en flujos
- Un flujo es la comunicación entre 2 nodos en la red
- *[[Ethernet Frame|Frames]]* en el mismo flujo serán *forwardeados* usando la misma interfaz física
- Si los *frames* fueran *forwardeados* en distintas interfaces físicas, algunos *frames* podrían llegar a su destino fuera de orden

Podemos cambiar algunos de los *inputs* que son usados en el calculo de selección de interfaz:
- [[MAC Address|MAC]] de origen
- MAC de destino
- MAC de origen y destino
- [[IPv4 Addressing|IP]] de origen
- IP de destino
- IP de origen y destino

### Configuración EtherChannel

1. **show etherchannel load-balance**
	1. Nos muestra el método actual de *load-balancing* e info asociada
2. **port-channel load-balance </metodo>**
	1. Lo usaremos para configurar el método de *load-balance* en el *switch*
3. Para configurar el "EtherChannel" en si usaremos el protocolo **LACP** (Link Aggregation Control Protocol)
	1. Estándar 802.3ad
	2. Dinámicamente negocia la creación y mantenimiento del "EtherChannel", parecido a [[DTP]]
	3. Posee los modos:
		1. **active**
		2. **passive**
	4. También existen:
		1. **PAgP** (Port Aggregation Protocol)
			1. Propietario de Cisco
			2. Posee los modos:
				1. **desirable**
				2. **auto**
		2. Static EtherChannel
			1. No usa protocolo para determinar la creación de un EtherChannel
			2. Las interfaces se configuran manualmente para crear uno
	5. Acepta hasta 16 interfaces en un EtherChannel
		1. 8 activas y 8 en *standby*

1. Elegimos las interfaces que conformarán el EtherChannel
2. Usamos el comando "channel-group </numero_interfaz_virtual> mode </mode>"
	1. Configura la interfaz como parte del *ether* y lo crea si es que no existe
	2. Podemos ver la interfaz con el comando  "show ip int brief"
	3. El numero del grupo no es necesario que sea el mismo con el del *switch* vecino
3. Existe el comando "channel-protocol </protocolo>"
	1. Le dice a las interfaces que protocolo se usará
4. Podemos configurar esta interface virtual formada, como cualquier otra interfaz física
	1. Como asignándola como *trunk*
5. Las interfaces que se configuren dentro del mismo *ether* deben tener la **MISMA CONFIGURACIÓN**
	1. Mismo duplex
	2. Misma velocidad
	3. Mismo *switchport mode*
	4. Mismas [[VLAN|VLAN's]], para *trunks*
6. Podemos ver la info sobre el *ether* con el comando "show etherchannel summary"
7. También podemos usar el comando "show ether-channel port-channel"

### L3 EtherChannel

Si usamos *[[Multilayer Switch]]* para las conexiones entre *switches*, no necesitaríamos [[STP]]. Debido a que los *routed ports* **no *forwardean broadcasts***.

1. Elegimos las interfaces
2. Usamos el [[CLI Commands|comando]] "no switchport" para formar las interfaces L3
3. Creamos el *ether*
4. Asignamos [[IPv4 Addressing|IP]] al *port channel*


Nota: LAG (Link Aggregation Group)??????
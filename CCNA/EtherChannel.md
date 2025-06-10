
If you connect two switches together with multiple links, all except one will be disabled by [[STP]].
EtherChannel groups multiple interfaces together to act as a **single interface**. It's represented by drawing a circle around the interfaces that are grouped together.
El tráfico  que pase por un "EtherChannel" va a ser *load balanced* entre las interfaces físicas en el grupo. Un algoritmo será usado para determinar que tráfico usará que interfaz física. STP tratará a este grupo como una sola interfaz.

When the bandwidth of the interfaces connected to end hosts is greater than the bandwidth of the connection to the distribution switches, this is called **oversubscription**.


ASW: Access layer switch. A switch that end hosts connect to
DSW: Distribution layer switch. a Switch that access layer switches connect to

## EtherChannel load-balancing

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

## Protocolo LACP

Link Aggregation protocol, correspondiente al estándar 802.3ad, es un protocolo de balanceo de cargas que se usa en un EtherChannel. Dinámicamente negocia la creación y mantenimiento de él. Puede aceptar hasta 16 interfaces en el canal, 8 activas y 8 en *standby*
Posee 2 modos:
- **Active**
	- Activa LACP de manera incondicional
- **Passive**
	- Activa LACP solo si se detecta otro equipo con LACP

Otro protocolo que se puede usar es Port Aggregation Protocol (PAgP). Es propietario de Cisco.
Incluye 2 modos:
- **Desirable**
	- Activa PAgP de manera incondicional
- **Auto**
	- Activa PAgP solo si se detecta otro equipo con PAgP

Finalmente, podemos elegir no configurar ningún protocolo. Este se llama **"Static EtherChannel"**. Nosotros seremos los responsables de las configuraciones.
Solo posee el modo **"on"** para activarlo


## Configuración EtherChannel

**Antes de configurar, veamos comandos informativos**
1. Con **show etherchannel load-balance**
	1. Nos muestra el método actual de *load-balancing* e info asociada
2. También **"show etherchannel summary"**
	1. Nos muestra el estado del "EtherChannel"
3. Y **"show ether-channel port-channel"**
	1. Nos muestra el modo del canal que configuramos y otras cosas

**Si queremos cambiar el método por el cual realizaremos el balanceo**
1. Usamos el comando **"port-channel load-balance </metodo>"**
2. Tenemos varias opciones para balancear, entre IP, MAC de origen, destino o origen y destino

**Ahora elegimos el rango de interfaces que usaremos para el "EtherChannel"**
1. con **"interface range </rango>"**

**Después, configuramos y creamos el canal**
1. Usando el comando **"channel-group </numero_interfaz_virtual> mode </mode>"**
	1. Configura las interfaces como parte del canal y lo crea
	2. No es necesario que el número del grupo sea el mismo con el del *switch* vecino
	3. La interfaz se verá en **"show ip int br"**


**Comando inútil**
1. Existe el comando "channel-protocol </protocolo>"
	1. Le dice a las interfaces que protocolo se usará
	2. Si con el comando anterior le configuramos un protocolo distinto al configurado aquí, tirará error
	3. No crea un canal y no hace nada

### Notas
1. Podemos configurar el EtherChannel como cualquier otra interfaz física
	1. Como asignándola como *trunk*

2. Las interfaces que se configuren dentro del mismo *ether* deben tener la **MISMA CONFIGURACIÓN**
	1. Mismo duplex
	2. Misma velocidad
	3. Mismo *switchport mode*
	4. Mismas [[VLAN|VLAN's]], para *trunks*

### L3 EtherChannel

Si usamos *[[Multilayer Switch]]* para las conexiones entre *switches*, no necesitaríamos [[STP]]. Debido a que los *routed ports* **no *forwardean broadcasts***.

1. Elegimos las interfaces
2. Usamos el [[CLI Commands|comando]] "no switchport" para formar las interfaces L3
3. Creamos el *ether*
4. Asignamos [[IPv4 Addressing|IP]] al *port channel*


Nota: LAG (Link Aggregation Group)
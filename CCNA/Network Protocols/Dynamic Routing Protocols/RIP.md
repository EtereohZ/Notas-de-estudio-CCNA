Routing Information Protocol

- Utiliza algoritmo "Distance Vector" y es [[Dynamic Routing|IGP]]
- Usa *hop count* como su métrica
	- Máximo de 15 *hops*
- Posee 3 versiones
	- RIPv1 y RIPv2: Usan [[IPv4 Addressing|IPV4]]
	- RIPng: Usa [[IPv6]]
- Usa 2 tipos de mensajes
	- **Request**
		- Le pide a otros [[Router|routers]] que estén usando RIP que envíen su *routing table*
	- **Response**
		- Usado para enviar la *routing table* a *routers* vecinos
- *Routers* con RIP compartirán su *routing table* cada 30 segundos

## Versiones

### RIPv1
- Versión muy antigua y casi ni se usa
	- **No usar**
- Solo anuncia direcciones *classful* (clase A, B, C)
- No soporta [[VLSM]] o [[CCNA/Subnetting/Subnetting|CIDR]]
- No incluye información de la [[Subnet Mask]] en sus anuncios
- Sus mensajes son *broadcasteados* a 255.255.255.255

### RIPv2
- Soporta VLSM y CIDR
- Incluye la info de las *netmask* en sus anuncios
- Sus mensajes son *multicast* a **224.0.0.9**
	- Mensajes *multicast* solo son enviados a los equipos que están dentro del *multicast group*

## Configuración

1. Ingresamos comando "router rip"
2. Elegimos la version, en este caso con el comando "version 2"
3. Seguimos con el comando "no auto-summary"
	1. "auto-summary" convierte automáticamente las redes que anuncia a *classful*, cosa que no queremos
4. Usamos el comando "network </direccion_de_red>"
	1. Este comando le dice al *router* en que interfaces activaremos RIP
	2. Usaremos la imagen abajo de ejemplo, red de 10.0.0.0
		1. "network" le dice al *router* que
			1. Busque interfaces con una dirección IP en el rango especificado
			2. Luego, activará RIP en las interfaces que se encuentren en ese rango
			3. Formará *adjacencies* con *routers* vecinos RIP
			4. Anunciará a los vecinos el prefijo real de la interfaz
		2. al ser "network" *classful*, se asume que 10.0.0.0 es 10.0.0.0/8
		3. 10.0.12.1 y 10.0.13.1 calzan, así que RIP se activará en las interfaces g0/0 y g1/0 
			1. Ambos calzan en los primeros 8 bits (/8)
5. Luego, usamos el mismo comando en 172.16.1.0/28
	1. "network 172.16.0.0"
	2. EL *router* buscará interfaces con direcciones IP que calcen con 172.16.0.0/16
	3. Activará RIP en su interfaz g2/0
	4. Como no hay vecinos RIP en esa red, no se formarán nuevas *adjacencies*
	5. El *router* anunciará la red 172.16.1.0/28 a sus vecinos RIP
		1. Aunque no hay vecinos RIP conectados a g2/0, el *router* igual enviará anuncios RIP a g2/0, lo que no queremos
6. Usamos el comando "passive-interface </interfaz>"
	1. En g2/0 
	2. Le dice al *router* que deje de enviar anuncios fuera de la interfaz especificada
	3. Usar este comando en interfaces que no tienen vecinos RIP
7. Creamos una *default route*
8. Podemos compartir esta *default route* a los otros *routers* con el comando "**default-information originate**"
	1. así anunciará la *default route* a los otros
9. Usamos el comando "show ip protocols"
	1. Puede usarse también en [[EIGRP]] y [[OSPF]]
![[RIP_example.png]]
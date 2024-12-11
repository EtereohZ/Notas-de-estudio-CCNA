Dynamic Host Configuration [[Networking Protocols|Protocol]] is used to allow hosts to dynamically learn various aspects of their network configuration, such as [[IPv4 Addressing|IP addresses]], [[Subnet Mask]], [[Routing|Default Gateway]], [[DNS|DNS Server]], etc, without manual configuration.
Equipos como *[[Router|routers]]*, servidores, etc, son manualmente configurados.

En redes pequeñas, el *router* generalmente actual como el servidor DHCP para *hosts* en la [[LAN]].

- Servidores DHCP usan UDP puerto **67**
- Clientes DHCP usan UDP puerto **68**

## DHCP Release

Podemos mandarle un mensaje DHCP de tipo "release" a nuestro servidor DHCP diciéndole que liberemos el IP que actualmente estamos usando en nuestro PC.
El comando es **"ipconfig /release"**. 

Podemos pedirle al servidor un nuevo ip con **"ipconfig /renew"**, iniciando la cadena de conseguir un IP

## Proceso para pedir IP - DORA

1. El equipo enviará un mensaje "DHCP Discover" como *broadcast* para averiguar si hay servidores DHCP en la red
	1. El IP de origen que el equipo asignará al paquete es 0.0.0.0
	2. Solicitará el IP que tuvo anteriormente
2. El servidor DHCP responderá con un "DHCP Offer"
	1. Será enviado al equipo y ofrecerá una IP para el equipo e info como *default gateway*, etc
3. El equipo responde con un "DHCP Request" diciéndole al servidor que quiere usar el IP que le fue ofrecido en el paso anterior, con un *broadcast*
4. Por último, el servidor responderá con un "DHCP Ack", diciéndole al equipo que puede usar el IP



## DHCP Relay

Podemos configurar un *router* como un "DHCP Relay Agent", logrando que el *router* *forwardee* los mensajes DHCP *broadcast* de los clientes como mensajes *unicast*, al servidor DHCP. El servidor responderá de forma normal.
Esto se usa para que podamos tener un servidor DHCP centralizado, en vez de varios servidores separados.



## Configuración DHCP Server

1. Primero, usamos el comando **"ip dhcp excluded-address </ip_inicial> </ip_final>"** para especificar un rango de direcciones que NO será dado a clientes DHCP y así reservarlos
	1. No es obligatorio
2. Seguimos con **"ip dhcp pool </nombe_de_pool>"** para crear una "DHCP Pool" 
	1. Es una *subnet* de direcciones que pueden ser asignadas a clientes DHCP
	2. Se debe crear una *pool* distinta para cada red en la que el *router* está funcionando como servidor DHCP
3. Ahora especificamos el rango de direcciones que podrán ser asignados a clientes con el comando **"network </ip_de_red> </prefijo>"**
4. Configuramos el servidor [[DNS]] que los clientes DHCP usarán con el comando **"dns-server </ip>"**
5. Especificamos el dominio que tendrá la red con **"domain-name </nombre>"** (opcional)
6. configuramos la *default gateway* con **"default-router </ip>"**
	1. Esto hará que el servidor DHCP le diga a sus clientes que usen esta dirección
7. Podemos configurar el *lease time* con **"lease </dias> </horas> </minutos>"** o que sea infinito (no recomendado)

- Podemos ver que clientes que fueron asignados direcciones IP con **"show ip dhcp binding"**
- Podemos ver las *pools* que existen y los rangos de IP excluidos con el comando **"show running-config | section dhcp"**

## Configuración DHCP Relay Agent

1. Primero, debemos entrar a la interfaz conectada a la *subnet*
2. Ingresamos el comando **"ip helper-address </ip_servidor_dhcp>"**
	1. Recordar que el *router* debe tener como llegar al servidor
	2. Este comando debe usarse en la interfaz que está conectada a los clientes DHCP
3. Podemos revisar esa config. en **"show ip interface </interfaz>"**
	1. Nos mostrará la IP del servidor


## Configuración DHCP Client

Esto nos permitirá configurar a un *router* como cliente DHCP y así poder asignarle un IP a sus interfaces.

1. Elegimos la interfaz a configurar
2. Ingresamos el comando **"ip address dhcp"** para decirle al *router* que use DHCP para aprender el IP de la interfaz seleccionada

- Podremos ver si DHCP se usó para asignarle un IP a esa interfaz con el comando **"show ip interface </interfaz>"**
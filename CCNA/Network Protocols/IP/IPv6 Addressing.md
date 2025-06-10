An IPv6 address has 128 bits, 2<sup>128</sup> possible addresses

IPv6 usa [[NDP]] en vez de [[ARP]] para averiguar direcciones [[MAC Address|MAC]]

## Abreviación de direcciones IPv6

- Los 0's que estén al inicio de cada cuarteto pueden ser eliminados
	- 0DB8->DB8 - 001B->1B
- Cuartetos consecutivos de puros 0's pueden ser eliminados y reemplazados por ":"
	- 2001:0DB8:0000:0000:0000:0000:0080:34BD->2001:0DB8::0080:34BD
- Podemos combinar ambos métodos
- 2001:0DB8:0000:0000:0000:0000:0080:34BD->2001:0DB8::0080:34BD        ->2001:DB8::80:34BD

Cuartetos consecutivos de 0's solo se pueden abreviar **UNA VEZ**



## Encontrar prefijo IPv6

- Típicamente una empresa recibirá un prefijo /48 de su ISP
- Usualmente [[CCNA/Subnetting/Subnetting|subnets]] usan prefijos de /64
	- Lo que significa que una empresa tendrá 16 *bits* para hacer subnets
- Los 64 *bits* que quedan pueden ser usados para *hosts*

1. Usando de ejemplo 2001:0DB8:8B00:0001:0000:0000:0000:0001/64
2. Convertimos la porción del *host* en puros 0's
	1. 2001:DB8:8B00:1::/64
		1. Abreviamos igual

1. Ahora 2001:0DB8:8B00:0001:FB89:**017B**:0020:0011/93
2. Dado que el prefijo no es divisible por 4, tenemos que entrar dentro del carácter hexadecimal para encontrar el *bit*
3. Ya que "017" tiene prefijo de /92 determinamos que /93 se encuentra el primer *bit* de "B" en 017B
4. Pasamos B a binario ->1011
5. Convertimos todos los números a la derecha de nuestro *bit* en 0    1011 -> 1000 con el resto de los hexadecimales a la derecha
6. Ahora convertimos a hexadecimal 1000 -> 8
7. reemplazamos la "B" por nuestro nuevo carácter hexadecimal
	1. 2001:0DB8:8B00:0001:FB89:0178:0000:0000/93
	2. 2001:DB8:8D00:1:FB89:178::/93



## Configuración direcciones IPv6 en *router*

1. Ingresamos el comando "ipv6 unicast-routing"
	1. Esto permite al *router* hacer routeo IPv6
2. Con el comando "ipv6 address </direccion_ipv6></prefijo>" le asignamos dirección Ipv6  la interfaz

Luego de asignar IP, si revisamos "show ipv6 interface brief", nos podemos dar cuenta que la interfaz tiene 2 direcciones ip, 1 extra a la que configuramos nosotros. Estas direcciones se configuran automáticamente cuando nosotros configuramos una IPv6, y se llaman "Link-Local Addresses".


### Configuración de IPv6 con EUI-64

Es un método para convertir direcciones [[MAC Address|MAC]] (48 *bits*) a un identificador de interfaz de 64 *bits*.
Este identificador puede usarse para volver la porción del *host* de una dirección IPv6 /64.

1. Elegimos interfaz
2. Ingresamos el comando **"ipv6 address </red></prefijo> eui-64"**


### Configuración de IPv6 con SLAAC

Stateless Address Auto-Configuration

Es otra manera de configurar direcciones IPv6's.
*Hosts* usan los mensajes RS/RA para aprender el prefijo IPv6 del *local link* (ej. 2001:db8::/64)  y así automáticamente generar una dirección IPv6, a diferencia de **EUI-64**.
Luego de aprender el prefijo, usará **EUI-64** para generar la dirección IPv6.

Se puede configurar con el comando **"ipv6 address autoconfig"**


## Tipos de direcciones Ipv6

### Global Unicast

- Son direcciones Ipv6 únicas que se pueden usar en internet
- Se deben registrar para poder usarlas, deben ser únicas
- Primeros 48 *bits* son parte del "global routing prefix"
	- Los siguientes 16 pertenecen a la *subnet*
- Equivalen a las [[IPv4 Addressing]] públicas


### Unique Local

- Son direcciones privadas que no se pueden usar en internet.
- No es necesario registrarlas, se pueden usar libremente dentro de una red interna.
- Sus primeros 2 dígitos (8 *bits*) deben ser "FD"
	- Los siguientes 40 *bits* se llaman "Global ID" y se recomienda que sean generados aleatoriamente
	- Los últimos 64 *bits* se usan para la porción del *host*, y se llama "interface identifier"
- Equivalen a las IPv4 privadas


### Link Local

- Son direcciones IPv6 generadas automáticamente en interfaces son IPv6 habilitado
- Estas direcciones se usan para comunicaciones dentro de la misma *[[CCNA/Subnetting/Subnetting|subnet]]*. *[[Router|Routers]]* **no *routearán* paquetes** con estas direcciones
- Usos comunes:
	- OSPFv3 para *adjacencies*
	- [[NPD]]: Reemplazo de IPv6 para [[ARP]]
- Usamos el comando "ipv6 enable" para habilitar IPv6 sin tener que asignar una dirección
	- Genera una dirección IPv6 Link Local
- Empiezan con "FE8"
- Se generan usando las reglas de EUI-64


### Multicast

- Comunicación de **uno a muchos**
- Direcciones *multicast* IPv6 empieza con "FF"
- IPv6 no usa *broadcast*
- Los números en los que terminan las direcciones *multicast* en IPv6 son los mismos en [[IPv4 Addressing|IPv4]]
- Terminado en ":1" actúa como si fuera *broadcast*

Propósito de distintos *multicast*
![[IPv6_multicast.png]]

#### Multicast Address Scopes

Ipv6 define varios *scopes* que determinan cuan lejos se *forwardeará* un paquete.

Scopes:
- **Interface-local** (FF01): El paquete no sale del equipo. Se puede usar para enviar tráfico dentro del mismo equipo
- **Link-local** (FF02): El paquete se queda dentro de la *subnet*. *Routers* no *routearán* paquetes fuera de ella
- **Site-local** (FF05): Si puede ser *forwardeado* por *routers*. Debe ser limitado a solo **un** lugar (no sobre la WAN)
- **Organisation-local** (FF08): Más amplias que **"site-local"**
- **Global** (FF0E): Sin límites

Imagen representativa de los *scopes*:
![[IPv6_scopes.png]]



### Anycast

Nuevo en IPv6, funciona como "one-to-one-of-many".
Se refiere a que hay varios posibles destinos, pero el tráfico sólo se envía a 1. 

En "anycast", varios *routers* están configurados con la **misma dirección IPv6** y anuncian esa dirección mediante protocolos de *routeo*. Cuando un *host* envía un paquete a esa dirección, los *routers* lo *forwardearán* **al *router* más cercano con esa IP**, basándose en métricas.

No hay rango de direcciones específicas para "anycast".

- Podemos configurar una dirección "anycast" con el comando "ipv6 address </direccion_ipv6></prefijo> anycast"
- La podemos revisar con "show ipv6 interface </interfaz>"

Cuadro comparativo:
![[routing_schemes.png|200x700]]

### Otras

#### ::

Es inespecífico, se puede usar cuando un dispositivo no sabe su dirección IPv6
	- *Default routes* IPv6 se configuran a ::/0
	- Es el equivalente a 0.0.0.0

#### ::1

Es la dirección *loopback* de IPv6, mensajes que se envíen a esta dirección serán procesados dentro de si mismo, sin ser enviados fuera.

Es el equivalente a 127.0.0.0/8.
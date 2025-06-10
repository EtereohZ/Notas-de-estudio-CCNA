 It's the primary L3 protocol in use today, and version 4 is the one that is used the most.

Un IPv4 tiene un largo de 32 bits, por lo que cada grupo de números representa 8 bits o 1 byte.
Nota: Revisar el *header* en [[IPv4 Header]]

Dirección IP de ejemplo: 192.168.1.0/24:

- Cada grupo de números es una representación de [[Hexadecimal - Decimal - Binario|binario]] 
- Los primero 3 grupos de números representan la red en sí, el **último numero** representa a **equipos específicos**.
	- El prefijo **determina** que porción del numero pertenece a la **porción de la red**
- si la porción del host termina en "0", significa que es la **dirección de la red misma**, como en el IP de arriba
	- Ej. 43.0.0.0/8 - 128.43.0.0/16
		- La dirección de la red es siempre el número más bajo posible.
- Si la porción del host termina en "255" o solo octetos de 1, esa porción es la **dirección broadcast** de la red. y no puede ser asignada a un equipo
	- No es lo mismo en [[CCNA/Subnetting/Subnetting|subnets]]
- El prefijo "/24" se refiere a que 256 hosts se pueden conectar a esa misma red
	- Sabemos que son 256 hosts porque elevamos 2<sup>bits de la porcion del host</sup> = 256 - 2
	- Se refiere a que los primeros 24 bits están reservados para representar a la red, y los restantes 8 bits representan al equipo
	- En equipos Cisco, el prefijo se escribe como la [[Subnet Mask|netmask]]
	- Se escribe en decimales punteados, igual que una dirección IPv4.
	- Por ejemplo, La subnet para una red de clase A es "255.0.0.0"
- El máximo de equipos que se le puede conectar a una red es 2 elevado a los bits de la porción del host.
	- Ej. red de dirección 192.168.1.0/24 >>> 2<sup>8</sup> - 2

**Nota**: 11111111 en binario es **255**
##### Direcciones IP están divididas en 5 clases distintas.

La clase está determinada por el **primer octeto** de la dirección.

Las direcciones de clase suelen terminar en 126, no 127. Esto se debe a que el 127 está reservado para *Loopback Address*. el *Loopback Address* se usa para probar el *network stack* (OSI, TCP/IP) en el equipo local.
Si un equipo envía cualquier trafico de red a alguna dirección en ese rango, será procesada dentro del stack TCP/IP como si fuera tráfico recibido por otro equipo.

- Direcciones clase A
	- Tienen el prefijo **"/8"**
	- En equipos Cisco,  la *netmask* es "255.0.0.0" o un octeto de puros 1
	- Considerar el rango desde 1-126 en vez de 0-127
- Direcciones clase B
	- Tienen el prefijo **"/16"**
	- En equipos Cisco, la netmask es "255.255.0.0" o dos octetos de puros 1
- Direcciones clase C
	- Tienen el prefijo **"/24"**
	- en equipos Cisco, la netmask es  "255.255.255.0" o tres octetos de puros1
- Direcciones clase D
	- Las direcciones en la clase **D** están reservadas para direcciones multicast.
- Direcciones clase E
	- Las direcciones clase **E** están reservadas para direcciones experimentales
![[IPv4_addressing_class.png]]

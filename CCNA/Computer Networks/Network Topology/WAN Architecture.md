It stands for Wide Area Network

Una WAN es una red que se extiende a lo largo de una gran área geográfica. Estas se usan para conectar [[LAN|LAN's]] separadas geográficamente.
Aunque se puede considerar el Internet como una WAN, típicamente se usa para referirse las conexiones de una empresa para conectarse a sus oficinas, servidores y otros sitios.
***

## Leased Lines

Es un *link* físico dedicado, conectando a 2 sitios entre si. Usan conexiones [[OSPF Networks|seriales]] (encapsulación PPP o HDLC)

Dado que estas son mas caras y son mas lentas, la tecnología Ethernet WAN se está haciendo más popular.
***


## MPLS

significa "Multiple Protocol Label Switching".
Permite la creación de [[VPN|VPN's]] dentro de la infraestructura MPLS a través del uso de *labels*. Estas *labels* se usan para separar el tráfico de distintos clientes, y evita que se mezcle. También se usan para realizar decisiones en cuanto a donde *forwardear* los paquetes, no el IP de destino.

términos importantes:
- **CE Router**
	- El *router* del cliente que se conecta al siguiente
- **PE Router**
	- Se encuentra entre los "CE Router" y "P Router"
	- Estos son los que agregan las *labels*
- **P Router**
	- Los *routers* que forman la red del ISP
***

## Internet

### DSL

"Digital Subscriber Line" provee conectividad a internet a través de líneas de teléfono.

- **Modem**
	- Se refiere a modulador-demoludador y se requiere para convertir la data en un formato que pueda ser enviado a través de internet

### Cable Internet

Provee internet a través de las mismas líneas de TV cable.


### Redundant Internet Connection

Se refiere a la redundancia de conexiones a internet

1. Si tenemos una conexión a 1 ISP se llama "**Single Homed**"
2. Si tenemos 2 conexiones a 1 ISP se llama "**Dual Homed**"
3. Si tenemos 1 conexión, cada una a un ISP distinto, se llama "**Multihomed**"
4. Si tenemos 2 conexiones, cada una a 2 ISP's distintos, se llama "**Dual Multihomed**"


### Internet VPN's

#### Site-to-Site VPN

Es una VPN entre 2 equipos y se usa para conectar 2 sitios entre si, a través de internet.
Se crea un "túnel" entre los 2 equipos donde se encapsulan paquetes con un *header* del VPN y un nuevo *header* [[IPv4 Addressing|IP]]. Utiliza IPsec para encapsular la información.

Limitaciones:
- No soporta trafico *broadcast* o *multicast*. Esto significa que no se puede usar [[OSPF]] u otros protocolos de *routeo*
	- Se puede resolver con "GRE over IPsec"
		- Permite usar varios protocolos L3 y enviar mensajes *broadcast* y *multicast*, además de mantener la encriptación
	- Configurar varios túneles entre sitios distintos toma tiempo

##### DMVPN

DMVPN es una solución desarrollada por Cisco que permite dinámicamente a los *routers* crear un *mesh* de túneles IPsec sin tener que configurar manualmente cada tunnel.


#### Remote Access VPN

Estas se usan para permitir acceso seguro a  *end devices* como teléfonos o computadores, a los recursos internos de una empresa. Generalmente usan TLS para encriptar la información.
Stands for Address Resolution Protocol.
It's used to discover the L2 address ([[MAC Address]]) of a known L3 address ([[IPv4 Addressing|IP address]]).

Consiste en 2 mensajes:
 - ARP request
	 - Enviado por el equipo que quiere **averiguar la MAC** de otro equipo
	 - Es enviado como un ***broadcast*** a todos los *hosts* en la red
	 - La destination MAC en el ARP Request es enviado como "FFFF.FFFF.FFFF"
		 - Esa MAC se usa cuando un equipo quiere enviar frames a todos los otros equipos en la red
 - ARP Reply
	 - Respuesta enviada para **informar** sobre la MAC solicitada
	 - la ARP Reply es unicast, solo le llega a quien realizó la ARP Request

Cuando queramos enviar un paquete fuera de nuestra red, necesitaremos generar un *ARP request* a nuestro router para mandarle el paquete. Ya que él será el encargado de enviar nuestro paquete.
Entonces, la dirección MAC de nuestro paquete será el router.
Cuando el *[[router]]* envié el paquete, necesitará la dirección MAC de el proximo *router* en la ruta para poder mandárselo. Cada *router* necesitará la dirección MAC del siguiente *router* en la ruta, para que el paquete pueda llegar a su destino final. Cada dirección de origen y destino MAC del paquete irá cambiando acorde se va moviendo en la red.

Todos los equipos guardarán una tabla con las direcciones **ARP** que hayan aprendido.

## Visualización

**Solo necesitamos 1 comando para visualizar la tabla**
- Con **"show arp"**
	- Resolución L3 a L2
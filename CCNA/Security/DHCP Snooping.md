
Funciona asignándole confianza a los puertos "downlink" (apuntan a los *end hosts*) y "uplink" (apuntan a los equipos de red ). Los *uplink* se consideran de confianza y los *downlink* no.
DCHP *snooping* no inspeccionará mensajes que lleguen de puertos de confianza, pero si de los de no confianza.
Según [[DHCP]], los mensajes que enviarán los *hosts*, de los DORA, son "discover" y "request". Estos son los que serán inspeccionados en las interfaces *downstream*.

Un ejemplo de ataque que ayuda prevenir este método es contra "[[Security Fundamentals|DHCP Starvation]]" o "DHCP Poisoning.
***

## Inspección de puertos

Estos son las acciones que se tomará en los puertos  según los mensajes que recibe:
- Si se recibe un mensaje DHCP es un puerto de confianza -> se *forwardea* de forma normal
- Si se recibe un mensaje en un puerto de no confianza:
	- Si es de "DHCP Server", **se descarta**
	- si es de "DHCP Client", se inspecciona:
		- Para Discover/Request: Se verifica que la dirección [[MAC Address|MAC]] de origen del *frame* y el campo CHADDR de DHCP coincidan. Si coinciden, se *forwardea*
		- Para Release/Decline: Se verifica que el IP de origen del paquete y la interfaz que recibe calzan en la *entry* que se encuentra en la "DHCP Snooping Binding Table". Si coinciden, se *forwardea*



## Configuración

**Primero, habilitamos *snooping* desde *config mode***
1. con **"ip dhcp snooping"**
2. Ahora debemos activarlo en cada [[VLAN]] necesaria, con **"ip dhcp snooping vlan </numero>"**
	1. Acordarse de activan en VLAN 1

**Ahora entramos a las interfaces y configuramos puertos de confianza**
1. Con **"ip dhcp snooping trust"**
	1. Por defecto, todos los puertos son de no-confianza

**Revisamos la tabla de DHCP *snooping***
1. Con **"show ip dhcp snooping binding"**



### DHCP Snooping Rate-Limiting

Se usa para limitar la tasa de mensajes DHCP que se permite que entren en una interfaz. Si este limita se traspasa, la interfaz se deshabilita.
Se puede volver a habilitar de forma manual, o también con "ErrDisable Recovery"

#### Configuración

**Elegimos interfaz/es y elegimos el limite**
1. Con el comando **"ip dhcp snooping limit rate </mensajes_x_segundo>"**

**Podemos configurar la reactivación automática del puerto con**
1. **"errdisable recovery cause dhcp-rate-limit"**
	1. Mismo formato que para [[Port Security]]
***


## DHCP Option 82

También conocido como "DHCP relay agent information option". Provee más información acerca de cual "DHCP relay agent" recibió el mensaje del cliente, en que interfaz, VLAN, etc.
Estos mensajes son regularmente agregados por los *relay agents* que reciban el mensaje pero, con "DHCP Snooping" habilitado, los *switches* automáticamente agregarán está opción cuando reciban mensajes de clientes, aunque **no estén actuando como *relay agents***.

Si tenemos "DHCP Snooping" habilitado, los [[Switch|switches]] de Cisco automáticamente botarán mensajes que posean este mensaje, cuando sean recibidos en puertos de no-confianza.

**Se deshabilita en *conf t* con**
**"no ip dhcp snooping information option"**
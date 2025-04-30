Es una característica de seguridad de los *[[Switch|switches]]* de Cisco. Nos permite controlar que direcciones [[MAC Address|MAC]] son permitidas para entrar en las interfaces.

Cuando habilitamos seguridad de puertos en una interfaz, la configuración, **por defecto**, solo permitirá 1 dirección MAC. Esto se puede configurar. LA dirección permitida será la primera que el *switch* reciba, lo que puede provocar problemas.
Otra manera de proteger los puertos es limitar la cantidad de MAC's que serán permitidas en una interfaz.
***

## Configuración

**Para habilitar seguridad de puertos debemos entrar a la interfaz deseada**
1. Ingresamos el comando **"switchport port-security"**
	1. Sólo funciona en puertos  "access" o "trunk" configurados manualmente, no los "dynamic"
2. Cambiamos el modo de puerto, de ser necesario, con **"switchport mode {access|trunk}"**
	1. Podemos ver el tipo de puerto con **"show interface </interfaz> switchport"**
Nota:
- Revisar [[VLAN]] y [[DTP]]

**Ahora revisaremos la configuración de seguridad de puertos**
1. Con el comando **"show port-security interface </interfaz>"**
2. Revisar la información que presenta el comando más abajo
3. con **"show port-security"** vemos información general

**Ahora configuraremos manualmente la dirección MAC autorizada**
1. Con **"switchport port-security mac-address </direccion_MAC>"**

**Configuraremos como queremos que el *switch* responda a una violación de seguridad**
1. con el comando **"switchport port-security violation </tipo>"**

**Ahora modificaremos los tiempos de *aging* que tendrán las direcciones**
1. Con el comando **"switchport port-security aging type {absolute|inactivity}"**

**Como configuraremos una dirección MAC *sticky***
1. Con el comando **"switchport port-security mac-address sticky"**
2. Todas las direcciones MAC dinámicas se convertirán en *sticky*

**Pasos a seguir en caso de ocurrir una violación de seguridad**
Tenemos 2 maneras de volver a activar un puerto
Manual:
1. Desconectar el equipo no autorizado
2. Ingresar el comando **"shutdown"** y luego **"no shutdown"**

Usando "ErrDisable Recovery":
1. Con el comando **"errdisable recovery cause </tipo_de_causa>"**
	1. "psecure-violation" para cuando llega una dirección no autorizada
	2. Reinicia la interfaz luego del tiempo indicado
***

### Show port-security interface

Veremos las opciones que nos presenta el comando

1. **Port Status**
	1. si muestra "Secure-up" significa que tanto seguridad como la interfaz están activadas
2. **Violation Mode**
	1. Indica que pasará si algún frame no autorizado entra a la interfaz
	2. Tipos:
		1. **shutdown**
			1. Apaga la interfaz y envía una notificación [[Syslog]] o [[SNMP]]
		2. **Restrict**
			1. Bota tráfico de direcciones MAC no autorizadas, sin deshabilitar la interfaz
			2. Por cada violación manda un mensaje Syslog o SNMP
		3. **Protect**
			1. Descarta tráfico de direcciones no autorizadas, sin deshabilitar la interfaz
			2. No manda mensajes Syslog o SNMP
			3. Tampoco aumenta el contador de violaciones
		4. Direcciones MAC configuradas manualmente no caducarán
3. **Aging Time**
	1. Tiempo para que las direcciones aceptadas caduquen
	2. Tipos:
		1. **Absolute**
			1. Luego de aprender una dirección MAC, empieza un *timer* que eliminará esa dirección luego de terminar, independiente de si el *switch* recibe *frames* de élla
		2. **Inactivity**
			1. Empieza un *timer* después de aprender una dirección MAC y, cada vez que reciba un *frame* de ella, el *timer* se reinicia
4. **Aging Type**
5. **SecureStatic Address Aging**
6. **Sticky Secure MAC Address**
	1. Estas son direcciones que **nunca** caducarán, independiente de la configuración de seguridad
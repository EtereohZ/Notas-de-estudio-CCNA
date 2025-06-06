
Simple Network Management Protocol is an industry standard framework and protocol.

SNMP Agent trabaja con UDP en **puerto 161**
SNMP Manager Trabaja con UDP en **puerto 162**

Hay tipos de equipos que son manejados en SNMP:
1. Managed Devices
	1. Los equipos que son manejados por SNMP (*[[Router|routers]]*, *[[Switch|switches]]*, etc)
	2. Usamos SNMP para monitorear el estado de los equipos en la red
2. Network Management Station (NMS)
	1. Los equipos que se encargan de manejar los "Managed Devices"
	2. Este es el servidor SNMP


## SNMP Operations

Hay 3 tipos de operaciones que son usadas en SNMP:
1. "Managed Devices" pueden notificar al NMS de eventos
2. EL NMS pueden preguntarle a los equipos sobre su estado
3. El NMS le puede decir a los equipos manejados que cambien su configuración



## SNMP Components

### NMS Components

Representan al *software* del NMS:
1. **SNMP Manager**: Es el *software* que en NMS que interactúa con los equipos manejados
	1. Recibe notificaciones, envía solicitudes, configuraciones, etc
2. **SNMP Application**: Le provee una interfaz al administrator de redes 
	1. Se pueden ver alertas, estadísticas, gráficos, etc


### Managed Devices Components

1. **SNMP Agents**: Es el *software* SNMP que está corriendo en los equipos que interactúan con el SNMP *manager* en el NMS
	1. envía notificaciones y recibe mensajes del NMS
2. **Management Information Base**: Es la estructura que contiene las variables que son manejadas por el SNMP
	1. Cada variables es identificada con un "Object ID" (OID)
	2. Ejemplos: Estado de interfaz, tráfico, uso de CPU, temperatura

#### SNMP OID's

Son organizados de manera jerárquica

## SNMP Versions

- SNMPv1
	- version original

- SNMPv2c
	- Le permite al SNMP extraer grandes cantidades de información en una solicitud, siendo más eficiente ("GetBulk").
	- la "c" se refiere a claves usadas en SCNMpv1, que fueron eliminadas en la v2 y agregadas nuevamente en esta versión.

- SNMPv3
	- Versión mucho mas segura que ls anteriores: Soportan encriptación y autenticación

## SNMP Messages

![[SNMP_messages.png]]

Trap: Se envía una notificación del agente al *manager*. El *manager* no envía una respuesta afirmando que recibió el mensaje, por lo que se considera poco confiable.
Inform: Un mensaje de notificación que es *acknowledged* con un mensaje de respuesta


## Configuración de un SNMP Agent

1. Podemos asignar información opcional:
	1. "snmp-server contact </mail_o_lo_que_sea>"
	2. "snmp-server location </lugar>"
2. Podemos configurar contraseñas con **"snmp-server community </contraseña> </permiso>"**
	1. Se puede asignar mas de 1 contraseña
	2. Cada contraseña tendrá distintos permisos asociados:
		1. ro: Read Only. No puede usar mensajes "Set"
		2. rw: Read/Write. Puede usar mensajes "Set"
3. configuramos la dirección del NMS con **"snmp-server host </direccion_ip> </version> </clave>"**
	1. La clave que le asignemos tendrá que ser una de las que configuramos arriba y vendrá con los permisos asociados y será asociada a la dirección IP
4. Configuraremos las "traps" **"snmp-server enable traps snmp linkdown linkup"**
	1. En este comando, si una interfaz cae o se habilita, recibiremos una *trap*
	2. "snmp-server enable traps config"
		1. Si se cambia configuración, se enviará una *trap*
Syslog is an industry standard protocol for message logging. 
On network devices, Syslog can be used to log events such as changes in interface status, changes in OSPF neighbours status, system restarts, etc.
Both Syslog and [[SNMP]] are used for monitoring and troubleshooting of devices. They are complimentary, but their functionalities are different.

## Message Format

seq: No es tan importante
time stamp: Siempre incluir en mensajes
facility: que proceso originó el mensaje, ej. OSPF

![[syslog_message_format.png]]

### Niveles de severidad Syslog

Van de 0 a 7, 0 siendo el más severo de todos.
Nivel 5 se llama "Notice", pero en la [[Cisco CLI]] se llama "Notification".

![[syslost_severity_levels.png]]



## Syslog Logging Locations

¿A que distintos lugares pueden llegar los mensajes de Syslog?

- Console Line: Los mensajes Syslog serán mostrados en la CLI cuando se esté conectado al equipo mediante el puerto "console"
- **VTY Lines**: Los mensajes Syslog serán mostrados en la CLI cuando se esté conectado al equipo mediante [[Telnet]]/[[SSH]]
	- Deshabilitado por defecto
- **Buffer**:Los mensajes Syslog serán guardados en la RAM
	- Está habilitado por defecto
	- Podemos ver los mensajes con el comando **"show logging"**
- **External Server**: Se pueden configurar los equipos para que envíen sus mensajes Syslog a un servidor externo
	- Servidores Syslog recibirán mensajes en el puerto 514 UDP


## Configuración

1. con **"logging console </nivel>"** podemos elegir hasta que nivel de severidad queremos que se muestre, desde 0 hasta el nivel elegido, en la "Console Line"
2. Para la "VTY Line" usamos el comando **"logging monitor </nivel>"**
3. Para el "Buffer" usamos **"logging buffered  </tamaño> </nivel>"
	1. El tamaño es para tamaño en *bytes*
4. Para *logear* a un servidor externo usamos **"logging </direccion_ip>"**
	1. Este mensaje solo dice a que servidor, pero no especifica la severidad
	2. Con **"logging trap </nivel>"** elegimos la severidad


### Como mostrar notificaciones en "VTY Line"

Aunque tengamos configurado el "VTY Line" con "logging monitor", los mensajes Syslog no se mostrarán en SSH o Telnet, por defecto.

Si queremos que se muestren debemos usar el comando "terminal monitor".
- El comando debe usar usado cada vez que nos conectemos al equipo via SSH/Telnet

### logging synchronous

Este comando hará que una nueva línea se cree si es que un mensaje interrumpe el comando que estábamos ingresando.

### service timestamps / service sequence-numbers

Estos comando nos permiten habilitar/desactivar que se muestren los mensajes *sequence* o de *timestamp*.

Acordarse de "servive timestamp log datetime"


## Syslog vs SNMP

- Syslog se usa para *logear* mensajes
	- Los mensajes que ocurran dentro del sistema se categorizan
	- Usado para análisis y *troubleshooting*
	- Los mensajes se envían del equipo al servidor. El servidor no puede pedirle información a los equipos o configurarlos.

- SNMP se usa para obtener y organizar información de sus "managed devices"
	- direcciones IP, temperatura, uso CPU
	- Los servidores SNMP pueden usar "Get" para obtener información de sus clientes y configurarlos

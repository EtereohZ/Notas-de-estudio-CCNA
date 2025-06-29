
### Lista de comandos para la [[Cisco CLI|CLI]]

- **?**: Nos muestra los comandos disponibles
	- Usado después de una letra, por ej. "e?", nos dará todos los comandos que empiezan con esa letra
	- Si lo usamos después de escribir la palabra y apretar espacio, nos dará la siguientes opciones que podemos ingresar
		- Ej. enable password ?
			- Nos dará de opción "7", "LINE" "Level"
				- "LINE" en este caso se refiere a ingresar una clave
- **exit**: Nos saca de *Global Configuration Mode* y nos devuelve  a *Privileged EXEC mode
	- Si lo ingresamos de nuevo nos deslogea
- **show** </archivo> : Muestra en consola el archivo seleccionado 
	- Ej. "show running-config"
	- Se usa desde *Privileged EXEC Mode*
- **hostname </nombre>**: Nos permite cambiar el nombre del equipo
- **copy**: Copia archivo
- **do**: Es como el comando "sudo" de Linux, nos da permisos de admin en ese comando
	- Nos da permisos *Privileged EXEC Mode*
- **tracert </ping>**
	- Funciona como un ping que además nos va mostrando la ruta que va tomando el paquete
- **no**: cancela o elimina un comando ingresado
	- Ej. "no service password-encryption" hará que las próximas contraseñas no estén hasheadas
		- No funciona para las que ya existen
-
- **show ip interface brief**: Comando utilizado para confirmar los estados de las interfaces y las direcciones IP de algún equipo
	- **Interface**: Listado de las interfaces en el equipo
	- **IP-Address**: Muestra la dirección IP asignada a cada interface.
	- **OK?**: No es relevante, nos dice si el IP asignado es válido o no.
	- **Method**: Indica el método mediante el cual una IP fue asignada a una interface.
	- **Status**: Muestra el estado L1 de la interface
		- Deshabilitados por defecto en *routers*
		- Habilitadas por defecto en *switches*
		- Muestra si la interface está cerrada, si hay cables conectados. etc.
	- **Protocol**: Muestra el estado L2
		- Muestra si [[Ethernet Frame]] está funcionando correctamente entre este y el equipo conectado
- **show interfaces </nombre_interface>**: Nos muestra información detallada de L1, L2 y L3 de esa interface.
- **show interfaces description**: Muy parecido a "show ip interface brief", pero muestra una línea de descripción
- **show interfaces status**: Nos muestra estado e info de las interfaces
	- ***Status*:** Igual al **show ip interface brief**, solo que se ven distintos
	- ***[[VLAN]]***: Nos muestra a que VLAN está conectada la interfaz, si es *trunk*, si está *routeando*
	- **[[Full - Half Duplex|Duplex]]**: *a-full* significa que el *switch* automáticamente negoció usar Full Duplex con el equipo conectado
	- **Speed**: Muestra la velocidad
	- **Type**: Nos muestra la velocidad a la que pueden operar los cables conectados
- **shutdown**: Deshabilita una interface
- **interface </nombre_de_la_interface>**: Nos permite entrar al modo para configurar la interface especificada
	- "in g0/0" es la versión acortada del comando, podemos acortar tanto "interface" como el nombre de la interface, siempre y cuando se ponga su numero
	- Nos permite asignar la dirección IP para la interface seleccionada
	- **interface range </interface_inicial> - </interface_final>**: Nos permite elegir un rango de interfaces para configurar al mismo tiempo
	- **interface range </interface_1>, </interface_2>**: Nos permite elegir varias interfaces específicas, separando con una "," 
- **ip address** </direccion_ip> </subnetmask>: Lo ejecutamos estando en modo para configurar la interface
		- Nos permite asignar dirección IP a la interface seleccionada
- **no shutdown**: Usado para habilitar una interface seleccionada
- **description ## </descripcion> ##**: Nos permite ingresar una descripción para esa interface
		- No hay reglas sobre la descripción, pero puede ser utilizar poner hacia donde está conectado 
- **speed </velocidad>**: Nos permite elegir una velocidad especifica o automático
- **duplex**: Nos permite determinar que tipo de duplex queremos


- **default interface </interfaz>**
	- Devuelve las configuraciones al *default* de la interfaz seleccionada
- **interface </numero_de_vlan>**
	- Crea una *switch virtual interface* (SVI) para la VLAN seleccionada y configurarla
	- Le podemos asignar IP
	
- **show interfaces </interfaz> switchport**
	- Nos muestra información del switchport en al interfaz
	- **switchport**: Enabled si es un puerto L2, si lo configuramos como *routed port*, se verá como deshabilitado
	- **Administrative mode**: es lo que configuramos en la interfaz
	- **Operational mode**: Si es un *trunk* o *access port*. Es el resultado final de configuración manual o negociación [[DTP]]
	- **Administrative trunking encapsulation**: si tenemos la negociación de la encapsulación prendida o no
	- **Negotiation of Trunking**: Muestra si DTP está habilitado o no
- **switchport mode dynamic </auto_o_desirable>**
	- Para activar [[DTP]] y elegir el modo que queremos
- **switchport nonegotiate**
	- Desactivamos DTP

- **show vtp status**
	- Nos muestra estado e info de VTP en el *switch*
- **vtp mode </modo>**
	- Elegimos el modo VTP que queremos
- **vtp domain </nombre_dominio>**
	- Elegimos o cambiamos el nombre del dominio [[VTP]]




- Para comandos detallados de *[[Dynamic Routing|dynamic routing protocols]]* ver las paginas de cada uno:
	- [[OSPF]]
	- [[EIGRP]]
	- [[RIP]]
- **router </protocolo_de_routeo> </numero>**
	- nos permite elegir el protocolo que usaremos para *routeo* dinámico
	- Parte del numero se explicará. opcional
- **[[Comando show ip protocols|show]] ip protocols**
	- Nos mostrará info de los protocolos de *routing* usados
- **ip route </ip_de_la_red></netmask></next_hop></AD>**
	- Nos permite configurar la [[Dynamic Routing|AD]] de una ruta estática, puede convertirse en *floating static route*
- **show ip </protocolo_dinamico> </opcion>**
	- Nos permite ver más detalles sobre el tráfico, métricas y otras cosas de los protocolos
	- Revisar la página de cada uno para mas detalles
- **maximum-paths </numero>**
	- Nos permite configurar la cantidad máxima de *paths*
		- Todavía no sabemos que es
- **distance </distancia>**
	- Nos permite configurar la AD del protocolo que estamos usando
- **eigrp router-id </ID>**
	- nos permite configurar el *router* ID en [[EIGRP]]
		- Debe ser un número de 32 bits
- **default-information originate**
	- Usado en el protocolos de routeo dinámico para anunciar a los otros *routers* la dirección a la *default route*
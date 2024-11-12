

## 802.11 Frame Format

Algunos campos difieren entre versiones
![[Pasted image 20241112154807.png]]

- **Frame Control**
	- Da información acerca del tipo de mensaje y subtipo
- **Duration/ID**
	- Puede indicar:
		- El tiempo que el canal le dedicara a la transmisión del *frame*
		- Como identificador de la asociación
- **Address**
	- Pueden haber hasta 4 direcciones
	- **Destination Address**
		- A quien estará dirigido el *frame*
	- **Source Address**
		- Quien originalmente envió el paquete
	- **Receiver Address**
		- Receptor inmediato del *frame*
	- **Transmitter Address**
		- Enviador inmediato del *frame*
- **Sequence Control**
	- Usado para reensamblar fragmentos y eliminar *frames* duplicados
- **QoS Control**
	- Usado para priorizar tráfico
- **HT Control**
	- Habilita "High Throughput operations"
- **FCS**
	- Mismo que el de [[Ethernet Frame]], usado para *chequear* errores


## 802.11 Association Process

Para que un equipo pueda enviar tráfico a través de una AP, debe estar asociado a ella.
- Existen 3 tipos de conexión:
1. No autenticado, no asociado
2. Autenticado, no asociado
3. Autenticado y asociado
El equipo debe estar autenticado y asociado para poder enviar tráfico.

- Existen 2 maneras que un equipo puede usar para escanear para una BSS:
1. **Active Scanning**
	1. El equipo envía solicitudes y escucha respuestas de una AP
2. **Passive Scanning**
	1. El equipo escucha mensajes "beacon" de una AP
	2. Los mensajes "beacon" son enviados periódicamente por las AP's para anunciar sus BSS

## 802.11 Message Types

Existen 3 tipos de mensajes:
1. **Management**
	1. Usado para manejar la BSS
	2. Aquí se encuentran los mensajes:
		1. **Beacon**
		2. **Probe Request**, **Probe Response**
		3. **Authentication**
		4. **Association Request**, **Association Response**
2. **Control**
	1. Usados para controlar acceso a un medio
	2. Asiste en el envío de los *frames* "management" y "data"
	3. Se encuentran los mensajes:
		1. **RTS**
		2. **CTS**
		3. **ACK**
3. **Data**
	1. Usado para enviar los paquetes de data


## AP Deployment Methods

Existen 3 tipos inalámbricos principales de desplegar AP's

### Autonomous AP's

Estos son sistemas autónomos que no necesitan de un WLC, por lo que deben configurarse individualmente. Esto se puede hacer mediante la interfaz "console", usando Telnet, [[SSH]], o una conexión GUI, por lo que se le debe configurar una dirección [[IPv4 Addressing|IP]] para poder acceder remotamente.

Las AP autónomas deben conectase a la red alámbrica mediante una conexión "trunk". El tráfico de manejo que se usa para conectarse remotamente a las AP's, u a otros equipos, debe estar en una [[VLAN]] separada.

En las AP autónomas, cada VLAN debe extenderse a lo largo de toda la red, lo que se considera mala practica. 
En primer lugar, porque esto genera grandes "broadcast domains", provocando que cada mensaje se envíe por toda la red.
Otra razón es porque [[STP]] desactiva *link* y que la configuración de varias VLAN's toma harto tiempo.

Es por esto que las AP autónomas pueden usarse en redes pequeñas, pero no en medianas o grandes.

### Lightweight AP's

En esta, las funciones de la AP se pueden dividir en la misma AP o en WLC (Wireless LAN Controller).
Las AP's manejan las operaciones en tiempo real, como transmitir/recibir trafico RF, encriptación/decriptación, etc. Mientras que otras funciones como seguridad, QoS, autenticación, son manejadas por la WLC.
Tanto WLC como las AP's *lightweight* pueden autenticarse entre ellas usando certificados digitales que se encuentran en ellas.

Las AP's y WLC usan un protocolo llamado CAPWAP para comunicarse. Se crean 2 túneles entre cada AP y el WLC:
1. **Control Tunnel**
	1. Este túnel se usa para configurar las AP's y manejar operaciones
	2. El tráfico se encuentra encriptado por defecto
2. **Data Tunnel**
	1. Todo el tráfico de los clientes inalámbricos es enviado por este túnel al WLC, no pasa directamente por la red
	2. Tráfico no se encuentra encriptado por defecto, pero puede serlo, mediante **DTLS**

Dado que todo el tráfico es los clientes inalámbricos es enviado por un túnel al WLC, las AP se conectan a los *switches* por puertos "access", no "trunk"

#### Beneficios
1. Escalabilidad
2. Asignación dinámica de canales
3. Load-Balancing
4. Roaming
5. Seguridad

#### Lightweight AP Modes

1. **Local**
	1. Modo por defecto. La AP ofrece BSS para que los clientes se conecten
2. **Flex Connect**
	1. Ofrece 1 o más BSS, pero permite a las AP cambiar el tráfico entre alámbrico e inalámbrico si los túneles al WLC caen
3. **Sniffer**
	1. La AP no ofrece BSS, sino que se dedica a capturar *frames* 802.11 y enviarlos a algún *software* de captura
4. **Monitor**
	1. La AP no ofrece BSS, se dedica a recibir  *frames* 802.11 y detectar equipos bandio's
	2. Si encuentra alguno, la AP los des-autentica y des-asocia
5. **Rogue Detector**
	1. La AP se dedica a escuchar tráfico en al red alámbrica y recibir una lista de equipos sospechosos por parte de la WLC.
6. **SE-Connect**
	1. Se dedica al análisis en el espectro RF, en todos los canales. 
7. **Bridge/Mesh**
	1. Como el modo "outdoor bridge", la AP se vuelve un puente entre sitios a largas distancias
8. **Flex plus Bridge**
	1. Agrega la funcionalidad **"Flex Connect"** al modo **"Bridge/Mesh"**
	2. Permite que AP's inalámbricos *forwardeen* tráfico aun cuando se pierda la conexión al WLC

Imagen comparativa. A la izquierda la *lightweight* y a la derecha la autónoma

![[AP autonoma y lightweight.png]]


### Cloud Based AP's

es un mix de las autónomas y de las *lightweight*. Son manejadas en la *cloud*


## WLC Deployments

Solo existe en la arquitectura "split-MAC", existen 4 modelos principales:
1. **Unified**
	1. El WLC es un equipo *hardware* en un lugar central de la red
	2. Es la que más AP's soporta
2. **Cloud Based**
	1. EL WLC es una VM corriendo en un servidor
3. **Embedded**
	1. La WLC está integrada en un *switch*
	2. No logra soportar muchas AP's, unas 200
4. **Mobility Express**
	1. La WLC está integrada dentro de una AP
	2. Es la que menos AP's soporta
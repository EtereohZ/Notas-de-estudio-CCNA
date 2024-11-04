Open Systems Interconnection model is a [[Networking Model|conceptual model]] that categorizes and standardizes the different functions in a network. I was created by the ISO in the late 1970's.
Each function is dividen into 7 layers, and they work together.

La información se va procesando desde la Application layer hasta la Physical layer, cada layer agregando algo a la información original, hasta que se convierte en señales eléctricas en al Physical layer. Esto se llama encapsulación.
Luego se envía al otro sistema y este realiza el proceso opuesto, se van removiendo las capas extra hasta que la información llega a la Application layer. Esto es desencapsulación.


### OSI layers:
- **Layer 7. Application layer**:
	- Es el nivel más cercano al usuario
	- Interactúa con las aplicaciones de software como el browser
	- [[HTTP]] y [[HTTPS]] son protocolos de nivel 7
	- no incluye las aplicaciones en si, sino los protocolos que interactúan con ellas.
	- Funciones:
		- Identifica socios para comunicar
		- Sincronizar la comunicación
- **Layer 6. Presentation layer**:
	- La info que se encuentra en la App layer está en un tipo de formato de esa layer, y necesita traducirse antes de ser enviada
	- La Presentation layer está encargada de procesarla entre el formato en el que está y los formatos de red
		- Esto se refiere a la encriptación y desencriptación de la info cuando es recibida
	- Tambien traduce entre distintos App layer formats
		- Así se asegura de que el host que reciba la info la pueda entender
- **Layer 6. Session layer**
	- Controla las sesiones entre 2 hosts que se estén comunicando
	- Establece, maneja y cierra conexiones entre la aplicación local y la aplicación remota
- **Layer 4. Transport layer**
	- Segmenta y reensambla información para comunicar entre hosts
		- Separa grandes trozos de información y los separa en segmentos mas pequeños que son enviados mas fácilmente por la red
			- También es menos probable que cause problemas de transmisión
	- Proveen host-to-host communication o end-to-end
	- Provee servicios como (TCP):
		- Envío confiable de data
		- Recuperación de errores
		- Secuenciación de info en orden correcto
		- Control de flujo
	- Provee *addressing* L4 (puertos) y "session multiplexing"
		- El puerto destino identifica el protocolo de L7
		- El puerto de origen es elegido al azar por el PC, y ayuda a identificar la sesión
	- Agrega un L4 header al segmento de información
**Hasta este punto, la unidad de información se llama SEGMENTO o SEGMENT**
- Layer 3. Network layer
	- Provee conectividad entre hosts en diferentes redes (fuera de la [[LAN]])
	- Provee direcciones [[IPv4 Addressing|IP]]
	- Elige el mejor camino entre la fuente y el destino de la información
		- Pueden haber muchos caminos
	- [[Router|Routers]] trabajan en L3
	- Agrega un L3 header, que incluye la dirección [[IPv4 Addressing|IP]] de origen y destino al segmento
	- Con el L3 header, el segmento pasa a llamarse **packet** o **paquete**
	- L3 protocols
		- [[OSPF]]
- **Layer 2. Data Link layer**
	- Provee conexión ***node-to-node*** y transferencia de información
		- Por ejemplo, PC a switch, switch a router, router a router
	- Define el formato que tendrá la información al ser enviada dentro de un medio físico
		- Por ejemplo, [[UTP Cables|cables UTP]]
	- Detecta y posiblemente corrige errores en la Physical layer
	- [[Switch|Switches]] operan en L2
	- Agrega un L2 header y un L2 trailer que usan direcciones
	- Con el L2 header y trailer, pasa a llamarse **frame**
	- L2 protocols
		- [[STP]]
- Layer 1. Physical layer
	- Define las características del medio usado para transferir info entre equipos
		- Por ejemplo, niveles de voltage,  [[Connector|conectores físicos]], especificaciones de cables, pines, distancias máximas de transmisión, 
	- bits son convertidos en señales eléctricas o radio
	- Cubre los estándares de [[Ethernet]] de los [[cables]]

La info, los segmentos, paquetes y frames, todos son partes de PDU o Protocol Data Unit
Un segmento sería un L4 PDU, un paquete seria un L3 PDU y un frame sería un L2 PDU, un bit sería un L1 PDU




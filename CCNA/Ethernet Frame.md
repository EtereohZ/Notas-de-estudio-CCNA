When being represented in diagrams, the header is placed to the left, and the trailer to the right.
*header* y *trailer* tienen un **largo total  de 18 bytes**, usualmente el *Preamble* y el *SFD* no se consideran partes del header, considerándolos tendría 26 bytes.

un ethernet frame pertenece a L2 del modelo [[OSI]].

Un frame tiene un **tamaño mínimo** de **64 bytes**, considerando el *header*, el *payload* y el *trailer*.
64 - 18 = 46 bytes. Esto significa que el tamaño mínimo que debe tener el *payload* es de 46 bytes.
Un *frame* tiene un **tamaño máximo** de 1518 bytes o 1500 bytes. si no contamos el *header* y *trailer* del *frame*

Sí un *frame* tiene un *payload* de menor de **46 bytes**, se le agrega un ***padding*** de bytes para lograr el mínimo.

#### Header
- **Preamble**
	- Se usa para sincronizar los ***receiver clocks de los dispositivos*** y preparar al equipo para recibir el resto de info en el frame
	- 7 Bytes de largo = 56 bits
- **SFD-Start Frame Delimiter**
	- Marca el fin del *Preamble* y el inicio del resto del frame
	- Su patrón es 10101010
	- 1 Byte de largo = 8 bits
- **Destination**
	- Dirección L2 del equipo donde se está enviando el frame
	- Consiste en la dirección ***[[MAC Address|MAC]]*** del equipo
	-  6 bytes de largo == 48 bits
- **Source**
	- Dirección L2 del equipo donde se origina el frame
	- Consiste en la dirección ***[[MAC Address|MAC]]*** del equipo
	-  6 bytes de largo == 48 bits
- **Type**
	- Indica el protocolo L3 que se encuentra encapsulado en el *frame*
		- **[[IPv4 Addressing|IPv4]]** = 0X0800 == 34525 en decimal
		- **[[IPv6 Addressing|IPv6]]** = 0X86DD == 2048 en decimal
		- **[[ARP]]** = 0x0806
	- Si es valor es de >=1536 indica el **tipo** de paquete encapsulado
	- **2 Bytes** de largo == 16 bits
**OR**
- **Length**
	- Un valor de <1500 indica el largo del paquete encapsulado
#### Trailer
- **FCS** - Frame Check Sequence
	- Lo usa el equipo que recibe el frame para **detectar data corrompida**
		- Usa el algoritmo ***Cyclic Redundancy Check*** (CRC) en la info recibida
			- Algoritmo de código cíclico que agrega 4 bytes al mensaje sin agregar información nueva que verifica la info
	- **4 Bytes** de largo = 32 bits

**Importante**: Revisar frecuentemente [[Hexadecimal - Decimal - Binario]].
#### Diagrama

![[ethernet_frame_structure.png]]
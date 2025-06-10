
# Network Devices

## Switches

Switches are used to forward traffic within a LAN.
En una LAN están conectados todos los dispositivos conectados al mismo switch, estos pueden ser impresoras, PC's, celulares, etc.
Pueden haber varios switches dentro de una misma red.

Los switches no pueden conectarse directamente al internet u otras LAN's, necesitan algo mas. Necesitan conectarse a un Router.

Generalmente tienen >24 interfaces o puertos para que los dispositivos se puedan conectar.
Las interfaces vienen activadas por defecto, en equipos Cisco.

## Speed/Duplex Autonegotiation

Interfaces that can run at different speeds (10/100 or 10/100/1000) have default settings of *speed auto* and *duplex auto*
Interfaces "advertise" their capabilities to the neighboring device, and they negotiate the best speed and duplex settings they are both capable of.

¿Qué pasa si la auto negociación está deshabilitada en el equipo conectado al switch?
**Velocidad**: El *Switch* tratará de detectar la velocidad en la que el otro equipo está operando y, si no puede, usará la velocidad más baja soportada ( 10Mbps en una interfaz de 10/100/1000 Mbps).
**Duplex**: Si la velocidad es de 10 o 100 Mbps, el *switch* usará *half duplex*. Si es 1000mbps, usará *full duplex*.
Si un equipo tiene configurado manualmente *Full Duplex* y el switch tiene auto negociación activado, el switch no podrá negociar con ese equipo y activará *Half Duplex*, esa diferencia se llama ***Duplex mismatch*** y ocurrirán colisiones. 

## Interface Errors

- **runts**:  *Frames* que son mas pequeñas que el tamaño mínimo de un *frame* (64 bytes)
- **giants**: *Frames* que son mas grandes que el tamaño mínimo de un *frame* (1518 bytes)
- **CRC**: son los *frames* que fallaron el chequeo CRC (en el *Ethernet frame trailer)
- **frame**: *Frames* que tienen un formato incorrecto (debido a un error)
- **input errors**: Total of various counters of errors received by the device, como el total de los errores de arriba
- **output errors**: son los *frames* que el *switch* intentó enviar, pero fallaron


## Duplex


***Half Duplex*** means that the device cannot send and receive data at the same time. If it's receiving a frame, it must wait before sending a frame.
Un ejemplo de un equipo *half duplex* sería un *hub*.

**CSMA/CD** Significa *Carrier Sense Multiple Access with Collision Detection* y describe como equipos evitan colisiones en situaciones *half duplex* y como deberían actuar si ocurren colisiones.
1. Antes de enviar *frames*, "escuchan" la red hasta detectar que no hay otros equipos enviando frames
2. Si ocurre la colisión, el equipo envía una señal para informar a los otros equipos que ocurrió una colisión
3. Cada equipo espera un periodo de tiempo aleatorio antes de volver a enviar un frame
4. Se repite el proceso


***Full Duplex*** means that a device can send and receive data at the same time. It does not have to wait.



### Multilayer Switches

- It's capable of both switching and routing. It knows about layer 3, so we can assign IP addresses to its interfaces, like a router.
- You can create virtual interfaces for each VLAN, and assign IP addresses to those interfaces.
- It can be used for inter-VLAN routing

## Switch Virtual Interfaces

-  Llamados **SVI's**, son las interfaces virtuales en las que son asignadas las direcciones IP en un *multilayer switch*. Le asignamos las direcciones IP de cada red de cada VLAN; igual como lo haríamos en un *router*.
	- El *multilayer switch* tendrá su propia *routing table*, si le llega un *frame* destinado a VLAN 10, sabrá por que interfaz enviarlo.
- Usaremos el SVI como gateway address, en vez del *router*. Y configuraremos cada PC para usarlo.
- Para enviar el tráfico a cada subnet/VLAN, los PC's le enviarán el tráfico al *switch*, y el *switch routeará* el tráfico.
- Si algún *host* quiere contactar destinos fuera de la LAN, como el *multilayer switch* está actuando como nuestro *default gateway*, todos los paquetes serán enviados a él
	- Tendremos que asignar una subnet a nuestro *point-to-point* entre el *multilayer switch* y el *router*
	- Luego le configuramos una default route al *switch* con destino al *router*

## Configuración MS para routear

1. Esta parte debe realizarse primero que la configuración para *routeo* inter-VLAN

**Primero, eliminamos las subinterfaces en el *router* (si existiesen) y reiniciamos la interfaz
1. Con **"no interface </interfaz>"**
2. Y "default interface </interfaz>" 
	1. Devolvemos la configuración estándar

**Le asignamos una dirección IP a la interfaz que conectará con el MS**

**Ahora habilitamos la capacidad del *switch* para routear***
1. Con el comando **"ip routing"**
	1. Habilita *routeo* L3
	2. Permite al *switch* crear una tabla de *routeo*

**Seguido, entramos a, y configuramos, la interfaz que conectará con el router para poder asignarle un IP**
1. Con **"no switchport"**
	1. cambia la interfaz desde un *switchport* L2 a un L3 *routed port*

**Finalmente, le configuramos la *default route* al *MS***
1. Con el comando **"ip route 0.0.0.0 0.0.0.0 </ip_router>"**

**Si queremos ver la nueva interfaz del *switch***
1. Usar comando **"show interfaces status"**

### Configurar SVI para inter-VLAN routing

**Primero, creamos la SVI que será usada para *routear* VLAN's**
1.  Ingresar comando **"interface </numero_de_vlan>"**
	1. Ejemplo: **"interface vlan10"**
	2. Esto creará una SVI para esa VLAN
	3. La VLAN debe crearse por separado

**Ahora le asignamos una IP a esa SVI, como cualquier otra interfaz**
1. Con **"ip address </ip_address> </netmask>"**

**Prendemos la interfaz**
1. Usamos **"no shutdown"** 
	1. Están *shutdown* por *default*

**Si queremos que la SVI esté up/up**
1. La VLAN debe existir en el *switch*
2. El *switch* debe tener algún *access port* que esté up/up o algún *trunk port* que permita a la VLAN que esté up/up
3. LA VLAN debe estar prendida


## Routers

Routers are used to provide connectivity between LAN's, they send data over the internet.
Poseen menos interfaces que los switches.

Routers separan las redes y poseen una dirección distinta para cada red a la que está conectado.
Los routers no hacen *flood* de paquetes.

Con el router ya tenemos conexión al internet, pero necesitamos algo que proteja nuestra red, necesitamos un Firewall

Las interfaces vienen deshabilitadas por defecto en equipos Cisco.

Algunos modelos:
- ISR 1000
- ISR 900
- ISR 4000

#### Interface Errors

- **runts**:  *Frames* que son más pequeñas que el tamaño mínimo de un *frame* (64 bytes)
- **giants**: *Frames* que son más grandes que el tamaño mínimo de un *frame* (1518 bytes)
- **CRC**: son los *frames* que fallaron el chequeo CRC (en el *Ethernet frame trailer)
- **frame**: *Frames* que tienen un formato incorrecto (debido a un error)
- **output errors**: son los *frames* que el *switch* intentó enviar, pero fallaron

![[networking.png|200x200]]


## Firewalls

Are a specialty network security devices that monitor and control network traffic entering and exiting the network, they can be placed *inside* or *outside* of the Router.

- Se deben configurar con reglas para determinar que tráfico debe ser permitido o denegado.
- Firewalls modernos pueden ser [[NGFW|NGFW's]] e incluir [[IPS|IPS']]. Estos incluyen capacidades avanzadas de filtración y monitoreo de redes.
- Pueden ser software/Host-based Firewall o hardware

## Network Firewalls
These are hardware devices that filter traffic between networks.

Algunos modelos, ambos NGFW:
- ASA5500-X:
- Firepower 2100:


## Hubs
It floods any frame it receives with no regards as to the destination address.
Solo operan en L1

Solo soporta *Half Duplex*.

## IP Phones


Estos teléfonos usan VoIP (Voice over IP) para permitir llamadas de teléfono a través de la red, como internet.
Se conectan al switch, como otros dispositivos de internet y consiguen electricidad mediante PoE

Estos teléfonos poseen 3 puertos:
1. El "uplink": Se conecta a un *switch* externo
2. El "Downlink": Se conecta al PC
3. Y el último puerto, que se conecta al teléfono mismo
Esto permite al PC y al teléfono user solo 1 puerto en el *switch*, ya que el tráfico desde el PC pasará por el teléfono hacia el *switch*.
Se recomienda separar el tráfico de voz del telefono y el tráfico de información del PC, separandolos en distintas VLAN's.


## Configuración

**Primero debemos entrar a la interfaz del *switch* y configurar el puerto**
1. **"interface </interface>"**
2. **"switchport mode access"**
3. **"switchport access vlan </numero>"**
4. **"switchport voice vlan </numero>**
	1. Este el el que usará el telefono
	2. Este tráfico será *tageado *
A pesar de que esta interfaz lleva tráfico de más de 1 VLAN, no se considera como una "trunk".












# Interfaces & Cables

## Ethernet Standards

  Ethernet is a collection of network protocols and standards.

Ethernet standards are defined in the IEEE 802.3 standard from 1983.

Ethernet standards for copper wires:

![[copper_wire_standards.png]]
Nota para memorización de velocidades:
- En cada escalón de velocidad se le va agregando 1 cero

Ethernet standards for Fiber-Optic:
![[fiber_optic_ethernet_standards.png]]

## RJ-45

It's used on the end of a copper Ethernet cable. It stands for Unshielded Twisted Pair.
- Unshielded significa que no tienen escudo metálico, lo que los hace vulnerables a interferencia eléctrica.
- Twisted pair significa lo que dice el nombre y ayuda a proteger contra EMI.
- Pairs significa que hay 4 pares de cables doblados entre ellos.

## UTP Cables

These are the copper cables used in Ethernet standards.
Usan un conector RJ-45 en ambos puntos para conectarse a los equipos de red.
tiene un máximo de 100m

Hay 8 pins para cada UTP Cable, pero no todos los Ethernet standards usan los 8:
- 10BASE-T y 100BASE-T usan 2 pares
- 1000BASE-T y 10GBASE-t usan los 4 pares

**SOLO PARA 10BASE-T Y 100BASE-T**
Necesitamos 2 tipos de cables si queremos conectar ciertos dispositivos o mismos dispositivos entre si, ya que algunos usan los mismos pines para transmitir y recibir información.
- PC's , routers y firewalls transmiten por pines 1 y 2  y reciben por pines 3 y 6.
- Switches transmiten por pines 3 y 6 y reciben por pines 1 y 2.
- El uso de pines para transmitir y recibir información se llama Full-Duplex, y evita colisiones.
Tenemos 2 tipos de cables:
 - Straight-through-cable
 - Crossover Cable

Los cables 1000BASE-T y 10GBASE-T son bidireccionales, pueden transmitir y recibir por cualquier pin.

[[Auto MDI-X]] se utiliza para que los equipos automáticamente detecten que pines están usando para transmitir y recibir y así ajustar que pines se usarán para cada caso.

Nota: Filtran señales?????

 





### Straight-through Cables

Es un tipo de UTP cable en el cual los cables en determinada posición conectan a los pines en la misma posición.
 Los usamos para conectar Pc's con switches.

Conexiones de pines de 2 pares:
- El primer par se encuentra en posición 1 y 2 y conectará a los pines en posición 1 y 2 en el switch.
	- el pin 1 y 2 se usarán para transmitir información al switch.
- El segundo par se encuentra en posición 3 y 6 y se conectará a los pines en posición 1 y 2 en el switch.
	- el pin 3 y 6 se usarán para recibir información enviada desde el switch.

### Crossover Cables

Es un tipo de UTP cable en el cual los cables en determinada posición conectan a los pines en posición cruzada.
Los necesitamos si queremos conectar 2 switches, o 2 routers, o 1 PC y 1 router, debido a que usan los mismos pines para transmitir y recibir información.

- Cables 1 y 2 conectarán a los pines 3 y 6, respectivamente
	- Cables 1 y 2 recibirán info.
- Cables 3 y 6 conectarán a los pines 1 y 2, respectivamente.
	- Cables 3 y 6 transmitirán info.



## Fiber-Optic Connections

Envían luz dentro de fibras de vidrio.
Tienen 2 cables y conectores, uno para transmitir y otro para recibir.
Los conectores que usan son los [[SFP Connector|SFP]].

tipos de cables de fibra óptica:
1. Single mode:
	1. De diámetro menor que multimode
	2. La luz entra por solo 1 ángulo
	3. Permite cables mas largos que multimode y UTP
2. Multimode
	1. El núcleo es mayor que el single mode
	2. Permite que varios ángulos o modos de haces de luz entren al núcleo
	3. Permite cables mas largos que cables UTP
	4. Mas baratos que single mode

Revisar Ethernet para estándares de cableado


Diagrama de un cable de fibra óptica:
![[fiber-optics-structure.jpg|400x400]]
1. Core: núcleo de fibra de vidrio por donde viaja la luz
2. Glas cladding: Revestimiento de vidrio que refleja la luz
3. Plastic buffer: Un amortiguador de plástico que protege la fibra de vidrio
4. Outer jacket: Cobertura del cable



# Networking Models

A networking model categorizes and provides a structure for networking protocols and [[standards]].

## OSI Model

Open Systems Interconnection model is a conceptual model that categorizes and standardizes the different functions in a network. I was created by the ISO in the late 1970's.
Each function is dividen into 7 layers, and they work together.

La información se va procesando desde la Application layer hasta la Physical layer, cada layer agregando algo a la información original, hasta que se convierte en señales eléctricas en al Physical layer. Esto se llama encapsulación.
Luego se envía al otro sistema y este realiza el proceso opuesto, se van removiendo las capas extra hasta que la información llega a la Application layer. Esto es desencapsulación.


## OSI layers:
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
- **Layer 5. Session layer**
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
- **Layer 3. Network layer**
	- Provee conectividad entre hosts en diferentes redes (fuera de la LAN)
	- Provee direcciones IP
	- Elige el mejor camino entre la fuente y el destino de la información
		- Pueden haber muchos caminos
	- Routers trabajan en L3
	- Agrega un L3 header, que incluye la dirección IP de origen y destino al segmento
	- Con el L3 header, el segmento pasa a llamarse **packet** o **paquete**
	- L3 protocols
		- OSPF
- **Layer 2. Data Link layer**
	- Provee conexión ***node-to-node*** y transferencia de información
		- Por ejemplo, PC a switch, switch a router, router a router
	- Define el formato que tendrá la información al ser enviada dentro de un medio físico
		- Por ejemplo, cables UTP
	- Detecta y posiblemente corrige errores en la Physical layer
	- Switches operan en L2
	- Agrega un L2 header y un L2 trailer que usan direcciones
	- Con el L2 header y trailer, pasa a llamarse **frame**
	- L2 protocols
		- STP
- **Layer 1. Physical layer**
	- Define las características del medio usado para transferir info entre equipos
		- Por ejemplo, niveles de voltage,  conectores físicos, especificaciones de cables, pines, distancias máximas de transmisión, 
	- bits son convertidos en señales eléctricas o radio
	- Cubre los estándares de Ethernet de los cables

La info, los segmentos, paquetes y frames, todos son partes de PDU o Protocol Data Unit
Un segmento sería un L4 PDU, un paquete seria un L3 PDU y un frame sería un L2 PDU, un bit sería un L1 PDU

![[osi_model_7_layers.png]]

## TCP/IP Model

- It's a conceptual model and set of communications protocols used in the internet and other networks.
- It's known as TCP/IP because those are 2 of the foundational protocols in the suite
- Developed by DoD through DARPA

## TCP/IP layers:
- Layer 4. Application layer
	- Engloba la App, Presentation y Session layer del OSI
- Layer 3. Transport layer
	- Exactamente igual a la Transport layer del OSI
- Layer 2. Internet layer
	- Exactamente igual a la Network layer del OSI
- Layer 1. Link layer
	- Engloba la Data Link y Physical layer del OSI

 ![[IP_stack_connections.svg.png]]



# Intro to the CLI

It's the interface you use to configure Cisco devices


## Conectándonos al dispositivo
Para conectarnos a un dispositivo Cisco y configurarlo con la CLI, podemos conectarnos al puerto *console* desde nuestro notebook. Para eso usamos un cable llamado [[Rollover Cable]], con un conector RJ-45 para el dispositivo y otro conector [[DV9]] para nuestro notebook. Si no tenemos ese puerto, necesitaremos un adaptador.

Luego de conectarnos, necesitamos un emulador de terminal para acceder a la CLI, como [PuTTY](https://putty.org/).
Abrimos el programa, apretamos tipo de conexión *Serial* y luego en *Open*. También podemos cambiar la configuración si apretamos en *Serial* en la columna de categorías.


## Primeros pasos
Luego de entrar a la consola, nos encontramos en **"User EXEC Mode"**, que se nota por el símbolo ">" en **Router>**, donde escribimos los comandos. *Router* sería el *hostname* del equipo.

**"User EXEC Mode"** es limitado, podemos observar pero no podemos cambiar la configuración del equipo. También puede ser llamado **"user mode"**. En resumen, es un modo sin privilegios para poder realizar cambios.

Podemos entrar en **"Privileged EXEC Mode"** con el comando **"enable"** en la CLI. El símbolo ">" cambiará por un "#". Ahora se verá como **Router#**.
Este modo nos da acceso a ver todas las configuraciones, reiniciar el dispositivo, y otras cosas, pero no cambiar la configuración.

Si queremos configurar y entrar al **"Global Configuration Mode"**, ahora ingresamos el comando **"configure terminal"** y aparecerá un "(config)" después del *hostname*. Se verá como **Router(config)**.

Con el comando "exit" salimos de *Global Configuration Mode* y si lo ingresamos de nuevo nos desloguea.
![[CLI modes.png]]

### Resumen

Recordar que no podemos entrar al modo privilegiado sin antes entrar al modo de usuario.

**Modo para entrar al modo de usuario**
1. **"enable"**

**Comando para entrar al modo privilegiado**
1. **"configure terminal"**

**Para salir del modo privilegiado**
1. **"exit"**
2. También existe el comando **"end"**, pero ese nos saca de todo

Revisar otros comandos básicos en CLI Commands
## Configuraciones de seguridad

Podemos aumentar la seguridad del equipo configurando contraseñas que limitarán el acceso a la consola.
El comando **"enable password </clave>"**, sirve para configurar una contraseña básica. Es sensible a mayúsculas. La contraseña ingresada no será *hasheada* por defecto, por lo que cualquier persona puede entrar a los archivos de configuración y verla, usando **"do show running-config"**. Podemos encriptarla para que no se pueda ver, con **"service password-encryption"**.
Ese *hash* (llamado "Type 7") se puede desencriptar muy fácilmente. **Nunca** usar en entornos reales.

Otra manera más segura es usando **"enable secret </clave>"**. Este comando usará el *hash* **MD5** para asegurar nuestra contraseña. Al igual que el tipo 7, no debe usarse en entornos reales.

Nota: Revisar [mejores practicas de contraseñas](https://media.defense.gov/2022/Feb/17/2002940795/-1/-1/1/CSI_CISCO_PASSWORD_TYPES_BEST_PRACTICES_20220217.PDF).

## Archivos de configuración

**Hay 2 archivos de configuración que se encuentran en el equipo: *running-config* y *startup-config***
- *running-config*:  Es el archivo de configuración que se encuentra activo en el equipo, todos los comandos que ingresemos a la CLI editarán este archivo.
- *startup-config*: Es el archivo de comunicación que se carga luego de prender o reiniciar el equipo.
- Podemos guardar la configuración con el comando **"write"** o **"write memory"**










## IOS File Systems


Los "File Systems" son una manera de controlar como la información es almacenada y recuperada.

Se pueden ver con el comando **"show file systems"**


## Disk Types

- **disk**: Equipos de almacenamiento como memoria flash
- **opaque**: Usado para funciones internas, se refiere a sistemas lógicos internos
- **nvram**: El archivo "startup-config" se encuentra almacenado aquí
- **network**: Representa sistemas de archivos externos, como servidores FTP



# Ethernet LAN Switching


## Antes de enviar el frame

Cuando queremos enviar un *frame* de PC1 a PC2, no conocemos la dirección MAC de PC2, sino que usamos la dirección IP que se encuentra encapsulada dentro del frame. Pero como los *switches* solo operan en L2, necesitan una dirección MAC para trabajar.
Para aprender la dirección MAC de PC2, vamos a usar el protocolo ARP. PC1 enviará un ARP request como broadcast y llegará primero al switch, guardando la MAC de PC1 en su *address table*. Luego, reconociendo el frame como un broadcast, enviará el frame a todos los equipos conectados a la red.
Cuando PC2 reciba el frame, responderá con un *known unicast* ARP reply que contendrá su MAC. El switch recibirá esté frame y grabara el MAC de PC2 en su *MAC address table*.
Finalmente PC1 recibirá está respuesta y guardara el MAC de PC2 en su *ARP table*, que asocia los IP's del equipo con su respectiva MAC.

Los próximos *frames* enviados conocerán la dirección MAC del PC2.
## Enviando el frame

PC1 envía el frame a PC2 a través de la red y el *Switch* recibe el frame. El frame revisa la dirección MAC de quien envío el frame y la usa para aprender donde está PC (suponiendo que no la conoce desde antes), agregando la dirección MAC y la interfaz por donde está conectado PC1 a su *MAC Address Table*. Esto se llama ***Dynamic MAC address***, porque se realiza automáticamente.
Ahora, el switch tiene la dirección MAC del destinatario, pero este no sabe donde está. Esto se llama ***Unknown Unicast frame***, ocurre cuando la dirección MAC del destinatario no se encuentra en la *address table* del switch.

Para resolver esto, el switch realiza un *flood* del frame, esto significa enviar el frame a todas las interfaces, menos por la donde recibió el frame. PC3 ignora el paquete porque la dirección MAC del destinatario no coincide con la suya, pero PC2 lo procesa normalmente. Si PC2 envía una respuesta, el proceso termina acá y el switch no aprende la dirección MAC del PC2.
Si PC2 envía una respuesta a PC1, el switch recibirá el frame y agregará la dirección MAC de PC2 a su *address table*. Y como ya conoce donde se encuentra PC1, el switch enviará directamente el frame a PC1, sin realizar un flood.

En *switches* Cisco , las direcciones MAC se eliminan de la *address table* luego de 5 minutos de inactividad.

Nota: La interfaz guardada en la *address table* del switch no necesariamente está conectada directamente a PC guardado, como pasa en LAN's de mas de 1 switch.

## Ethernet Frame

When being represented in diagrams, the header is placed to the left, and the trailer to the right.
*header* y *trailer* tienen un **largo total  de 18 bytes**, usualmente el *Preamble* y el *SFD* no se consideran partes del header, considerándolos tendría 26 bytes.

un ethernet frame pertenece a L2 del modelo OSI.

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
	- Consiste en la dirección ***MAC*** del equipo
	-  6 bytes de largo == 48 bits
- **Source**
	- Dirección L2 del equipo donde se origina el frame
	- Consiste en la dirección ***MAC*** del equipo
	-  6 bytes de largo == 48 bits
- **Type**
	- Indica el protocolo L3 que se encuentra encapsulado en el *frame*
		- **IPv4** = 0X0800 == 34525 en decimal
		- **IPv6** = 0X86DD == 2048 en decimal
		- **ARP** = 0x0806
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

**Importante**: Revisar frecuentemente Hexadecimal - Decimal - Binario.
#### Diagrama

![[ethernet_frame_structure.png]]

## MAC Address

It's a 6 byte physical address assigned to the device when it's made, it stands for *Media Access Control*.
The first 3 bytes are the OUI (Organizationally Unique Identifier), which is assigned to the company making the device, the last 3 bytes are unique to the device itself.

Está escrito como **12** carácteres hexadecimales.

Cuando el Switch aprende una Source MAC y la guarda en su *MAC address table* al recibir un frame, se llama ***Dynamic MAC Address***.

En *switches* Cisco , las direcciones MAC se eliminan de la *address table* luego de 5 minutos de inactividad. Se le llama *aging*.

**Para mostrar la tabla MAC**
- **"show mac address-table"**
	- Usada por el *switch*
	- *Routers* no tienen esta tabla

**Si queremos borrar los registros dinámicos de la tabla MAC**
- **clear mac address-table dynamic**
	- incluyendo al final **"address </MAC>"** podemos especificar que MAC borrar
	- Incluyendo al final **"interface </interface>"** podemos especificar que interface borrar


## ARP

Stands for Address Resolution Protocol.
It's used to discover the L2 address (MAC Address) of a known L3 address (IP address).

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
Cuando el *router* envié el paquete, necesitará la dirección MAC de el proximo *router* en la ruta para poder mandárselo. Cada *router* necesitará la dirección MAC del siguiente *router* en la ruta, para que el paquete pueda llegar a su destino final. Cada dirección de origen y destino MAC del paquete irá cambiando acorde se va moviendo en la red.

Todos los equipos guardarán una tabla con las direcciones **ARP** que hayan aprendido.

## Visualización

**Solo necesitamos 1 comando para visualizar la tabla**
- Con **"show arp"**
	- Resolución L3 a L2

## NDP

Protocolo parecido a ARP usado con IPv6, con la función de aprender la dirección MAC de otros *hosts*.
Usa ICMPv6 y "solicited-node multicast addresses" para aprender la MAC.


##  Solicited-node Multicast Addresses

Son mucho mas eficientes que los mensajes *broadcast* de ARP, debido a que son *addressed* a un *host* específico.

Tipos de mensajes:
- **Neighbour Solicitation** (NS) = ICMPv6 Type 135
- **Neighbour Advertisement** (NA) = ICMPv6 type 136


Pasos al enviar el NS
1. PC1 hace *ping* a PC2 con dirección 2001:db8:78:9abc
2. Como no tenemos la MAC, PC1 envía un mensaje **NS**
3. Se crea un paquete (NS) con dirección origen con IPv6 de PC y de dirección se destino se usa la  dirección "solicited-node multicast" de PC2
	1. ¿Cómo PC1 sabe esa dirección?
		1. A partir de la dirección *unicast* que nosotros ingresamos, PC1 automáticamente podrá calcularla
4. De dirección MAC, se usará la MAC de PC1 para origen y para destino se usará una dirección MAC *multicast* basada en la "solicited-node" de PC2 

Pasos al responder
1. PC2 recibe el mensaje y enviará su MAC en un mensaje **NA**


Para ver la tabla de MAC's usamos el comando **"show ipv6 neighbor"**

## Automatically Discover Neighbours

Otra función de NDP permite a que los equipos automáticamente descubran a otros *routers* en la red.
Tipos de mensajes:
- **Router Solicitation** (RS) = ICMPv6 Type 133
	- Se envía a dirección *multicast* FF02::2 (todos los *routers*)
	- Le pide a todos los *routers* que se identifiquen
	- Se envía cuando una interfaz se conecta a la red
- **Router Advertisement** (RA) = ICMPv6 Type 134
	- Se envían a dirección *multicast* FF02::1 (todos los nodos)
	- Estos mensajes se envían como respuesta a un **RS**
		- También se envían de forma periódica, con o sin **RS**



## Duplicate Address Detection

Duplicate Address Detection (DAD) permite a los *hosts* *chequear* si otros equipos en el *local link* están usando la misma dirección IPv6.

Cada vez que una interfaz IPv6 se habilita, o se configura una dirección IPv6, ejecuta **DAD**. DAD envía un mensaje **NS** a su propia IPv6 y, si no le llega una respuesta, sabrá que su dirección es única. Si le llega una respuesta, significa que otro equipo ya está usando esa dirección.
Este mensaje **NS** es enviado a su propia dirección "solicited-node multicast"

## Ping

Network utility that is used to test reachability.
Measures round-trip time.

Usa 2 mensajes:
- ICMP Echo Request
- ICMP Echo Reply

No funciona como *broadcast*, por lo que hay que conocer la MAC del destinatario. Por lo que ARP tiene que usarse antes.
También podemos especificar el tamaño de cada ping

En Cisco CLI, un "." significa que un ping fallo y un "!" significa que el ping fue exitoso.
¿Por qué falló el primer ping en la imagen?
- Porque como PC1 no conocía la dirección MAC de PC2, tuvo que usar ARP y en el tiempo que demoró en obtener su MAC, el primer ping falló.

###### Imagen de un ping en Cisco CLI:
![[cisco_cli_ping.png]]



# IPv4 

## IPv4 Addressing

 It's the primary L3 protocol in use today, and version 4 is the one that is used the most.

Un IPv4 tiene un largo de 32 bits, por lo que cada grupo de números representa 8 bits o 1 byte.
Nota: Revisar el *header* en IPv4 Header

Dirección IP de ejemplo: 192.168.1.0/24:

- Cada grupo de números es una representación de binario 
- Los primero 3 grupos de números representan la red en sí, el **último numero** representa a **equipos específicos**.
	- El prefijo **determina** que porción del numero pertenece a la **porción de la red**
- si la porción del host termina en "0", significa que es la **dirección de la red misma**, como en el IP de arriba
	- Ej. 43.0.0.0/8 - 128.43.0.0/16
		- La dirección de la red es siempre el número más bajo posible.
- Si la porción del host termina en "255" o solo octetos de 1, esa porción es la **dirección broadcast** de la red. y no puede ser asignada a un equipo
	- No es lo mismo en subnets
- El prefijo "/24" se refiere a que 256 hosts se pueden conectar a esa misma red
	- Sabemos que son 256 hosts porque elevamos 2<sup>bits de la porcion del host</sup> = 256 - 2
	- Se refiere a que los primeros 24 bits están reservados para representar a la red, y los restantes 8 bits representan al equipo
	- En equipos Cisco, el prefijo se escribe como la netmask
	- Se escribe en decimales punteados, igual que una dirección IPv4.
	- Por ejemplo, La subnet para una red de clase A es "255.0.0.0"
- El máximo de equipos que se le puede conectar a una red es 2 elevado a los bits de la porción del host.
	- Ej. red de dirección 192.168.1.0/24 >>> 2<sup>8</sup> - 2

**Nota**: 11111111 en binario es **255**
##### Direcciones IP están divididas en 5 clases distintas.

La clase está determinada por el **primer octeto** de la dirección.

Las direcciones de clase A suelen terminar en 126, no 127. Esto se debe a que el 127 está reservado para *Loopback Address*. el *Loopback Address* se usa para probar el *network stack* (OSI, TCP/IP) en el equipo local.
Si un equipo envía cualquier trafico de red a alguna dirección en ese rango, será procesada dentro del stack TCP/IP como si fuera tráfico recibido por otro equipo.

- Direcciones clase A
	- Tienen el prefijo **"/8"**
	- En equipos Cisco,  la *netmask* es "255.0.0.0" o un octeto de puros 1
	- Considerar el rango desde 1-126 en vez de 0-127
- Direcciones clase B
	- Tienen el prefijo **"/16"**
	- En equipos Cisco, la netmask es "255.255.0.0" o dos octetos de puros 1
- Direcciones clase C
	- Tienen el prefijo **"/24"**
	- en equipos Cisco, la netmask es  "255.255.255.0" o tres octetos de puros1
- Direcciones clase D
	- Las direcciones en la clase **D** están reservadas para direcciones multicast.
- Direcciones clase E
	- Las direcciones clase **E** están reservadas para direcciones experimentales
![[IPv4_addressing_class.png]]


## IPv4 Header


## IPv4 Header

- **Version**: 
	- Identifica la IPv usada
		- IPv4 se identifica con un 4 o 0100
		- IPv6 se identifica con un 6 o 0110
	- 4 Bits de largo
- **IHL - Internet Header Length**:
	- Indica el largo total del ***IPv4 header**
	- Identifica el largo total del *header* en incrementos de 4 bytes
		- Por ej. si el valor es 5, significa que el largo total es de 20 bytes
	- El valor mínimo es de 5 == 20 bytes
	- El valor máximo es de 15 == 60 bytes
	- 4 Bits de largo
- **DSCP**:
	- "Differentiated Services Code Point"
	- Usado para QoS
		- Usado para priorizar info sensible a retraso (streaming, video, etc.)
	- 6 bits de largo
- **ECN - Explicit Congestion Notification**:
	- Provides end-to-end (between 2 endpoints) notification of network congestion **without dropping packets**
	- Es opcional, debe ser soportado por los 2 *endpoints* y la red
	- 2 Bits de largo
- **Total Length**:
	- Indica el largo total del paquete (L3 header + L4 segment + *payload*)
	- Indica el largo total en bytes, no en incrementos
	- Valor mínimo de 20 (IPv4 *header* sin data encapsulada)
	- Valor máximo de 65.535 (valor máximo de 16 *bits*)
	- 16 bits de largo
- **Identification**:
	- Si un paquete está fragmentado, este campo nos permite identificar a que paquete pertenece el fragmento
	- Todos los fragmentos del mismo paquete tendrán su propio paquete IPv4 con el mismo valor en este campo 
	- Paquetes serán fragmentados si son mas largos que el ***MTU*** (Maximum Transmission Unit)
		- El MTU es usualmente 1500 bytes
	- 16 Bits de largo
- **Flags:**
	- Usado para identificar y controlar fragmentos
	- 3 Bits de largo
		- Bit 0 siempre es 0, está reservado
		- Bit 1 (DF bit) indica si el paquete debe ser fragmentado o no
		- Bit 2 (MF bit) es 1 para decir que hay mas fragmentos en el paquete, 0 para decir que ese es el último paquete
- **Fragment Offset:**
	- Indica la posición del fragmento dentro del paquete original
	- Permite que los fragmentos sean reensamblados aunque lleguen desordenados
	- 13 Bits de largo
- **TTL:**
	- Time To Live
	- Usado para evitar *loops* infinitos
	- Indica el número de saltos, cada vez que el paquete llega a un router, disminuirá el TTL en 1
	- Un router botará un *packet* con un TTL de 0
	- 8 Bits de largo
- **Protocol:**
	- Indica el protocolo del L4 PDU 
		- 6 Significa TCP
		- 17 Significa UDP
		- 1 Significa [[ICMP]]
		- 59 Significa OSPF
	- 8 Bits de largo
- **Header Checksum:**
	- Usado para verificar si hay **errores** en el **IPv4 *header***
	- Cuando un router recibe el paquete, calcula el *checksum* del header y lo compara con el valor en este campo
		- Si no calzan, significa que algún error ocurrió en la transmisión, así que  el router bota el paquete
	- Para comprobar errores en el *payload*, se usa el protocolo encapsulado para *chequear*
		- Como UDP y TCP
	- 16 Bits de largo
- **Source IP Address**: 
	- Quién envía el paquete
	- 4 Bytes de largo == 32 bits
- **Destination IP Address**:
	- 4 Bytes de largo == 32 bits
	- A quien fue enviado el paquete
- **Options:**
	- Es opcional (ja)
	- 0 - 320 bits de largo
	- Si el **IHL** es > 5, significa que *Options* está presente

![[IPv4_header_structure.png]]


# IPv6

## IPv6 Addressing

An IPv6 address has 128 bits, 2<sup>128</sup> possible addresses

IPv6 usa NDP en vez de ARP para averiguar direcciones MAC

## Abreviación de direcciones IPv6

- Los 0's que estén al inicio de cada cuarteto pueden ser eliminados
	- 0DB8->DB8 - 001B->1B
- Cuartetos consecutivos de puros 0's pueden ser eliminados y reemplazados por ":"
	- 2001:0DB8:0000:0000:0000:0000:0080:34BD->2001:0DB8::0080:34BD
- Podemos combinar ambos métodos
- 2001:0DB8:0000:0000:0000:0000:0080:34BD->2001:0DB8::0080:34BD        ->2001:DB8::80:34BD

Cuartetos consecutivos de 0's solo se pueden abreviar **UNA VEZ**



## Encontrar prefijo IPv6

- Típicamente una empresa recibirá un prefijo /48 de su ISP
- Usualmente subnets usan prefijos de /64
	- Lo que significa que una empresa tendrá 16 *bits* para hacer subnets
- Los 64 *bits* que quedan pueden ser usados para *hosts*

1. Usando de ejemplo 2001:0DB8:8B00:0001:0000:0000:0000:0001/64
2. Convertimos la porción del *host* en puros 0's
	1. 2001:DB8:8B00:1::/64
		1. Abreviamos igual

1. Ahora 2001:0DB8:8B00:0001:FB89:**017B**:0020:0011/93
2. Dado que el prefijo no es divisible por 4, tenemos que entrar dentro del carácter hexadecimal para encontrar el *bit*
3. Ya que "017" tiene prefijo de /92 determinamos que /93 se encuentra el primer *bit* de "B" en 017B
4. Pasamos B a binario ->1011
5. Convertimos todos los números a la derecha de nuestro *bit* en 0    1011 -> 1000 con el resto de los hexadecimales a la derecha
6. Ahora convertimos a hexadecimal 1000 -> 8
7. reemplazamos la "B" por nuestro nuevo carácter hexadecimal
	1. 2001:0DB8:8B00:0001:FB89:0178:0000:0000/93
	2. 2001:DB8:8D00:1:FB89:178::/93



## Configuración direcciones IPv6 en *router*

1. Ingresamos el comando "ipv6 unicast-routing"
	1. Esto permite al *router* hacer routeo IPv6
2. Con el comando "ipv6 address </direccion_ipv6></prefijo>" le asignamos dirección Ipv6  la interfaz

Luego de asignar IP, si revisamos "show ipv6 interface brief", nos podemos dar cuenta que la interfaz tiene 2 direcciones ip, 1 extra a la que configuramos nosotros. Estas direcciones se configuran automáticamente cuando nosotros configuramos una IPv6, y se llaman "Link-Local Addresses".


### Configuración de IPv6 con EUI-64

Es un método para convertir direcciones MAC (48 *bits*) a un identificador de interfaz de 64 *bits*.
Este identificador puede usarse para volverse la porción del *host* de una dirección IPv6 /64.

1. Elegimos interfaz
2. Ingresamos el comando "ipv6 address </red></prefijo> eui-64"


### Configuración de IPv6 con SLAAC

Stateless Address Auto-Configuration

Es otra manera de configurar direcciones IPv6's.
*Hosts* usan los mensajes RS/RA para aprender el prefijo IPv6 del *local link* (ej. 2001:db8::/64)  y así automáticamente generar una dirección IPv6, a diferencia de **EUI-64**.
Luego de aprender el prefijo, usará **EUI-64** para generar la dirección IPv6.

Se puede configurar con el comando "ipv6 address autoconfig"


## Tipos de direcciones Ipv6

### Global Unicast

- Son direcciones Ipv6 únicas que se pueden usar en internet
- Se deben registrar para poder usarlas, deben ser únicas
- Primeros 48 *bits* son parte del "global routing prefix"
	- Los siguientes 16 pertenecen a la *subnet*
- Equivalen a las IPv4 Addressing públicas


### Unique Local

- Son direcciones privadas que no se pueden usar en internet.
- No es necesario registrarlas, se pueden usar libremente dentro de una red interna.
- Sus primeros 2 dígitos (8 *bits*) deben ser "FD"
	- Los siguientes 40 *bits* se llaman "Global ID" y se recomienda que sean generados aleatoriamente
	- Los últimos 64 *bits* se usan para la porción del *host*, y se llama "interface identifier"
- Equivalen a las IPv4 privadas


### Link Local

- Son direcciones IPv6 generadas automáticamente en interfaces son IPv6 habilitado
- Estas direcciones se usan para comunicaciones dentro de la misma *subnet*. *Routers* **no *routearán* paquetes** con estas direcciones
- Usos comunes:
	- OSPFv3 para *adjacencies*
	- [[NPD]]: Reemplazo de IPv6 para ARP
- Usamos el comando "ipv6 enable" para habilitar IPv6 sin tener que asignar una dirección
	- Genera una dirección IPv6 Link Local
- Empiezan con "FE8"
- Se generan usando las reglas de EUI-64


### Multicast

- Comunicación de **uno a muchos**
- Direcciones *multicast* IPv6 empieza con "FF"
- IPv6 no usa *broadcast*
- Los números en los que terminan las direcciones *multicast* en IPv6 son los mismos en IPv4
- Terminado en ":1" actúa como si fuera *broadcast*

Propósito de distintos *multicast*
![[IPv6_multicast.png]]

#### Multicast Address Scopes

Ipv6 define varios *scopes* que determinan cuan lejos se *forwardeará* un paquete.

Scopes:
- **Interface-local** (FF01): El paquete no sale del equipo. Se puede usar para enviar tráfico dentro del mismo equipo
- **Link-local** (FF02): El paquete se queda dentro de la *subnet*. *Routers* no *routearán* paquetes fuera de ella
- **Site-local** (FF05): Si puede ser *forwardeado* por *routers*. Debe ser limitado a solo **un** lugar (no sobre la WAN)
- **Organisation-local** (FF08): Más amplias que **"site-local"**
- **Global** (FF0E): Sin límites

Imagen representativa de los *scopes:
![[IPv6_scopes.png]]



### Anycast

Nuevo en IPv6, funciona como "one-to-one-of-many".
Se refiere a que hay varios posibles destinos, pero el tráfico sólo e envía a 1. 

En "anycast", varios *routers* están configurados con la **misma dirección IPv6** y anuncian esa dirección mediante protocolos de *routeo*. Cuando un *host* envía un paquete a esa dirección, los *routers* lo *forwardearán* **al *router* más cercano con esa IP**, basándose en métricas.

No hay rango de direcciones especificas para "anycast".

- Podemos configurar una dirección "anycast" con el comando "ipv6 address </direccion_ipv6></prefijo> anycast"
- La podemos revisar con "show ipv6 interface </interfaz>"

Cuadro comparativo:
![[routing_schemes.png|200x700]]

### Otras

#### ::

Es inespecífico, se puede usar cuando un dispositivo no sabe su dirección IPv6
	- *Default routes* IPv6 se configuran a ::/0
	- Es el equivalente a 0.0.0.0

#### ::1

Es la dirección *loopback* de IPv6, mensajes que se envíen a esta dirección serán procesados dentro de si mismo, sin ser enviados fuera.

Es el equivalente a 127.0.0.0/8.

## IPv6 Header

El largo del *header* de IPv6 siempre será de 40 *bytes*

- **Versión**
	- 4 *bits* de largo
	- Indica la version de IP que se está usando
	- Valor de 6 o 0110 (binario) indica IPv6
- **Traffic Class**
	- 8 *bits* de largo
	- Usado para QoS para **indicar tráfico de alta prioridad**
		- Teléfono, videollamadas, llamadas
- **Flow Label**
	- 20 *bits* de largo
	- Usado para **identificar "flujos"**, comunicaciones entre orígenes y destinos específicos 
- **Payload Length**
	- 16 *bits* de largo
	- Indica el **largo del *payload*** (segmento encapsulado) en *bytes*
- **Next Header**
	- 8 *bits* de largo
	- Indica el **tipo de protocolo L4 encapsulado**, por ej. TCP/UDP
- **Hop Limit**
	- 8 *bits* de largo
	- Misma función que **"TTL"** en IPv4 Header
	- Valor disminuye en 1 cada vez que un router lo *forwardea*
- **Source Address/Destination Address**
	- 128 *bits* de largo cada uno
	- Contienen las direcciones del origen y fuente del paquete



## Routing Fundamentals

Routing is the process that routers use to determine the path that IP packets should take over a network to reach their destination. 
Routers store routes to all of their known destination in a ***routing table***.
When a router receives a packet, they look in the routing table to find the best route to forward the packet to the correct destination.

Existen 2 métodos principales de *routeo:*
- **Dynamic Routing:** *Routers* usan protocolos dinámicos de *routeo* (ej. OSPF) para compartir información de routeo entre ellos automáticamente y construir sus **tablas de routeo**.
- **Static Routing:** Alguien configura manualmente las rutas en el *router.*


**Nota**: IPv6 Static Routing para ver *routeo* IPv6

## ¿Qué es una ruta?

Una ruta le da instrucciones al router sobre su destino final y el siguiente router inmediato (**next-hop**) ,al cual debe enviarle el paquete, en su camino al destino final.
O, si el destino es la propia dirección IP del router, recibir el paquete y no mandarlo.

### Connected and Local routes

Las *connected routes* son las rutas a la red en la que la interfaz del router está conectada (con la netmask que se configuró en la interfaces).
Si tenemos que enviarle un paquete a cualquier *host* en la red a la que estoy conectado directamente, se que debo mandarlo directamente por la interfaz conectada a esa red.

Las *local routes* son las **rutas** para la dirección IPv4 configurada en la interfaz (tienen una Subnet Mask de /32)
Usamos las netmask /32 para especificar la IP exacta de la interfaz. /32 nos dice que todos los 32 *bits* están fijos, no pueden cambiar
**ES LA RUTA PARA LA DIRECCIÓN IP**

a route **matches** a packet's destination if the packet's destination IP address is part of the network specified in the route.
Esto nos dice que si llega un paquete con una porción IP de la red distinta, no puede usar esa red porque las direcciones son distintas. El router puede enviar el paquete usando una ruta distinta o botar el paquete si no hay ruta que calce.

El router siempre elegirá el camino **más especifico** a la hora de *forwardear* el paquete.
Si no hay *matching routes* en la *matching table* del router, botará el paquete

### Route Selection
 ¿Qué pasa si el router recibe un paquete que calza con una ruta local y una ruta conectada?
 El router elegirá la ruta que haga *match* mas específicamente. **Ver foto**.
 Con mas específicamente se refiere a las rutas con *match* que tengan el prefijo más largo, en este caso, /32
 Para eso sirven las rutas locales, el router podrá determinar si el paquete es para el, y que no lo *forwardee*.
 ![[routing_table_info.png]]
 "192.168.1.0/24 is variably subnetted, 2 subnets, 2 masks"
 There are two routes to subnets that fit within the 192.168.1.0/24 class C network, with two different netmasks (/24 and /32).
 Explicado en subnets.

### Static Routing

Lo usamos cuando nosotros queremos determinar las rutas que usará el *router* para enviar paquetes a una dirección que no esté directamente adyacente.

Cada *router* en el camino necesita **2** rutas, la ruta hacia la red de origen y hacia la red de destino.
Esto asegura que ambos equipos puedan comunicarse efectivamente.
Los *routers* no necesitan las rutas a todas las redes en el camino hacia el destino, cada router se ocupa de su camino.
Los *routers* que no tienen ***connected routes***, necesitarán ser configurados.

En las rutas estáticas en las que solo especificamos la interfaz de salida, dependen de una característica llamada **Proxy ARP**.

## Default Gateway

*Default Gateway* es un termino antiguo para referirse a *router*
*Default Gateway configuration*, también llamado *default route*.
Es una ruta a 0.0.0.0/0 (la menos específica posible) e incluye todas las direcciones IP posibles, desde 0.0.0.0. hasta 255.255.255.255. Nos dice que si no tenemos alguna dirección más especifica donde enviar el paquete, entonces envíala a la *default route*. 
Es usualmente usado para dirigir tráfico en internet.
El *default route* es la ruta **menos especifica** posible, incluye **todas** las direcciones IP. 
 
Para enviar paquetes fuera de nuestra red local, PC1 enviará paquetes a nuestro router, no es necesario que conozcan rutas específicas.


Al querer enviar el paquete de PC1 a PC2 , el *destination IP* será el de PC2, pero antes de hacerlo hay que mandarle el paquete al *router*. PC1 encapsulará el paquete en un frame y la dirección MAC será la de la interfaz del *router*. PC1 aprenderá la MAC del *router* enviando un *ARP request* a la dirección IP del *router*.
Cuando el *router* reciba el *frame*, lo desencapsulará , removiendo el *L2 header y trailer*, luego buscará en su *routing table* para la *most-specific matching route*.
Si no hay rutas que calcen, necesitaremos rutas hacia la red destino.
**Nota**: Recordar que las rutas son instrucciones, nos dicen hacia donde ir y que camino tomar.

## Configuración

**Primero que todo, podemos revisamos la tabla de *routeo***
1. Con **"show ip route"**
	1.  **Codes**: Nos muestra los distintos protocolos que el router puede usar para aprender rutas
	2. **L**: Código para *local*, es la ruta para la dirección IP configurada en al interfaz (tienen una netmask /32)
	3. **C**: Código para *connected*, son las rutas a la red en la que la interfaz del router está conectada(con la netmask que se configuró en la interface)
	4. **S**: Muestra rutas estáticas
		1. 1/0] son la *administrative distance* y *metric* del la ruta
	5. S*: Nos india que es una *static route*    

**Ahora configuramos rutas estáticas**
1. Usando **"ip route </ip_address> </netmask> </next_hop>"**
	1. Lo usamos para determinar la IP destino, su netmask, y el IP del siguiente router en el camino
2. O también **"ip route </ip_address> </netmask> </exit_interface>"**
	- Especifica por cual interfaz el *router* debería mandar sus paquetes, en vez de indicar el IP del siguiente router
	- Si configuramos con este comando,  **show ip route** mostrará la ruta como *connected*
- Finalmente, con **"ip route </ip_address> </netmask> </exit_interface> </next_hop>"**
	- Podemos especificar tanto la interfaz como el siguiente salto

**Por último, también podemos configurar una "default route"***
1. Con **"ip route 0.0.0.0. 0.0.0.0 </next_hop>"**



## Subnetting


## CIDR

Cuando el internet recién fue creado, nadie esperaba que se volviera tan grande como lo es actualmente.
Esto resultó en varias direcciones IP desperdiciadas.
En 1993 se introduce CIDR para reemplazar el sistema de clases de las direcciones.

Como el sistema de clases fijas fue removido, esto permite a grandes redes dividir sus redes, permitiendo mayor eficiencia. Estas redes mas pequeñas se llaman "subnetworks" o "subnets."

Podemos elegir los prefijos específicos para nuestras necesidades y así no desperdiciar IP's
Por ej., una conexión *point-to-point* entre 2 *routers*. 
En vez de elegir una dirección con prefijo de /24, donde perderíamos 253 IP's, podríamos determinar un prefijo de **/30**, lo que equivaldría a 2<sup>2</sup> = 4, le restamos IP de red y *broadcast* y nos da **2** direcciones IP disponibles.
También podemos elegir un prefijo de **/31** lo que equivaldría a 2<sup>1</sup> = 2, y como es *point-to-point*
no necesitamos IP de red o de *broadcast*.

Recomendado usar /31 cuando es *point-to-point*.

También podemos usar /32 cuando queremos crear una ruta estática a un *host* especifico.

### Como calcular subnets
Si queremos averiguar a que subnet pertenece determinada dirección IP:
1. escribir la dirección en binario
2. Cambiar todos los hostbits a 0
3. Cambiar el nuevo número a decimal

#### **EN *BOLD* QUEDARÁN MARCADOS LOS BITS QUE SE LE PRESTAN AL HOST***

#### Ej. 172.25.217.192/21
- En binario sería 10101100.00011001.**11011**001.11000000
- Como es /21 le estamos pasando 5 bits a la porción de la red
1. Cambiamos todos los bits que pertenecen al host a 0's
	1. 10101100.00011001.**11011**000.00000000
2. Pasamos a decimal y nos da que pertenece a la subnet 172.25.216.0

Si queremos averiguar el broadcast address de una determinada red, Hacemos lo mismo de arriba, pero en vez de cambiar a 0 los *bits* del *host*, **los cambiamos a 1**
#### Ej. 192.168.**91.78/26** 
- Le estamos pasando 2 bits a la porción de la red y queda >> 01011011.**01**001110
1. Cambiamos a 01011011.01111111
2. La *broadcast* es 91.127

#### PARA CALCULAR LA NETMASK
1. Pasar todos los bits de la porción **DE LA RED** a 1's
2. Convertir a decimal
#### **La RED toma bits de la porción de lost HOST, NO al revés**


![[usable_addresses_calculation.png]]


![[CIDR_notation.png]]

![[host_and_subnet _number.png]]


Ignorar de aquí hasta abajo hasta que me den ganas de hacerlo entendible.


127.16.0.0/16
**00**000000.00000000
Le pasamos 2 bits a la porción de la red 2<sup>2</sup>
01000000.00000000
127.16.64.0 red 2
127.16.127.255 broadcast de red 2

127

**Ejercicio:**

Te dan la red 10.0.0.0/8. Debes crear 2000 subnets que deben ser distribuidas.
Cual debe ser el largo del prefijo?
cuantos hosts deben haber en cada subnet?

La red debe tomar 11 bits para crear 2000 subnets, específicamente se pueden crear 2048: 
- 2<sup>11</sup> = posibles subnets
00001010.**00000000.000**00000.00000000
Los 13 bits que le quedan al hostbit nos permite tener 8190 hosts:
- 2<sup>13</sup> - 2 =  posibles hosts

11111111.11111111.11100000.00000000
Nuestro prefijo será /19
255.255.224.0 es la subnetmask

PC1 tiene una IP de 10.217.182.223/11

1. Identificar la red
2. Broadcast
3. Firstusable address
4. Last usable address
5. Numero de hosts

Estamos prestando 3 bits >> 00001010.**110**11001.10110110.11011111
1. 
Para identificar red pasamos todos los hostbits a 0 >> 00001010.**110**00000.00000000.00000000
La red es 10.192.0.0/11
2. 
Para el broadcast pasamos todos los hostbits a 1 >> 00001010.**110**11111.11111111.11111111
10.223.255.255/11
3. 
First usable address es 10.192.0.1/11
4. 
Last usable address es 10.223.255.254/11
5. 
Numero de hosts 2<sup>21</sup> - 2 es 2.097.150

subnetmask es 11111111.11100000.0000000 >>255.224.0.0


### VLSM


Variable-Length Subnet Masks
Until now, we have practiced subnetting using FLSM (Fixed-Length Subnet Masks)
This means that all of the subnets use the same prefix length (Ex. subnetting a class C network into 4 subnets using /26)

 VLSM is the process of creating subnets of different sizes, to make sure your use of network addresses more efficient.


Pasos
1. Asignar la subnet mas grande al inicio del espacio de la dirección
2. Asignar la mas grande que venga
3. Repetir hasta asignar todas

![[VLSM.png]]

#### To LAN A
192.168.1.0/24 requiere 110 hosts

11000000.10101000.00000001.**0**0000000
tomo 1 bit
Red: 192.168.1.0/25
Broad: 192.168.1.127/25
First usable: 192.168.1.1/25
Last usable: 192.168.1.126/25
Total: 126

#### Toronto *LAN* B
 
192.168.1.128/26 requiere 45 hosts
11000000.10101000.00000001.**10**000000
Tomo 2 bits

Red: 192.168.1.128/26
Broadcast   192.168.1.191/26
First usable 192.168.1.129/26
Last usable 192.168.1.190/26
Total: 62

#### Toronto LAN A

192.168.1.129/24  requiere 29 hosts
11000000.10101000.00000001.**110**00000
Tomo 3 bits

red 192.168.1.192/27
Broad: 192.168.1.223/27
First usable: 192.168.1.193/27
Last usable: 192.168.1.222/27
Total: 30

#### Tokyo LAN B requiere 8 hosts
192.168.1.224
11000000.10101000.00000001.**1110**0000
Tomo 4 bits

red 192.168.1.224/28
Broad: 192.168.1.239/28
First usable: 192.168.1. 225/28
Last usable: 192.168.1.238/28
Total: 14

#### Point-to-Point
192.168.1.240
11000000.10101000.00000001.**111100**00
Tomo 6 bits

Red: 192.168.1.240/30
Broadcast: 192.168.1.243/30
First usable:192.168.1.241/30
Last usable: 192.168.1.242/30
Total: 2


192.168.5.0/24

#### LAN2 requiere 64 hosts 
192.168.5.0/24
11000000.10101000.00000101.**0**0000000
Tomamos 1 bit

Broad: 192.168.5.127/25
First usable: 192.168.5.1/25
Last usable: 192.168.5.126/25
Total: 126
Subnet mask: 255.255.255.128
#### LAN1 requiere 45 hosts
192.168.5.128/25
11000000.10101000.00000101.**10**000000
Tomamos 2 bits

Broad: 192.168.5.191/26
First usable: 192.168.5.129/26
Last usable: 192.168.5.190/26
Total: 62
Subnet mask: 255.255.255.192

#### LAN3 requiere 14 hosts
192.168.5.192
11000000.10101000.00000101.**1100**0000
tomamos 4 bits

Broad: 192.168.5.207/28
First usable: 192.168.5.193/28
Last usable: 192.168.5.206/28
Total: 14
Subnet mask: 255.255.255.240

#### LAN 4 requiere 9 hosts
192.168.5.208
11000000.10101000.00000101.**1101**0000
tomamos 4 bits

Broad: 192.168.5.223/28
First usable: 192.168.5.209/28
Last usable: 192.168.5.222/28
total: 14
Subnet mask: 255.255.255.240

#### Point-to-Point
192.168.5.224
11000000.10101000.00000101.**111000**00
Tomamos 6 bits

Broad: 192.168.5.227/30
First usable 192.168.5.225/30
Last usable: 192.168.5.226/30
Total: 2
Subnet mask: 255.255.255.252



192.168.5.0/24

LAN2


red: 192.168.5.0/25
Broad: 192.168.5.127/25
First usable: 192.168.5.1/25
Last usable: 192.168.5.126/25
Total: 126

LAN1
**1**0000000

red: 192.168.5.128/26
Broad: 192.168.191
First usable: 192.168.5.129/26
Last usable: 192.168.5.190/26
Total: 62

LAN3
**1**0000000

red: 192.168.5.192/27
Broad: 192.168.5.223/27
First usable: 192.168.5.193/27
Last usable: 192.168.5.222/27
Total: 30

LAN4


red: 192.168.5.224
Broad: 192.168.5.239
First usable: 192.168.5.225
Last usable: 192.168.5.238/28
Total: 14

Point to Point


red: 192.168.5.240/30
Broad: 192.158.5.243
First usable: 192.168.5.241
Last usable: 192.168.5.242/30
Total: 2


B management
10.00000000.00000000.**0001**00000





# LAN's & VLAN's

## LAN


It's a network contained within a relatively small area, like an office floor or a home.
A more specific definition: A LAN is a single **broadcast domain**, includes all devices in that broadcast domain.
We can connect LAN's through a Router.

Equipos dentro de una LAN se comunican mediante un switch.



### LAN Architectures


## Two-Tier Campus LAN Design

Consiste en 2 niveles de jerarquía:
1. **Access layer**
	1. Es el nivel al que conectan los *end hosts*
	2. Aquí se realizan las marcas QoS y servicios de seguridad
2. **Distribution layer**
	1. Agrega las conexiones entre los *switches* de la "access layer"
	2. Conecta a otros servicios, como [[WAN]], internet, etc.
	3. Actúa como la frontera entré L2 y L3

Para ayudar a la escalabilidad de grandes redes LAN, se recomienda agregar un nivel superior si existen mas de 3 "distribution layers" en un mismo lugar. Los que nos lleva al siguiente diseño.

![[two-tier_lan_design.png]]

## Three-tier Campus LAN Design

En este diseño de red, ahora tenemos 3 distintos niveles de jerarquía:
1. **Access layer**
2. **Distribution layer**
3. **Core layer**
	1. Conecta "distribution layers" en grandes redes LAN
	2. Está enfocada en velocidad
	3. Operaciones de alta demanda de CPU deben evitarse
	4. Solo existen conexiones L3

![[three-tier_lan_design.png]]



## Spine-Leaf Architecture

Usada generalmente en "Data Centers". Estos son espacios dedicados para almacenar sistemas de computadores, como servidores y equipos de red.

en primera instancia se ve como un diseño *two-tier*, pero presenta diferencias importantes:
1. Cada "leaf switch" se conecta a cada "spine switch"
2. Cada "spine switch" se conecta a cada "leaf switch"
3. "Leaf switches" no están conectados entre si
4. "Spine switches" no están conectados entre si
5. *End hosts* solo conectan a otros *leaf*

- El camino que tomará el tráfico es *load balanced* para balancear la demanda entre los *spine*
- Cada servidor está separado por el mismo numero de *hops*, dando una latencia consistente
![[spine-leaf architecture.png]]


## SOHO Networks

Las "Small Office/Home Office" se refieren a la oficina, o compañía pequeña, con pocos equipos. Una casa con conexión a internet también cuenta.
Al no tener necesidades complejas, todas las funciones son cumplidas por solo 1 equipo. Este equipo hace de *router*, *firewall*, *switch*, [[WAP]], [[modem]].

### WAN Architectures

It stands for Wide Area Network

Una WAN es una red que se extiende a lo largo de una gran área geográfica. Estas se usan para conectar LAN's separadas geográficamente.
Aunque se puede considerar el Internet como una WAN, típicamente se usa para referirse las conexiones de una empresa para conectarse a sus oficinas, servidores y otros sitios.
***

## Leased Lines

Es un *link* físico dedicado, conectando a 2 sitios entre si. Usan conexiones seriales (encapsulación PPP o HDLC)

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
Se crea un "túnel" entre los 2 equipos donde se encapsulan paquetes con un *header* del VPN y un nuevo *header* IP. Utiliza IPsec para encapsular la información.

Limitaciones:
- No soporta trafico *broadcast* o *multicast*. Esto significa que no se puede usar OSPF u otros protocolos de *routeo*
	- Se puede resolver con "GRE over IPsec"
		- Permite usar varios protocolos L3 y enviar mensajes *broadcast* y *multicast*, además de mantener la encriptación
	- Configurar varios túneles entre sitios distintos toma tiempo

##### DMVPN

DMVPN es una solución desarrollada por Cisco que permite dinámicamente a los *routers* crear un *mesh* de túneles IPsec sin tener que configurar manualmente cada tunnel.


#### Remote Access VPN

Estas se usan para permitir acceso seguro a  *end devices* como teléfonos o computadores, a los recursos internos de una empresa. Generalmente usan TLS para encriptar la información.

## VLAN's

- It stands for Virtual Local Area Network. They're configured on switches on a **per-interface** basis.
- A switch **will not** forward traffic between VLAN's, including broadcast/unknown unicast traffic.
- The switch does not perform **inter-VLAN** routing. It must send the traffic through the router.
- They **logically** separate end hosts/broadcast domains at **layer 2**
- VLAN's help reduce unnecessary broadcast traffic, which helps prevent network congestion. They also improve network security
- We want to make sure that network traffic isn't sent unnecessarily to other devices.
- Paquetes entre VLAN's se enviarán primero a un *router*
- Si la fuente y destino pertenecen al mismo VLAN, los *frames* no necesitan ser enviados al *router*
***

## VLAN Ranges
- VLAN normales
	- Rango de 1-1005
	- VLAN extendidas
		- 1006-4094

## Access Port

Es un *switchport* que pertenece a una sola VLAN y conecta a *endhosts*, como un PC

## Trunk Port

¿Qué es un *trunk port*?
Un *trunk port* lleva trafico de varias VLAN's en una sola interface, al contrario de *access ports*

### ¿Qué pasa si tenemos un switch en access y el otro en trunk?

Habrá un error y no podrán comunicarse
##### ¿Cómo sabe un *switch* a que VLAN pertenece un *frame*?
Con VLAN *tagging*, *switches* le ponen un *tag* a todos los *frames* que se envíen por un *trunk link*.
*Access ports* no necesitan un *tag* porque la interface pertenece a solo un VLAN.

##### ¿Configuración de un *trunk port?
Primero debemos elegir la interface que queremos configurar.
Para configurar interface como *trunk port* debemos usar el comando "switchport mode trunk", ese comando determina las interfaces elegidas como *trunk*, para ser utilizadas por más de 1 VLAN.
Consultar la pestaña de configuración de más abajo.

Puede ser que tengamos que elegir el tipo de encapsulación (802.1Q o ISL) antes de configurar la interface como *trunk port*, pero es solo en *switches* viejos. Para eso es el comando "switchport trunk encapsulation </tipo_de_encapsulacion>"
Después, debemos elegir que VLAN's serán permitidas por el *trunk port*, para eso usamos el comando "switchport trunk allowed vlan".

Para propósitos de seguridad, es buena idea cambiar la *native* VLAN a una VLAN que no esté siendo usada (recordar cambiar en ambos switches).
Para esto usaremos el comando "switchport trunk native vlan </numero_vlan>".

Por último, debemos configurar ROAS. Ver al final.
## VLAN Tagging

Existen 2 protocolos para hacer *tag*
Se usa para identificar a que VLAN pertenece el *frame*
- **ISL**
	- Viejo y nadie lo usa
- **IEE 802.1q**
	- Es un estándar de la industria
	- "dot1q"
	- Se inserta entre el "Source" y el "Type/Length" de un Ethernet Frame
	- 4 Bytes de largo == 32 *bits*
	- Está compuesto de 2 campos
		- **TPID**
			- Tag Protocol Identifier
			- Tiene un valor de 0x8100
				- Esto indica que el *frame* tiene un tag 802.1Q
			- 2 Bytes de largo == 16 *bits*
		- **TCI**
			- Tag Control Information
			- 2 Bytes de largo == 16 *bits* de largo
			- Está compuesto de 3 subcampos
				- **PCP**
					- Priority Code Point
					- Usado para Class of Service (CoS)
						- Usado para priorizar tráfico importante
					- 3 *bits* de largo
				- **DEI**
					- Drop Eligible Indicator
					- Usado para indicar *frames* que pueden ser botados en caso de que la red esté congestionada
					- 1 *bit* de largo
				- **VID**
					- VLAN ID
					- Indica a que VLAN pertenece el *frame*
					- 12 *bits* de largo
						- 2<sup>12</sup> = 4096 VLAN's totales
					- IMPORTANTE
![[dot1q_structure.png]]
#### Native VLAN
- Es una característica del protocolo 802.1Q
- Su *default* es VLAN 1 en todos los *trunk ports*
- El *switch* no le agrega un *tag* 802.1Q a los frames en el *native* VLAN
- Cuando un *switch* recibe un *frame* sin *tag* en un *trunk port*, asume que ese *frame* pertenece a la *native* VLAN
- Es muy importante que la *native* VLAN calce entre *switches*
- EL numero de *native* VLAN debe calzar con el numero de VLAN. Si queremos que VLAN 10 envíe *frames* sin *tag*, la *native* VLAN debe ser 10
- Si no estamos usando *native vlan* es mejor asignarla a una VLAN que no estemos usando



2. Configurar la dirección IP para la VLAN nativa en la interfaz física del *router*, sin tener que usar el comando del método previo
	1. En este método no usamos subinterfaces para la VLAN nativa, usamos una interfaz física
	2. Al mostrar las interfaces, veremos que la nativa está usando la interfaz física, y las otras VLAN's usarán las subinterfaces
	3. Borramos la subinterfaz que estaba usando la VLAN que queremos determinar como nativa y la asignamos a una interfaz física

***

## Configuración

 **Primero elegiremos las interfaces que agregaremos a los puertos *access***
1. Elegimos interfaces **"interface range </interfaces>"**
2. Las convertimos en *access* con **"switchport mode access"**
3. Les asignamos una VLAN con **"switchport access vlan </numero_vlan>"**
	1. Asigna el puerto a la VLAN y, la crea si es que no existe
4. Repetimos por cada VLAN que vayamos a crear

**Ahora entramos a la interfaz y configuramos los puertos *trunk que necesitemos***
1. Primero elegimos encapsulación con **"switchport trunk encapsulation dot1q"**
	1. Necesario en equipos que soporten ISL
2. Configuramos con **"switchport mode trunk"**

**Dentro de la interfaz, procedemos con configurar que VLAN's serán permitidas en el *trunk***
2. . Con **"switchport trunk allowed vlan </vlan_id>"**
	1. Tiene varias opciones para agregar

**Si queremos cambiar la VLAN nativa**
1. Usamos **"switchport trunk native vlan </vlan_id>"**

**Para revisar y cambiar nombres de nuestras VLAN's**
1. **"show vlan brief"** nos muestra los puertos *access*
2. **"show interfaces trunk"** nos muestra los puertos *trunk*
	1. Recordar que "show vlan brief" no muestra los *trunk ports*
	2. **Mode**
		- **on** nos dice que fue configurado manualmente
	3. **Encapsulation**
		- Tipo de encapsulación
	4. **Status**
		- Que está *trunkeando*
	5. **VLAN's allowed and active in management domain**
		- Nos da las VLAN's que tenemos configuradas
3. Entramos al modo de configuración de una VLAN con **"vlan </numero_vlan>"**
	1. También sirve para crear una VLAN directamente
	2. Ingresamos el comando **"name </nombre>"** para cambiar nombre de VLAN


RECORDAR QUE DEBEN EXISTIR TODAS LAS VLANS EN TODOS LOS *SWITCHES*. DEBEN CREARSE SI NO SALEN EN LA SECCIÓN DE "MANAGEMENT" EN "SHOW VLAN BRIEF"
***

### Router on a Stick

Es el nombre usado para *inter VLAN routing*, solo tendremos una interfaz física conectando el router y el switch.
Es usado para *routear* entre multiples VLANS usando solo 1 interfaz entre el *router* y el *switch*.
Se convertirán en subinterfaces de una interfaz, Ej.  g0/0 se dividirá en g0/0.10, g0/0.20, g0/0.30. Y pueden operar como 3 interfaces separadas. Usar los mismos número que tengan las VLAN's

##### ¿Como configurar ROAS?

**Ingresamos a la interfaz deseada y luego entramos a cada subinterfaz (esto la crea)**
1. Con el comando **"interface </subinterface>"**
	1. Idealmente usar una que calce con el ID de la VLAN

**Ahora le asignamos cada subinterfaz a una VLAN**
1. Con el comando **"encapsulation dot1q </id_vlan>"**
	1. Cualquier *frame* que llegue con el id de la VLAN configurada será tratado por el *router* como si hubiese llegado por esta subinterfaz
	2. También *tageará* todos los *frames* que tengan como destino a esa VLAN con el id de la VLAN
2. Repetir para cada subinterfaz

**Para configurar VLAN nativa en la subinterfaz
1. **encapsulation dot1q </vlan_id> native**
	1. Configuramos esa subinterfaz como VLAN nativa
	2. Asumirá que *frames* que no tengan *tag* pertenecen a la VLAN nativa y que frames que se envíen a la VLAN nativa no serán *tageados*




# Spanning Tree Protocol

Spanning Tree Protocol is a L2 protocol, it enables redundant L2 networks and prevents L2 loops by placing redundant ports in a blocking state, essentially disabling the interface.

Estas interfaces actúan como respaldos que pueden entrar a un *forwarding state* si una interface activa falla. Las interfaces bloqueadas solo pueden recibir mensajes STP, llamadas BPDU's (Bridge Protocol Data Unit) (interpretemos el "Bridge" como "switch").

Como elige que puertos están *forwardeando* y que puertos están bloqueando, STP crea un único camino desde/hacia la red. Esto previene bucles L2.

El estándar actual es 802.1s, todos los *switches* Cisco lo ocupan por defecto, además de RSTP. El STP estándar solo permite 1 instancia para todas las VLAN's, al contrario de [[PVST+]] y su versión actualizada, Rapid PVST+ y la versión actualizada de STP, Multiple Spanning Tree Protocol (802.1S). El estándar del STP clásico es 802.1D

## Broadcast Storms

Es cuando se forma un bucle infinito de *broadcast frames* y terminan congestionando la red.
Ese no es el único problema, cada vez que el mismo *frame* llega al switch desde distintas interfaces, el *switch* estará continuamente actualizando su MAC address table. Eso se llama **MAC Address Flapping**.

## ¿Cómo funciona STP?

Existe un proceso que STP usa para determinar que puertos deberían estar bloqueados o *forwardeando*:
*Switches* mandan/reciben  "Hello BPDU's" desde todas sus interfaces, esto lo usan para anunciarse y saber de otros *switches* , y ocurre cada 2 segundos. Si un *switch* recibe un "Hello BPDU" en una interface, sabrá que esa interface está conectada a otro *switch* (solo *switch* usa SPT). 
La vida máxima de un BPDU es de 20 segundos.

Los *switches* usan un **campo** en el STP BDPU, el campo "Bridge ID", para elegir a un "root bridge" para la red. El *switch* con el "Bridge ID" mas bajo se convierte en el "root bridge". El BDPU también lleva el costo acumulado del "root cost", que va incrementando a medida que ese BPDU es *forwardeado* a través de la red. Gracias a ese costo es que los *switches* sabrán que puerto elegir como "root port".
El "root cost" solo incrementa cuando llega a un nuevo puerto, no cuando sale.

Cuando un *switch* sea elegido como "root bridge", todos sus puertos se convertirán en puertos designados, y el resto de los switches en la red **deben tener un camino para llegar a él**. Cada *switch* deberá elegir solo **un** "root port", que apuntará al "root bridge". Cada interfaz conectada con ese "root bridge" será un puerto designado

Luego, **cada dominio de colisión** entre *switches* restantes deberán elegir 1 puerto designado entre ellos, donde **una** interfaz será seleccionada como puerto designado y la otra estará bloqueada.

Recordemos usar el comando **"show spanning-tree"** para revisar.

Si el *switch* no recibe BPDU's, sabe que es seguro entrar en modo *forwarding*.
**Las prioridades y quien será "root bridge" o no varían según cada VLAN en PVST**

### Estados de un puerto STP

Un puerto puede encontrarse en un estado "estable" o "transicional", dependiendo de si ya está configurado o está en proceso/ocurrieron cambios en la red, respectivamente.
1. **Blocking**
	1. Estable
	2. Puertos no designados se encuentran en este estado
	3. No envían/reciben tráfico regular
	4. Si reciben BDPU's
		1. Pero no son *forwardeadas*
	5. No aprenden direcciones MAC
2. **Listening**
	1. Transición
	2. Solo los "root ports" o puertos designados pueden entrar a este estado
		1. Los no designados siempre se encuentran bloqueando.
	3. Por defecto dura 15 segundos
		1. Determinado por el **"forward delay"**
	4. En este estado **solo** son *forwardeados*/recibidos BPDU's
		1. No recibe/envía cualquier otro tráfico
	5. No aprenden direcciones MAC
3. **Learning**
	1. Transición
	2. Ocurre luego de la etapa anterior
	3. Dura 15 segundos, por defecto
		1. Determinado por el **"forward delay"**
	4. En este estado **solo** son *forwardeados*/recibidos BPDU's, igual que el estado anterior
	5. Si aprende direcciones MAC
4. **Forwarding**
	1. Estable
	2. Envía y recibe BPDUs al igual que tráfico normal
	3. Aprende direcciones MAC
5. **Disabled**
	1. Fue desactivada

![[STP_port_states.png]]
##### Spanning Tree Timers

![[STP_timers.png]]

Si no se reciben BPDU's y en tiempo del "Max Age" llega a 0, el switch reevaluará sus decisiones STP, incluyendo el "root bridge", "root port" y sus puertos designados/no designados.
Si un puerto no designado se elige como "root port", va a **transicionar** del estado bloqueado a "Listening" --> "Learning" y finalmente a "Forwarding".

Los tiempo determinados en el "root bridge" determinan los tiempos para toda la red.
### Pasos de STP

1. El *switch* con el menor "bridge ID" es elegido como el "root bridge". Todos sus puertos son designados como puertos designados
	1. Si hay empate, se usa la dirección MAC
2. Cada *switch* restante elegirá una de sus interfaces como "**root port**". La interface con el "root cost" mas bajo será el "root port"
	1. "Root ports" también se encuentran en *forwarding state*
	2. El costo dependerá de la velocidad de la interface, a mayor velocidad, menor costo
	4. El "root cost"  es el costo total de la interfaz del *switch* con la interfaz del *switch* que está conectado a él.
		1. En caso de empate, se elige la menor "bridge ID" del vecino
			1. Y si no, se elige la interfaz que esté conectado a la interfaz del vecino que tenga el "port ID" más bajo
				1. Es por defecto "128" + el número del puerto
				2. Esto se ven en el comando **"show spanning-tree"** bajo "Prio.Nbr" 
				3. Se elige el que tenga **numero menor**
	5. Puertos conectados al "root port" siempre serán puertos designados
3. Cada dominio de colisión (entre 2 *switches*) restante debe elegir **una** interfaz como puerto designado
	1. Elegir la que tenga menor "root cost"
	2. Si el "root cost" es el mismo, Se elegirá al que tenga menor "bridge ID"
	3. El otro *switch* bloqueará su puerto

Nota: Designated port == forwarding state

- Cuando se prende un *switch*, asume que que es el "root bridge".
- Solo dejará su posición si le llega un BPDU con menor "bridge ID"
- Una vez que la topología está lista y todos los *switches* se ponen de acuerdo en un "root bridge", solo el "root bridge" envía BPDU's.
- Otros *switches* en la red harán *forward* de BPDU's, pero no generarán sus propios.

### Separación del bridge ID

- Dividido en
	- Bridge Priority
		- Dividido en:
		1. Bridge Priority
			- 4 bits de largo
		2. Extended System ID
			- Cisco switches usan una versión de STP llamada PVST (Per-VLAN Spanning Tree). PVST usa una instancia separada en cada VLAN, por lo que en cada VLAN hay diferentes interfaces *forwardeando*/bloqueando.
			- Igual a VLAN ID
			- 12 Bits de largo
	- MAC Address
		- 48 bits
![[bridgeid_structure.png]]
Esencialmente, Es la dirección MAC la que determina quien será el "root bridge", excepto en PVST.

"root costs" a continuación
![[stop_root_cost.png]]

## Configuración


**Antes de configurar, revisaremos comandos informativos**
1. **"show spanning-tree"**
	- Nos muestra la información de costos, ID's de cada VLAN en la red, que protocolo STP usamos
	- **Root ID**
		- Nos muestra la info del "root bridge"
	- **Bridge ID**
		- Nos muestra la info del *switch* que estamos viendo
	- **No muestra el "root cost total**
	- También se ve está info en Spanning Tree BPDU
2. **"show spanning-tree</vlan> root"**
	- Nos deja ver info específica sobre STP en la VLAN especificada
3. **"show spanning-tree detail"**
	- Mucho mas detallado
4. **"show spanning-tree summary"**
	- Estados de los puertos de cada VLAN
	- Muestra el "root cost" total

**Primero, elegimos el tipo de STP**
1. Con **"spanning-tree mode </modo>"**

**Luego podemos configurar el "root bridge" primario**
1. **"spannin-tree vlan </VLAN_ID> root primary"**
	- Cambia la prioridad STP del equipo a 24576

**Y el secundario**
1. **"spanning-tree vlan </VLAN_ID> root secondary"**
	- Cambia la prioridad STP a 28672

**También podemos configurar la prioridad STP de forma manual**
1. Con **"spanning-tree vlan </VLAN_ID> priority </prioridad>"**

**Configuraciones que podemos realizar sobre interfaces**
1. **"spanning-tree vlan </VLAN_ID> cost  </valor>"**
	- Nos permite cambiar el **"root cost"** de la interfaz
2. **"spanning-tree vlan </VLAN_ID> port-priority </valor>"**
	1. Nos permite configurar el **"port ID de la interfaz"**

## STP Toolkit

Aquí veremos algunas herramientas de STP que facilitarán nuestras vidas.
## Portfast

It can be enabled on interfaces that are connected to end-hosts.

Cuando un *switch* se conecta a otro equipo, debe pasar por las etapas STP de "Listening" y "Learning" antes de poder formar un puerto designado. Pero en el caso de conectarlo a un end-host, ya que no pueden ocurrir bucles infinitos.
Portfast le permite al *switch* entrar inmediatamente al estado "Forwarding", saltándose los pasos de "Listening" y "Learning".
**SOLO USAR EN PUERTOS CONECTADOS A END-HOSTS.**

### Hay 2 tipos de Portfast

- **edge**
	- El que usaremos, configurado por defecto
- **network**
	- Se usa en "Bridge Assurance"
	- No es parte de CCNA

### Configuración

**Primero elegimos la interfaz que queramos configurar y activamos "Portfast"**
1. Con el comando **"spanning-tree portfast"**

**También lo podemos activar de forma global**
1.  En los access ports, con **"spanning-tree portfast default"**
2. Podemos desactivar Portfast en interfaces específicas con **"spanning-tree portfast disable"**

**Si queremos usar "Portfast" en ROAS**

- Usamos el comando **"spanning-tree portfast trunk"**
	- Activa "portfast" en un *trunk*


## BPDU Guard

Si una interfaz con "BPDU Guard"  activado recibe un BPDU de otro *switch*, la interfaz se apagará y evitará que se forme un bucle

### Configuración

**Elegimos la interfaz deseada y configuramos**
1. Usando el comando **"spanning-tree bpduguard enable"**

**También se puede activar, de forma global, en todas las interfaces con "Portfast" activado
1. Con **"spanning-tree portfast bpduguard enable"**

## Root Guard

Si activamos "root guard" en una interfaz, aunque reciba un BPDU superior ("bridge ID" menor) en esa interfaz, el *switch* no aceptará el nuevo *switch* como "root bridge". La interfaz se deshabilitará.

Ayuda a prevenir cambios en la topología de red, al ser un ingresado un nuevo *switch*, con menor **"Bridge ID"**.

Un puerto desactivado por **Root Guard** volverá a activarse automáticamente cuando los BPDU's con mayor preferencia dejen de llegar.

### Configuración

**Entramos a la interfaz deseada y configuramos el Root Guard**
1. Con el comando **"spanning-tree guard root"**


## Loop Guard

Si habilitamos "loop guard" en una interfaz, aún cuando la interfaz deje de recibir BPDU's, no entrará en estado "forwarding". La interfaz se deshabilitará.

**Unidirectional link**

## BPDU Filter

Funciona de manera similar a BPDU Guard, solo que el filtro evita que el puerto envía o procese BPDU's recibidos.

## RSTP

- Elige un "root bridge" con las mismas reglas que STP
- Elige "root ports" con las mismas reglas que STP
- Elige puertos designados con las mismas reglas que STP
- Se cambia el costo STP según la velocidad: ![[RSTP_cost.png]]
- Se cambian los estados de los puertos: 
	- Un puerto deshabilitado ahora se llama se encuentra en el estado "discarding"
	- Un puerto que se encuentra bloqueando tráfico también se encuentra "discarding"
	- Sigue existiendo "forwarding" y "learning"
![[RSTP_port_state.png]]
## Roles de puertos

- **Root port**
- **Designated port**
- El rol de puerto no designado se divide
	- **Alternate role**
		- Es un puerto en estado "discarding" que solo va a recibir BPDU's
		- Funciona como respaldo al "root port"
			- Si el *root* falla, este puerto puede cambiar a *root* sin pasar por estados de transición
		- Es lo que era un puerto que se encontraba bloqueando
	- **Backup role**
		- Es un puerto "discarding" que recibe un BPDU por otra interfaz en el mismo switch
			- Solo ocurre cuando 2 interfaces están conectadas al mismo dominio de colisión (via *hub*)
- En RSTP **todos los *switches*** crean y envían sus propios BPDU's a través de sus puertos designados.
- En RSPT, el *switch* considera a su vecino perdido si es que falla en recibir 3  "Hello BPDU" (6 segundos)
	- Luego, olvidará todas las direcciones MAC aprendidas en esa interfaz

## RSPT Link Types

- **Edge port**
	- Es un puerto conectado a un *end host*, se mueve directamente a *forwarding* sin necesitar de negociación
	- Es lo mismo que "portfast"
- **point-to-point**
	- Nos indica una conexión entre 2 *switches*
	- Funcionan en **full-duplex**
	- Se detecta automáticamente, no es necesario configurarlo
- **shared**
	- Se una en conexiones a un *hub*. Deben operar en "half-duplex"
	- También se detecta automáticamente
Podemos elegir el "link type" con el comando **"spanning-tree link-type </tipo>"**

## Toolkit

Se encuentran integrados en RSTP:
- Portfast
- UplinkFast
	- 
- Backbone Fast
	- Ayuda al *switch* a ahorrarse el tiempo de espera **"MAX AGE"** y empezar a transmitir por ese puerto.

## Configuración

**Podemos pasar un puerto a link type "edge con el comando de portfast**
1. Con **"spanning-tree portfast"**

**Si queremos configurar manualmente una interfaz**
1. **"spanning-tree link-type </tipo>"**
	1. Para **"point-to-point"** o **"shared"**

## STP BPDU's

A un BPDU se le pone como destino **PVST+** y la dirección MAC específica de **01:00:0c:cc:cc:cd**
La version regular de STP usa la dirección MAC de **0180.c200.000**

PVST solo soporta encapsulación ISL
PVST+ soporta encapsulación 802.1Q

El ID del protocolo es 0x0000

En rapid SPT (RSPT) el identificador del protocolo será el número 2
En SPT será un 0



# EtherChannel


If you connect two switches together with multiple links, all except one will be disabled by STP.
EtherChannel groups multiple interfaces together to act as a **single interface**. It's represented by drawing a circle around the interfaces that are grouped together.
El tráfico  que pase por un "EtherChannel" va a ser *load balanced* entre las interfaces físicas en el grupo. Un algoritmo será usado para determinar que tráfico usará que interfaz física. STP tratará a este grupo como una sola interfaz.

When the bandwidth of the interfaces connected to end hosts is greater than the bandwidth of the connection to the distribution switches, this is called **oversubscription**.


ASW: Access layer switch. A switch that end hosts connect to
DSW: Distribution layer switch. a Switch that access layer switches connect to

## EtherChannel load-balancing

- El balanceo de carga de "EtherChannel" está basado en flujos
- Un flujo es la comunicación entre 2 nodos en la red
- *Frames* en el mismo flujo serán *forwardeados* usando la misma interfaz física
- Si los *frames* fueran *forwardeados* en distintas interfaces físicas, algunos *frames* podrían llegar a su destino fuera de orden

Podemos cambiar algunos de los *inputs* que son usados en el calculo de selección de interfaz:
- MAC de origen
- MAC de destino
- MAC de origen y destino
- IP de origen
- IP de destino
- IP de origen y destino

## Protocolo LACP

Link Aggregation protocol, correspondiente al estándar 802.3ad, es un protocolo de balanceo de cargas que se usa en un EtherChannel. Dinámicamente negocia la creación y mantenimiento de él. Puede aceptar hasta 16 interfaces en el canal, 8 activas y 8 en *standby*
Posee 2 modos:
- **Active**
	- Activa LACP de manera incondicional
- **Passive**
	- Activa LACP solo si se detecta otro equipo con LACP

Otro protocolo que se puede usar es Port Aggregation Protocol (PAgP). Es propietario de Cisco.
Incluye 2 modos:
- **Desirable**
	- Activa PAgP de manera incondicional
- **Auto**
	- Activa PAgP solo si se detecta otro equipo con PAgP

Finalmente, podemos elegir no configurar ningún protocolo. Este se llama **"Static EtherChannel"**. Nosotros seremos los responsables de las configuraciones.
Solo posee el modo **"on"** para activarlo


## Configuración EtherChannel

**Antes de configurar, veamos comandos informativos**
1. Con **show etherchannel load-balance**
	1. Nos muestra el método actual de *load-balancing* e info asociada
2. También **"show etherchannel summary"**
	1. Nos muestra el estado del "EtherChannel"
3. Y **"show ether-channel port-channel"**
	1. Nos muestra el modo del canal que configuramos y otras cosas

**Si queremos cambiar el método por el cual realizaremos el balanceo**
1. Usamos el comando **"port-channel load-balance </metodo>"**
2. Tenemos varias opciones para balancear, entre IP, MAC de origen, destino o origen y destino

**Ahora elegimos el rango de interfaces que usaremos para el "EtherChannel"**
1. con **"interface range </rango>"**

**Después, configuramos y creamos el canal**
1. Usando el comando **"channel-group </numero_interfaz_virtual> mode </mode>"**
	1. Configura las interfaces como parte del canal y lo crea
	2. No es necesario que el número del grupo sea el mismo con el del *switch* vecino
	3. La interfaz se verá en **"show ip int br"**


**Comando inútil**
1. Existe el comando "channel-protocol </protocolo>"
	1. Le dice a las interfaces que protocolo se usará
	2. Si con el comando anterior le configuramos un protocolo distinto al configurado aquí, tirará error
	3. No crea un canal y no hace nada

### Notas
1. Podemos configurar el EtherChannel como cualquier otra interfaz física
	1. Como asignándola como *trunk*

2. Las interfaces que se configuren dentro del mismo *ether* deben tener la **MISMA CONFIGURACIÓN**
	1. Mismo duplex
	2. Misma velocidad
	3. Mismo *switchport mode*
	4. Mismas VLAN's, para *trunks*

### L3 EtherChannel

Si usamos *Multilayer Switch* para las conexiones entre *switches*, no necesitaríamos STP. Debido a que los *routed ports* **no *forwardean broadcasts***.

1. Elegimos las interfaces
2. Usamos el comando "no switchport" para formar las interfaces L3
3. Creamos el *ether*
4. Asignamos IP al *port channel*


Nota: LAG (Link Aggregation Group)



# Dynamic Routing

*Routers* pueden usar protocolos de *routeo* dinámico para anunciar información acerca de sus rutas conectadas y las rutas que aprendieron de otros equipos.


## Elección de ruta

Si existen multiples rutas a un destino, el *router* determina que ruta es superior y la agrega a su *routing table*. Usa la métrica de la ruta para decidir cual es superior (menor métrica == superior)
Similar a cuando un *switch* elige el mejor camino al *root bridge*, los protocolos de *routeo* dinámico usan un modelo similar para elegir el mejor camino hacia su destino.

## Tipos de protocolos 

Podemos dividirlos en 2 grandes categorías:
- **IGP**
- **EGP**

### IGP

Interior Gateway Protocol
Se usan para compartir rutas dentro de un "autonomous system" (AS), el cual es solo una organización.
Podemos subdividirlos según el algoritmo que usan.

#### Algoritmo Link State

En este algoritmo cada *router* crea un "connectivity map" de la red, este mapa será el mismo en cada *router*.
- Esto se logra debido a que cada *router* anuncia información acerca de sus interfaces (sus redes conectadas) a sus vecinos. Estos anuncios se van pasando a otros *routers*, hasta que todos los *routers* en la red desarrollan el mismo mapa de la red.
- Cada *router* usará este mapa para calcular la mejor ruta a su destino, independientemente.
- Este algoritmo usa mas recursos, pero es más rápido frente a cambios.
##### OSPF
Open Shortest Path First

##### IS-IS
Intermediate System to Intermediate System

### Algoritmo Distance Vector

Fueron inventados primero que los "Link State".
Funcionan enviando lo siguiente a sus vecinos directos:
- Las redes a las que saben como llegar
- La métrica para llegar a sus destinos conocidos
Este método de compartir información se le llama "routing by rumour".
- Esto es porque el *router* no sabe de la red mas allá de sus vecinos, solo conoce la información que sus vecinos le dicen

##### RIP
Routing Information Protocol

##### EIGRP
Enhanced Interior Gateway Routing

### EGP
Exterior Gateway Protocol
Usados para compartir rutas entre distintos *autonomous systems*.

#### Algoritmo Path Vector
##### BGP
Border Gateway Protocol


## Métricas

¿Cómo determinamos cual es la mejor ruta?
Usa el valor de las rutas para determinar cual es mejor. Mientras menos sea el valor, mejor.
Muy parecido al "root cost" de STP.
- Cada protocolo usa una métrica distinta para determinar que ruta es mejor.
- Si un *router* aprende 2 o más rutas gracias al mismo protocolo de *routeo* al mismo destino, y con las mismas métricas. ambas rutas serán agregadas a la *routing table*. El tráfico será *load-balanced*
	- Esto se llama **ECMP** (Equal Cost Multi-Path)

Imagen resumen de las métricas que usa cada protocolo
![[dynamic_routing_metric.png]]

## Administrative Distance

Se usa para determinar que protocolo de *routeo* es preferido cuando se está usando **mas de 1**.
Un AD menor es preferido, e indica que el protocolo es mas "confiable".

Podemos cambiar la AD, según lo que queramos.
Si configuramos la AD de una ruta estática para que sea mas alta que una aprendida por un protocolo de *routeo* dinámico, pasará a llamarse "floating static route".
La ruta estática pasará a ser inactiva, a menos que la ruta aprendida dinámicamente sea removida.

Mapa de cada AD según protocolo![[routing_AD.png]]

## RIP

Routing Information Protocol

- Utiliza algoritmo "Distance Vector" y es IGP
- Usa *hop count* como su métrica
	- Máximo de 15 *hops*
- Posee 3 versiones
	- RIPv1 y RIPv2: Usan IPV4
	- RIPng: Usa IPv6
- Usa 2 tipos de mensajes
	- **Request**
		- Le pide a otros routers que estén usando RIP que envíen su *routing table*
	- **Response**
		- Usado para enviar la *routing table* a *routers* vecinos
- *Routers* con RIP compartirán su *routing table* cada 30 segundos

## Versiones

### RIPv1

- Versión muy antigua y casi ni se usa
	- **No usar**
- Solo anuncia direcciones *classful* (clase A, B, C)
- No soporta VLSM o CIDR
- No incluye información de la Subnet Mask en sus anuncios
- Sus mensajes son *broadcasteados* a 255.255.255.255

### RIPv2

- Soporta VLSM y CIDR
- Incluye la info de las *netmask* en sus anuncios
- Sus mensajes son *multicast* a **224.0.0.9**
	- Mensajes *multicast* solo son enviados a los equipos que están dentro del *multicast group*

## Configuración

1. Ingresamos comando "router rip"
2. Elegimos la version, en este caso con el comando "version 2"
3. Seguimos con el comando "no auto-summary"
	1. "auto-summary" convierte automáticamente las redes que anuncia a *classful*, cosa que no queremos
4. Usamos el comando "network </direccion_de_red>"
	1. Este comando le dice al *router* en que interfaces activaremos RIP
	2. Usaremos la imagen abajo de ejemplo, red de 10.0.0.0
		1. "network" le dice al *router* que
			1. Busque interfaces con una dirección IP en el rango especificado
			2. Luego, activará RIP en las interfaces que se encuentren en ese rango
			3. Formará *adjacencies* con *routers* vecinos RIP
			4. Anunciará a los vecinos el prefijo real de la interfaz
		2. al ser "network" *classful*, se asume que 10.0.0.0 es 10.0.0.0/8
		3. 10.0.12.1 y 10.0.13.1 calzan, así que RIP se activará en las interfaces g0/0 y g1/0 
			1. Ambos calzan en los primeros 8 bits (/8)
5. Luego, usamos el mismo comando en 172.16.1.0/28
	1. "network 172.16.0.0"
	2. EL *router* buscará interfaces con direcciones IP que calcen con 172.16.0.0/16
	3. Activará RIP en su interfaz g2/0
	4. Como no hay vecinos RIP en esa red, no se formarán nuevas *adjacencies*
	5. El *router* anunciará la red 172.16.1.0/28 a sus vecinos RIP
		1. Aunque no hay vecinos RIP conectados a g2/0, el *router* igual enviará anuncios RIP a g2/0, lo que no queremos
6. Usamos el comando "passive-interface </interfaz>"
	1. En g2/0 
	2. Le dice al *router* que deje de enviar anuncios fuera de la interfaz especificada
	3. Usar este comando en interfaces que no tienen vecinos RIP
7. Creamos una *default route*
8. Podemos compartir esta *default route* a los otros *routers* con el comando "**default-information originate**"
	1. así anunciará la *default route* a los otros
9. Usamos el comando "show ip protocols"
	1. Puede usarse también en EIGRP y OSPF
![[RIP_example.png]]

## EIGRP

Enhanced Interior Gateway Routing Protocol
Pertenece a los protocolos de dynamic routing

- No tiene limite de *hop-count* como RIP
- Envía mensajes usando la dirección *multicast* de **224.0.0.10**
- Es el único IGP que realiza "unqual-cost load-balancing"
	- Podemos configurar que haga *load-balancing* en varios distintos *paths* que no tengan igual costo
	- También puede hacer *load-balance* en proporción a su *bandwidth*
		- Más tráfico se enviará en *paths* con una métrica menor, ya que son más rápidos

## Metric

- By default, EIGRP usa ***bandwidth*** y ***delay*** para calcular la métrica
- Valores por defecto son:
	- K1 = 1
	- K2 = 0
	- K3 = 1
	- K4 = 0
	- K5 = 0
- Simplificar la formula de calculo como: métrica = bandwidth + delay
	- pero *bandwidth* del *link* más lento + el *delay* de todos los *links* en el camino al destino
- **Feasible Distance**: El valor de la métrica un router especifico al destino de la ruta.
- **Reported/Advertised Distance**: El valor de la métrica del vecino
	- El vecino el va a contar al *router* cual es su métrica para llegar al destino
	- Ver foto justo abajo
		- Los números antes del "/" son las "feasible distance"
		- Los números después del "/" son la "reported distance"
![[EIGRP_map.png]]
- **Successor**: La ruta con la métrica menor (la mejor ruta)
- **Feasible Successor**: Una ruta alterna al destino que cumple con la ***Feasibility Condition***
- **Feasibility Condition**: Se considerará como "feasible successor" si la "reported distance" es **MENOR**  que la "feasible distance" del "successor"
	- Dicho de otra forma, si la "reported distance" (de la ruta alternativa) es **menor** que la "feasible distance" del "successor" (la mejor ruta actual), se puede considerar como el **"feasible successor"**. Por lo que cumpliría la "feasibility condition"
- En rojo la "feasible distance" y en morado la "reported distance" 
![[feasible_distance_&_reported_distance.png]]
## Wildcard Masks

It's an inverted subnet mask
- Todos los 1's en la subnetmask serán 0's en una *wildcard*
- Todos los 0's en la subnetmask serán 1's en una *wildcard* 
Una subnetmask típica de 255.255.255.0 sería equivalente a una *wildcard* de 0.0.0.255

- Un 0 en la *wildcard* significa que los *bits* entre la dirección IP de la interfaz y la red que ingresamos en el comando "network" de EIGRP deben calzar
- Un 1 en la *wildcard* significa que los *bits* no necesitan calzar

Determina que porción de los bits debe calzar para que se active EIGRP
Mientras mas grande sea la *wildcard* (ej. 7.255.255.255) menos especifica será, y mientras mas pequeña sea (ej. 0.0.0.0, sería una /32 *wildcard), mas especifica será
**LOS NUMEROS QUE QUEDEN A LA IZQUIERDA DE LOS 1'S DE LA WILDCARD (los 0's que sobren) SERÁN LOS QUE TENGAN QUE CALZAR ENTRE LOS IP'S**
![[EIGRP_wildcard.png]]


## Configuración

1. Usaremos la imagen del final como ejemplo
2. Empezamos con el comando "router eigrp </AS_number>"
	1. El número AS debe calzar entre routers, no formarán *adjacency* y no compartirán información de rutas si no es el mismo
3. Deshabilitamos auto-summary con "no auto-summary"
	1. Ya que "auto-summary" anuncia redes de forma *classful*
4. Configuramos las interfaces que no tendrán *routers* como pasivas, usando el comando "passive-interface </interfaz>"
	1. En la imagen sería la interfaz **g2/0**
5. Elegimos las interfaces donde activaremos el protocolo
	1. "network </red>"
	2. En el ejemplo sería "network 10.0.0.0"
	3. Podemos usar el comando con una *wildcard* como "network 172.16.1.0 0.0.0.15"
6. Recordar el comando **"show ip protocols"**
7. Podemos filtrar ciertas rutas dentro del comando "show ip route"
	1. Por ej. "show ip route eigrp" solo nos mostrará las rutas EIGRP
	2. Otro ej. "show ip route connected" nos muestra las rutas conectadas directamente
8. Podemos obtener mas información sobre la topologia con el comando "show ip eipgr </opcion>"
	1. Si ponemos "topology" nos dará la las rutas que está usando como "successors", los "feasible successors" y las metricas como "feasible distance" y "reported distance"
### Configuración Unequal-Cost en Load-Balancing

1. entramos al modo de configuración de **EIGRP**
2. Ingresamos el comando "variance </numero>"
	1. Él número ingresado es un multiplicador
	2. Un valor de **2** significa que rutas que sean "feasible successor" con hasta un 2x de la *feasible distance* de la ruta del "successor" pueden ser usadas para *load-balance*

## OSPF

Stands for "Open Shortest Path First". Uses the same name algorithm (aka Dijkstra's algorithm)

Es un protocolo de routeo dinámico de tipo *link state*
Revisar generalidades del *routeo* dinámico

- LSA: Link State Advertisement
	- Son organizados en una LSDB
	- **Aging timer** de 30 minutos
- LSDB: Link State Database
	- Todos los routers en una misma área comparten LSDB
## Versiones

- OSPFv1
	- Publicada en 1989, muy viejo
	- OSPFv2
		- Publicada en 1998
		- Usada para IPv4
	- OSPFv3
		- Usada para IPv6


## Como compartir LSA's y determinar mejor ruta

1. Router debe convertirse en vecino de otros *routers* conectados al mismo segmento
2. Compartir LSA's con *routers* vecinos
3. Cada *router* calcula, independientemente, la mejor ruta para cada destino y la inserta en su *routing table*
No todos los *routers* comparten sus LSA's con todos, revisar OSPF Networks.

### Tipos de LSA

- Tipo 1: "Router LSA"
- Tipo 2: "Network LSA"
- Tipo 5: "AS External LSA"
Podemos verlas con el comando "show ip ospf database"

#### Tipo 1 - Router LSA

Este es el tipo de LSA que cada *router* OSPF genera. Identifica al *router* usando su ID y lista las redes que están conectadas a las interfaces OSPF del *router*.

#### Tipo 2 - Network LSA

Generadas por el **DR** en en cada red "multi-access" (ej. la red tipo *broadcast*) y listan a los *routers* que están conectadas a la red *multi-access*.

#### Tipo 5 - AS-External LSA

Generadas por ASBR's para describir rutas a destinos fuera de la AS (el dominio OSPF).



## OSPF Areas

¿Qué es una área?
Una área es un conjunto de *routers* y *links* que comparten una misma LSDB

Existe un área llamada "backbone area" y es una área a la cual todas las otras áreas se deben conectar. Es la área 0.
*Routers* que se encuentren conectados al área 0 se conocen como **"backbone routers"**.

Los *routers* con todas sus interfaces en la misma área se conocen como **"internal routers"**.
*Routers* con interfaces en multiples áreas se conocen como **"area border routers"** (ABR's).
- ABR's mantienen una LSDB para cada área en la que estén conectados.
- Se recomienda que un ABR no se conecte a más de 2 áreas
	- De lo contrario uno arriesga sobrecargar el *router*
Un "Autonomous System Boundary Router" conecta a un *router* de la red OSPF a una red externa.

### Reglas

- Cada área debe ser contigua, no pueden haber rutas separadas o divididas.
- Todas las áreas OSPF debe tener **al menos un** ABR conectado a la "backbone area"
- Interfaces OSPF en la misma subnet deben estar en la misma área
### Nombres de rutas

- Una **"intra-area router"** es una ruta a un destino dentro de la misma área OSPF
- Una **"interarea route"** es una ruta a un destino en una área OSPF distinta

### ¿Por qué dividir OSPF en áreas?
OSPF utiliza áreas para dividir la red, pero redes pequeñas pueden solo ser de una área sin comprometer el rendimiento.
Si utilizamos solo 1 área en redes grandes:
- El algoritmo tomará más tiempo en calcular rutas
- El algoritmo requerirá exponencialmente más poder de procesamiento en los *routers*
- Mientras mas grande sea el LSDB más memoria tomará en los *routers*
- Cualquier cambio en la red, por pequeño que sea, provocará que cada *router* haga un *flood* de LSA's y corra el algoritmo de nuevo
Al dividir una red OSPF en **varias áreas pequeñas**, podemos evitar estas consecuencias.



## OSPF Neighbours

Es esencial para el funcionamiento de OSPF. Una vez que los *routers* se convierten en vecinos, automáticamente realizan el trabajo de compartir información de red, calculo de rutas, etc.

Cuando OSPF se activa en una interfaz, el *router* envía mensajes "hello" fuera de la interfaz a intervalos regulares (10 segundos por defecto, configurable). Estos son usados para encontrar y establecer vecinos OSPF.
Los mensajes "hello" son *multicast* a **224.0.0.5**, y es la misma dirección para todos los *routers* OSPF.


### OSPF Neighbours Requirements

Requisitos para que 2 *routers* puedan convertirse en vecinos:
1. Deben estar en la misma área
2. Las interfaces debe estar en la misma *subnet*
3. El OSPF *process* debe no estar "shutdown"
	1. Entramos a configuración OSPF y usamos el comando "shutdown"
4. LAs ID's OSPF entre *routers* deben ser únicas
	1. Podemos eliminar un ID configurado manualmente con "no router-id"
5. los tiempos *hello* y *dead* deben calzar
	1. El *hello* se puede configurar con "ip ospf hello-interval </valor>"
	2. El *dead* se puede configurar con "ip ospf dead-interval </valor>"
6. Las configuraciones de *authentication* deben calzar, podemos elegir contraseñas para proteger el OSPF
	1. "ip ospf authentication-key </contraseña>"
		1. Para elegir la contraseña
	2. "ip ospf authentication"
		1. Para activar la autenticación
7. Configuración "IP MTU" deben calzar
	1. "IP MTU" es el tamaño máximo de un paquete IP que puede ser enviado fuera de la interfaz
	2. Configurable con "ip mtu </valor>"
8. El tipo de red OSPF debe calzar

### Hello Message

- Incluye el *Router* ID del *router* que envía el mensaje y quien lo recibe
- Como R1, que envía el mensaje, inicialmente no conoce al otro *router*, el ID del destinatario es 0.0.0.0
- Cuando el R2 reciba el mensaje, agregará a R1 a su tabla de vecinos OSPF
- R2 luego enviará un "hello" a R1, este contiene las ID's de R2 y R1
- Finalmente, R1 enviará otro "hello" a R2, está vez conteniendo el ID de R2
### Neighbor states

#### Down State

En este estado, los *routers* no poseen conocimiento del resto.
R1 enviará un "hello" multicast 224.0.0.5 el cual será recibido por R2.

#### Init State

R2, al recibir el *hello*, se actualizará a estado **"init"**. Ahora, R2 le enviará otro "hello" a R1, conteniendo los Id's de R2 y R1.
Mientras R1 no reciba ningún mensaje, seguirá en estado **"down"**.

#### 2-way State

R1 recibirá el "hello" proveniente de R2 y entrará en estado **"2-way"**. Un *router* entra a este estado cuando recibe un "hello" que contiene su propio ID.
Ahora, R1 le enviará otro "hello" a R2, está vez conteniendo tanto el ID de R1 como el de R2, causando que R2 también entre a estado **"2-way"**

- Las condiciones para convertirse en vecino se encontrarán cumplidas cuando ambos *routers* se encuentren en el estado **"2-way"**
- En este estado **se elige DR y BDR**
#### Exstart State

Ya estando los 2 *routers* en estado "2-way", deben determinar quién iniciará el intercambio de LSDB. Esto ocurrirá en el estado **"exstart"**.
El *router* que tenga el ID más alto se convertirá en el "master" y el otro será el "slave".
Puede considerarse una etapa de preparación.

Estos determinan que posición tomarán intercambiando paquetes "Database Description" (DBD).

#### Exchange State

En este estado intercambiarán DBD's, estos contendrán **una lista** de los LSA's que tiene cada uno. No intercambiarán sus LSA's en si.
Luego, deben comparar la info recibida con su propia LSDB para determinar que LSA's deben solicitar de su vecino.

#### Loading State

Ya sabiendo que LSA's necesitan, los routers enviarán mensajes "link State Request" (LSR) a su vecino para solicitar que le envíen las LSA's que no poseen.
Las LSA's serán enviadas en mensajes "Link State Update" (LSU).
Finalmente, enviarán un LSAck para informar que recibieron las LSA's.

#### Full State

En este estado, ya son vecinos completos y tienen la misma LSDB. Pero continuarán enviando mensajes "hello" (10 segundos por defecto) para mantenerse actualizados.

Cada vez que se reciba un "hello", un temporizador llamado **"dead timer"** se reiniciará (40 segundos por defecto).
Si se dejan de recibir *hello's* y el temporizador llega a 0, se eliminará a ese vecino.

Resumen de los distintos tipos de mensaje:
![[OSPF_messages.png]]



## OSPF Metric

- La métrica de OSPF se llama **"cost"**
	- El costo de llegar a un destino es el costo total de los saltos en el camino (igual que STP)
	- El costo de llegar a una *loopback interface* es de 1
- Es calculado automáticamente basado en el *bandwidth* de la interfaz.
- Se calcula dividiendo el **"reference bandwidth"** por el *bandwidth* de la interfaz
	- RBw / IBw = cost
	- El "reference bandwidth" por defecto es de 100mbps
	- Cualquier mbps >= al del RBw tendrá un costo de 1
- Podemos cambiar el valor del RBw con el comando **"auto-cost reference-bandwidth </valor_en_mbps>"**

### Configuración de costo de una interfaz

Además de cambiar la métrica con el comando de arriba, podemos cambiar específicamente el costo de una interfaz.
- Ingresamos el comando "ip ospf cost </valor>"
	- Recordar elegir la interfaz a configurar
	- Tomará prioridad sobre el costo calculado automáticamente
- También podemos cambiar el *bandwidth* de la ecuación, sin cambiar la velocidad actual a la que trabajará el *router*
	- Con el comando "bandwidth </valor>"
	- **No se recomienda**



## Configuración OSPF

**Antes de entrar a configurar, revisamos comandos informativos**
1. "show ip protocols"
2. **show ip ospf neighbor**
3. show ip ospf interface </opcion>
		1. Tiene varias opciones, como "brief"

**Primero, entramos al modo de configuración de OSPF**
1. Con el comando **"router ospf </ID>"**
	1. **No es necesario que los ID's sean iguales** entre distintos *routers* para convertirse en vecinos, al contrario de EIGRP.
	2. No tiene relación con la configuración de áreas
	3. Pueden haber variso ID's distintos en un mismo *router*

**Ahora configuramos las redes que queremos agregar a OSPF**
1. Usando **"network </red> </wildcard> area </area>"**
	1. Este comando le dice al *router* que busque interfaces en el rango de IP especificado
		1. Y activar OSPF en la interfaces que calcen, y en el área especificada
		2. El *router*, entonces, tratará de buscar vecinos OSPF en *routers* con OSPF activado
	2. Se debe especificar el área

**Si tenemos una red donde no habrán vecinos OSPF**
1. Y ahora el comando "passive-interface </interface>"
	1. Le dice al *router* que no envíe mensajes OSPF "hello" fuera de esa interfaz
	2. Usado si no hay *routers* a los que anunciar fuera de esa interfaz (no tiene vecinos)

**Ahora configuramos una "default gateway" para OSPF**
1. Configuramos una "default route" en el *router*
2. Ingresamos el comando "default-information originate"
	1. Le dice al ¨router que se anuncie a la red OSPF como el "default gateway"

						
3. Configurar métricas cambiando RBw o directamente el costo de una interfaz, si se require
4. Podemos usar el comando "distance </numero>" para cambiar la AD
5. Revisamos info con comandos


#### Método alternativo de activación de OSPF en una interfaz

1. Elegimos la interfaz
2. Ingresamos el comando "ip ospf </process_id>" area </area>
	1. Si ingresamos el comando "show ip protocols", en vez de ver las redes en las que activamos OSPF, se verán las interfaces donde lo activamos .

### Router ID

#### Orden de prioridad
Orden de prioridad al determinar el *router* ID OSPF
1. Configuración manual
2. IP más alto en la *loopback interface*
3. IP más alto en la interfaz física

#### Configuración router ID

1. Usamos el comando "router-id </ID>"
2. Ahora seguimos con el comando **"clear ip ospf process"**
	1. Para que tome efecto el cambio de ID
	2. El *router* perderá todas sus rutas OSPF
	3. No recomendado usar en una red real


### OSPF Networks


The OSPF "network type" refers to the type of connection between OSPF neighbours (ethernet, etc)


## DR/BDR

Es el rol que tomará cada *router* en la subnet. Su elección varía dependiendo del tipo de red.

Se elige 1 DR y 1 BDR por cada *subnet*
"DROther" solo pasaran al estado "Full" con DR's y BDR's. Y mantendrán estado "2-way" con otros *DRother's*. Esto significa que los *routers* solo intercambiarán LSA's con el DR y BDR y, DROther's no compartirán LSA's entre ellos.
DR y BDR formarán estado "Full" con **todos** los *routers* en la *subnet*, mientras que DROther's solo tendrán estado "Full" con DR/BDR


Mensajes al DR/BDR son *multicast* a 224.0.0.6

### Elección de DR/BDR

Se eligen en el estado "2-way, el que quede en primer lugar se convertirá en DR y el que quede en segundo lugar será DBR
A continuación la lista de prioridades:
1. La prioridad de interfaz OSPF más alta
	1. Todas las interfaces tienen la misma prioridad, por defecto
2. ID OSPF de *router* más alta

La elección de DR/BDR es **"non-preemptive"**, esto significa que una vez los roles sean elegidos, **no podrán ser cambiados** a menos que OSPF sea reiniciado, la interfaz falle, etc.
Si el DR cae, **es el BDR quien se convertirá** en DR, independiente de la prioridad de los otros *routers*. Se hará una **elección** para determinar **al siguiente BDR**.
## Network Types

Podemos cambiar el tipo de red con el comando "**ip ospf network </red>**"
Podemos cambiar la configuración del tipo de red a la estándar (broadcast) con el comando "no ip ospf network </red>"

ELEGIR INTERFAZ PARA CAMBIAR RED

### Broadcast

Esta es la que hemos estado viendo en clases anteriores.

Enabled by default on **Ethernet** interfaces and **FDDI** (Fiber Distributed Data Interface).
*Routers* dynamically discover neighbours by sending/listening to OSPF Hello messages using multicast address 224.0.0.5

Un DR (designated router) y **BDR** (backup designated router) deben ser elegidos en cada *subnet* (solo DR si no hay vecinos OSPF fuera de determinada interfaz).
*Routers* que no son DR o BDR se convierten en **DROther**

#### Cambio de prioridad de interfaz OSPF

- **ip ospf priority </valor>**
- Si elegimos un valor de 0 el *router* nunca podrá ser DR o BDR


### Point-to-Point

Enabled by default on **PPP** (Point-to-Point Protocol) and HDLC (High Level Data Link).
Tanto **PPP** como **HDLC** son encapsulaciones de L2, similar a Ethernet. Solo que este tipo de red se una en conexiones ***serial***. 
"Point-to Point" no soporta *frames*.

*Routers* dinámicamente descubren vecinos enviado *Hello's* a través de *multicast* 244.0.0.5.

Dado que es un tipo de conexión "point-to-point" entre 2 *routers* no es necesario elegir un DR/BDR. Ambos *routers* tendrán *full adjacencies* entre ellos.

#### Serial Interfaces

Un lado funciona como **DCE** (Data Comms Equipment) y el otro lado funciona como **DTE** (Data Terminal Equipment).
El lado DCE especifica el *clock rate*, velocidad de la conexión.

- Podemos cambiar la encapsulación con **encapsulation </encapsulacion>**
	- Usan HDCL por defecto
- Podemos averiguar quien es DCE/DTE con el comando **show controllers </interfaz>**
- Podemos cambiar el *clock rate* con el comando **clock rate </Bps>**


### Non-broadcast

Enabled by default on **Frame Relay** and **X.25** interfaces
No es necesario aprenderla.

### Comando show ip protocols

Nota: No todos los campos aparecerán en cada protocolo

1. Routing Protocol is "</protocolo_elegido>"
	1. Es el protocolo que nosotros determinamos
	2. Si es OSPF, nos muestra el "process ID que configuramos"
2. Sección de timers:
	1. Sending updates every </tiempo>
	2. Next due in </tiempo>
	3. Invalid after </tiempo>
	4. hold down </indeterminado>
	5. flushed after </indeterminado>
3. Default version control
	1. información sobre la versión siendo usada
4. Metric weight:
	1. Nos muestra que tipo de métricas está usando
	2. K1, K2, K3, K4, K5 en EIGRP
5. **EIGRP maximum metric variance </numer0>**
	1. Un valor de 1 nos dice que solo se hace **ECMP** *load-balancing*
6. Router-ID:
	1. Si el ID se configura manualmente, ese será
	2. Si no se configura, la dirección IP más alta de sus direcciones *loopback* será elegida
	3. finalmente, se elegirá la dirección IP más alta de una de las interfaces físicas
	4. Se puede configurar con el comando "eigrp router-id </numero>"
	5. Parece una dirección IP, pero no lo es
7. "Autonomous system boundary router"
	1. También llamado ASBR, es un *router* OSPF que conecta la red OSPF a una red externa
		1. Por ejemplo, al usar el comando "default-information originate" el *router* se convierte en el ASBR
8. Automatic network summarization 
	1. Nos dice si la estamos usando o no
9. "Number of areas in this router is" (no necesario para CCNA)
	1. normal
	2. stub
	3. nssa
10. Maximum path: </numero>
	1. Se refiere a ECMP *load-balancing*
	2. configurable con el comando "maximum-paths </numero>"
	3. OSPF acepta *load-balancing* usando ECMP en determinados *paths*
11. Routing for networks:
	1. Las redes que ingresamos con el comando "network"
12. Passive Interface
	1. Nos muestra las interfaces pasivas que configuramos
13. Routing Information Sources:
	1. nos muestra los vecinos con el mismo protocolo de *routeo* dinámico
14. Distance:
	1. Nos muestra la AD del protocolo elegido



# First Hop Redundancy Protocols

 A First Hop Redundancy Protocol is a computer networking protocol which is designed to protect the default gateway used on a subnetwork by allowing two or more routers to provide backup for that address; in the event of failure of an active router, the backup router will take over the address, usually withing a few seconds.

Los *routers* deben ponerse de acuerdo sobre quién tomará el rol activo de ser el *default gateway*, y otro tomará el papel de *standby*. Se pondrán de acuerdo y mantendrán su comunicación usando *Hello's*. Además, se debe configurar una IP virtual que será usada por los 2 *routers* y se generará una MAC virtual para ese IP.

Cuando un PC quiera comunicarse fuera de la red, usará la MAC  virtual de los *routers* que estén configurados como *default gateway*. Estas las aprenderá mediante ARP.

Si el *router* activo cae, el *router* secundario deberá actualizar la tabla de direcciones MAC de los switches mediante ARP *reply* que enviará de manera autónoma (estos son *broadcast*).Si el *router* que cayó vuelve a estar en línea, este no volverá a ser el activo, debido a que FHRP's son "non-preemptive", a menos que se configure lo contrario.

Veremos 3 tipos distintos:
- HSRP
- VRRP
- GLBP

Cuadro resumen
![[FHRP_protocols.png]]

## HSRP

Hot Standby Router Protocol es propietario de Cisco.
Nos provee redundancia en cuanto al *default gateway* para *hosts* en una *subnet*

Todos los routers, por defecto, poseen la misma prioridad. Se deben configurar para cambiarla.

- En este protocolo se elige un *router* activo y uno *standby*
- Posee 2 versiones, versión 2 soporta IPv6
- V1 envía multicast a 224.0.0.2 y V2 a 224.0.0.102
- MAC virtual
	- V1 de 0000.0c07.acXX (XX=HSRTP group number)
	- V2 de 0000.0c9f.fXXX (XXX=HSRP group number)

## Configuración

1. HSRP se configura directamente en la interfaz
2. Podemos cambiar la version con "standby </version>"
	1. *Routers* deben estar con la misma versión
3. Ingresamos el comando "standby </numero_de_grupo> ip </ip_virtual>"
	1. El número de grupo debe ser igual entre los *routers*
	2. Usado para elegir IP virtual
4. Seguimos con "standby </numero_grupo> priority </valor>"
	1. Lo usamos para determinar que *router* será activo
	2. Más alto el numero -> más alta la prioridad
5. Ahora podemos usar "standby </numero_grupo> preempt"
	1. Esto fuerza a que el *router* tome el rol de activo
6. "show standby" nos muestra la configuración

## VRRP

Virtual Router Redundancy Protocol es de estándar libre

- Se elige un *router* "master" y un "backup"
	- misma funcionalidad del activo y *standby*
- *Hello's* son multicast a 224.0.0.18
- La MAC virtual es 0000.5e00.01XX (XX = VRRP group number)
- No puede hacer *load-balance* en una sola subnet, pero si entre distintas *subnets*

## GLBP

Gateway Load Balancing Protocol es propietario de Cisco.

- Puede realizar *load-balance* entre varios routers dentro de la misma subnet
- Se elige un **AVG** (Active Virtual Gateway)
- AVG elige hasta 4 **AVF's** (Active Virtual Forwarders), pudiendo también elegirse a si mismo.
	- Cada AVF actúa como *default gateway* para una porción de los *hosts* en la *subnet*
- Multicast a 224.0.0.102, igual que V2 de HSRP
- MAC virtual es 0007.b400.XXYY (XX = GLBP group number e YY = AVF number)



# TCP & UDP

## TCP

Transmission Control Protocol is an L4 connection oriented protocol.
Eso significa que antes de enviar información a su destino, los 2 *hosts* se comunican para establecer una conexión. Una vez que la conexión está establecida, se comienza a intercambiar info.

TCP se asegura de que la comunicación sea confiable, quien reciba la información debe reportar haber recibido cada segmento TCP. Si no se reciba confirmación, se envía de nuevo. 
También permite ordenar los segmentos en el orden correcto. 

Por último, proporciona *flow control*, esto significa que el *host* de destino puede avisarle al origen que aumente/disminuya el flujo de info que se está enviando.


## ¿Cómo establecemos conexiones?

TCP utiliza un "Three-Way Handshake" dado que involucra 3 mensajes que se enviarán entre los *hosts*:
1. El *host* de origen (PC) le envía un "SYN" al *host* de destino (servidor)
2. El servidor responde enviando un "SYN" y "ACK" al PC
3. PC ahora le envía al servidor un "ACK"
4. Ahora la conexión está establecida


## ¿Cómo cerramos conexiones?

TCP utiliza un "Four-Way-Handshake" dado que involucra 4 mensajes que se enviarán entre los *hosts*:
1. PC le envía un "FIN" al servidor
2. Servidor responde con un "ACK"
3. Servidor seguidamente envía un "FIN"
4. PC finalmente responde con un "ACK"
5. Se cierra la conexión 



## TCP - Secuencia y *Acknowledgment*

Cuando un equipo envía en primer "SYN" le agrega un número de secuencia aleatorio. Luego, cuando el servidor responde con "SYN ACK", este le agrega otro número de secuencia aleatorio y hace un *acknowledge* del número de secuencia que puso el equipo, agregando el número siguiente que espera recibir en su "ACK".
Finalmente, el equipo agrega como su número de secuencia el número en el "ACK" que recibió del servidor y en su "ACK" agrega el número siguiente que el servidor puso como su número de secuencia.

![[TCP_sequence.png]]




## Control de Flujo

Necesitar *acknowledge* para cada segmento no es muy eficiente, es por esto que el "Window Size" del TCP *header* permite que se envíe más información antes de necesitar un *acknowledge*, y así poder controlar el flujo.

Se puede ajustar el tamaño dinámicamente.


Nota: Revisar comparaciones con UDP

## UDP

UDP is not connection oriented protocol, from L4.
Esto significa que no establece una conexión con el destino antes de enviar información, simplemente la envía.

UDP nos nos provee de comunicación confiable. Si un segmento se pierde, UDP no tiene mecanismo para recuperarlo.

UPD no nos provee de secuenciación. Si un segmento llega fuera de orden, UDP no posee ningún mecanismo para poder reordenarlos.

UDP no posee control de flujo, no tiene como manejar el flujo de información que se envía.


Nota: Revisar comparaciones con TCP

## Comparación


- TCP nos provee mas opciones que UDP, pero con el costo de necesitar más recursos.
- Para aplicaciones que necesitan comunicaciones confiables (ej. descargas), se prefiere TCP
- Para aplicaciones en tiempo real, como de video o voz, se prefiere UDP
- Hay algunas aplicaciones que usan UDP y en la misma aplicación le agregan confiabilidad
- Algunas aplicaciones usan TCP y UDP, como DNS

![[TCP_&_UDP.png]]

## Puertos

Asociados a su aplicación L7

![[TCP_&_UDP_protocols.png]]



# Access Control Lists

Provides a form of control to allow or deny access to a network.

Funcionan como un filtro de paquetes, con instrucciones para que el *router* permita o descarte tráfico específico.
Pueden filtrar tráfico basado en varios parámetros: Origen/dirección IP, origen/destino de puertos L4, etc.



## Tipos de ACL

### Standard ACL's

Filtran según IP de origen y nada más. Por eso mismo, se deben aplicar lo más cerca del destino como sea posible.

Un máximo de **una** ACL puede ser aplicada a una interfaz, por dirección:   1 *inbound* y 1 *outbound*. Aplica tanto para la "standard" como las "extended".


- Las con número se identifican con un número y abarcan desde 1-99 y 1300-1999.
- Las con nombre se identifican con nombre

#### Configuración

Primero, podemos acceder a un modo de configuración dedicado para las ACL:

1. Ingresamos el comando "ip access-list standard </numero_o_nombre>"
2. Configuramos con "[entry_number] {deny|permit} </ip> </wildcard>"
3. Para aplicar la configuración entramos a la interfaz e ingresamos el comando "ip access-group </numero_o_nombre> {in|out} "

Este modo nos permite eliminar *entries* de las listas con "no </numero_entry>"


##### Resecuenciamiento

Si alguien escribe las entradas de las listas con enumeración "1, 2, 3, 4", no nos deja espacio para agregar entradas entremedio. Esto lo podemos arreglar con el comando "ip access-list resequence </numero_acl> </numero_inicial> </incremento>".
Ese comando cambiará la primera entrada a 10 e ira aumentando las entradas por el número de incremento ingresado.



### Extended ACL's

Filtran según varios distintos parámetros y abarcan desde 100-199 y 2000-2699.

Se divide en 2 tipos de ACL's:
1. Extended Numbered
2. Extended Named

Extended ACL's deben configurarse lo mas cercano a la fuente como sea posible, a diferencia de los *standard*.
#### Configuración
1. Entramos al modo de configuración con "ip access-list extended {number|name}"
2. Configuramos una *entry* con "[entry_number] {deny|permit} {[protocol]  [src-ip]  [dst-ip]}"

En las *extended*, para referirnos a un *host* debemos escribir "host" antes del IP o la *wildcard*, ya no nos sirve solo poner el IP.
Si queremos seleccionar puertos específicos, van después de las direcciones y usamos: 
eq: para decir igual al puerto X
gt: Para referir a > puerto X
lt: Para referir a < puerto X
neq: Para referir a distintos a puerto X
range: Para referir rango desde puerto X a Y






# Layer 2 Discovery Protocols


## CDP

Protocolo propietario de Cisco y se encuentra habilitado por defecto.

Funciona enviando mensajes CDP (cada 60 seg.) a dirección *multicast* 0100.0CCC.CCCC y, una vez un equipo recibe el mensaje, se procesa y descarta, por lo que solo vecinos directos pueden recibir estos mensajes.
Los registros se guardarán en una tabla CDP por 180 segundos ("Holdtime"). Si no se vuelve a recibir un mensaje en ese tiempo, el vecino se elimina de la tabla.

Puede compartir información de VTP.


## Configuración
 1. Para habilitarlo o deshabilitarlo, de manera global, usamos el comando **"[no] cdp run"**
	 1. Podemos usarlo para habilitar/deshabilitar una interfaz en específico
 2. Podemos configurar el *timer* con **"cdp timer </segundos>"**
 3. Podemos configurar el *holdtime* con **"cdp holdtime </segundos>"**
 4. Habilitamos CDPv2 con **"[no] cdp advertise-v2"**
 5. Podemos ver información general de CDP con **"show cdp"** 
	 1. Y ver tráfico con **"show cdp traffic"**
	 2. Revisar otras opciones 

## LLDP

Estándar de la industria (802.1AB) y se encuentra deshabilitado por defecto en equipos Cisco.

 Funciona enviando mensajes LLDP (cada 30 seg.) a dirección MAC *multicast* 0180.C200.000E. Funciona de manera casi idéntica a CDP.
 Los registros se guardarán en la tabla LLDP por 120 segundos.

No puede compartir información VTP

## Configuración

**Antes de configurar, revisamos comando informativo**
1. Con **"show lldp"** vemos configuración e info
	1. tiene varias opciones, muy parecido a CDP

**Si queremos activar de forma global en el equipo**
1. Usamos el comando **"[no] lldp run"**

**Si queremos activar LLDP en interfaces especificas**
1. Usamos **"lldp transmit"** en una interfaz
	1. Para habilitar la transmisión de mensajes LLDP
2. Usamos **"lldp receive"** en una interfaz
	1. Para habilitar el poder recibir mensajes LLDP

**Si queremos cambiar el timer de los mensajes**
1. Usamos **"lldp timer </segundos>"**

**Si queremos cambiar el *holdtime***
1. Usamos **"lldp holdtime </segundos>"**




# NTP


- Network Time Protocol (NTP) nos permite sincronizar automáticamente la hora en la red.
- Clientes NTP solicitarán la hora a los servidores NTP
- Un equipo puede ser cliente y servidor al mismo tiempo
- NTP nos permite una precisión de hasta 50 milisegundos si nos estamos conectando al servidor NTP a través de internet
- NTP usa puerto 128 UDP
- **Stratum** es la distancia de un servidor NTP al reloj de referencia

## Reference Clock

Son relojes muy precisos, son la fuente de la hora. Se consideran con un ***stratum*** de 0. Los servidores NTP que están conectados a ellos son ***stratum*** 1. Cada 1 salto en la jerarquía le agrega 1 al *stratum*.

Los equipos también pueden entrar en un estado "symmetric active mode", donde formarán relaciones con equipos en el mismo *stratum* para proveer una hora mas precisa. también, un cliente puede sacar su hora de mas de 1 servidor.


## Configuración

1. Usamos el comando "ntp server </ip_servidor> [prefer]"
	1. El servidor que elijamos nos dará la hora
	2. La opción del final es para decirle al equipo que prefiera ese servidor
	3. **"show ntp associations"** nos muestra los servidores que configuramos
4. **"show ntp status"** nos muestra el *stratum* del equipo y otra info
5. **Recordar** que luego hay que configurar manualmente la *timezone*
6. Con el comando "ntp source </interfaz>" le diremos al *router* que use esa interfaz para la fuente de sus mensajes NTP
	1. Puede ser buena idea usar como interface la dirección *loopback*. Así, si una interfaz cae, el *router* podrá seguir comunicando esa interfaz por otras interfaces físicas que tenga disponible

### Configuración de un *master*

Esto se puede usar en caso de que no tengamos conexión a internet y queramos configurar a un servidor NTP como "master clock", y será usado como el reloj de referencia.

1. Usamos el comando "ntp master {stratum_number}"
	1. Si no ingresamos número, el *stratum* por defecto será 8  


### Configuración Symmetric Active Mode

Donde equipos NTP forman relaciones con otros para poder sincronizar sus tiempos y trabajar como *backups* en caso de que pierdan a su servidor.

1. Con comando **"ntp peer </direccion_ip>"**


## Manual Time Configuration

 
## Manual Time Configuration

1. Usamos el comando "clock set </hh:mm:ss> </dia> </mes> </año>"
	1. Se hace fuera de *config mode*
2. Con **"show clock detail"** vemos info sobre la fecha y quien la configuró


### Hardware Clock Configuration

El reloj del *hardware* se refiere como "calendar".
Se hacen fuera de *config mode*

1. Ingresamos comando **"calendar set </hh:mm:ss> </dia> </mes> </año>"**
2. Con **"clock update-calendar"** sincronizamos el calendario con el reloj
3. Con **"clock read-calendar"** sincronizamos el *clock* con el calendario

Podemos actualizar con NTP el *calendar* con **"nt update-calendar"**

## Time Zone Configuration

1. Usamos el comando **"clock timezone </nombre> </offset_desde_UTC>"**
	1. Este se hace desde config mode



# DNS

Domain Name System (DNS) is used to resolve human-readable names (google.com) to IP addresses. The DNS servers a device uses can be manually configured or learned through DHCP.

DNS usa tanto TCP como UDP, a través del puerto 53. TCP se usará cuando los mensajes DNS sean mayor a 512 *bytes*.

## DNS cache

Equipos guardarán las respuestas del servidor DNS  un *cache* local. Esto significa que los equipos no tendrán que preguntarle al servidor cada vez que quieran acceder a un destino.
El "CNAME" es un nombre canónico que *mapea* un nombre a otro nombre.

- Podemos ver el *cache* en Windows con el comando **"ipconfig /displaydns"**
- Podemos reiniciar el *cache* con **"ipconfig /flushdns"**


## Configuración

### Configuración de *router* como servidor DNS

1. Usamos el comando **"ip dns server"** para configurar al router como servidor DNS
2. Nuestro servidor no tendrá registros, se los tenemos que dar
	1. Usamos el comando **"ip host </nombre_de_host> </direccion_ip>"**
	2. Aquí podemos agregar otros equipos que están en nuestra red
3. Con el comando **"ip name-server </ip>"** configuraremos un servidor externo que nuestro servidor DNS podrá usar en caso de que no tenga registros solicitados
4. Ahora, con el comando **"ip domain lookup"** habilitamos a nuestro servidor para que pueda hacerle *queries* al servidor externo que configuramos
	1. Generalmente está configurado por defecto 
5. Para ver los *hosts* configurados o aprendidos por DNS usamos el comando **"show hosts"**


### Configuración de *router* como cliente DNS

1. Debemos configurarle un servidor DNS al que pueda acceder
	1. Usamos el comando **"ip name-server </IP>"**
2. Habilitamos las *queries* al servidor con **"ip domain lookup"**
	1. Generalmente habilitado por defecto


## Configuración de un domain name

Definen un área de control administrativo en internet

1. Usamos el comando **"ip domain name </nombre.x>"**
	1. Automáticamente agregará este nombre de dominio a cualquier *hostname* que no tenga un dominio especifico
		1. Por ejemplo, **"ping PC1"** se convertirá en **"ping PC1.dominio.com"**



# DHCP

Dynamic Host Configuration Protocol (DHCP) is used to allow hosts to dynamically learn various aspects of their network configuration, such as IP addresses, Subnet Mask, Default Gateway, DNS Server, etc, without manual configuration.
Equipos como *routers*, servidores, etc, son manualmente configurados.

En redes pequeñas, el *router* generalmente actúa como el servidor DHCP para *hosts* en la LAN.

- Servidores DHCP usan UDP puerto **67**
- Clientes DHCP usan UDP puerto **68**

## DHCP Release

Podemos mandarle un mensaje DHCP de tipo "release" a nuestro servidor DHCP diciéndole que liberemos el IP que actualmente estamos usando en nuestro PC.
El comando es **"ipconfig /release"**. 

Podemos pedirle al servidor un nuevo ip con **"ipconfig /renew"**, iniciando la cadena de conseguir un IP

## Proceso para pedir IP - DORA

1. El equipo enviará un mensaje "DHCP Discover" como *broadcast* para averiguar si hay servidores DHCP en la red
	1. El IP de origen que el equipo asignará al paquete es 0.0.0.0
	2. Solicitará el IP que tuvo anteriormente
2. El servidor DHCP responderá con un "DHCP Offer"
	1. Será enviado al equipo y ofrecerá una IP para el equipo e info como *default gateway*, etc
3. El equipo responde con un "DHCP Request" diciéndole al servidor que quiere usar el IP que le fue ofrecido en el paso anterior, con un *broadcast*
4. Por último, el servidor responderá con un "DHCP Ack", diciéndole al equipo que puede usar el IP



## DHCP Relay

Podemos configurar un *router* como un "DHCP Relay Agent", logrando que el *router* *forwardee* los mensajes DHCP *broadcast* de los clientes como mensajes *unicast*, al servidor DHCP. El servidor responderá de forma normal.
Esto se usa para que podamos tener un servidor DHCP centralizado, en vez de varios servidores separados.



## Configuración DHCP Server

1. Primero, usamos el comando **"ip dhcp excluded-address </ip_inicial> </ip_final>"** para especificar un rango de direcciones que NO será dado a clientes DHCP y así reservarlos
	1. No es obligatorio
2. Seguimos con **"ip dhcp pool </nombe_de_pool>"** para crear una "DHCP Pool" 
	1. Es una *subnet* de direcciones que pueden ser asignadas a clientes DHCP
	2. Se debe crear una *pool* distinta para cada red en la que el *router* está funcionando como servidor DHCP
3. Ahora especificamos el rango de direcciones que podrán ser asignados a clientes con el comando **"network </ip_de_red> </prefijo>"**
4. Configuramos el servidor DNS que los clientes DHCP usarán con el comando **"dns-server </ip>"**
5. Especificamos el dominio que tendrá la red con **"domain-name </nombre>"** (opcional)
6. configuramos la *default gateway* con **"default-router </ip>"**
	1. Esto hará que el servidor DHCP le diga a sus clientes que usen esta dirección
7. Podemos configurar el *lease time* con **"lease </dias> </horas> </minutos>"** o que sea infinito (no recomendado)

- Podemos ver que clientes que fueron asignados direcciones IP con **"show ip dhcp binding"**
- Podemos ver las *pools* que existen y los rangos de IP excluidos con el comando **"show running-config | section dhcp"**

## Configuración DHCP Relay Agent

1. Primero, debemos entrar a la interfaz conectada a la *subnet*
2. Ingresamos el comando **"ip helper-address </ip_servidor_dhcp>"**
	1. Recordar que el *router* debe tener como llegar al servidor
	2. Este comando debe usarse en la interfaz que está conectada a los clientes DHCP
3. Podemos revisar esa config. en **"show ip interface </interfaz>"**
	1. Nos mostrará la IP del servidor


## Configuración DHCP Client

Esto nos permitirá configurar a un *router* como cliente DHCP y así poder asignarle un IP a sus interfaces.

1. Elegimos la interfaz a configurar
2. Ingresamos el comando **"ip address dhcp"** para decirle al *router* que use DHCP para aprender el IP de la interfaz seleccionada

- Podremos ver si DHCP se usó para asignarle un IP a esa interfaz con el comando **"show ip interface </interfaz>"**



# SNMP


Simple Network Management Protocol is an industry standard framework and protocol.

SNMP Agent trabaja con UDP en **puerto 161**
SNMP Manager Trabaja con UDP en **puerto 162**

Hay tipos de equipos que son manejados en SNMP:
1. Managed Devices
	1. Los equipos que son manejados por SNMP (*routers*, *switches*, etc)
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



# Syslog

Syslog is an industry standard protocol for message logging. 
On network devices, Syslog can be used to log events such as changes in interface status, changes in OSPF neighbours status, system restarts, etc.
Both Syslog and SNMP are used for monitoring and troubleshooting of devices. They are complimentary, but their functionalities are different.

## Message Format

seq: No es tan importante
time stamp: Siempre incluir en mensajes
facility: que proceso originó el mensaje, ej. OSPF

![[syslog_message_format.png]]

### Niveles de severidad Syslog

Van de 0 a 7, 0 siendo el más severo de todos.
Nivel 5 se llama "Notice", pero en la Cisco CLI se llama "Notification".

![[syslost_severity_levels.png]]



## Syslog Logging Locations

¿A que distintos lugares pueden llegar los mensajes de Syslog?

- Console Line: Los mensajes Syslog serán mostrados en la CLI cuando se esté conectado al equipo mediante el puerto "console"
- **VTY Lines**: Los mensajes Syslog serán mostrados en la CLI cuando se esté conectado al equipo mediante [[Telnet]]/SSH
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




# SSH


Fue desarrollado en 1995 para reemplazar Telnet.
SSH usa TCP en puerto **22**.
El equipo que está siendo accesado cuenta como el servidor, y quien está usando SSH para conectarse, es el cliente.
 <p>En computación, una "shell es un programa de computador que expone los servicios de un sistema operativo a un usuario u otro programa</p>
VTY Lines (Virtual TeleType) indica las posibles conexiones simultaneas que son permitidas a un equipo en especifico. Generalmente se admiten hasta 16 líneas.



## Configuración previa - de la console line

Por defecto, no se necesita ninguna contraseña para acceder a la CLI de Cisco mediante el "Console Port". Podemos configurar una contraseña en la misma consola, para que cualquier persona que quiera acceder a la CLI deba primero conocer la contraseña.

**Primero estableceremos una contraseña y usuario para acceder a la consola**
1. Ingresamos el comando **"enable secret </clave>"**
	1. El uso de "secret" crea una clave con encriptación MD5
2. Luego **""username </usuario> secret </misma_clave_de_arriba>**
	1. Este se usará cuando queramos entrar a la *console line*

### L2 Switch - Management IP

Como ya sabemos, los *switches* no realizan *routeo* de paquetes y tampoco tienen tablas de *routeo*.
Pero, podemos asignarle una dirección IP a una SVI para permitir conexiones remotas a la CLI del *switch* (mediante SSH o Telnet).

1. Partimos con **"interface </interfaz_virtual>"**
	1. Ej. **"interface vlan </numero_vlan>"**
		1. El número tiene que ser el ID de la VLAN en la que estemos trabajando
2. Le asignamos dirección ip **"ip address </ip> </netmask>"**
3. La activamos **"no shutdown"**
4. Ahora debemos asignarle una *default gateway*, con **""ip default-gateway </default_gateway_ip>""**

### Login local

Ahora configuraremos el puerto "console" para requerir que los usuarios se *logeen* usando algunos de los *usernames* anteriormente configurados, cuando se conecten por ella.

**Empezamos entrando a su modo de configuración**
- Con **line console 0"**

**Ahora configuraremos el requerimiento de tener que *logearse***
- Con **"login local"**
	- Se requerirá usuario y contraseña configurados anteriormente

**Finalmente, configuraremos tiempo de inactividad para que se desconecte**
- Con **"exec-timeout </algo> </algo>"**


## Configuración SSH

Primero, debemos verificar que el IOS del equipo soporte SSH, cualquier versión que tenga en el nombre un "K9", lo soporta.
Podemos ver la versión con **"show version"**, o vemos ver directamente si el quipo lo soporta con **"show ip shh"**.

Recordar que para que podamos conectarnos a un *switch* mediante SSH, necesitamos primero configurar una IP en su SVI.
### Habilitando SSH

**Partiremos configuramos un *hostname* y nombre de dominio
1. **"hostname </nombre>"** para *hostname*
2. **"ip domain name </nombre>"** para nombre de dominio
	1. Esto es necesario ya que el FQDM **(Fully Qualified Domain Name = host name + domain name)** se usa para nombrar las llaves RSA

**Ahora crearemos el par de llaves pública y privada de encriptación**
1. Con el comando **"crypto key generate rsa"**
	1. Deberemos elegir el tamaño de las *keys*
	2. Tamaño de *keys* para SSHv2 debe ser > 768 *bits*

**Elegiremos la version 2 de SSH, para mejorar la seguridad**
1. Con **"ip ssh version 2"**

**Podemos configurar una ACL para limitar aún más las probabildiades de acceso**
1. Con **"access-list </numero_o_nombre> permit host </ip>"**

### Configurando SSH

**Primero, tenemos que entrar al modo de configuración de las líneas VTY**
1. Con **"line vty 0 15"**
	1. Configura las 16 lineas disponibles

**Ahora configuraremos el requerimiento de logearse para poder acceder a la línea**
1. Con el comando previo **"login local"**
	1. Usará los mismos usuarios y contraseñas configurados previamente
	2. No se puede solo usar "login" para SSH

**También podemos configurar el tiempo de inactividad**
1. Con **"exec-timeout </minuto> </segundos>"**

**Ahora restringiremos acceso a las vty solo con SSH**
1. Con el comando **"transport input ssh"**

**Finalmente, aplicamos la ACL que configuramos previamente**
3. Con **"access-class </numero_o_nombre_de_ALC> {in|out}"** aplicamos la ACL a las "VTY lines"
	1. "access-class" es para "VTY lines"
	2. "ip access-group" es para una interfaz
	3. esto se hace en el modo de config de la *line*

## Conectándonos mediante SSH

Podemos conectarnos con el comando **"ssh -l [username ip_address | ssh </usernam@ip_address>]"**

La que ip que se usa en el comando es la de la SVI



# FTP & TFTP

## FTP

FTP along with TFPT are industry standard protocols used to transfer files over a network.

Fue estandarizado por primera vez en 1971.
FTP trabaja con TCP en puerto 20 y 21.

Ambos trabajan con un modelo cliente-servidor:
- Clientes los pueden usar para copiar archivos desde/hacia un servidor

FTP puede usar autenticación, pero no encriptación. Es por esto que existe FTPS (usa SSL/TLS), que si puede encriptar la información.
También existe SFTP, el cual es un protocolo nuevo y, trabaja con SSH.

FTP es más complejo que TFPT, además de permitir el traspaso de información también le deja al cliente navegar/eliminar/agregar directorios, listas, etc.



## Conexiones FTP

FTP usa 2 tipos de conexiones:
- **FTP Control** (TCP 21): Es usada para enviar comandos y respuestas FTP
- **FTP Data** (TCP 20): Es el puerto por el cual se transfiere la info
	- Hay 2 métodos para establecer conexiones **"FTP Data"**
		- **Active Mode**: El **servidor** inicia la conexión FTP
		- **Passive Mode**: El **cliente** inicia la conexión FTP. Esto es generalmente necesario cuando el cliente está detrás de un *firewall*, que puede bloquear la conexión desde el servidor



## Upgrading Cisco IOS (TFPT)

Recordemos que podemos ver la version con **"show version"**


1. Usamos el comando **"copy </fuente> </destino>"**
	1. El la fuente ingresamos FTP o TFPT como servidores de origen
2. Ahora tenemos que ingresar la dirección del servidor FTP/TFPT
3. Ahora debemos ingresar el nombre del archivo que se encuentra en el servidor
4. Ahora el archivo se descargará a nuestro equipo
5. Podemos usar **"show flash"** para ver el archivo descargado
6. Para decirle al router que use el archivo, debemos usar el comando **"boot system </direccion_archivo>"**
7. Guardamos la configuración con **"write memory"**
8. Reiniciamos el *router* con **"reload"**
9. Para borrar el archivo de la version antigua usamos el comando **"delete </direccion_archivo>"**



## Upgrading Cisco IOS (FPT)

1. Configuramos usuario con **"ip fpt username </usuario>"**
2. Configuramos contraseña con **"ip fpt password </clave>"**
3. Mismos pasos que con TFPT

## TFTP

Trivial File Transfer Protocol fue estandarizado en 1981.
Trabaja con UDP en puerto 69.

Se llama "Trivial" porque es más simple y tiene funcionalidades más básicas comparadas con FTP.
- Solo permite que un cliente copie archivos desde/hacia un servidor
- Nada más

No es un reemplazo de FTP, solo es otra herramienta que puede ser usada cuando simplicidad es más importante que funcionalidades más avanzadas.
- Por ejemplo, no posee autenticación o encriptación



## TFPT Reliability

UDP no nos da confiabilidad para en cuanto a la información transmitida,  pero TFPT posee mecanismos internos para darnos integridad y confiabilidad e la información. Esto se llama "lock step".

- Cada mensaje TFPT es *acknowledged*
	- Si un cliente transfiere información a un servidor, el servidor nos responderá con un "Ack", y vice versa
- Se usan *timers*, si no se recibe un mensaje a tiempo, el equipo volverá a enviar el mensaje previo



## TFPT Connections

el proceso de transmisión de archivos tiene 3 fases:
1. **Conexión**: El cliente TFPT envía una solicitud al servidor, y el servidor responde, iniciando la conexión
2. **Transmisión de data**: El cliente y servidor intercambian mensajes TFPT. Uno envía la data y el otro los *ack*
3. **Fin de conexión**: Después de la transmisión del último mensaje, un *ack* final es enviado para terminar la conexión



# NAT

As we know, private addresses cannot be used over the internet. NAT will allow my PC to borrow the unique IP address of my **router** to access the internet.

NAT se usa para modificar el IP de la fuente/destino de los paquetes. Cuando un equipo envíe un paquete fuera de nuestra red, una vez que el paquete llegue al *router*, NAT traducirá el IP privado de PC1, al IP público de la interfaz del *router*. Cuando el *router* reciba la respuesta del servidor, NAT se encargará de traducir la IP pública del *router* a la IP privada de PC1.
****

## Static NAT

NAT estático involucra la configuración manual *one-to-one* de los *mapeos* de dirección IP privadas a públicas.
Esto significa que si hay más de 1 equipo en la red que quiera conectarse al internet, cada uno necesitará su propia IP.

Una dirección IP "inside local" es *mapeada* a una "inside global".
- "inside local" se refiere a la IP del *host* en la red, desde la perspectiva de la red local. Es la **IP privada** del *host*
- "inside global" se refiere a la IP del *host* en la red, desde la perspectiva de los equipos fuera de la red local. Es la IP del *host* después del **NAT**, o sea, el **IP público**

En "Static NAT" no usamos la IP pública que se encuentra en la interfaz del *router*.


### Configuración

**Antes de configurar, revisamos comando informativo**
1. Con **"show ip nat statistics"**
	1. Nos muestra las estadísticas de NAT
	2. total active translations: Nos muestra las traducciones estáticas, dinámicas y "extended"
	3. Peak translations: el número máximo de traducciones que han habido en un determinado momento

**Lo primero que debemos hacer es configurar las interfaces del *router***
1. Elegimos la interfaz que configuraremos como "interna"
	1. Ingresamos el comando **"ip nat inside"** para definir la interfaz como "interna"
2. Ahora entramos a la interfaz que configuraremos como "externa"
	1. Ingresamos el comando **"ip nat outside"**

**Ahora deberemos configurar los *mapeos* de las direcciones IP**
1. Con el comando **"ip nat inside source static </ip_local> </ip_global>"**
	1. "ip local" será la IP privada del equipo
	2. "ip global" será la IP pública que usará el PC

**¿Cómo podemos ver las direcciones configuradas?**
1. Con **"show ip nat translations"**
	1. Nos mostrará las direcciones "inside global", "inside local" que configuramos
	2. Entradas dinámicas se crearán cuando las direcciones se usen, incluirán las direcciones "outside local" y "outside global" y los protocolos usados, con puertos respectivos

- **outside local** es la dirección IP del *host* externo, desde la perspectiva de la red local
- **"outside global"** es la dirección IP del *host* externo, desde la perspectiva de la red externa


### Eliminación de las entradas en tabla NAT

Podemos elegir borrar las traducciones dinámicas de la tabla, con:
- **"clear ip nat translation * "**
****

## Dynamic NAT

Aquí, el *router* de forma dinámica *mapea* la dirección "inside local" a "inside global", como se necesite.
Usa una ACL para determinar que trafico debe ser traducido. Si la dirección IP de origen es permitida por la ACL, esta será traducida.
Las direcciones IP disponibles que existirán para ser usadas se configurarán dentro de una "POOL". Cualquier IP que sea traducida tomará una de las IP dentro de la *pool* apropiada para comunicarse por internet. Si por algún motivo no hay suficientes IP's dentro de la *pool*, se le llamará "NAT pool exhaustion". Esto provocará que paquetes con origen en estás IP que no se pudieron traducir sean botados por el *router*.

### Configuración 

**Al igual que en NAT estática, configuramos las interfaces del *router***
1. Elegimos la interfaz que configuraremos como "interna"
	1. Ingresamos el comando **"ip nat inside"** para definir la interfaz como "interna"
2. Ahora entramos a la interfaz que configuraremos como "externa"
	1. Ingresamos el comando **"ip nat outside"**

**Ahora debemos crear una ACL para definir el tráfico que será traducido**
1. **"ip access-list standard </numero_o_nombre>"** para entrar al modo de config de ACL
2. Configuramos entradas con  "[entry_number] {deny|permit} </ip> </wildcard>"

**A continuación definimos la *pool* de los "inside global"**
1. **"ip nat pool </nombre_pool> </rango> </rango> netmask </netmask>"**

**Finalmente, configuramos la NAT *mapeando* la ACL a la *pool**
1. Usamos **"ip nat inside source list </numero_o_nombre_ACL> pool </nombre_pool>"**
****

## PAT

Port Address Translation (AKA NAT overload) translates both the IP address and the port number (if necessary).

¿Cúal es el proposito de traducir un puerto?
Al usar un número único de puerto para cada flujo de comunicación entre *hosts* internos y externos, 1 IP puede ser usado por varios *hosts* internos. 
El *router* mantendrá un registro de estos flujos gracias a estos puertos únicos. 


## Configuración

**La configuración es exactamente la misma a la NAT dinámica, hasta que llegamos a la parte de configurar la *pool* y la ACL, donde agregamos "overload" al final**
1. Usamos **"ip nat inside source list </numero_o_nombre_ACL> pool </nombre_pool> overload"


### Configuración alternativa

En este modo le decimos al *router* que traduzca las IP's privadas al IP de su interfaz externa.

**En vez de especificar una *pool*, especificamos la interfaz***
1. Usamos **"ip nat inside source list </numero_o_nombre_ACL> interface </interfaz> overload"



# QoS

Las redes modernas  son redes convergidas de teléfonos IP, video, tráfico regular, etc.
Todos estos distintos tipos de tráfico deben competir por la *bandwidth* disponible, ahí es donde entra **"Quality of Service"** (QoS).

QoS es un grupo de herramientas usadas por los equipos de la red para aplicar distintos tratos a distintos paquetes. Maneja las siguientes características:
1.  **Bandwidth**
	1. QoS nos permite reservar una cierta cantidad de *bandwidth* para tráficos distintos
2. **Delay**
	1. **One-way delay** mide el tiempo que se demora en llegar a su destino
	2. **Two-way delay** mide el tiempo que se demora en ir y volver
3. **Jitter**
	1. Es la variación de la demora de "one-way delay" de varios paquetes enviados por la misma aplicación
4. **Loss**
	1. el % de paquetes que no llega a su destino


## Estándares recomendados para audio

1. **One-way delay**: <= 150ms
2. **Jitter**: <= 30ms
3. **Loss**: <= 1%
***

## Queuing

si un equipo recibe mensajes más rápido de lo que puede *forwardear*, los mensajes entrar en una lista de espera.
Los paquetes saldrán en el mismo orden en el que llegan. si la lista de espera se llena, paquetes nuevos que lleguen serán botados, esto se llama **"tail drop"**. "Trail drop" puede llevarnos a **"TCP global synchronization"**.

Cuando "trail drop" ocurra, los *hosts* TCP que se encuentren enviando tráfico bajaran el volumen que es enviado. Al estar la red siendo poco usada, ahora aumentarán el volumen de tráfico enviado, generando oleadas de de alto tráfico. Este proceso se repetirá de nuevo.

Una solución para prevenir esto es **"Random Early Detection"** (RED). Cuando la *queue* llegue a un cierto nivel, el equipo empezará a botar paquetes de flujos específicos. Estos paquetes que sean botados reducirán el volumen de tráfico que está siendo enviado, pero evitará la "global TCP synchronization".


### Congestion Management

Una parte esencial de QoS es que existen **multiples *queues***. El tráfico podrá entrará a estas distintas filas dependiendo de varios factores, como las marcas.
El equipo usa un "scheduler" para determinar que *queue* *forwardeará* tráfico de forma siguiente. Esto le permite asignarle diferente prioridad a las listas.

Un método común de *scheduling* es **"weighted round-robin"**. Funciona tomando paquetes de cada lista, de forma cíclica y, tomando más paquetes de las listas de alta prioridad, cuando es su turno.

**CBWFQ** es otro método que toma elementos del "weighted round-robin" y le asegura a cada 
*queue* un porcentaje del ancho de banda de la interfaz, durante congestión.

Ambos métodos no son ideales para voz o video, ya que como deben esperar su turno en la cola, puede agregar "delay" o "jitter".
Esto lo podemos resolver con **"Low Latency Queuing"** (LLQ). Funciona estableciendo "strict priority queues", estas colas siempre serán priorizadas sobre otras mientras existan paquetes dentro de ella.

#### Shaping and Policing

Son usadas para controlar el flujo del tráfico.

- "Shaping" funciona como amortiguador si es que la tasa de tráfico en una *queue* aumenta sobre lo configurado.
- "Policing" bota el tráfico si es que la tasa aumenta sobre lo configurado.
	- Policing nos permite aceptar tráfico que viene en *bursts*, sin tener que botarlo
	- También nos permite cambiar la marca del tráfico en vez de solo botarlo
****

## Classification

El propósito de QoS es darle a algunas redes prioridad, sobre otras, en cuanto al tráfico, durante periodos de congestión.
Hay varios métodos para clasificar trafico, entre los cuales se encuentran:
1. Una ACL
2. Network Based Application Recognition (NBAR)
3. Campos específicos dentro de los *header* de L2 y L3
	1. PCP en el campo de 802.1Q
	2. DSCP en el paquete L3

### PCP

Su largo es de 3 *bits*, dando 8 posibles valores, cada valor representa una distinta prioridad. 
Debido a que PCP solo se encuentra en el header 802.1Q, solo se puede usar en estas conexiones:
1. Interfaces *trunk*
2. Interfaces *access* con una *voice VLAN*

 ![[PCP_values.png]]

### DSCP

Algunas de las marcas que son importante saber:
1. **Default Forwarding**
	1. "Best Effort Traffic"
	2. Significa que no hay garantía de que el tráfico llegará a destino. o que cumple algún estándar QoS
2. **Expedited Forwarding**
	1. Para tráfico de voz
3. **Assured Forwarding**
	1. No es una marca en si, sino una *set* de 12 valores distintos
	2. Incluye video interactivo, con marca AF4x y *streaming*, con marca AF3x
	3. Incluye info de alta prioridad, con marca AF2x
4. **Class Selector**
	1. Otro *set* de 8 valores, nos da compatibilidad con IPP


## Trust Boundaries

Definen en que lugar los equipos confiarán, o no, las marcas de QoS recibidas en los mensajes. Si las marcas son confiables, los equipos *forwardearán* el mensaje sin cambiar las marcas. Si no se confía en las marcas, el equipo las cambiará de acuerdo a las políticas configuradas.

En general, puede ser necesario cambiar el lugar de la *boundary* para evitar que la marca en el paquete sea cambiada, como puede pasar con teléfonos IP (queremos que su prioridad sea más alta).



# Security Fundamentals


## CIA

La triada CIA es una de las bases de seguridad informatica

- **Confidentiality**
	- Solo usuarios autorizados deben poder acceder a la información
- **Integrity**
	- La información debe mantenerse sin manipular por usuarios no autorizados, y mantenerse correcta
- **Availability**
	- Las redes y sistemas deben mantenerse operativos y accesibles a usuarios autorizados.


## Conceptos de seguridad

### Vulnerabilidad

Una vulnerabilidad es cualquier debilidad potencias que puede comprometer la **CIA** de un sistema, pero en si, una vulnerabilidad en si no es un problema,


### Exploit

Un ***exploit*** es algo que puede ser usado para explotar la vulnerabilidad, pero tampoco es un problema por si solo.


### Threat

Una **amenaza** es el potencial de una **vulnerabilidad**  para ser **explotada**.
Un *hacker* explotando una vulnerabilidad en el sistema es una **amenaza**.


### Mitigation

Una **técnica de mitigación** es algo que puede proteger frente a amenazas. Deben ser implementadas en cualquier parte que una **vulnerabilidad** puede ser explotada.


## Ataques comunes

- **DoS**
	- Los ataques "Denial of Service" amenazan la disponibilidad de un sistema
	- Una forma de ataque común es un *flood* de ataques TCP SYN
		- El atacante nunca responde con ACK y provoca que las conexiones incompletas llenen la tabla de conexiones TCP
	- Un **DDoS** usa varios equipos distintos para atacar
- **Spoofing Attacks**
	- To spoof an address is to use a fake address
	- un ejemplo es "DHCP exhaustion"
		- Un atacante usa un MAC *spoofeado* para *floodear* mensajes "DHCP Discover"
- **Reflection/Amplification attacks**
	- En un ataque "reflexion", el atacante envía tráfico a un reflector, que *spoofea* la dirección de origen de los paquetes a la dirección IP del objetivo
	- Un ataque "reflexion" se vuelve de **amplificación** cuando el volumen de tráfico enviado por el atacante es pequeño, pero gatilla que un gran volumen de tráfico se envíe  del **reflector** al **objetivo**
- **Man in the middle**
	- El atacante se ubica entre el origen y el destino para escuchar las conversaciones o manipularlas
	- Un ejemplo es **"ARP Spoofing/Poisoning"**
		- Un *host* envía un ARP Request, que también recibirá el mensaje y entregará su dirección MAC, haciéndose pasar por el objetivo original del ARP Request
- **Reconnaissance Attacks**
	- Se usan para ganar información acerca del objetivo
	- **nslookup**
- **Malware**
	- Virus
	- **Worms**
		- Se propagan por si mismos
	- **Trojan Horse**
		- *Software* malicioso que aparenta ser legitimo. Se propagan a través de interacción por usuarios., como correos o descargando archivos
- **Social Engineering**
	- Su objetivo son las personas
	- **Phishing**
		- Correos fraudulentos que aparentan ser de negocios o personas legitimas, y contienen *links* fraudulentos que contienen *malware* o a sitios maliciosos
		- **Spear Phishing** 
			- Es empleado frente a personas especificas
		- **Whaling** 
			- Es empleado frente a personas muy importantes dentro de la organización
		- **Vishing**
			- Empleado en llamadas telefónicas
		- **Smishing**
			- Empleado en mensajes SMS
	- **Watering Hole**
		- Compromete sitios web que el objetivo visita con frecuencia
	- **Tailgating**
		- Entrar a areas restringidas al seguir una persona autorizada mientras ellos entran
- **Password Attacks**
	- **Dictionary Attack**
		- Un programa usa un "diccionario" de contraseñas comunes para encontrar la del objetivo
	- **Brute Force Attack**
		- Un programa intenta cada combinación posible de carácteres para encontrar la contraseña


## MFA

Involucra necesitar mas que solo una contraseña y usuario para probar la identidad:
- Algo que tu sabes
	- Contraseña, PIN, etc
- Algo que tienes
	- ID, celular
- Algo que eres
	- Huella digital y otros datos biométricos


## Certificados Digitales

Se usan para autentificar la identidad de sitios web o programas



## Authentication, Authorization and Accounting

- **Authentication** es el proceso de verificar la identidad de alguien
- **Authorization** es el proceso de otorgar permisos apropiados según el tipo de usuario
- **Accounting** es el proceso de guardar registros de la actividad de los usuarios en el sistema

Empresas usan servidores AAA para proveer estos servicios. Usan los siguientes protocolos:
- **RADIUS**
	- Estándar abierto, usa puertos UDP 1812 y 11813
- **TACACS+**
	- Protocolo propietario de Cisco, usa puerto TCP 49



## Security Program Elements

- **Programas "User Awareness** 
	- Son diseñados para enseñar a empleados sobre riesgos y amenazas potenciales.
- **User Training**
	- Pogramas mas formales que los de arriba
- **Physical Access Control**
	- Protege Equipos y data mediante controles físicos a zonas protegidas

## Port Security

Es una característica de seguridad de los *switches* de Cisco. Nos permite controlar que direcciones MAC son permitidas para entrar en las interfaces.

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
- Revisar VLAN y DTP

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
			1. Apaga la interfaz y envía una notificación Syslog o SNMP
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

## DHCP Snooping


Funciona asignándole confianza a los puertos "downlink" (apuntan a los *end hosts*) y "uplink" (apuntan a los equipos de red ). Los *uplink* se consideran de confianza y los *downlink* no.
DCHP *snooping* no inspeccionará mensajes que lleguen de puertos de confianza, pero si de los de no confianza.
Según DHCP, los mensajes que enviarán los *hosts*, de los DORA, son "discover" y "request". Estos son los que serán inspeccionados en las interfaces *downstream*.

Un ejemplo de ataque que ayuda prevenir este método es contra "DHCP Starvation" o "DHCP Poisoning.
***

## Inspección de puertos

Estos son las acciones que se tomará en los puertos  según los mensajes que recibe:
- Si se recibe un mensaje DHCP es un puerto de confianza -> se *forwardea* de forma normal
- Si se recibe un mensaje en un puerto de no confianza:
	- Si es de "DHCP Server", **se descarta**
	- si es de "DHCP Client", se inspecciona:
		- Para Discover/Request: Se verifica que la dirección MAC de origen del *frame* y el campo CHADDR de DHCP coincidan. Si coinciden, se *forwardea*
		- Para Release/Decline: Se verifica que el IP de origen del paquete y la interfaz que recibe calzan en la *entry* que se encuentra en la "DHCP Snooping Binding Table". Si coinciden, se *forwardea*



## Configuración

**Primero, habilitamos *snooping* desde *config mode***
1. con **"ip dhcp snooping"**
2. Ahora debemos activarlo en cada VLAN necesaria, con **"ip dhcp snooping vlan </numero>"**
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
	1. Mismo formato que para Port Security
***


## DHCP Option 82

También conocido como "DHCP relay agent information option". Provee más información acerca de cual "DHCP relay agent" recibió el mensaje del cliente, en que interfaz, VLAN, etc.
Estos mensajes son regularmente agregados por los *relay agents* que reciban el mensaje pero, con "DHCP Snooping" habilitado, los *switches* automáticamente agregarán está opción cuando reciban mensajes de clientes, aunque **no estén actuando como *relay agents***.

Si tenemos "DHCP Snooping" habilitado, los switches de Cisco automáticamente botarán mensajes que posean este mensaje, cuando sean recibidos en puertos de no-confianza.

**Se deshabilita en *conf t* con**
**"no ip dhcp snooping information option"**

## Dynamic ARP Inspection


DAI is a feature of switches that is used to filter ARP messages received on untrusted ports. 

Al igual que en DHCP Snooping, todos los puertos son de no-confianza, por defecto. Pero en este caso, todos los puertos que estén conectados a otros equipos de red (routers, switches) serán de confianza, mientras que las interfaces que se encuentren conectadas a *end hosts* serán de no-confianza.



## ¿Cómo funciona?

DAI inspecciona la MAC e IP de origen en el mensaje ARP al ser recibido en puertos de no-confianza y verifica que exista una entrada que coincida, con esa MAC e IP, en la "DHCP snooping binding table".
Si existe una entrada que calce con la información del mensaje, este se *forwardea*. si no, no.

Se pueden configurar ACL's de ARP manualmente, para *mapear* direcciones IP a direcciones MAC para ser verificadas por DAI. Útil para *hosts* que no usen DHCP.



## Configuración

**Partimos habilitando DAI **
1. Con el comando **"ip arp inspection vlan </numero>"**
	1. Habilitar en la 1 si no estamos usando otras
	2. Habilitar para cada VLAN

**Ahora elegimos interfaces y elegimos puertos de confianza**
1. Con **"ip arp inspection trust"**

**Podemos ver las interfaces DAI con**
1. **"show ip arp inspection interfaces"**
	1. "Burst Interval" se refiere a limitar X paquetes cada Y segundos

**Ahora elegiremos interfaz y configuraremos "rate limiting"**
1. Con **"ip arp inspection limit rate </paquetes> burst interval </segundos>"**

**También podemos volver a activar la interfaz de forma manual o con "ErrDisable"**
1. Con **"errdisable recovery cause arp-inspection"**


### *Chequeos* opcionales

Por defecto, DAI verifica el IP y MAC de origen en el mensaje, pero también se puede configurar para que *chequee* MAC de destino e IP's inesperados. Estas verificaciones se hacen de forma extra a las por defecto, elegir una no las reemplaza.

**Esto se puede hacer con **
1. **ip arp inspection validate [dst-mac | ip | src-mac]**


## ARP ACL's

**Podemos configurar una con**
1. **"arp access-list </nombre_acl>"**
2. Configuramos la lista con **"permit ip host </ip> mac host </mac>"**
3. Ahora la aplicamos con **"ip arp inspection filter </nombre_acl> vlan </numero_vlan>"**




# Virtualization & Cloud


## Tipos de Virtualización

Existen 2 tipos distintos que son usados:
1. **Type 1 Hypervisor**
	1. Corre directamente sobre el *hardware*
	2. Se usa en *data centers*
2. **Type 2 Hypervisor**
	1. Corre como un programa cualquiera en un OS
	2. El OS que corre directamente sobre el *hardware* se llama "Host OS" y el que se encuentra corriendo en la VM se llama "Guest OS"


## Conectando VM's a la red

LAs VM's se conectan entre ellas y con la red externa mediante switches virtuales que corren en el hipervisor Estos pueden funcionar de manera normal, con puertos *trunk* y *access*, uso de VLAN's, etc.
Las interfaces en el *switch* virtual se conectan a la NIC del servidor para comunicarse con la red externa.

## Cloud Services




## Características esenciales

Poseen 5 características esenciales:
1. **On demand self-service**
	1. Una consumidor puede, de forma unilateral, adquirir más capacidades de computación, como almacenamiento y funcionalidades, sin tener que requerir interacción humana con el proveedor *cloud*
2. **Broad network access**
	1. Los servicios se encuentran disponibles para ser accedidos mediante conexiones de red estándar y, pueden ser accedidos por una variedad de equipos distintos.
3. **Resource pooling**
	1. Los recursos de computación del proveedor se encuentran disponibles para servir a una gran cantidad de clientes. Con una capacidad de dinámicamente ajustar lo recursos físicos y virtuales para adecuarse a la demanda
4. **Rapid elasticity**
	1. Clientes pueden expandir, o disminuir, rápidamente los servicios que usan, desde una *pool* que aparenta ser infinita
5. **Measured service**
	1. El proveedor *cloud* mide el uso de los servicios por parte del usuario y, les es cobrado basado en ese uso (ej. x pesos por GB por día)


## Modelos de servicio

Todo es provisto por un modelo de servicio. Hay distintos tipo de modelo que ofrecen distintos niveles de servicio, necesitando adoptar distintos niveles de responsabilidad según cada cual.

Existen 3 modelos:
1. **Software as a Service**
	1. el cliente utiliza las aplicaciones que se encuentran en la *cloud* del proveedor.
	2. El cliente no maneja o controla la infraestructura subyacente, solo una limitada configuración de la aplicación
2. **Platform as a Service**
	1. El cliente despliega en la infraestructura *cloud* sus propias aplicaciones, herramientas o servicios.
	2. El consumidor no maneja la infraestructura subyacente, pero tiene control sobre las aplicaciones y algunas de las configuraciones del ambiente
3. **Infrastructure as a Service**
	1. Al cliente se le otorga capacidad de procesamiento, almacenamiento, redes y otros servicios computacionales, donde es capaz de correr *software* arbitrario
	2. El consumidor maneja OS, almacenamiento, aplicaciones y algunos componentes de red


## Modelos de Despliegue de *Cloud*

Se refiere a si la *cloud* es privada o pública:
1. **Private cloud**
	1. La infraestructura de provista solo para uso de la organización
	2. Aunque es privada, el dueño puede que sea un tercero
2. **Community cloud**
	1. Es de uso exclusivo por una comunidad o consumidores de organizaciones con objetivos en común
3. **Public cloud**
	1. Provista de uso público para el público general
4. **Hybrid cloud**
	1. La infraestructura *cloud* está compuesta de 2 o más infraestructuras *cloud* únicas, pero unidas por tecnologías propietarias




## Containers

Son paquetes de *software* que contienen una aplicación con todas las dependencias para que pueda funcionar. Cada contenedor corre en un "Container Engine", que corren es un OS.

Un "Container Orchestrator" es una plataforma de *software* para automatizar el manejo de los contenedores.
Por ejemplo:
- **Kubernetes**
- **Docker Swarm**


![[Container.png]]

## VRF


"Virtual Routing & Forwarding" es usado para dividir un *router* en varios *routers* virtuales. Es similar a dividir LAN's en varias VLAN's.
Esto provoca que el *router* maneje varias tablas de *routeo* separadas. Cada ruta e interfaz (solo L3) se configura para estar en una **VRF especifica**.

VRF es usado por proveedores para permitir que 1 equipo lleve tráfico de varios clientes distintos. El tráfico de cada uno estará aislado del resto.
Otra cosa importante es que VRF le permite a los usuarios que las direcciones IP puedan ser iguales.

![[VRF.png]]
***

## Configuración

**Primero debemos crear la VRF**
1. Empezamos con el comando **"ip vrf </nombre_vrf>"**

**Ahora debemos entrar a una interfaz y asignarle una VRF
1. Y usamos el comando **"ip vrf forwarding </nombre_vrf>"**

**Podemos ver todas las VRF's configuradas **
1. Con **"show ip vrf"**

**Si queremos ver la tabla de *routeo* tenemos que ver una especifica para cada VRF**
1. Se usa **"show ip route vrf </nombre_vrf>"**



# Wireless 

Los estándares para LAN's *wireless* están definidos en IEEE 802.11.


## Características

1. Todos los equipos en el rango de la red reciben los paquetes (como un Hub)
		1. Puede generar inquietudes en cuanto a la privacidad/seguridad
		2. Se usa CSMA/CA para facilitar conexiones **half-duplex** y evitar colisiones
2. Están reguladas por varios cuerpos internacionales
3. Se debe considerar el rango de cobertura de la red		
	1. Considerar:
		1. **Absorción**
			1. La señal es absorbida por los materiales donde pasa y se pierde una parte 
		2. **Reflexión**
			1. LA señal rebota al llegar a un material
		3. **Refracción**
			1. La señal cambia de rumbo al entrar a un medio diferente
		4. **Difracción**
			1. Ocurre cuando una onda, al atravesar un orificio pequeño, se distorsiona y propaga en todas direcciones
		5. **Dispersión**
			1. Separación de las ondas a ondas de distinta frecuencia al atravesar un material
2. Otros equipos usando el mismo canal pueden causar interferencia
***


## Frecuencias de Radio

Para enviar señales *wireless*, se le debe aplicar corriente alterna a la antena, creando campos electromagnéticos que se propagan como ondas.
Las ondas electromagnéticas pueden ser medidas en cuanto a su **amplitud** y **frecuencia**:
- **Amplitud**: La fuerza maxima del campo electromagnético
- **Frecuencia**: La cantidad de veces que se repite la onda en 1 segundo


### Bandas de Frecuencias de Radio

Wi-Fi usa 2 bandas de radio (rangos de frecuencia):
- **2.4 GHz**
	- Nos da más alcance y mayor penetración de obstáculos
	- Al ser usada por más equipos es más probable encontrarse con interferencia
- **5 GHz**
	- Más rápidas que 2.4 GHz


#### Canales

Cada banda se divide en multiples canales y, cada equipo está configurado para transmitir y recibir tráfico en uno o más de estos.
En redes pequeñas con un solo *access point*, podemos usar cualquier canal. Pero en redes grandes con varios *access points*, es importante que no se usen canales que se superpongan, y así evitar interferencias.
En la banda **2.4 GHz**, se recomienda que se usan los canales 1, 6 y 11. Si ubicamos los *access points* en forma de panal de abeja, podemos proveer un area de completa cobertura sin generar interferencia entre canales

![[panal de abeja.png]]


## Estándares 802.11

![[Estándares 802.11.png]]
***

## Service Sets

Los "service sets" son grupos de equipos de conexión inalámbrica.
Existen 3 tipos:
- **Independent**
- **Infrastructure**
- **Mesh**
Todos los equipos en un mismo *set* comparten el mismo **SSID** (service set identifier). El **SSID** es un nombre facil de leer que identifica un *set* en especifico.

### Tipos
- **IBSS**
	- Un **IBSS** es una red inalámbrica en la cual 2 o más equipo inalámbricos están conectados directamente **sin usar un** **AP**
- **BSS**
	- Es un tipo de *set* de infraestructura en el cual los clientes se **conectan entre ellos mediante un AP**
	- Un **BSSID** es usado para identificar al AP
		- Es la direccion MAC
	- Para ser parte de la BSS, los equipos deben solicitar asociarse
	- La **BSA** (Basic Service Area) es el área de servicio alrededor de la AP donde la señal se puede usar ![[service_set_BSS.png]]
- **ESS**
	- Se usa para crear un gran red inalámbrica LAN más allá del rango de una AP
	- Cada AP tendrá el **mismo SSID** y **distinto BSSID**
	- Los clientes pueden pasar entra AP's sin tener que reconectarse
	- Pueden estar en distintos canales![[Service Set - ESS.png]]
	- **MBSS**
		- Puede usarse en situaciones donde es difícil conectar cada AP al Ethernet
		- Se usa una *mesh* para conectar ciertas AP's de forma inalámbrica y formar una red, además de proveer de internet a los clientes
		- La AP que se conecta a la red alambrica se llama **RAP** (Root Access Point)
		- Las otras AP's se llaman **MAP** (Mesh Access Point)
		- Se usa un protocolo para determinar el mejor camino a través de la *mesh*
***


## Distribution System

La mayoría de la redes inalámbricas no son *standalone*, sino que son una manera de conectar a los clientes a la red alámbrica. Cada **BSS** o **ESS** está *mapeado* a una VLAN en la red alámbrica y conectada a ella mediante un *trunk.
La red *upstream* se llama **DS**, para "Distribution System".


## Modos operacionales AP

Un AP puede actuar como **repetidor**, extendiendo el rango de una **BSS**. Debe operar en el mismo canal, pudiendo reducir el rendimiento.

Otro modo operacional es **"Workgroup Bridge"**. Opera como cliente de una AP, y puede ser usado para conectar equipos alámbricos a la red inalámbrica.

Un **"Outdoor Bridge"** puede usar para conectar redes a grandes distancias sin un cable físico.

## Wireless Architectures



## 802.11 Frame Format

Algunos campos difieren entre versiones
![[802.11_frame.png]]

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
	- Mismo que el de Ethernet Frame, usado para *chequear* errores
***

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
***

## AP Deployment Methods

Existen 3 tipos inalámbricos principales de desplegar AP's

### Autonomous AP's

Estos son sistemas autónomos que no necesitan de un WLC, por lo que deben configurarse individualmente. Esto se puede hacer mediante la interfaz "console", usando Telnet, SSH, o una conexión GUI, por lo que se le debe configurar una dirección IP para poder acceder remotamente.

Las AP autónomas deben conectase a la red alámbrica mediante una conexión "trunk". El tráfico de manejo que se usa para conectarse remotamente a las AP's, u a otros equipos, debe estar en una VLAN separada.

En las AP autónomas, cada VLAN debe extenderse a lo largo de toda la red, lo que se considera mala practica. 
En primer lugar, porque esto genera grandes "broadcast domains", provocando que cada mensaje se envíe por toda la red.
Otra razón es porque STP desactiva *link* y que la configuración de varias VLAN's toma harto tiempo.

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

![[lightweight_&_autonomous_AP.png]]


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

## Wireless Security



## Authentication Methods

- **Open Authentication**
	- Se autoriza a todos
	- Puede ser que luego de estar autenticado, el usuario necesite autenticarse por otros medios antes de ser otorgado acceso a internet (wifi públicos)
- **WEP**
	- Autentica y encripta tráfico
	- Encripta usando el algoritmo RC4
	- WEP usa llaves simétricas para encriptar
	- **No es seguro** y no debería usarse
- **EAP**
	- Es un *framework* de autenticación, define un grupo estándar de funciones de autenticación
	- Está integrado con 802.1X, el cual se usa para limitar acceso a la red a clientes, hasta que se autentiquen
- **LEAP**
	- Clientes deben proveer usuario y contraseña
	- Tanto el cliente como el servidor se envían "challenge phrases"
	- Se usan llaves WEP dinámicas
	- **No debería usarse**
- **EAP-FAST**
	- Consiste de 3 fases:
		1. Se genera un "PAC" y se pasa del servidor al cliente
		2. Se establece un túnel TLS entre el cliente y el servidor de autenticación
		3. Dentro del túnel se realiza el proceso de autenticación/autorización
-  **PEAP**
	- También se establece un túnel TLS pero , en vez de un PAC, el servidor usa un certificado digital
	- El cliente se autentica dentro del túnel TLS
- **EAP-TLS**
	- Requiere tanto un certificado en el servidor como en cada cliente
	- Es el método de conexión inalámbrica más seguro


## Encryption and Integrity Methods


- TKIP
	- Se creo como una solución temporal al problema que presentaba WEP, hasta que se creara un nuevo estándar
	- Usa un *hash* para validar la integridad de los mensajes
	- Una un "key mixing algorithm" para crear llaves WEP únicas
	- Se usa en **WPA**
- **CCMP**
	- Desarrollado después de TKIP
	- Es usado en **WPA2**
	- No puede ser usado en *hardware* antiguo
	- Consiste en 2 algoritmos para dar encriptación y *hash*:
		- **AES**
			- Es el protocolo de encriptación más seguro, actualmente.
		- **CBC-MAC**
			- Es usado como MIC (*hash*) para validar la integridad de los mensajes
- **GCMP**
	- Más seguro y eficiente que CCMP
	- Esa usado en **WPA3**
	- Consiste de 2 algoritmos:
	- **AES Counter Mode** para encriptar
	- **GMAC** usado para generar el *hash*



## WAP

Este protocolo incluye WPA, WPA2 y WPA3. Cada uno de ellos soporta 2 modos de autenticación: 

1. **Personal Mode**: Mediante uso de una "pre-shared key" (PSK), donde la autenticación se hace mediante una contraseña. La PSK no en enviada a través del aire, sino que se se usa un *handshake* de 4 pasos para lograr la autenticación. La PSK se usa para generar las llaves de encriptación
2. **Enterprise Mode**: 802.1X es usado con un servidor de autenticación (como RADIUS). Soporta todos los métodos EAP

**WPA** se desarrolló luego que se demostrara que **WEP** era vulnerable, incluye los siguientes protocolos:
- **TKIP**
- **802.1X o PSK**

**WPA2** se lanzó en 2004 e incluye los siguientes protocolos:
- **CCMP**
- **802.1X o PSK**

**WPA3** se lanzó en 2018 e incluye los siguientes protocolos:
- **GCMP**
- **802.1X o PSK**
También incluye otras funcionalidades como **PMF**, **SAE** y **Forward Secrecy**.

## Wireless Configuration


**Lo primero que debemos hacer es configurar las VLAN's**
1. Empezamos con **"vlan <numero_vlan>"**
2. Le ponemos nombre **"name </nombre>"**
3. Repetimos con el rest de las VLAN's que queramos configurar
	1. Management, internal o guest

**Ahora configurar las interfaces conectadas a las AP en modo *access***
1. Elegimos interfaz **"interface </interfaz>"**
2. Elegimos el modo con **"switchport mode </modo>"**
3. Asignamos la VLAN a al *access* con **"switchport access vlan </numero_vlan>"**
4. Habilitamos portfast **"spanning-tree portfast"**

**Configuramos el EtherChannel entre el WLC y el *switch***
1. Con **"interface </rango>"**
2. Y **"channel-group </grupo> mode </modo>"**
	1. WLC's solo soportan el modo estático

**Ahora configuramos la interfaz con la WLC en modo *trunk***
1. Elegimos la interfaz *ether* **"interface </interfaz>"**
2. La designamos como *trunk* **"switchport mode trunk"**
3. Configuramos las VLAN's permitidas con **"switchport trunk allowed vlan </numero>"**

**Configuraremos una SVI para cada VLAN, que será  usada como la "default gateway de sus subnets"**
1. Elegimos vlan **"interface vlan </numero_vlan>"**
2. Asignamos ip **"ip address </ip> </netmask>"**
3. Repetimos para cada VLAN

**Ahora configuramos DHCP para cada VLAN**
1. empezamos con **"ip dhcp pool </nombre_pool>"**
2. elegimos las direcciones de la pool **"network </ip_red> </netmask>"**
3. Determinamos la "default gateway" que usaran **"default-router </ip>"**
4. (Opcional)Usamos **"option 43 ip </ip_del_WLC>"**
	1. Esto sirve para decirle a las AP's cual es el IP del WLC, y así formar los túneles CAPWAP
	2. Es necesario usarlo si el WLC no está dentro del área *broadcast* de las AP's
5. Repetimos para cada VLAN

**Ahora configuramos NTP**
1. Elegimos un *switch* como servidor con **"ntp master"**



## WLC Ports & Interfaces

En una WLC, los puertos son físicos donde se conectan los cables y las interfaces son interfaces "logicas" dentro del WLC.

1. **Management Interface**
	1. Usada para el tráfico de manejo, como Telnet, SSH, HTTPS, RADIUS
	2. Túneles CAPWAP también se forman de esta interfaz
2. **Redundancy Management Interface**
	1. Cuando 2 WLC's se conectan por sus puertos de redundancia, 1 WLC se pone "activa" y la otra en "standby". Está interfaz se usa para manejar la "standby"
3. **Virtual Interface**
	1. Se usa para comunicar con clientes inalámbricos para enviar solicitudes DHCP, autenticación de web, etc
4. **Service Port Interface**
	1. Usada para manejo "out-of-band"
5. **Dynamic Interface**
	1. Usadas para *mapear* la WLAN a una VLAN



CPU ACL's se usan para limitar acceso a la CPU de la WLC



# Network Automation


Tener que configurar cada equipo por separado trae varias desventajas, entre ellas, pequeños errores de tipeo y que toman mucho tiempo en configurar en redes más grandes.

La automatización, por el otro lado, trae muchas ventajas: Se reducen los errores por tipeo, se pueden ejecutar cambios a gran escala en una fracción del tiempo y el aumento de la eficiencia en las operaciones.
***

## Logical Planes

Antes de continuar, daremos una pasada por las **"logical planes"** de las funciones de red.
Estas funciones no las maneja el CPU, ya que es lento, relativamente hablando. Sino que las maneja *hardware* especializado, un **ASIC**.

¿Qué hace un *router*? ¿Qué hace un *switch*?
Las funciones que los equipos de red realizan pueden dividirse "lógicamente" en distintas categorías, las *planes*:
- **Data Plane**
	- Todas las tareas que involucren *forwardear* trafico de una interfaz a otra
	- Decidir si descartar o no el tráfico
- **Control Plane**
	- Todo lo que involucre tomar las decisiones que toman los equipos al *forwardear* paquetes
		- Table de *routeo*, STP, etc
	- Todas las funciones que se encarguen de construir estas tablas
	- Controla lo que la "data plane" hace
- **Management Plane**
	- No afecta directamente el *forwardeo* de mensajes
	- Consiste en protocolos usados para manejar los equipos
		- SSH, Syslog, NTP

![[data planes.png]]

## Softwared-Defined Networking

Es un acercamiento a las redes que centraliza la "control plane" en una aplicación llamada "controller".

### Southbound interface

Es usado para que el controlador pueda comunicarse con los equipos de red que controla. Consiste en un protocolo de comunicación y una API.

Existe otro tipo de interfaz, llamada **Northbound Interface**. Esta interfaz es la que nos permite interactuar con el controlador y acceder a los datos que colecta y, configurar la red.
Una **REST API** se usa en el controlador como una interfaz para que las aplicaciones puedan interactuar con él.
La información se lleva estructurada en JSON o XML.

## JSON, XML & YAML

## Data Serialization

Antes de empezar con JSON en si, introduciremos "Data Serialization".

Es el proceso de convertir información en un formato estandarizado que puede ser almacenado o transmitido y reconstruido después. Esto permite que la información pueda ser comunicada efectivamente entre distintas aplicaciones.
Estos nos permite representan variables en texto.
***

JSON es un estándar abierto de formatos de archivos e intercambio de información que usa texto fácilmente legible para almacenar la información. En JSON los espacios vacíos no importan.

## Tipos de información

Tipos de datos no estructurados:
1. **String**
	1. Representa texto, siempre está rodeado por " "
2. **Number**
	1. Es un valor numérico
3. **Boolean**
	1. Representa "true" y "false"
4. **"Null**
	1. Representa la ausencia intencional de algún valor

Tipos de datos estructurados:
1. **Object**
	1. Una lista de variables "key-value" no ordenados
	2. Rodeados de { }
	3. La "key" es un "string"
	4. el "value" puede ser cualquier tipo de dato válido
	5. La "key" y el "value" se separan con un :
2. **Array**
	1. una serie de valores separados por comas
	2. No es necesario que sean el mismo tipo de dato


## XML

Usado como lenguaje para "data serialization".
- No es tan legible como JSON
- Espacios en blanco no importan
- Se representa igual que en HTML -> <key>value</key>


## YAML

ES usado por la herramienta de automatización de red "Ansible".
- Es muy facil de leer
- Los espacios en blanco si importan
	- Indentación es importante
- - Se usa para indicar una lista
- "Keys" y "values" se representan como "key:value"

Comparación entre JSON y YAML
![[JSON_vs_YAML.png]]

## RESTP API's

An API is a software interface that allows two applications to communicate with each other.
They encode data in either XML or JSON.

En una arquitectura Software-Defined Networking, las API's se usan para comunicar entre apps y el controlador SDN, y entre el controlador SDN y los equipos de red.



## CRUD

Se refiere a las operaciones que realizamos cuando usamos REST API's. Significa "Create, Read, Update, Delete".

- **Create**
	- Operaciones en las que creamos nuevas variables y designamos sus valores
	- Ej. Crear variable "ip" y asignarle el valor "10.1.1.1"
- **Read**
	- Usadas para obtener el valor de una variable
- **Update**
	- Usadas para cambiar el valor de alguna variable
- **Delete**
	- Usadas para eliminar variables

### HTTP

HTTP usa "verbs" o "methods" para *mapear* a las operaciones CRUD. Cuando un cliente HTTP envía una solicitud al servidor HTTP, este incluye un "verb" que indica lo que el cliente quiere hacer.
Es por esto que las REST API's típicamente usan HTTP como su protocolo L7 para comunicarse a través de la red.

![[HTTP_verbs.png]]

#### HTTP Request

Cuando un cliente HTTP envía una solicitud a un servidor HTTP, el *header* HTTP incluye:
- Un verbo HTTP
- Un URI (Uniform Resource Identifier)
	- Indica el recurso que se está intentando acceder


#### HTTP Response

La respuesta del servidor incluirá un código de estado que indicará si la solicitud fue exitosa o fracasó, y otros detalles.

Los dígitos indican el tipo de respuesta:
- **1XX**
	- Es una respuesta de tipo informativa. Indica que la solicitud fue recibida
- **2XX**
	- La solicitud fue recibida exitosamente, entendida y aceptada
- **3XX**
	- Se necesita mas procesos para que la poder completar la orden
- **4XX**
	- La solicitud contiene sintaxis equivocada o no puede ser completada
- **5XX**
	- El servidor fracasó en cumplir una solicitud válida, del cliente


## REST

REST no es una API en especifico, sino un grupo de reglas sobre como debería funcionar la API.

La arquitectura REST contiene 6 características:
1. **Client-Server**
	1. El cliente realiza llamadas API para acceder a los recursos del servidor
	2. El cliente y el servidor son independientes entre si
		1. Pero cuando se produzca algún cambio entre ellos, la interface entre ellos no puede romperse
2. **Stateless**
	1. Cada intercambio API es un evento separado.
	2. El servidor no guarda información sobre solicitudes pasadas para determinar como responder
3. **Cacheable or Non-Cacheable**
	1. REST API's deben ser capaces de *cachear* info
	2. No es necesario que todos los recursos sean *cacheables*




# Software-Defined Networking

Es un acercamiento a las redes que centraliza la "control plane" en una aplicación llamada "controller".
El controlador puede interactuar con los equipos de red usando API's.

- La **SBI** se usa para comunicar el controlador con los equipos de red que controla
- La **NBI** es lo que nos permite interactuar con el controlador


## SDN Architecture

- **Application Layer**
	- Contiene las aplicaciones/*scripts* que manejan el controlador
- **Control Layer**
	- Contiene el controlador y recibe las instrucciones del nivel de la aplicación
- **Infrastructure Layer**
	- Contiene los equipos de red que son responsables de *forwardear* los mensajes a través de la red

![[SDN_architecture.png]]








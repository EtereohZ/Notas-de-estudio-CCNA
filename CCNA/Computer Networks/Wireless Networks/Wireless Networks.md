Los estándares para LAN's *wireless* están definidos en IEEE 802.11.


## Características

1. Todos los equipos en el rango de la red reciben los paquetes (como un [[Hub]])
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
		- Es la direccion [[MAC Address|MAC]]
	- Para ser parte de la BSS, los equipos deben solicitar asociarse
	- La **BSA** (Basic Service Area) es el área de servicio alrededor de la AP donde la señal se puede usar ![[service_set_BSS.png]]
- **ESS**
	- Se usa para crear un gran red inalámbrica [[LAN]] más allá del rango de una AP
	- Cada AP tendrá el **mismo SSID** y **distinto BSSID**
	- Los clientes pueden pasar entra AP's sin tener que reconectarse
	- Pueden estar en distintos canales![[Service Set - ESS.png]]
	- **MBSS**
		- Puede usarse en situaciones donde es difícil conectar cada AP al [[Ethernet]]
		- Se usa una *mesh* para conectar ciertas AP's de forma inalámbrica y formar una red, además de proveer de internet a los clientes
		- La AP que se conecta a la red alambrica se llama **RAP** (Root Access Point)
		- Las otras AP's se llaman **MAP** (Mesh Access Point)
		- Se usa un protocolo para determinar el mejor camino a través de la *mesh*
***


## Distribution System

La mayoría de la redes inalámbricas no son *standalone*, sino que son una manera de conectar a los clientes a la red alámbrica. Cada **BSS** o **ESS** está *mapeado* a una [[VLAN]] en la red alámbrica y conectada a ella mediante un *trunk.
La red *upstream* se llama **DS**, para "Distribution System".


## Modos operacionales AP


Un AP puede actuar como **repetidor**, extendiendo el rango de una **BSS**. Debe operar en el mismo canal, pudiendo reducir el rendimiento.

Otro modo operacional es **"Workgroup Bridge"**. Opera como cliente de una AP, y puede ser usado para conectar equipos alámbricos a la red inalámbrica.

Un **"Outdoor Bridge"** puede usar para conectar redes a grandes distancias sin un cable físico.
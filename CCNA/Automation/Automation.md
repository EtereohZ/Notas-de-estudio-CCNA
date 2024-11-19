
Tener que configurar cada equipo por separado trae varias desventajas, entre ellas, pequeños errores de tipeo y que toman mucho tiempo en configurar en redes más grandes.

La automatización, por el otro lado, trae muchas ventajas: Se reducen los errores por tipeo, se pueden ejecutar cambios a gran escala en una fracción del tiempo y el aumento de la eficiencia en las operaciones.
***

## Logical Planes

Antes de continuar, daremos una pasada por las **"logical planes"** de las funciones de red.
Estas funciones no las maneja el CPU, ya que es lento, relativamente hablando. Sino que las maneja *hardware* especializado, un **ASIC**.

¿Qué hace un *[[Router|router]]*? ¿Qué hace un *[[Switch|switch]]*?
Las funciones que los equipos de red realizan pueden dividirse "lógicamente" en distintas categorías, las *planes*:
- **Data Plane**
	- Todas las tareas que involucren *forwardear* trafico de una interfaz a otra
	- Decidir si descartar o no el tráfico
- **Control Plane**
	- Todo lo que involucre tomar las decisiones que toman los equipos al *forwardear* paquetes
		- Table de *routeo*, [[STP]], etc
	- Todas las funciones que se encarguen de construir estas tablas
	- Controla lo que la "data plane" hace
- **Management Plane**
	- No afecta directamente el *forwardeo* de mensajes
	- Consiste en protocolos usados para manejar los equipos
		- [[SSH]], [[Syslog]], [[Network Time Protocol|NTP]]

![[data planes.png]]

## Softwared-Defined Networking

Es un acercamiento a las redes que centraliza la "control plane" en una aplicación llamada "controller".

### Southbound interface

Es usado para que el controlador pueda comunicarse con los equipos de red que controla. Consiste en un protocolo de comunicación y una API.

Existe otro tipo de interfaz, llamada **Northbound Interface**. Esta interfaz es la que nos permite interactuar con el controlador y acceder a los datos que colecta y, configurar la red.
Una **REST API** se usa en el controlador como una interfaz para que las aplicaciones puedan interactuar con él.
La información se lleva estructurada en [[JSON]] o XML.
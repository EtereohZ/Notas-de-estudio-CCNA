An API is a software interface that allows two applications to communicate with each other.
They encode data in either XML or [[JSON]].

En una arquitectura [[Soitware-Defined Networking]], las API's se usan para comunicar entre apps y el controlador SDN, y entre el controlador SDN y los equipos de red.



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
Es por esto que las REST API's típicamente usan HTTP como su protocolo [[OSI|L7]] para comunicarse a través de la red.

![[HTTP verbs.png]]

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



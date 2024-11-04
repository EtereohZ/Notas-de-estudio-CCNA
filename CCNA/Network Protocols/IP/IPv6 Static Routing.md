Funciona de la misma manera que [[Routing|routeo IPv4]].

Si el *routeo* IPv6 está deshabilitado, el *[[Router|router]]* podrá enviar y recibir tráfico IPv6, pero no podrá *routearlo*.

- Una "connected network route" es agregada automáticamente para cada red.
- Una "local host route" es automáticamente agregada para cada direction configurada en el *router*
- Rutas para direcciones "link-local" no son agregadas a la tabla

## Lectura de comandos en documentación

Lo que se encuentre dentro de "{}" significa que tienes que elegir **una** de las opciones que se encuentra dentro, estando las opciones separadas por una "|". Lo que se encuentre dentro de un "[]" es opcional.

- Usaremos de ejemplo "ipv6 route destination/prefix-length {next-hop | exit-interface [next-hop]} [ad]"
	- Debemos elegir **entre** un "next-hop" **O** una "exit-interface", con la opción de un "next-hop" 

## Nombres de rutas estáticas según elección de "next-hop" o "exit-interface"

- **Directly Attached** static route: Sólo se especificó la interfaz de salida
	- No se puede usar en interfaces [[Ethernet]], en IPv6
- **Recursive** static route: Sólo se especificó el *next-hop*
- **Fully Specified** static route: Tanto la interfaz de salida como el *next-hop* se especificaron

## Configuración *default routes*

1. Usamos el comando "ipv6 route ::/0 </next_hop>"
	1. El "::" es equivalente a la red 0.0.0.0
	2. El "/0" se refiere a la netmask 0.0.0.0
	3. El conjunto de estos 2 "::/0" se refiere a todas las redes posibles



## Configuración *floating static*

Recordar que convertimos una ruta estática a *floating* cuando aumentamos su AD sobre el de AD del protocolo de *routeo* dinámico que se está usando. Tambien visto en [[Dynamic Routing]].



## Configuración de un *next-hop* con dirección "link-local"

Si queremos usar una dirección IPv6 "link-local" como *next-hop*, debemos también ingresar la interfaz de salida, antes de de ella.
Debemos especificar la interfaz de salida porque solo usando la dirección "link-local", el *router* no es capaz de averguar por si mismo por que interfaz debería enviar el mensaje.
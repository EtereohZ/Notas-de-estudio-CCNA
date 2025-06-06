Routing is the process that [[Router|routers]] use to determine the path that [[IPv4 Addressing|IP]] packets should take over a network to reach their destination. 
Routers store routes to all of their known destination in a ***routing table***.
When a router receives a packet, they look in the routing table to find the best route to forward the packet to the correct destination.

Existen 2 métodos principales de *routeo:*
- **Dynamic Routing:** *Routers* usan protocolos dinámicos de *routeo* (ej. [[OSPF]]) para compartir información de routeo entre ellos automáticamente y construir sus **tablas de routeo**.
- **Static Routing:** Alguien configura manualmente las rutas en el *router.*


**Nota**: [[IPv6 Static Routing]] para ver *routeo* IPv6

#### ¿Qué es una ruta?

Una ruta le da instrucciones al router sobre su destino final y el siguiente router inmediato (**next-hop**) ,al cual debe enviarle el paquete, en su camino al destino final.
O, si el destino es la propia dirección IP del router, recibir el paquete y no mandarlo.

#### Connected and Local routes

Las *connected routes* son las rutas a la red en la que la interfaz del router está conectada (con la netmask que se configuró en la interfaces).
Si tenemos que enviarle un paquete a cualquier *host* en la red a la que estoy conectado directamente, se que debo mandarlo directamente por la interfaz conectada a esa red.

Las *local routes* son las **rutas** para la dirección [[IPv4 Addressing|IPv4]] configurada en la interfaz (tienen una [[Subnet Mask]] de /32)
Usamos las netmask /32 para especificar la IP exacta de la interfaz. /32 nos dice que todos los 32 *bits* están fijos, no pueden cambiar
**ES LA RUTA PARA LA DIRECCIÓN IP**

a route **matches** a packet's destination if the packet's destination IP address is part of the network specified in the route.
Esto nos dice que si llega un paquete con una porción IP de la red distinta, no puede usar esa red porque las direcciones son distintas. El router puede enviar el paquete usando una ruta distinta o botar el paquete si no hay ruta que calce.

El router siempre elegirá el camino **más especifico** a la hora de *forwardear* el paquete.
Si no hay *matching routes* en la *matching table* del router, botará el paquete

#### Route Selection
 ¿Qué pasa si el router recibe un paquete que calza con una ruta local y una ruta conectada?
 El router elegirá la ruta que haga *match* mas específicamente. **Ver foto**.
 Con mas específicamente se refiere a las rutas con *match* que tengan el prefijo más largo, en este caso, /32
 Para eso sirven las rutas locales, el router podrá determinar si el paquete es para el, y que no lo *forwardee*.
 ![[routing_table_info.png]]
 "192.168.1.0/24 is variably subnetted, 2 subnets, 2 masks"
 There are two routes to subnets that fit within the 192.168.1.0/24 class C network, with two different netmasks (/24 and /32).
 Explicado en [[CCNA/Subnetting/Subnetting|subnets]].

#### Default Gateway

*Default Gateway* es un termino antiguo para referirse a *router*
*Default Gateway configuration*, también llamado *default route*
Es una ruta a 0.0.0.0/0 e incluye todas las direcciones desde 0.0.0.0. hasta 255.255.255.255. Ningunas de los *bits* en la dirección son fijos.
El *default route* es la ruta **menos especifica** posible, incluye **todas** las [[IPv4 Addressing|direcciones IP]]. 
 
Para enviar paquetes fuera de nuestra red local, PC1 enviará paquetes a nuestro router, no es necesario que conozcan rutas específicas.

#### Default Route

Es una ruta a 0.0.0.0/0.
Es la ruta menos especifica posible, incluye cada posible dirección IP. Nos dice "si no hay *specific matches* para este paquete, envíalo por esta ruta en vez de botarlo."
Es usualmente usado para dirigir trafico en internet.

*Gateway of last resort* se refiere a *Gateway*,  y significa que no se ha configurado una *default route*. 

Se configura una *default route* con el comando **ip route 0.0.0.0 0.0.0.0 </next_hop>**


Al querer enviar el paquete de PC1 a PC2 , el *destination IP* será el de PC2, pero antes de hacerlo hay que mandarle el paquete al *router*. PC1 encapsulará el paquete en un frame y la dirección [[MAC Address|MAC]] será la de la interfaz del *router*. PC1 aprenderá la MAC del *router* enviando un [[ARP|*ARP request*]] a la dirección IP del *router*.
Cuando el *router* reciba el *frame*, lo desencapsulará , removiendo el *[[Ethernet Frame|L2 header y trailer]]*, luego buscará en su *routing table* para la *most-specific matching route*.
Si no hay rutas que calcen, necesitaremos rutas hacia la red destino.
**Nota**: Recordar que las rutas son instrucciones, nos dicen hacia donde ir y que camino tomar.
### Static Routing

Lo usamos cuando nosotros queremos determinar las rutas que usará el *router* para enviar paquetes a una dirección que no esté directamente adyacente.

Cada *router* en el camino necesita **2** rutas, la ruta hacia la red de origen y hacia la red de destino.
Esto asegura que ambos equipos puedan comunicarse efectivamente.
Los *routers* no necesitan las rutas a todas las redes en el camino hacia el destino, cada router se ocupa de su camino.
Los *routers* que no tienen ***connected routes***, necesitarán ser configurados.

En las rutas estáticas en las que solo especificamos la interfaz de salida, dependen de una característica llamada **Proxy ARP**
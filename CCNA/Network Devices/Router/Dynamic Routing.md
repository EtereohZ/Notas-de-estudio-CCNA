*[[Router|Routers]]* pueden usar protocolos de *routeo* dinámico para anunciar información acerca de sus rutas conectadas y las rutas que aprendieron de otros equipos.


## Elección de ruta
Si existen multiples rutas a un destino, el *router* determina que ruta es superior y la agrega a su *routing table*. Usa la métrica de la ruta para decidir cual es superior (menor métrica == superior)
Similar a cuando un *[[Switch|switch]]* elige el mejor camino al *root bridge*, los [[Networking Protocols|protocolos]] de *routeo* dinámico usan un modelo similar para elegir el mejor camino hacia su destino.

## Tipos de protocolos 

Podemos dividirlos en 2 categorías:

### IGP
Interior Gateway Protocol
Se usan para compartir rutas dentro de un "autonomous system" (AS), el cual es solo una organización.
Podemos subdividirlos según el algoritmo que usan.

#### Algoritmo Link State

En este algoritmo cada *router* crea un "connectivity map" de la red, este mapa será el mismo en cada *router*.
- Esto se logra debido a que cada *[[router]]* anuncia información acerca de sus interfaces (sus redes conectadas) a sus vecinos. Estos anuncios se van pasando a otros *routers*, hasta que todos los *routers* en la red desarrollan el mismo mapa de la red.
- Cada *router* usará este mapa para calcular la mejor ruta a su destino, independientemente.
- Este algoritmo usa mas recursos, pero es más rápido frente a cambios.
##### [[OSPF]]
Open Shortest Path First

##### IS-IS
Intermediate System to Intermediate System

#### Algoritmo Distance Vector

Fueron inventados primero que los "Link State".
Funcionan enviando lo siguiente a sus vecinos directos:
- Las redes a las que saben como llegar
- La métrica para llegar a sus destinos conocidos
Este método de compartir información se le llama "routing by rumour".
- Esto es porque el *router* no sabe de la red mas allá de sus vecinos, solo conoce la información que sus vecinos le dicen

##### [[RIP]]
Routing Information Protocol

##### [[EIGRP]]
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
Muy parecido al "root cost" de [[STP]].
- Cada protocolo usa una métrica distinta para determinar que ruta es mejor.
- Si un *router* aprende 2 o más rutas gracias al mismo protocolo de *routeo* al mismo destino, y con las mismas métricas. ambas rutas serán agregadas a la *routing table*. El tráfico será *load-balanced*
	- Esto se llama **ECMP** (Equal Cost Multi-Path)

Imagen resumen de las métricas que usa cada protocolo
![[Captura de pantalla 2024-09-10 153329.png]]

## Administrative Distance

Se usa para determinar que protocolo de *routeo* es preferido cuando se está usando **mas de 1**.
Un AD menor es preferido, e indica que el protocolo es mas "confiable".

Podemos cambiar la AD, según lo que queramos.
Si configuramos la AD de una ruta estática para que sea mas alta que una aprendida por un protocolo de *routeo* dinámico, pasará a llamarse "floating static route".
La ruta estática pasará a ser inactiva, a menos que la ruta aprendida dinámicamente sea removida.

Mapa de cada AD según protocolo![[Captura de pantalla 2024-09-10 155959.png]]
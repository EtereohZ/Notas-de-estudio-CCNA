Enhanced Interior Gateway Routing Protocol
Pertenece a los protocolos de [[dynamic routing]]

- No tiene limite de *hop-count* como [[RIP]]
- Envía mensajes usando la dirección *multicast* de **224.0.0.10**
- Es el único [[Dynamic Routing|IGP]] que realiza "unqual-cost load-balancing"
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
- **Feasible Distance**: El valor de la métrica un [[Router|router]] especifico al destino de la ruta.
- **Reported/Advertised Distance**: El valor de la métrica del vecino
	- El vecino el va a contar al *router* cual es su métrica para llegar al destino
	- Ver foto justo abajo
		- Los números antes del "/" son las "feasible distance"
		- Los números después del "/" son la "reported distance"
![[Captura de pantalla 2024-09-11 175953.png]]
- **Successor**: La ruta con la métrica menor (la mejor ruta)
- **Feasible Successor**: Una ruta alterna al destino que cumple con la ***Feasibility Condition***
- **Feasibility Condition**: Se considerará como "feasible successor" si la "reported distance" es **MENOR**  que la "feasible distance" del "successor"
	- Dicho de otra forma, si la "reported distance" (de la ruta alternativa) es **menor** que la "feasible distance" del "successor" (la mejor ruta actual), se puede considerar como el **"feasible successor"**. Por lo que cumpliría la "feasibility condition"
- En rojo la "feasible distance" y en morado la "reported distance" 
![[Captura de pantalla 2024-09-11 181158.png]]
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
![[Captura de pantalla 2024-09-11 175756.png]]


## Configuración

1. Usaremos la imagen del final como ejemplo
2. Empezamos con el comando "router eigrp </AS_number>"
	1. El número AS debe calzar entre [[Router|routers]], no formarán *adjacency* y no compartirán información de rutas si no es el mismo
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
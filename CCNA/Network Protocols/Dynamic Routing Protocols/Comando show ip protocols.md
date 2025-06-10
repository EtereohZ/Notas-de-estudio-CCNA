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
	2. K1, K2, K3, K4, K5 en [[EIGRP]]
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
	1. Nos muestra la [[Dynamic Routing|AD]] del protocolo elegido
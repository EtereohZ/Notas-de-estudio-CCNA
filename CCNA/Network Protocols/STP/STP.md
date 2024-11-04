Spanning Tree Protocol is a [[OSI|L2]] protocol, it enables redundant L2 networks and prevents L2 loops by placing redundant ports in a blocking state, essentially disabling the interface.

Estas interfaces actúan como respaldos que pueden entrar a un *forwarding state* si una interface activa falla. Las interfaces bloqueadas solo pueden recibir mensajes STP, llamadas BPDU's (Bridge Protocol Data Unit) (interpretemos el "Bridge" como "switch").

Como elige que puertos están *forwardeando* y que puertos están bloqueando, STP crea un único camino desde/hacia la red. Esto previene bucles L2.

El estándar actual es 802.1s, todos los *switches* Cisco lo ocupan por defecto, además de [[RSTP]]. El STP estándar solo permite 1 instancia para todas las [[VLAN|VLAN's]], al contrario de [[PVST+]] y su versión actualizada, [[RSTP|Rapid PVST+]] y la versión actualizada de STP, Multiple Spanning Tree Protocol (802.1S). El estándar del STP clásico es 802.1D

#### Broadcast Storms
Es cuando se forma un bucle infinito de *broadcast frames* y terminan congestionando la red.
Ese no es el único problema, cada vez que el mismo *frame* llega al [[Switch|switch]] desde distintas interfaces, el *switch* estará continuamente actualizando su [[MAC Address|MAC address table]]. Eso se llama **MAC Address Flapping**.

#### ¿Cómo funciona STP?
Existe un proceso que STP usa para determinar que puertos deberían estar bloqueados o *forwardeando*:
*Switches* mandan/reciben  "Hello BPDU's" desde todas sus interfaces, esto lo usan para anunciarse y saber de otros *switches* , y ocurre cada 2 segundos. Si un *switch* recibe un "Hello BPDU" en una interface, sabrá que esa interface está conectada a otro *switch* (solo *switch* usa SPT).

Los *switches* usan un **campo** en el STP BDPU, el campo "Bridge ID", para elegir a un "root bridge" para la red. El *switch* con el "Bridge ID" mas bajo se convierte en el "root bridge". El BDPU también lleva el costo acumulado del "root cost", que va incrementando a medida que ese BPDU es *forwardeado* a través de la red. Gracias a ese costo es que los *switches* sabrán que puerto elegir como "root port".
El "root cost" solo incrementa cuando llega a un nuevo puerto, no cuando sale.

Cuando un *switch* sea elegido como "root bridge", todos sus puertos se convertirán en puertos designados, y el resto de los [[Switch|switches]] en la red **deben tener un camino para llegar a él**. Cada *switch* deberá elegir solo **un** "root port", que apuntará al "root bridge". Cada interfaz conectada con ese "root bridge" será un puerto designado

Luego, **cada dominio de colisión** entre *switches* restantes deberán elegir 1 puerto designado entre ellos, donde **una** interfaz será seleccionada como puerto designado y la otra estará bloqueada.

Recordemos usar el [[CLI Commands|comando]] "show spanning-tree" para revisar 

Si el *switch* no recibe BPDU's, sabe que es seguro entrar en modo *forwarding*.
**Las prioridades y quien será "root bridge" o no varían según cada VLAN en PVST**

##### Estados de un puerto STP
Un puerto puede encontrarse en un estado "estable" o "transicional", dependiendo de si ya está configurado o está en proceso/ocurrieron cambios en la red, respectivamente.
1. **Blocking**
	1. Estable
	2. Puertos no designados se encuentran en este estado
	3. No envían/reciben tráfico regular
	4. Si reciben BDPU's
		1. Pero no son *forwardeadas*
	5. No aprenden direcciones [[MAC Address|MAC]]
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

##### Spanning Tree Timers

![[Captura de pantalla 2024-09-05 171023.png]]

Si no se reciben BPDU's y en tiempo del "Max Age" llega a 0, el [[Switch|switch]] reevaluará sus decisiones STP, incluyendo el "root bridge", "root port" y sus puertos designados/no designados.
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
			1. Y si no, se elige el "port ID" de de la interfaz del *switch* vecino/conectado
				1. Esto se ven en el [[CLI Commands|comando]] "show spanning-tree" en "Prio.Nbr" 
				2. Se elige el que tenga **numero menor**
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

##### Separación del bridge ID
- Dividido en
	- Bridge Priority
		- 4 bits de largo
	- Extended System ID
		- Cisco switches usan una versión de STP llamada PVST (Per-VLAN Spanning Tree). PVST usa una instancia separada en cada VLAN, por lo que en cada VLAN hay diferentes interfaces *forwardeando*/bloqueando.
		- Igual a VLAN ID
		- 12 Bits de largo
![[Captura de pantalla 2024-09-04 174633.png]]
Esencialmente, Es la dirección [[MAC Address|MAC]] la que determina quien será el "root bridge", excepto en PVST.



"root costs" a continuación
![[Captura de pantalla 2024-09-04 182030.png]]


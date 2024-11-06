
## Two-Tier Campus LAN Design

Consiste en 2 niveles de jerarquía:
1. **Access layer**
	1. Es el nivel al que conectan los *end hosts*
	2. Aquí se realizan las marcas [[QoS]] y servicios de seguridad
2. **Distribution layer**
	1. Agrega las conexiones entre los *switches* de la "access layer"
	2. Conecta a otros servicios, como [[WAN]], internet, etc.
	3. Actúa como la frontera entré L2 y L3

Para ayudar a la escalabilidad de grandes redes [[LAN]], se recomienda agregar un nivel superior si existen mas de 3 "distribution layers" en un mismo lugar. Los que nos lleva al siguiente diseño.


## Three-tier Campus LAN Design

En este diseño de red, ahora tenemos 3 distintos niveles de jerarquía:
1. **Access layer**
2. **Distribution layer**
3. **Core layer**
	1. Conecta "distribution layers" en grandes redes LAN
	2. Está enfocada en velocidad
	3. Operaciones de alta demanda de CPU deben evitarse
	4. Solo existen conexiones L3

![[Three-tier layer design.png]]



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
Al no tener necesidades complejas, todas las funciones son cumplidas por solo 1 equipo. Este equipo hace de *[[Router|router]]*, *[[Firewall|firewall]]*, *switch*, [[WAP]], [[modem]].
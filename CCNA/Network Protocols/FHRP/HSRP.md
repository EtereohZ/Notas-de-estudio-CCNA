Hot Standby Router Protocol es propietario de Cisco.
Nos provee redundancia en cuanto al *default gateway* para *hosts* en una *subnet*

Todos los [[Router|routers]], por defecto, poseen la misma prioridad. Se deben configurar para cambiarla.

- En este protocolo se elige un *router* activo y uno *standby*
- Posee 2 versiones, versión 2 soporta [[IPv6]]
- V1 envía multicast a 224.0.0.2 y V2 a 224.0.0.102
- MAC virtual
	- V1 de 0000.0c07.acXX (XX=HSRTP group number)
	- V2 de 0000.0c9f.fXXX (XXX=HSRP group number)

## Configuración

1. HSRP se configura directamente en la interfaz
2. Podemos cambiar la version con "standby </version>"
	1. *Routers* deben estar con la misma versión
3. Ingresamos el comando "standby </numero_de_grupo> ip </ip_virtual>"
	1. El número de grupo debe ser igual entre los *routers*
	2. Usado para elegir IP virtual
4. Seguimos con "standby </numero_grupo> priority </valor>"
	1. Lo usamos para determinar que *router* será activo
5. Ahora podemos usar "standby preempt"
	1. Esto fuerza a que el *router* tome el rol de activo
6. "show standby" nos muestra la configuración
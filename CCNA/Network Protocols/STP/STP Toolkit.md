
### Portfast

It can be enabled on interfaces that are connected to end-hosts.

Cuando un *[[switch]]* se conecta a otro equipo, debe pasar por las etapas [[STP]] de "Listening" y "Learning" antes de poder formar un puerto designado. Pero en el caso de conectarlo a un end-host, ya que no pueden ocurrir bucles infinitos.
Portfast le permite al *switch* entrar inmediatamente al estado "Forwarding", saltándose los pasos de "Listening" y "Learning".
**SOLO USAR EN PUERTOS CONECTADOS A END-HOSTS.**

Pasos
1. Elegimos la interface en la que queremos usar "Portfast"
2. Ingresamos el [[CLI Commands|comando]] "spanning-tree portfast"
- Podemos activar Portfast de forma global en todos los [[VLAN|access ports]] con el comando "spanning-tree portfast default"
	- Podemos desactivar "portfast" conm 
#### Hay 2 tipos de Portfast

- **edge**
- **network**
	- Se usa en "Bridge Assurance"
	- No es parte de CCNA

### BPDU Guard

Si una interfaz con "BPDU Guard"  activado recibe un BPDU de otro *switch*, la interfaz se apagará y evitará que se forme un bucle
1. Elegimos interfaz
2. Lo activamos con el comando "spanning-tree bpduguard enable"
- Lo podemos activar de forma global en todas las interfaces con "portfast" usando el comando "spanning-tree portfast bpduguard enable"

### Root Guard

Si activamos "root guard" en una interfaz, aunque reciba un BPDU superior ("bridge ID" menor) en esa interfaz, el *switch* no aceptará el nuevo *switch* como "root bridge". La interfaz se deshabilitará.

Ayuda a prevenir cambios en la topología al ser un ingresado un nuevo *switch* o en otros casos.

### Loop Guard

Si habilitamos "loop guard" en una interfaz, aún cuando la interfaz deje de recibir BPDU's, no entrará en estado "forwarding". La interfaz se deshabilitará.

**Unidirectional link**

### BPDU Filter

Funciona de manera similar a BPDU Guard, solo que el filtro evita que el puerto envía o procese BPDU's recibidos.
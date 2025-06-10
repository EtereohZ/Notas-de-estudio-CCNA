Aquí veremos algunas herramientas de [[STP]] que facilitarán nuestras vidas.
## Portfast

It can be enabled on interfaces that are connected to end-hosts.

Cuando un *[[switch]]* se conecta a otro equipo, debe pasar por las etapas STP de "Listening" y "Learning" antes de poder formar un puerto designado. Pero en el caso de conectarlo a un end-host, ya que no pueden ocurrir bucles infinitos.
Portfast le permite al *switch* entrar inmediatamente al estado "Forwarding", saltándose los pasos de "Listening" y "Learning".
**SOLO USAR EN PUERTOS CONECTADOS A END-HOSTS.**

### Hay 2 tipos de Portfast

- **edge**
	- El que usaremos, configurado por defecto
- **network**
	- Se usa en "Bridge Assurance"
	- No es parte de CCNA

### Configuración

**Primero elegimos la interfaz que queramos configurar y activamos "Portfast"**
1. Con el comando **"spanning-tree portfast"**

**También lo podemos activar de forma global**
1.  En los [[VLAN|access ports]], con **"spanning-tree portfast default"**
2. Podemos desactivar Portfast en interfaces específicas con **"spanning-tree portfast disable"**

**Si queremos usar "Portfast" en [[VLAN|ROAS]]**

- Usamos el comando **"spanning-tree portfast trunk"**
	- Activa "portfast" en un *trunk*


## BPDU Guard

Si una interfaz con "BPDU Guard"  activado recibe un BPDU de otro *switch*, la interfaz se apagará y evitará que se forme un bucle

### Configuración

**Elegimos la interfaz deseada y configuramos**
1. Usando el comando **"spanning-tree bpduguard enable"**

**También se puede activar, de forma global, en todas las interfaces con "Portfast" activado
1. Con **"spanning-tree portfast bpduguard enable"**

## Root Guard

Si activamos "root guard" en una interfaz, aunque reciba un BPDU superior ("bridge ID" menor) en esa interfaz, el *switch* no aceptará el nuevo *switch* como "root bridge". La interfaz se deshabilitará.

Ayuda a prevenir cambios en la topología de red, al ser un ingresado un nuevo *switch*, con menor **"Bridge ID"**.

Un puerto desactivado por **Root Guard** volverá a activarse automáticamente cuando los BPDU's con mayor preferencia dejen de llegar.

### Configuración

**Entramos a la interfaz deseada y configuramos el Root Guard**
1. Con el comando **"spanning-tree guard root"**


## Loop Guard

Si habilitamos "loop guard" en una interfaz, aún cuando la interfaz deje de recibir BPDU's, no entrará en estado "forwarding". La interfaz se deshabilitará.

**Unidirectional link**

## BPDU Filter

Funciona de manera similar a BPDU Guard, solo que el filtro evita que el puerto envía o procese BPDU's recibidos.
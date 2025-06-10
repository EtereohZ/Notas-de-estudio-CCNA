- Elige un "root bridge" con las mismas reglas que [[STP]]
- Elige "root ports" con las mismas reglas que STP
- Elige puertos designados con las mismas reglas que STP
- Se cambia el costo STP según la velocidad: ![[RSTP_cost.png]]
- Se cambian los estados de los puertos: 
	- Un puerto deshabilitado ahora se llama se encuentra en el estado "discarding"
	- Un puerto que se encuentra bloqueando tráfico también se encuentra "discarding"
	- Sigue existiendo "forwarding" y "learning"
![[RSTP_port_state.png]]
## Roles de puertos

- **Root port**
- **Designated port**
- El rol de puerto no designado se divide
	- **Alternate role**
		- Es un puerto en estado "discarding" que solo va a recibir BPDU's
		- Funciona como respaldo al "root port"
			- Si el *root* falla, este puerto puede cambiar a *root* sin pasar por estados de transición
		- Es lo que era un puerto que se encontraba bloqueando
	- **Backup role**
		- Es un puerto "discarding" que recibe un BPDU por otra interfaz en el mismo [[Switch|switch]]
			- Solo ocurre cuando 2 interfaces están conectadas al mismo dominio de colisión (via *hub*)
- En RSTP **todos los *switches*** crean y envían sus propios BPDU's a través de sus puertos designados.
- En RSPT, el *switch* considera a su vecino perdido si es que falla en recibir 3  "Hello BPDU" (6 segundos)
	- Luego, olvidará todas las direcciones [[MAC Address|MAC]] aprendidas en esa interfaz

## RSPT Link Types

- **Edge port**
	- Es un puerto conectado a un *end host*, se mueve directamente a *forwarding* sin necesitar de negociación
	- Es lo mismo que "portfast"
- **point-to-point**
	- Nos indica una conexión entre 2 *switches*
	- Funcionan en **full-duplex**
	- Se detecta automáticamente, no es necesario configurarlo
- **shared**
	- Se una en conexiones a un *hub*. Deben operar en "half-duplex"
	- También se detecta automáticamente
Podemos elegir el "link type" con el comando **"spanning-tree link-type </tipo>"**

## Toolkit

Se encuentran integrados en RSTP:
- Portfast
- UplinkFast
	- 
- Backbone Fast
	- Ayuda al *switch* a ahorrarse el tiempo de espera **"MAX AGE"** y empezar a transmitir por ese puerto.

## Configuración

**Podemos pasar un puerto a link type "edge con el comando de portfast**
1. Con **"spanning-tree portfast"**

**Si queremos configurar manualmente una interfaz**
1. **"spanning-tree link-type </tipo>"**
	1. Para **"point-to-point"** o **"shared"**
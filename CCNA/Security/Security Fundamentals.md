
## CIA

La triada CIA es una de las bases de seguridad informatica

- **Confidentiality**
	- Solo usuarios autorizados deben poder acceder a la información
- **Integrity**
	- La información debe mantenerse sin manipular por usuarios no autorizados, y mantenerse correcta
- **Availability**
	- Las redes y sistemas deben mantenerse operativos y accesibles a usuarios autorizados.


## Conceptos de seguridad

### Vulnerabilidad

Una vulnerabilidad es cualquier debilidad potencias que puede comprometer la **CIA** de un sistema, pero en si, una vulnerabilidad en si no es un problema,


### Exploit

Un ***exploit*** es algo que puede ser usado para explotar la vulnerabilidad, pero tampoco es un problema por si solo.


### Threat

Una **amenaza** es el potencial de una **vulnerabilidad**  para ser **explotada**.
Un *hacker* explotando una vulnerabilidad en el sistema es una **amenaza**.


### Mitigation

Una **técnica de mitigación** es algo que puede proteger frente a amenazas. Deben ser implementadas en cualquier parte que una **vulnerabilidad** puede ser explotada.


## Ataques comunes

- **DoS**
	- Los ataques "Denial of Service" amenazan la disponibilidad de un sistema
	- Una forma de ataque común es un *flood* de ataques TCP SYN
		- El atacante nunca responde con ACK y provoca que las conexiones incompletas llenen la tabla de conexiones TCP
	- Un **DDoS** usa varios equipos distintos para atacar
- **Spoofing Attacks**
	- To spoof an address is to use a fake address
	- un ejemplo es "DHCP exhaustion"
		- Un atacante usa un [[MAC Address|MAC]] *spoofeado* para *floodear* mensajes "DHCP Discover"
- **Reflection/Amplification attacks**
	- En un ataque "reflexion", el atacante envía tráfico a un reflector, que *spoofea* la dirección de origen de los paquetes a la dirección IP del objetivo
	- Un ataque "reflexion" se vuelve de **amplificación** cuando el volumen de tráfico enviado por el atacante es pequeño, pero gatilla que un gran volumen de tráfico se envíe  del **reflector** al **objetivo**
- **Man in the middle**
	- El atacante se ubica entre el origen y el destino para escuchar las conversaciones o manipularlas
	- Un ejemplo es **"ARP Spoofing/Poisoning"**
		- Un *host* envía un ARP Request, que también recibirá el mensaje y entregará su dirección MAC, haciéndose pasar por el objetivo original del ARP Request
- **Reconnaissance Attacks**
	- Se usan para ganar información acerca del objetivo
	- **nslookup**
- **Malware**
	- Virus
	- **Worms**
		- Se propagan por si mismos
	- **Trojan Horse**
		- *Software* malicioso que aparenta ser legitimo. Se propagan a través de interacción por usuarios., como correos o descargando archivos
- **Social Engineering**
	- Su objetivo son las personas
	- **Phishing**
		- Correos fraudulentos que aparentan ser de negocios o personas legitimas, y contienen *links* fraudulentos que contienen *malware* o a sitios maliciosos
		- **Spear Phishing** 
			- Es empleado frente a personas especificas
		- **Whaling** 
			- Es empleado frente a personas muy importantes dentro de la organización
		- **Vishing**
			- Empleado en llamadas telefónicas
		- **Smishing**
			- Empleado en mensajes SMS
	- **Watering Hole**
		- Compromete sitios web que el objetivo visita con frecuencia
	- **Tailgating**
		- Entrar a areas restringidas al seguir una persona autorizada mientras ellos entran
- **Password Attacks**
	- **Dictionary Attack**
		- Un programa usa un "diccionario" de contraseñas comunes para encontrar la del objetivo
	- **Brute Force Attack**
		- Un programa intenta cada combinación posible de carácteres para encontrar la contraseña


## MFA

Involucra necesitar mas que solo una contraseña y usuario para probar la identidad:
- Algo que tu sabes
	- Contraseña, PIN, etc
- Algo que tienes
	- ID, celular
- Algo que eres
	- Huella digital y otros datos biométricos


## Certificados Digitales

Se usan para autentificar la identidad de sitios web o programas



## Authentication, Authorization and Accounting

- **Authentication** es el proceso de verificar la identidad de alguien
- **Authorization** es el proceso de otorgar permisos apropiados según el tipo de usuario
- **Accounting** es el proceso de guardar registros de la actividad de los usuarios en el sistema

Empresas usan servidores AAA para proveer estos servicios. Usan los siguientes protocolos:
- **RADIUS**
	- Estándar abierto, usa puertos UDP 1812 y 11813
- **TACACS+**
	- Protocolo propietario de Cisco, usa puerto TCP 49



## Security Program Elements

- **Programas "User Awareness** 
	- Son diseñados para enseñar a empleados sobre riesgos y amenazas potenciales.
- **User Training**
	- Pogramas mas formales que los de arriba
- **Physical Access Control**
	- Protege Equipos y data mediante controles físicos a zonas protegidas
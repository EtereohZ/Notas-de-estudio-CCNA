

## Authentication Methods

- **Open Authentication**
	- Se autoriza a todos
	- Puede ser que luego de estar autenticado, el usuario necesite autenticarse por otros medios antes de ser otorgado acceso a internet (wifi públicos)
- **WEP**
	- Autentica y encripta tráfico
	- Encripta usando el algoritmo RC4
	- WEP usa llaves simétricas para encriptar
	- **No es seguro** y no debería usarse
- **EAP**
	- Es un *framework* de autenticación, define un grupo estándar de funciones de autenticación
	- Está integrado con 802.1X, el cual se usa para limitar acceso a la red a clientes, hasta que se autentiquen
- **LEAP**
	- Clientes deben proveer usuario y contraseña
	- Tanto el cliente como el servidor se envían "challenge phrases"
	- Se usan llaves WEP dinámicas
	- **No debería usarse**
- **EAP-FAST**
	- Consiste de 3 fases:
		1. Se genera un "PAC" y se pasa del servidor al cliente
		2. Se establece un túnel TLS entre el cliente y el servidor de autenticación
		3. Dentro del túnel se realiza el proceso de autenticación/autorización
-  **PEAP**
	- También se establece un túnel TLS pero , en vez de un PAC, el servidor usa un certificado digital
	- El cliente se autentica dentro del túnel TLS
- **EAP-TLS**
	- Requiere tanto un certificado en el servidor como en cada cliente
	- Es el método de conexión inalámbrica más seguro


## Encryption and Integrity Methods


- TKIP
	- Se creo como una solución temporal al problema que presentaba WEP, hasta que se creara un nuevo estándar
	- Usa un *hash* para validar la integridad de los mensajes
	- Una un "key mixing algorithm" para crear llaves WEP únicas
	- Se usa en **WPA**
- **CCMP**
	- Desarrollado después de TKIP
	- Es usado en **WPA2**
	- No puede ser usado en *hardware* antiguo
	- Consiste en 2 algoritmos para dar encriptación y *hash*:
		- **AES**
			- Es el protocolo de encriptación más seguro, actualmente.
		- **CBC-MAC**
			- Es usado como MIC (*hash*) para validar la integridad de los mensajes
- **GCMP**
	- Más seguro y eficiente que CCMP
	- Esa usado en **WPA3**
	- Consiste de 2 algoritmos:
	- **AES Counter Mode** para encriptar
	- **GMAC** usado para generar el *hash*



## WAP

Este protocolo incluye WPA, WPA2 y WPA3. Cada uno de ellos soporta 2 modos de autenticación: 

1. **Personal Mode**: Mediante uso de una "pre-shared key" (PSK), donde la autenticación se hace mediante una contraseña. La PSK no en enviada a través del aire, sino que se se usa un *handshake* de 4 pasos para lograr la autenticación. La PSK se usa para generar las llaves de encriptación
2. **Enterprise Mode**: 802.1X es usado con un servidor de autenticación (como RADIUS). Soporta todos los métodos EAP

**WPA** se desarrolló luego que se demostrara que **WEP** era vulnerable, incluye los siguientes protocolos:
- **TKIP**
- **802.1X o PSK**

**WPA2** se lanzó en 2004 e incluye los siguientes protocolos:
- **CCMP**
- **802.1X o PSK**

**WPA3** se lanzó en 2018 e incluye los siguientes protocolos:
- **GCMP**
- **802.1X o PSK**
También incluye otras funcionalidades como **PMF**, **SAE** y **Forward Secrecy**.
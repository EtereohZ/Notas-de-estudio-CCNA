Es un acercamiento a las redes que centraliza la "control plane" en una aplicación llamada "controller".
El controlador puede interactuar con los equipos de red usando [[REST API's|API's]].

- La **SBI** se usa para comunicar el controlador con los equipos de red que controla
- La **NBI** es lo que nos permite interactuar con el controlador


## SDN Architecture

- **Application Layer**
	- Contiene las aplicaciones/*scripts* que manejan el controlador
- **Control Layer**
	- Contiene el controlador y recibe las instrucciones del nivel de la aplicación
- **Infrastructure Layer**
	- Contiene los equipos de red que son responsables de *forwardear* los mensajes a través de la red

![[SDN_architecture.png]]


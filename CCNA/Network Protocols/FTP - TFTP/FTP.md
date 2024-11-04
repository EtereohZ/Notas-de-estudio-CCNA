FTP along with TFPT are industry standard protocols used to transfer files over a network.

Fue estandarizado por primera vez en 1971.
FTP trabaja con [[TCP]] en puerto 20 y 21.

Ambos trabajan con un modelo cliente-servidor:
- Clientes los pueden usar para copiar archivos desde/hacia un servidor

FTP puede usar autenticación, pero no encriptación. Es por esto que existe FTPS (usa SSL/TLS), que si puede encriptar la información.
También existe SFTP, el cual es un protocolo nuevo y, trabaja con [[SSH]].

FTP es más complejo que [[TFPT]], además de permitir el traspaso de información también le deja al cliente navegar/eliminar/agregar directorios, listas, etc.



## Conexiones FTP

FTP usa 2 tipos de conexiones:
- **FTP Control** (TCP 21): Es usada para enviar comandos y respuestas FTP
- **FTP Data** (TCP 20): Es el puerto por el cual se transfiere la info
	- Hay 2 métodos para establecer conexiones **"FTP Data"**
		- **Active Mode**: El **servidor** inicia la conexión FTP
		- **Passive Mode**: El **cliente** inicia la conexión FTP. Esto es generalmente necesario cuando el cliente está detrás de un *[[Firewall|firewall]]*, que puede bloquear la conexión desde el servidor



## Upgrading Cisco IOS (TFPT)

Recordemos que podemos ver la version con **"show version"**


1. Usamos el comando **"copy </fuente> </destino>"**
	1. El la fuente ingresamos FTP o TFPT como servidores de origen
2. Ahora tenemos que ingresar la dirección del servidor FTP/TFPT
3. Ahora debemos ingresar el nombre del archivo que se encuentra en el servidor
4. Ahora el archivo se descargará a nuestro equipo
5. Podemos usar **"show flash"** para ver el archivo descargado
6. Para decirle al [[Router|router]] que use el archivo, debemos usar el comando **"boot system </direccion_archivo>"**
7. Guardamos la configuración con **"write memory"**
8. Reiniciamos el *router* con **"reload"**
9. Para borrar el archivo de la version antigua usamos el comando **"delete </direccion_archivo>"**



## Upgrading Cisco IOS (FPT)

1. Configuramos usuario con **"ip fpt username </usuario>"**
2. Configuramos contraseña con **"ip fpt  password </contraseña>"**
3. Mismos pasos que con TFPT
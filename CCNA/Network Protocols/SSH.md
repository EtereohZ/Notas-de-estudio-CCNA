
Fue desarrollado en 1995 para reemplazar Telnet.
SSH usa TCP en puerto **22**.
El equipo que está siendo accesado cuenta como el servidor, y quien está usando SSH para conectarse, es el cliente.
 <p>En computación, una "shell es un programa de computador que expone los servicios de un sistema operativo a un usuario u otro programa</p>
VTY Lines (Virtual TeleType) indica las posibles conexiones simultaneas que son permitidas a un equipo en especifico. Generalmente se admiten hasta 16 líneas.



## Configuración previa


### Console Port Security

Por defecto, no se necesita ninguna contraseña para acceder a la [[Cisco CLI|CLI]] de Cisco mediante el "Console Port". Podemos configurar una contraseña en la misma consola, para que cualquier persona que quiera acceder a la CLI deba primero conocer la contraseña.

- Ingresamos el comando **"line console 0"**
	- Solo queremos que haya 1 persona configurando al mismo tiempo, por eso ponemos 0
	- nos permite entrar al modo de configuración de la *line*
- Con **"password </clave>"** configuramos la clave
- Después ponemos **"login"** para decirle al equipo que un usuario necesitará *logearse* para acceder a la CLI mediante el "console port"


### Login local

También podemos configurar la consola para requerir que los usuarios se *logeen* usando algunos de los *usernames* anteriormente configurados.

- Con **"username </username> secret </clave>"** configuramos usuario y su contraseña
- Seguimos con **"line console 0"**
	- Entramos al modo de configuración de la "line"
- Ahora usamos **"login local"** para decirle al equipo que un usuario necesitará *logearse* con un *username* ya existente
- Por último, podemos poner un *timeout* para que se desconecté automáticamente después de un tiempo de inactividad, con **"exec-timeout </algo> </algo>"**



### L2 Switch - Management IP

Como ya sabemos, los *[[Switch|switches]]* no realizan *routeo* de paquetes y tampoco tienen tablas de *routeo*.
Pero, podemos asignarle una dirección [[IPv4 Addressing|IP]] a una SVI para permitir conexiones remotas a la CLI del *switch* (mediante SSH o Telnet).

1. Partimos con **"interface </interfaz_virtual>"**
2. Le asignamos dirección ip **"ip address </ip> </netmask>"**
3. La activamos **"no shutdown"**
4. Ahora debemos asignarle una *default gateway*, con **""ip default-gateway </default_gateway_ip>** 




## Configuración SSH

Primero, debemos verificar que el IOS del equipo soporte SSH, cualquier versión que tenga en el nombre un "K9", lo soporta.
Podemos ver la versión con **"show version"**, o vemos ver directamente si el quipo lo soporta con **"show ip shh"**.

Recordar que para que podamos conectarnos a un *switch* mediante SSH, necesitamos primero configurar una IP en su SVI.
### Habilitando SSH

1. Debemos configurar un *hostname* y un nombre de dominio en el equipo con **"ip domain name </nombre>"**
	1. Esto es necesario ya que el FQDM **(Fully Qualified Domain Name = host name + domain name)** se usa para nombrar las llaves RSA
2. Debemos generar un par de llaves públicas y privadas, usando **"crypto key generate rsa"**
	1. Deberemos elegir el tamaño de las *keys*
	2. Tamaño de *keys* para SSHv2 debe ser > 768 *bits*

### Configurando SSH

1. Configuramos usuario y contraseña con **"enable secret </clave>"** y **"username </username> secret </clave>"** (VER Console Port Security)
	1. Utilizar "secret" utiliza encriptación MD5
2. Podemos configurar una ACL para restringir quien queremos que se pueda conectar por SSH
3. elegimos la version de SSH: **#ip ssh version 2#**
4. Usamos el comando **"line vty 0 15"** para permitir 16 conexiones simultaneas
	1. Y entramos al modo de configuración de las líneas VTY
5. Habilitamos autenticación con **"login local"**
	1. VER CONSOLE PORT SECURITY
	2. No podemos usar solo **"login"** para SSH
6. Configuramos *timeout* con **"exec-timeout </minutos> </segudos>"**
	1. Ver CONSOLE PORT SECURITY
7. Ahora, con el comando **"transport input ssh"** configuramos que tipo de conexión queremos permitir en las "VTY Lines"
	1. Con SSH solo permitimos SSH y ninguna otra
8. Finalamente, con **"access-class </numero_o_nombre_de_ALC> {in|out}"** aplicamos la ACL a las "VTY lines"
	1. "access-class" es para "VTY lines"
	2. "ip access-group" es para una interfaz
	3. esto se hace en el modo de config de la *line*

## Conectándonos mediante SSH

Podemos conectarnos con el comando **"ssh -l [username p_address | ssh </usernam@ip_address>]"**

La que ip que se usa en el comando es la de la SVI
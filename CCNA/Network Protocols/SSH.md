
Fue desarrollado en 1995 para reemplazar Telnet.
SSH usa TCP en puerto **22**.
El equipo que está siendo accesado cuenta como el servidor, y quien está usando SSH para conectarse, es el cliente.
 <p>En computación, una "shell es un programa de computador que expone los servicios de un sistema operativo a un usuario u otro programa</p>
VTY Lines (Virtual TeleType) indica las posibles conexiones simultaneas que son permitidas a un equipo en especifico. Generalmente se admiten hasta 16 líneas.



## Configuración previa - de la console line

Por defecto, no se necesita ninguna contraseña para acceder a la [[Cisco CLI|CLI]] de Cisco mediante el "Console Port". Podemos configurar una contraseña en la misma consola, para que cualquier persona que quiera acceder a la CLI deba primero conocer la contraseña.

**Primero estableceremos una contraseña y usuario para acceder a la consola**
1. Ingresamos el comando **"enable secret </clave>"**
	1. El uso de "secret" crea una clave con encriptación MD5
2. Luego **""username </usuario> secret </misma_clave_de_arriba>**
	1. Este se usará cuando queramos entrar a la *console line*

### L2 Switch - Management IP

Como ya sabemos, los *[[Switch|switches]]* no realizan *routeo* de paquetes y tampoco tienen tablas de *routeo*.
Pero, podemos asignarle una dirección [[IPv4 Addressing|IP]] a una SVI para permitir conexiones remotas a la CLI del *switch* (mediante SSH o Telnet).

1. Partimos con **"interface </interfaz_virtual>"**
	1. Ej. **"interface vlan </numero_vlan>"**
		1. El número tiene que ser el ID de la VLAN en la que estemos trabajando
2. Le asignamos dirección ip **"ip address </ip> </netmask>"**
3. La activamos **"no shutdown"**
4. Ahora debemos asignarle una *default gateway*, con **""ip default-gateway </default_gateway_ip>""**

### Login local

Ahora configuraremos el puerto "console" para requerir que los usuarios se *logeen* usando algunos de los *usernames* anteriormente configurados, cuando se conecten por ella.

**Empezamos entrando a su modo de configuración**
- Con **line console 0"**

**Ahora configuraremos el requerimiento de tener que *logearse***
- Con **"login local"**
	- Se requerirá usuario y contraseña configurados anteriormente

**Finalmente, configuraremos tiempo de inactividad para que se desconecte**
- Con **"exec-timeout </algo> </algo>"**


## Configuración SSH

Primero, debemos verificar que el IOS del equipo soporte SSH, cualquier versión que tenga en el nombre un "K9", lo soporta.
Podemos ver la versión con **"show version"**, o vemos ver directamente si el quipo lo soporta con **"show ip shh"**.

Recordar que para que podamos conectarnos a un *switch* mediante SSH, necesitamos primero configurar una IP en su SVI.
### Habilitando SSH

**Partiremos configuramos un *hostname* y nombre de dominio
1. **"hostname </nombre>"** para *hostname*
2. **"ip domain name </nombre>"** para nombre de dominio
	1. Esto es necesario ya que el FQDM **(Fully Qualified Domain Name = host name + domain name)** se usa para nombrar las llaves RSA

**Ahora crearemos el par de llaves pública y privada de encriptación**
1. Con el comando **"crypto key generate rsa"**
	1. Deberemos elegir el tamaño de las *keys*
	2. Tamaño de *keys* para SSHv2 debe ser > 768 *bits*

**Elegiremos la version 2 de SSH, para mejorar la seguridad**
1. Con **"ip ssh version 2"**

**Podemos configurar una ACL para limitar aún más las probabildiades de acceso**
1. Con **"access-list </numero_o_nombre> permit host </ip>"**

### Configurando SSH

**Primero, tenemos que entrar al modo de configuración de las líneas VTY**
1. Con **"line vty 0 15"**
	1. Configura las 16 lineas disponibles

**Ahora configuraremos el requerimiento de logearse para poder acceder a la línea**
1. Con el comando previo **"login local"**
	1. Usará los mismos usuarios y contraseñas configurados previamente
	2. No se puede solo usar "login" para SSH

**También podemos configurar el tiempo de inactividad**
1. Con **"exec-timeout </minuto> </segundos>"**

**Ahora restringiremos acceso a las vty solo con SSH**
1. Con el comando **"transport input ssh"**

**Finalmente, aplicamos la ACL que configuramos previamente**
3. Con **"access-class </numero_o_nombre_de_ALC> {in|out}"** aplicamos la ACL a las "VTY lines"
	1. "access-class" es para "VTY lines"
	2. "ip access-group" es para una interfaz
	3. esto se hace en el modo de config de la *line*

## Conectándonos mediante SSH

Podemos conectarnos con el comando **"ssh -l [username ip_address | ssh </usernam@ip_address>]"**

La que ip que se usa en el comando es la de la SVI
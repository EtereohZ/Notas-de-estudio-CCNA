As we know, private addresses cannot be used over the internet. NAT will allow my PC to borrow the unique [[IPv4 Addressing|IP]] address of my **[[Router|router]]** to access the internet.

NAT se usa para modificar el IP de la fuente/destino de los paquetes. Cuando un equipo envíe un paquete fuera de nuestra red, una vez que el paquete llegue al *router*, NAT traducirá el IP privado de PC1, al IP público de la interfaz del *router*. Cuando el *router* reciba la respuesta del servidor, NAT se encargará de traducir la IP pública del *router* a la IP privada de PC1.
****

## Static NAT

NAT estático involucra la configuración manual *one-to-one* de los *mapeos* de dirección IP privadas a públicas.
Esto significa que si hay más de 1 equipo en la red que quiera conectarse al internet, cada uno necesitará su propia IP.

Una dirección IP "inside local" es *mapeada* a una "inside global".
- "inside local" se refiere a la IP del *host* en la red, desde la perspectiva de la red local. Es la **IP privada** del *host*
- "inside global" se refiere a la IP del *host* en la red, desde la perspectiva de los equipos fuera de la red local. Es la IP del *host* después del **NAT**, o sea, el **IP público**

En "Static NAT" no usamos la IP pública que se encuentra en la interfaz del *router*.


### Configuración

**Antes de configurar, revisamos comando informativo**
1. Con **"show ip nat statistics"**
	1. Nos muestra las estadísticas de NAT
	2. total active translations: Nos muestra las traducciones estáticas, dinámicas y "extended"
	3. Peak translations: el número máximo de traducciones que han habido en un determinado momento

**Lo primero que debemos hacer es configurar las interfaces del *router***
1. Elegimos la interfaz que configuraremos como "interna"
	1. Ingresamos el comando **"ip nat inside"** para definir la interfaz como "interna"
2. Ahora entramos a la interfaz que configuraremos como "externa"
	1. Ingresamos el comando **"ip nat outside"**

**Ahora deberemos configurar los *mapeos* de las direcciones IP**
1. Con el comando **"ip nat inside source static </ip_local> </ip_global>"**
	1. "ip local" será la IP privada del equipo
	2. "ip global" será la IP pública que usará el PC

**¿Cómo podemos ver las direcciones configuradas?**
1. Con **"show ip nat translations"**
	1. Nos mostrará las direcciones "inside global", "inside local" que configuramos
	2. Entradas dinámicas se crearán cuando las direcciones se usen, incluirán las direcciones "outside local" y "outside global" y los protocolos usados, con puertos respectivos

- **outside local** es la dirección IP del *host* externo, desde la perspectiva de la red local
- **"outside global"** es la dirección IP del *host* externo, desde la perspectiva de la red externa


### Eliminación de las entradas en tabla NAT

Podemos elegir borrar las traducciones dinámicas de la tabla, con:
- **"clear ip nat translation * "**
****

## Dynamic NAT

Aquí, el *router* de forma dinámica *mapea* la dirección "inside local" a "inside global", como se necesite.
Usa una [[Access Control Lists|ACL]] para determinar que trafico debe ser traducido. Si la dirección IP de origen es permitida por la ACL, esta será traducida.
Las direcciones IP disponibles que existirán para ser usadas se configurarán dentro de una "POOL". Cualquier IP que sea traducida tomará una de las IP dentro de la *pool* apropiada para comunicarse por internet. Si por algún motivo no hay suficientes IP's dentro de la *pool*, se le llamará "NAT pool exhaustion". Esto provocará que paquetes con origen en estás IP que no se pudieron traducir sean botados por el *router*.

### Configuración 

**Al igual que en NAT estática, configuramos las interfaces del *router***
1. Elegimos la interfaz que configuraremos como "interna"
	1. Ingresamos el comando **"ip nat inside"** para definir la interfaz como "interna"
2. Ahora entramos a la interfaz que configuraremos como "externa"
	1. Ingresamos el comando **"ip nat outside"**

**Ahora debemos crear una ACL para definir el tráfico que será traducido**
1. **"ip access-list standard </numero_o_nombre>"** para entrar al modo de config de ACL
2. Configuramos entradas con  "[entry_number] {deny|permit} </ip> </wildcard>"

**A continuación definimos la *pool* de los "inside global"**
1. **"ip nat pool </nombre_pool> </rango> </rango> netmask </netmask>"**

**Finalmente, configuramos la NAT *mapeando* la ACL a la *pool**
1. Usamos **"ip nat inside source list </numero_o_nombre_ACL> pool </nombre_pool>"**
****

## PAT

Port Address Translation (AKA NAT overload) translates both the IP address and the port number (if necessary).

¿Cúal es el proposito de traducir un puerto?
Al usar un número único de puerto para cada flujo de comunicación entre *hosts* internos y externos, 1 IP puede ser usado por varios *hosts* internos. 
El *router* mantendrá un registro de estos flujos gracias a estos puertos únicos. 


## Configuración

**La configuración es exactamente la misma a la NAT dinámica, hasta que llegamos a la parte de configurar la *pool* y la ACL, donde agregamos "overload" al final**
1. Usamos **"ip nat inside source list </numero_o_nombre_ACL> pool </nombre_pool> overload"


### Configuración alternativa

En este modo le decimos al *router* que traduzca las IP's privadas al IP de su interfaz externa.

**En vez de especificar una *pool*, especificamos la interfaz***
1. Usamos **"ip nat inside source list </numero_o_nombre_ACL> interface </interfaz> overload"
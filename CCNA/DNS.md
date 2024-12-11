Domain Name System is used to resolve human-readable names (google.com) to [[IPv4 Addressing|IP]] addresses. The DNS servers a device uses can be manually configured or learned through [[DHCP]].

DNS usa tanto [[TCP]] como [[UDP]], a través del puerto 53. TCP se usará cuando los mensajes DNS sean mayor a 512 *bytes*.

## DNS cache

Equipos guardarán las respuestas del servidor DNS  un *cache* local. Esto significa que los equipos no tendrán que preguntarle al servidor cada vez que quieran acceder a un destino.
El "CNAME" es un nombre canónico que *mapea* un nombre a otro nombre.

- Podemos ver el *cache* en Windows con el comando **"ipconfig /displaydns"**
- Podemos reiniciar el *cache* con **"ipconfig /flushdns"**


## Configuración


### Configuración de *router* como servidor DNS

1. Usamos el comando **"ip dns server"** para configurar al [[Router|router]] como servidor DNS
2. Nuestro servidor no tendrá registros, se los tenemos que dar
	1. Usamos el comando **"ip host </nombre_de_host> </direccion_ip>"**
	2. Aquí podemos agregar otros equipos que están en nuestra red
3. Con el comando **"ip name-server </ip>"** configuraremos un servidor externo que nuestro servidor DNS podrá usar en caso de que no tenga registros solicitados
4. Ahora, con el comando **"ip domain lookup"** habilitamos a nuestro servidor para que pueda hacerle *queries* al servidor externo que configuramos
	1. Generalmente está configurado por defecto 
5. Para ver los *hosts* configurados o aprendidos por DNS usamos el comando **"show hosts"**


### Configuración de *router* como cliente DNS

1. Debemos configurarle un servidor DNS al que pueda acceder
	1. Usamos el comando **"ip name-server </IP>"**
2. Habilitamos las *queries* al servidor con **"ip domain lookup"**
	1. Generalmente habilitado por defecto


## Configuración de un domain name

Definen un área de control admistrativo en internet

1. Usamos el comando **"ip domain name </nombre.x>"**
	1. Automáticamente agregará este nombre de dominio a cualquier *hostname* que no tenga un dominio especifico

DAI is a feature of [[Switch|switches]] that is used to filter [[ARP]] messages received on untrusted ports. 

Al igual que en [[DHCP Snooping]], todos los puertos son de no-confianza, por defecto. Pero en este caso, todos los puertos que estén conectados a otros equipos de red ([[Router|routers]], switches) serán de confianza, mientras que las interfaces que se encuentren conectadas a *end hosts* serán de no-confianza.



## ¿Cómo funciona?

DAI inspecciona la [[MAC Address|MAC]] e [[IPv4 Addressing|IP]] de origen en el mensaje ARP al ser recibido en puertos de no-confianza y verifica que exista una entrada que coincida, con esa MAC e IP, en la "DHCP snooping binding table".
Si existe una entrada que calce con la información del mensaje, este se *forwardea*. si no, no.

Se pueden configurar [[Access Control Lists|ACL's]] de ARP manualmente, para *mapear* direcciones IP a direcciones MAC para ser verificadas por DAI. Útil para *hosts* que no usen [[DHCP]].



## Configuración

**Partimos habilitando DAI **
1. Con el comando **"ip arp inspection vlan </numero>"**
	1. Habilitar en la 1 si no estamos usando otras
	2. Habilitar para cada VLAN

**Ahora elegimos interfaces y elegimos puertos de confianza**
1. Con **"ip arp inspection trust"**

**Podemos ver las interfaces DAI con**
1. **"show ip arp inspection interfaces"**
	1. "Burst Interval" se refiere a limitar X paquetes cada Y segundos

**Ahora elegiremos interfaz y configuraremos "rate limiting"**
1. Con **"ip arp inspection limit rate </paquetes> burst interval </segundos>"**

**También podemos volver a activar la interfaz de forma manual o con "ErrDisable"**
1. Con **"errdisable recovery cause arp-inspection"**


### *Chequeos* opcionales

Por defecto, DAI verifica el IP y MAC de origen en el mensaje, pero también se puede configurar para que *chequee* MAC de destino e IP's inesperados. Estas verificaciones se hacen de forma extra a las por defecto, elegir una no las reemplaza.

**Esto se puede hacer con **
1. **ip arp inspection validate [dst-mac | ip | src-mac]**


## ARP ACL's

**Podemos configurar una con**
1. **"arp access-list </nombre_acl>"**
2. Configuramos la lista con **"permit ip host </ip> mac host </mac>"**
3. Ahora la aplicamos con **"ip arp inspection filter </nombre_acl> vlan </numero_vlan>"**

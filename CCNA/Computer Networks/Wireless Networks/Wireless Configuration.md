
**Lo primero que debemos hacer es configurar las [[VLAN|VLAN's]]**
1. Empezamos con **"vlan <numero_vlan>"**
2. Le ponemos nombre **"name </nombre>"**
3. Repetimos con el rest de las VLAN's que queramos configurar
	1. Management, internal o guest

**Ahora configurar las interfaces conectadas a las AP en modo *access***
1. Elegimos interfaz **"interface </interfaz>"**
2. Elegimos el modo con **"switchport mode </modo>"**
3. Asignamos la VLAN a al *access* con **"switchport access vlan </numero_vlan>"**
4. Habilitamos portfast **"spanning-tree portfast"**

**Configuramos el [[EtherChannel]] entre el WLC y el *[[Switch|switch]]***
1. Con **"interface </rango>"**
2. Y **"channel-group </grupo> mode </modo>"**
	1. WLC's solo soportan el modo estático

**Ahora configuramos la interfaz con la WLC en modo *trunk***
1. Elegimos la interfaz *ether* **"interface </interfaz>"**
2. La designamos como *trunk* **"switchport mode trunk"**
3. Configuramos las VLAN's permitidas con **"switchport trunk allowed vlan </numero>"**

**Configuraremos una SVI para cada VLAN, que será  usada como la "default gateway de sus subnets"**
1. Elegimos vlan **"interface vlan </numero_vlan>"**
2. Asignamos ip **"ip address </ip> </netmask>"**
3. Repetimos para cada VLAN

**Ahora configuramos DHCP para cada VLAN**
1. empezamos con **"ip dhcp pool </nombre_pool>"**
2. elegimos las direcciones de la pool **"network </ip_red> </netmask>"**
3. Determinamos la "default gateway" que usaran **"default-router </ip>"**
4. (Opcional)Usamos **"option 43 ip </ip_del_WLC>"**
	1. Esto sirve para decirle a las AP's cual es el IP del WLC, y así formar los túneles CAPWAP
	2. Es necesario usarlo si el WLC no está dentro del área *broadcast* de las AP's
5. Repetimos para cada VLAN

**Ahora configuramos NTP**
1. Elegimos un *switch* como servidor con **"ntp master"**



## WLC Ports & Interfaces

En una WLC, los puertos son físicos donde se conectan los cables y las interfaces son interfaces "logicas" dentro del WLC.

1. **Management Interface**
	1. Usada para el tráfico de manejo, como Telnet, [[SSH]], HTTPS, RADIUS
	2. Túneles CAPWAP también se forman de esta interfaz
2. **Redundancy Management Interface**
	1. Cuando 2 WLC's se conectan por sus puertos de redundancia, 1 WLC se pone "activa" y la otra en "standby". Está interfaz se usa para manejar la "standby"
3. **Virtual Interface**
	1. Se usa para comunicar con clientes inalámbricos para enviar solicitudes [[DHCP]], autenticación de web, etc
4. **Service Port Interface**
	1. Usada para manejo "out-of-band"
5. **Dynamic Interface**
	1. Usadas para *mapear* la WLAN a una VLAN



CPU ACL's se usan para limitar acceso a la CPU de la WLC
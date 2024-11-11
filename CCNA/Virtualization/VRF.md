
"Virtual Routing & Forwarding" es usado para dividir un *[[Router|router]]* en varios *routers* virtuales. Es similar a dividir [[LAN|LAN's]] en varias [[VLAN|VLAN's]].
Esto provoca que el *router* maneje varias tablas de *routeo* separadas. Cada ruta e interfaz (solo L3) se configura para estar en una **VRF especifica**.

VRF es usado por proveedores para permitir que 1 equipo lleve tráfico de varios clientes distintos. El tráfico de cada uno estará aislado del resto.
Otra cosa importante es que VRF le permite a los usuarios que las direcciones [[IPv4 Addressing|IP]] puedan ser iguales.

![[VRF.png]]
***

## Configuración

**Primero debemos crear la VRF**
1. Empezamos con el comando **"ip vrf </nombre_vrf>"**

**Ahora debemos entrar a una interfaz y asignarle una VRF
1. Y usamos el comando **"ip vrf forwarding </nombre_vrf>"**

**Podemos ver todas las VRF's configuradas **
1. Con **"show ip vrf"**

**Si queremos ver la tabla de *routeo* tenemos que ver una especifica para cada VRF**
1. Se usa **"show ip route vrf </nombre_vrf>"**
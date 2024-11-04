Routers are used to provide connectivity between [[LAN|LAN's]], they send data over the internet.
Poseen menos interfaces que los [[Switch|switches]].

Routers separan las redes y poseen una dirección distinta para cada red a la que está conectado.
Los routers no hacen *flood* de paquetes.

Con el router ya tenemos conexión al internet, pero necesitamos algo que proteja nuestra red, necesitamos un [[Firewall]]

Las interfaces vienen deshabilitadas por defecto en equipos Cisco.

Algunos modelos:
- ISR 1000
- ISR 900
- ISR 4000

#### Interface Errors

- **runts**:  *Frames* que son mas pequeñas que el tamaño mínimo de un *frame* (64 bytes)
- **giants**: *Frames* que son mas grandes que el tamaño mínimo de un *frame* (1518 bytes)
- **CRC**: son los *frames* que fallaron el chequeo [[Ethernet Frame|CRC]] (en el *Ethernet frame trailer)
- **frame**: *Frames* que tienen un formato incorrecto (debido a un error)
- **output errors**: son los *frames* que el *switch* intentó enviar, pero fallaron

![[networking.png|200x200]]

 A First Hop Redundancy Protocol is a computer [[Networking Protocols|networking protocol]] which is designed to protect the [[Routing|default gateway]] used on a subnetwork by allowing two or more [[Router|routers]] to provide backup for that address; in the event of failure of an active router, the backup router will take over the address, usually withing a few seconds.

Los *routers* deben ponerse de acuerdo sobre quién tomará el rol activo de ser el *default gateway*, y otro tomará el papel de *standby*. Se pondrán de acuerdo y mantendrán su comunicación usando *Hello's*. Además, se debe configurar una IP virtual que será usada por los 2 *routers* y se generará una MAC virtual para ese IP.

Cuando un PC quiera comunicarse fuera de la red, usará la [[MAC Address|MAC]]  virtual de los *routers* que estén configurados como *default gateway*. Estas las aprenderá mediante [[ARP]].

Si el *router* activo cae, el *router* secundario deberá actualizar la tabla de direcciones MAC de los [[Switch|switches]] mediante ARP *reply* que enviará de manera autónoma (estos son *broadcast*).Si el *router* que cayó vuelve a estar en línea, este no volverá a ser el activo, debido a que FHRP's son "non-preemptive", a menos que se configure lo contrario.

Veremos 3 tipos distintos:
- [[HSRP]]
- [[VRRP]]
- [[GLBP]]

Cuadro resumen
![[Captura de pantalla 2024-10-05 171346.png]]
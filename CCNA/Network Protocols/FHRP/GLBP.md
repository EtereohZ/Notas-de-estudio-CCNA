Gateway Load Balancing Protocol es propietario de Cisco.

- Puede realizar *load-balance* entre varios [[Router|routers]] dentro de la misma [[Subnetting|subnet]]
- Se elige un **AVG** (Active Virtual Gateway)
- AVG elige hasta 4 **AVF's** (Active Virtual Forwarders), pudiendo también elegirse a si mismo.
	- Cada AVF actúa como *default gateway* para una porción de los *hosts* en la *subnet*
- Multicast a 224.0.0.102, igual que V2 de [[HSRP]]
- MAC virtual es 0007.b400.XXYY (XX = GLBP group number e YY = AVF number)
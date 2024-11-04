Virtual Router Redundancy Protocol es de estándar libre

- Se elige un *router* "master" y un "backup"
	- misma funcionalidad del activo y *standby*
- *Hello's* son multicast a 224.0.0.18
- La [[MAC Address|MAC virtual]] es 0000.5e00.01XX (XX = VRRP group number)
- No puede hacer *load-balance* en una sola [[CCNA/Subnetting/Subnetting|subnet]], pero si entre distintas *subnets*


## Tipos de Virtualización

Existen 2 tipos distintos que son usados:
1. **Type 1 Hypervisor**
	1. Corre directamente sobre el *hardware*
	2. Se usa en *data centers*
2. **Type 2 Hypervisor**
	1. Corre como un programa cualquiera en un OS
	2. El OS que corre directamente sobre el *hardware* se llama "Host OS" y el que se encuentra corriendo en la VM se llama "Guest OS"


## Conectando VM's a la red

LAs VM's se conectan entre ellas y con la red externa mediante [[Switch|switches]] virtuales que corren en el hipervisor Estos pueden funcionar de manera normal, con puertos *trunk* y *access*, uso de [[VLAN|VLAN's]], etc.
Las interfaces en el *switch* virtual se conectan a la NIC del servidor para comunicarse con la red externa.
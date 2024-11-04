VLAN Trunking Protocol allows you to configure [[vlan|VLANs]] on a central VTP [[server]] [[switch]], and other switches (VTP clients) will synchronize their VLAN database to the server.
It is designer for large networks with many VLANs, so that you don't have to configure each VLAN on every switch.
It's rarely used, and it's recommended that it not be used.

Hay 3 versiones: 1, 2 y 3 
Hay 3 modos: "server", "client" y "transparent"
*Switches* operan en "server" for defecto

Si un *switch* sin dominio VTP recibe un anuncio VTP con un dominio VTP, automáticamente se unirá a ese dominio. 
Elegimos el modo VTP con "vtp mode </modo>"

## VTP Modes
### VTP servers
- Pueden agregar, modificar y eliminar VLANs
- guardan la BD en RAM no volátil (NVRAM)
- Aumentará el *revision number* cada vez que una VLAN es agregada/modificada/eliminada
- Anunciarán la última version de la BD en las interfaces *[[VLAN|trunk]]*, y los clientes VTP sincronizarán su BD VLAN a ella 
- También funcionan como clientes VTP


### VTP Clients
- No pueden agregar/modificar/eliminar VLANs
- No guardan la BD en NVRAM (excepto en v3)
- Sincronizarán su BD VLAN con la del servidor que tenga el *revision number* más alto en el dominio
- Anunciarán su BD VLAN y harán un *forward* de anuncios VTP a other clientes a través de sus *trunk ports*

Nota: Revisar [[CLI Commands|comandos]] si se olvidan.
### VTP Transparent
- No participa en el dominio VTP (no sincroniza su BD)
- Mantiene su propia BD VLAN en NVRAM
- Puede agregar/modificar/eliminar VLANs, pero no las anunciará
- *forwardeará* anuncios VTP que estén en su mismo dominio, pero no su propia BD

## Peligros de VTP
Si agregamos un *switch* viejo con un numero de revisión mas alto a nuestra red (con el mismo nombre de dominio), todos los *switches* en el dominio se sincronizarán a su BD VLAN. 

- Cambiar el dominio VTP a uno no usado va a reiniciar el numero de revisión
- Cambiar el modo a "transparent" también reiniciará el número de revisión.
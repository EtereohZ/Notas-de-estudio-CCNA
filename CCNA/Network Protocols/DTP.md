  Dynamic Trunking Protocol is a Cisco proprietary [[Networking Protocols|protocol]] that allows Cisco [[Switch|switches]] to dynamically determine their interface status (access or trunk) without manual configuration.
It's enabled by default.
According to the instructor, DTP is a security risk and should be disabled.


## Configuración

Usamos el comando "show interfaces </interfaz> switchport" para revisar que tipo de *switchport mode* está configurado.
Podemos activar DTP con el comando "switchport mode dynamic </auto_o_desirable>". El modo "desirable" tratará de formar un *trunk* con otros *switches*.  Tratará de formar un *trunk* si los otros equipos que estén conectados tengan los modos:
-  **switchport mode trunk**
- **switchport mode dynamic desirable**
- **switchport mode dynamic auto**
Si el otro equipo no presenta esos modos, se formará un "static access". Es un *access port* que pertenece a una [[VLAN]] que no cambia. es un *access port* como lo hemos entendido ahora, **no frustrarse**.

El modo "auto" no trata de formar un *trunk* activamente. **Solo** formará un *trunk port* **si el otro equipo quiere formar un *trunk**.* Formará un *trunk* si los otros equipos tengan configurados los modos:
- **switchport mode trunk**
- **switchport mode dynamic desirable**
2 *switches* en "auto" o uno en "auto" y el otro en "access", formarán un *access port*

DTP no formará un *trunk* con un [[router]], PC, etc. El *switchport* estará en "access mode"
Podemos desactivar DTP con el comando "switchport nonegotiate" o con "switchport mode access"


#### DPT en encapsulación
También podemos usar DTP en equipos que soportan tanto 802.1Q como ISL, donde negociarán que tipo de encapsulación van a usar.
Esta negociación está habilitada por defecto, y eso "switchport trunk encapsulation negotiate".

ISL es favorecido sobre 802.1Q si ambos *switches* lo soporta, así que acuérdate de deshabilitarlo.
802.1Q envía DTP [[Ethernet Frame|frames]] for la VLAN [[VLAN|nativa]], e ISL lo hace por VLAN1
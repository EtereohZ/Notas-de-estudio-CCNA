Switches are used to forward traffic within a [[LAN]].
En una LAN están conectados todos los dispositivos conectados al mismo switch, estos pueden ser impresoras, PC's, celulares, etc.
Pueden haber varios switches dentro de una misma red.

Los switches no pueden conectarse directamente al internet u otras LAN's, necesitan algo mas. Necesitan conectarse a un [[Router]].

Generalmente tienen >24 interfaces o puertos para que los dispositivos se puedan conectar.
Las interfaces vienen activadas por defecto, en equipos Cisco.

#### Speed/Duplex Autonegotiation

Interfaces that can run at different speeds (10/100 or 10/100/1000) have default settings of *speed auto* and *duplex auto*
Interfaces "advertise" their capabilities to the neighboring device, and they negotiate the best speed and duplex settings they are both capable of.

¿Qué pasa si la auto negociación está deshabilitada en el equipo conectado al switch?
**Velocidad**: El *[[Switch]]* tratará de detectar la velocidad en la que el otro equipo está operando y, si no puede, usará la velocidad más baja soportada ( 10Mbps en una interfaz de 10/100/1000 Mbps).
**[[Full - Half Duplex|Duplex]]**: Si la velocidad es de 10 o 100 Mbps, el *switch* usará *half duplex*. Si es 1000mbps, usará *full duplex*.
Si un equipo tiene configurado manualmente *Full Duplex* y el switch tiene auto negociación activado, el switch no podrá negociar con ese equipo y activará *Half Duplex*, esa diferencia se llama ***Duplex mismatch*** y ocurrirán colisiones. 

#### Interface Errors

- **runts**:  *Frames* que son mas pequeñas que el tamaño mínimo de un *frame* (64 bytes)
- **giants**: *Frames* que son mas grandes que el tamaño mínimo de un *frame* (1518 bytes)
- **CRC**: son los *frames* que fallaron el chequeo [[Ethernet Frame|CRC]] (en el *Ethernet frame trailer)
- **frame**: *Frames* que tienen un formato incorrecto (debido a un error)
- **input errors**: Total of various counters of errors received by the device, como el total de los errores de arriba
- **output errors**: son los *frames* que el *switch* intentó enviar, pero fallaron

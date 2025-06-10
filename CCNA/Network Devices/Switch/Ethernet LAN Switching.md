
## Antes de enviar el frame

Cuando queremos enviar un *frame* de PC1 a PC2, no conocemos la dirección MAC de PC2, sino que usamos la [[IPv4 Addressing|dirección IP]] que se encuentra encapsulada dentro del frame. Pero como los *[[Switch|switches]]* solo operan en L2, necesitan una dirección [[MAC Address|MAC]] para trabajar.
Para aprender la dirección MAC de PC2, vamos a usar el protocolo [[ARP]]. PC1 enviará un ARP request como broadcast y llegará primero al switch, guardando la MAC de PC1 en su *address table*. Luego, reconociendo el frame como un broadcast, enviará el frame a todos los equipos conectados a la red.
Cuando PC2 reciba el frame, responderá con un *known unicast* ARP reply que contendrá su MAC. El switch recibirá esté frame y grabara el MAC de PC2 en su *MAC address table*.
Finalmente PC1 recibirá está respuesta y guardara el MAC de PC2 en su *ARP table*, que asocia los IP's del equipo con su respectiva MAC.

Los próximos *frames* enviados conocerán la dirección MAC del PC2.
## Enviando el frame

PC1 envía el [[Ethernet Frame|frame]] a PC2 a través de la red y el *[[Switch]]* recibe el frame. El frame revisa la dirección [[MAC Address|MAC]] de quien envío el frame y la usa para aprender donde está PC (suponiendo que no la conoce desde antes), agregando la dirección MAC y la interfaz por donde está conectado PC1 a su *MAC Address Table*. Esto se llama ***Dynamic MAC address***, porque se realiza automáticamente.
Ahora, el switch tiene la dirección MAC del destinatario, pero este no sabe donde está. Esto se llama ***Unknown Unicast frame***, ocurre cuando la dirección MAC del destinatario no se encuentra en la *address table* del switch.

Para resolver esto, el switch realiza un *flood* del frame, esto significa enviar el frame a todas las interfaces, menos por la donde recibió el frame. PC3 ignora el paquete porque la dirección MAC del destinatario no coincide con la suya, pero PC2 lo procesa normalmente. Si PC2 envía una respuesta, el proceso termina acá y el switch no aprende la dirección MAC del PC2.
Si PC2 envía una respuesta a PC1, el switch recibirá el frame y agregará la dirección MAC de PC2 a su *address table*. Y como ya conoce donde se encuentra PC1, el switch enviará directamente el frame a PC1, sin realizar un flood.

En *switches* Cisco , las direcciones MAC se eliminan de la *address table* luego de 5 minutos de inactividad.

Nota: La interfaz guardada en la *address table* del switch no necesariamente está conectada directamente a PC guardado, como pasa en [[LAN|LAN's]] de mas de 1 switch.
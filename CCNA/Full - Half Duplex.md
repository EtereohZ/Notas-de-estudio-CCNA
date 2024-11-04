
***Half Duplex*** means that the device cannot send and receive data at the same time. If it's receiving a frame, it must wait before sending a frame.
Un ejemplo de un equipo *half duplex* sería un *[[hub]]*.

**CSMA/CD** Significa *Carrier Sense Multiple Access with Collision Detection* y describe como equipos evitan colisiones en situaciones *half duplex* y como deberían actuar si ocurren colisiones.
1. Antes de enviar *frames*, "escuchan" la red hasta detectar que no hay otros equipos enviando frames
2. Si ocurre la colisión, el equipo envía una señal para informar a los otros equipos que ocurrió una colisión
3. Cada equipo espera un periodo de tiempo aleatorio antes de volver a enviar un frame
4. Se repite el proceso


***Full Duplex*** means that a device can send and receive data at the same time. It does not have to wait.


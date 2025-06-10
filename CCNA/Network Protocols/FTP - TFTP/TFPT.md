Trivial File Transfer Protocol fue estandarizado en 1981.
Trabaja con [[UDP]] en puerto 69.

Se llama "Trivial" porque es más simple y tiene funcionalidades más básicas comparadas con [[FTP]].
- Solo permite que un cliente copie archivos desde/hacia un servidor
- Nada más

No es un reemplazo de FTP, solo es otra herramienta que puede ser usada cuando simplicidad es más importante que funcionalidades más avanzadas.
- Por ejemplo, no posee autenticación o encriptación



## TFPT Reliability

UDP no nos da confiabilidad para en cuanto a la información transmitida,  pero TFPT posee mecanismos internos para darnos integridad y confiabilidad e la información. Esto se llama "lock step".

- Cada mensaje TFPT es *acknowledged*
	- Si un cliente transfiere información a un servidor, el servidor nos responderá con un "Ack", y vice versa
- Se usan *timers*, si no se recibe un mensaje a tiempo, el equipo volverá a enviar el mensaje previo



## TFPT Connections

el proceso de transmisión de archivos tiene 3 fases:
1. **Conexión**: El cliente TFPT envía una solicitud al servidor, y el servidor responde, iniciando la conexión
2. **Transmisión de data**: El cliente y servidor intercambian mensajes TFPT. Uno envía la data y el otro los *ack*
3. **Fin de conexión**: Después de la transmisión del último mensaje, un *ack* final es enviado para terminar la conexión
Transmission Control Protocol is an L4 connection oriented protocol.
Eso significa que antes de enviar información a su destino, los 2 *hosts* se comunican para establecer una conexión. Una vez que la conexión está establecida, se comienza a intercambiar info.

TCP se asegura de que la comunicación sea confiable, quien reciba la información debe reportar haber recibido cada segmento TCP. Si no se reciba confirmación, se envía de nuevo. 
También permite ordenar los segmentos en el orden correcto. 

Por último, proporciona *flow control*, esto significa que el *host* de destino puede avisarle al origen que aumente/disminuya el flujo de info que se está enviando.


## ¿Cómo establecemos conexiones?

TCP utiliza un "Three-Way Handshake" dado que involucra 3 mensajes que se enviarán entre los *hosts*:
1. El *host* de origen (PC) le envía un "SYN" al *host* de destino (servidor)
2. El servidor responde enviando un "SYN" y "ACK" al PC
3. PC ahora le envía al servidor un "ACK"
4. Ahora la conexión está establecida


## ¿Cómo cerramos conexiones?

TCP utiliza un "Four-Way-Handshake" dado que involucra 4 mensajes que se enviarán entre los *hosts*:
1. PC le envía un "FIN" al servidor
2. Servidor responde con un "ACK"
3. Servidor seguidamente envía un "FIN"
4. PC finalmente responde con un "ACK"
5. Se cierra la conexión 



## TCP - Secuencia y *Acknowledgment*

Cuando un equipo envía en primer "SYN" le agrega un número de secuencia aleatorio. Luego, cuando el servidor responde con "SYN ACK", este le agrega otro número de secuencia aleatorio y hace un *acknowledge* del número de secuencia que puso el equipo, agregando el número siguiente que espera recibir en su "ACK".
Finalmente, el equipo agrega como su número de secuencia el número en el "ACK" que recibió del servidor y en su "ACK" agrega el número siguiente que el servidor puso como su número de secuencia.

![[TCP_sequence.png]]




## Control de Flujo

Necesitar *acknowledge* para cada segmento no es muy eficiente, es por esto que el "Window Size" del TCP *header* permite que se envíe más información antes de necesitar un *acknowledge*, y así poder controlar el flujo.

Se puede ajustar el tamaño dinámicamente.


Nota: Revisar [[Comparación entre TCP y UDP|comparaciones]] con [[UDP]]
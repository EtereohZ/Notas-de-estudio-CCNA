UDP is not connection oriented protocol, from L4.
Esto significa que no establece una conexión con el destino antes de enviar información, simplemente la envía.

UDP nos nos provee de comunicación confiable. Si un segmento se pierde, UDP no tiene mecanismo para recuperarlo.

UPD no nos provee de secuenciación. Si un segmento llega fuera de orden, UDP no posee ningún mecanismo para poder reordenarlos.

UDP no posee control de flujo, no tiene como manejar el flujo de información que se envía.


Nota: Revisar [[Comparación entre TCP y UDP|comparaciones]] con [[TCP]]
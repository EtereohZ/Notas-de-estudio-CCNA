Protocolo propietario de Cisco y se encuentra habilitado por defecto.

Funciona enviando mensajes CDP (cada 60 seg.) a dirección *multicast* 0100.0CCC.CCCC y, una vez un equipo recibe el mensaje, se procesa y descarta, por lo que solo vecinos directos pueden recibir estos mensajes.
Los registros se guardarán en una tabla CDP por 180 segundos ("Holdtime"). Si no se vuelve a recibir un mensaje en ese tiempo, el vecino se elimina de la tabla.

Puede compartir información de VTP.


## Configuración
 1. Para habilitarlo o deshabilitarlo, de manera global, usamos el comando **"[no] cdp run"**
	 1. Podemos usarlo para habilitar/deshabilitar una interfaz en específico
 2. Podemos configurar el *timer* con **"cdp timer </segundos>"**
 3. Podemos configurar el *holdtime* con **"cdp holdtime </segundos>"**
 4. Habilitamos CDPv2 con **"[no] cdp advertise-v2"**
 5. Podemos ver información general de CDP con **"show cdp"** 
	 1. Y ver tráfico con **"show cdp traffic"**
	 2. Revisar otras opciones 
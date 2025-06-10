Estándar de la industria (802.1AB) y se encuentra deshabilitado por defecto en equipos Cisco.

 Funciona enviando mensajes LLDP (cada 30 seg.) a dirección [[MAC Address|MAC]] *multicast* 0180.C200.000E. Funciona de manera casi idéntica a CDP.
 Los registros se guardarán en la tabla LLDP por 120 segundos.

No puede compartir información VTP

## Configuración

**Antes de configurar, revisamos comando informativo**
1. Con **"show lldp"** vemos configuración e info
	1. tiene varias opciones, muy parecido a [[CDP]]

**Si queremos activar de forma global en el equipo**
1. Usamos el comando **"[no] lldp run"**

**Si queremos activar LLDP en interfaces especificas**
1. Usamos **"lldp transmit"** en una interfaz
	1. Para habilitar la transmisión de mensajes LLDP
2. Usamos **"lldp receive"** en una interfaz
	1. Para habilitar el poder recibir mensajes LLDP

**Si queremos cambiar el timer de los mensajes**
1. Usamos **"lldp timer </segundos>"**

**Si queremos cambiar el *holdtime***
1. Usamos **"lldp holdtime </segundos>"**

Estándar de la industria (802.1AB) y se encuentra deshabilitado por defecto en equipos Cisco.

 Funciona enviando mensajes LLDP (cada 30 seg.) a dirección [[MAC Address|MAC]] *multicast* 0180.C200.000E. Funciona de manera casi idéntica a CDP.
 Los registros se guardarán en la tabla LLDP por 120 segundos.

No puede compartir información VTP

## Configuración

1. Se habilita o deshabilita con **"[no] lldp run"**
2. Para habilitar la transmisión de mensajes LLDP **"lldp transmit"**  en una interfaz
3. Para habilitar el poder recibir mensajes LLDP usamos **"lldp receive"** en una interfaz
4. Para cambiar el *timer* de los mensajes usamos **"lldp timer </segundos>"**
5. Para cambiar el *holdtime* usamos **"lldp holdtime </segundos>"**
6. Con **"show lldp"** vemos configuración e info
	1. tiene varias opciones, muy parecido a [[CDP]]
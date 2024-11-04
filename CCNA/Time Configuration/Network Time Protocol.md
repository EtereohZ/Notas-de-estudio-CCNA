
- NTP nos permite sincronizar automáticamente la hora en la red.
- Clientes NTP solicitarán la hora a los servidores NTP
- Un equipo puede ser cliente y servidor al mismo tiempo
- NTP nos permite una precisión de hasta 50 milisegundos si nos estamos conectando al servidor NTP a través de internet
- NTP usa puerto 128 UDP
- **Stratum** es la distancia de un servidor NTP al reloj de referencia


## Reference Clock

Son relojes muy precisos, son la fuente de la hora. Se consideran con un ***stratum*** de 0. Los servidores NTP que están conectados a ellos son ***stratum*** 1. Cada 1 salto en la jerarquía le agrega 1 al *stratum*.

Los equipos también pueden entrar en un estado "symmetric active mode", donde formarán relaciones con equipos en el mismo *stratum* para proveer una hora mas precisa. también, un cliente puede sacar su hora de mas de 1 servidor.




## Configuración

1. Usamos el comando "ntp server </ip_servidor> [prefer]"
	1. El servidor que elijamos nos dará la hora
	2. La opción del final es para decirle al equipo que prefiera ese servidor
	3. **"show ntp associations"** nos muestra los servidores que configuramos
4. **"show ntp status"** nos muestra el *stratum* del equipo y otra info
5. **Recordar** que luego hay que configurar manualmente la *timezone*
6. Con el comando "ntp source </interfaz>" le diremos al *[[Router|router]]* que use esa interfaz para la fuente de sus mensajes NTP
	1. Puede ser buena idea usar como interface la dirección *loopback*. Así, si una interfaz cae, el *router* podrá seguir comunicando esa interfaz por otras interfaces físicas que tenga disponible

### Configuración de un *master*

Esto se puede usar en caso de que no tengamos conexión a internet y queramos configurar a un servidor NTP como "master clock", y será usado como el reloj de referencia.

1. Usamos el comando "ntp master {stratum_number}"
	1. Si no ingresamos número, el *stratum* por defecto será 8  



### Configuración Symmetric Active Mode

Donde equipos NTP forman relaciones con otros para poder sincronizar sus tiempos y trabajar como *backups* en caso de que pierdan a su servidor.

1. Con comando **"ntp peer </direccion_ip>"**

It's the interface you use to configure Cisco devices


### Conectándonos al dispositivo
Para conectarnos a un dispositivo Cisco y configurarlo con la CLI, podemos conectarnos al puerto *console* desde nuestro notebook. Para eso usamos un cable llamado [[Rollover Cable]], con un conector [[RJ-45 Connector|RJ-45]] para el dispositivo y otro conector [[DV9]] para nuestro notebook. Si no tenemos ese puerto, necesitaremos un adaptador.

Luego de conectarnos, necesitamos un emulador de terminal para acceder a la CLI, como [PuTTY](https://putty.org/).
Abrimos el programa, apretamos tipo de conexión *Serial* y luego en *Open*. También podemos cambiar la configuración si apretamos en *Serial* en la columna de categorías.


### Entrando a los distintos modos
Luego de entrar a la CLI, nos encontramos en *User EXEC Mode*, que se nota por el símbolo ">" en 
*Router>*, donde escribimos los comandos. *Router* sería el hostname del equipo.

*User EXEC Mode* es limitado, podemos observar pero no podemos cambiar la configuración. También puede ser llamado *user mode*.

Podemos entrar en *Privileged EXEC Mode* con el comando [[CLI Commands|"enable"]] en la CLI. El símbolo ">" cambiará por un "#".
Este modo nos da acceso a ver todas las configuraciones, reiniciar y dispositivo, y otras cosas, pero no cambiar la configuración.

Si queremos configurar y entrar al *Global Configuration Mode, ahora ingresamos el comando [[CLI Commands|"configure terminal"]] y aparecerá un "(config)" después del hostname.

Con el comando "exit" Salimos de *Global Configuration Mode* y si lo ingresamos de nuevo nos deslogea.


### Configuración

##### Seguridad
Para darle seguridad al equipo, Ingresamos el comando [[CLI Commands|"enable password <clave>"]], donde podemos elegir nuestra contraseña, es sensible a mayúsculas.
Podemos encriptarla para que no se pueda ver, con "service password-encryption"
##### Archivos de configuración
Hay 2 archivos de configuración que se encuentran en el equipo: *running-config* y *startup-config*
- *running-config*:  Es el archivo de configuración que se encuentra activo en el equipo, todos los comandos que ingresemos a la CLI editarán este archivo.
- *startup-config*: Es el archivo de comunicación que se carga luego de prender o reiniciar el equipo.
##### Cancelar un comando ingresado
El comando "no" cancela o eliminar un comando ingresado









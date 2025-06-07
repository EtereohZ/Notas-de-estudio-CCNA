It's the interface you use to configure Cisco devices


## Conectándonos al dispositivo
Para conectarnos a un dispositivo Cisco y configurarlo con la CLI, podemos conectarnos al puerto *console* desde nuestro notebook. Para eso usamos un cable llamado [[Rollover Cable]], con un conector [[RJ-45 Connector|RJ-45]] para el dispositivo y otro conector [[DV9]] para nuestro notebook. Si no tenemos ese puerto, necesitaremos un adaptador.

Luego de conectarnos, necesitamos un emulador de terminal para acceder a la CLI, como [PuTTY](https://putty.org/).
Abrimos el programa, apretamos tipo de conexión *Serial* y luego en *Open*. También podemos cambiar la configuración si apretamos en *Serial* en la columna de categorías.


## Primeros pasos
Luego de entrar a la consola, nos encontramos en **"User EXEC Mode"**, que se nota por el símbolo ">" en **Router>**, donde escribimos los comandos. *Router* sería el *hostname* del equipo.

**"User EXEC Mode"** es limitado, podemos observar pero no podemos cambiar la configuración del equipo. También puede ser llamado **"user mode"**. En resumen, es un modo sin privilegios para poder realizar cambios.

Podemos entrar en **"Privileged EXEC Mode"** con el comando **"enable"** en la CLI. El símbolo ">" cambiará por un "#". Ahora se verá como **Router#**.
Este modo nos da acceso a ver todas las configuraciones, reiniciar y dispositivo, y otras cosas, pero no cambiar la configuración.

Si queremos configurar y entrar al **"Global Configuration Mode"**, ahora ingresamos el comando **"configure terminal"** y aparecerá un "(config)" después del *hostname*. Se verá como **Router(config)**.

Con el comando "exit" Salimos de *Global Configuration Mode* y si lo ingresamos de nuevo nos deslogea.

### Resumen

Recordar que no podemos entrar al modo privilegiado sin antes entrar al modo de usuario.

**Modo para entrar al modo de usuario
1. **"enable"**

**Comando para entrar al modo privilegiado**
1. **"configure terminal"**

**Para salir del modo privilegiado**
1. **"exit"**
2. También existe el comando **"end"**, pero ese nos saca de todo

Revisar otros comandos básicos en [[CLI Commands]]
## Configuraciones de seguridad

Podemos aumentar la seguridad del equipo configurando contraseñas que limitarán el acceso a la consola.
El comando **"enable password </clave>"**, sirve para configurar una contraseña básica. Es sensible a mayúsculas. La contraseña ingresada no será *hasheada* por defecto, por lo que cualquier persona puede entrar a los archivos de configuración y verla, usando **"do show running-config"**. Podemos encriptarla para que no se pueda ver, con **"service password-encryption"**.
Ese *hash* (llamado "Type 7") se puede desencriptar muy fácilmente. **Nunca** usar en entornos reales.

Otra manera más segura es usando **"enable secret </clave>"**. Este comando usará el *hash* **MD5** para asegurar nuestra contraseña. Al igual que el tipo 7, no debe usarse en entornos reales.

## Archivos de configuración


**Hay 2 archivos de configuración que se encuentran en el equipo: *running-config* y *startup-config***
- *running-config*:  Es el archivo de configuración que se encuentra activo en el equipo, todos los comandos que ingresemos a la CLI editarán este archivo.
- *startup-config*: Es el archivo de comunicación que se carga luego de prender o reiniciar el equipo.

- Podemos guardar la configuración con el comando **"write"** o **"write memory"**









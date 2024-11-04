Es una característica de seguridad de los *[[Switch|switches]]* de Cisco. Nos permite controlar que direcciones [[MAC Address|MAC]] son permitidas para entrar en las interfaces.

Cuando habilitamos seguridad de puertos en una interfaz, la configuración, **por defecto**, solo permitirá 1 dirección MAC. Esto se puede configurar.
Otra manera de proteger los puertos es limitar la cantidad de MAC's que serán permitidas en una interfaz.
***

## Configuración

**Para habilitar seguridad de puertos debemos entrar a la interfaz deseada**
1. Ingresamos el comando **"switchport port-security"**
	1. Sólo funciona en puertos  "access" o "trunk" configurados manualmente, no los "dynamic"
2. Cambiamos el modo de puerto, de ser necesario, con **"switchport mode {access|trunk}"**


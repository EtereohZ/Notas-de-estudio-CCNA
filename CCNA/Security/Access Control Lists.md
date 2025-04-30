Provides a form of control to allow or deny access to a network.

Funcionan como un filtro de paquetes, con instrucciones para que el *[[router]]* permita o descarte tráfico específico.
Pueden filtrar tráfico basado en varios parámetros: Origen/dirección IP, origen/destino de puertos L4, etc.



## Tipos de ACL

### Standard ACL's

Filtran según IP de origen y nada más. Por eso mismo, se deben aplicar lo más cerca del destino como sea posible.

Un máximo de **una** ACL puede ser aplicada a una interfaz, por dirección:   1 *inbound* y 1 *outbound*. Aplica tanto para la "standard" como las "extended".


- Las con número se identifican con un número y abarcan desde 1-99 y 1300-1999.
- Las con nombre se identifican con nombre

#### Configuración

Primero, podemos acceder a un modo de configuración dedicado para las ACL:

1. Ingresamos el comando "ip access-list standard </numero_o_nombre>"
2. Configuramos con "[entry_number] {deny|permit} </ip> </wildcard>"
3. Para aplicar la configuración entramos a la interfaz e ingresamos el comando "ip access-group </numero_o_nombre> {in|out} "

Este modo nos permite eliminar *entries* de las listas con "no </numero_entry>"


##### Resecuenciamiento

Si alguien escribe las entradas de las listas con enumeración "1, 2, 3, 4", no nos deja espacio para agregar entradas entremedio. Esto lo podemos arreglar con el comando "ip access-list resequence </numero_acl> </numero_inicial> </incremento>".
Ese comando cambiará la primera entrada a 10 e ira aumentando las entradas por el número de incremento ingresado.



### Extended ACL's

Filtran según varios distintos parámetros y abarcan desde 100-199 y 2000-2699.

Se divide en 2 tipos de ACL's:
1. Extended Numbered
2. Extended Named

Extended ACL's deben configurarse lo mas cercano a la fuente como sea posible, a diferencia de los *standard*.
#### Configuración
1. Entramos al modo de configuración con "ip accesss-list {number|name}"
2. Configuramos una *entry* con "[entry_number] {deny|permit} {[protocol]  [src-ip]  [dst-ip]}"

En las *extended*, para referirnos a un *host* debemos escribir "host" antes del IP o la *wildcard*, ya no nos sirve solo poner el IP.
Si queremos seleccionar puertos específicos, van después de las direcciones y usamos: 
eq: para decir igual al puerto X
gt: Para referir a > puerto X
lt: Para referir a < puerto X
neq: Para referir a distintos a puerto X
range: Para referir rango desde puerto X a Y



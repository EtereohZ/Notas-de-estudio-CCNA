
#### CIDR
Cuando el internet recién fue creado, nadie esperaba que se volviera tan grande como lo es actualmente.
Esto resultó en varias direcciones [[IPv4 Addressing|IP]] desperdiciadas.
En 1993 se introduce CIDR para reemplazar el sistema de clases de las direcciones.

Como el sistema de clases fijas fue removido, esto permite a grandes redes dividir sus redes, permitiendo mayor eficiencia. Estas redes mas pequeñas se llaman "subnetworks" o "subnets."

Podemos elegir los prefijos específicos para nuestras necesidades y así no desperdiciar IP's
Por ej., una conexión *point-to-point* entre 2 *routers*. 
En vez de elegir una dirección con prefijo de /24, donde perderíamos 253 IP's, podríamos determinar un prefijo de **/30**, lo que equivaldría a 2<sup>2</sup> = 4, le restamos IP de red y *broadcast* y nos da **2** direcciones IP disponibles.
También podemos elegir un prefijo de **/31** lo que equivaldría a 2<sup>1</sup> = 2, y como es *point-to-point*
no necesitamos IP de red o de *broadcast*.

Recomendado usar /31 cuando es *point-to-point*.

También podemos usar /32 cuando queremos crear una ruta estática a un *host* especifico.

### Como calcular subnets
Si queremos averiguar a que subnet pertenece determinada dirección IP:
1. escribir la dirección en binario
2. Cambiar todos los hostbits a 0
3. Cambiar el nuevo número a decimal

#### **EN *BOLD* QUEDARÁN MARCADOS LOS BITS QUE SE LE PRESTAN AL HOST***

#### Ej. 172.25.217.192/21
- En binario sería 10101100.00011001.**11011**001.11000000
- Como es /21 le estamos pasando 5 bits a la porción de la red
1. Cambiamos todos los bits que pertenecen al host a 0's
	1. 10101100.00011001.**11011**000.00000000
2. Pasamos a decimal y nos da que pertenece a la subnet 172.25.216.0

Si queremos averiguar el broadcast address de una determinada red, Hacemos lo mismo de arriba, pero en vez de cambiar a 0 los *bits* del *host*, **los cambiamos a 1**
#### Ej. 192.168.**91.78/26** 
- Le estamos pasando 2 bits a la porción de la red y queda >> 01011011.**01**001110
1. Cambiamos a 01011011.01111111
2. La *broadcast* es 91.127

#### PARA CALCULAR LA NETMASK
1. Pasar todos los bits de la porción **DE LA RED** a 1's
2. Convertir a decimal
#### **La RED toma bits de la porción de lost HOST, NO al revés**


![[usable_addresses_calculation.png]]


![[CIDR_notation.png]]

![[host_and_subnet _number.png]]


Ignorar de aquí hasta abajo hasta que me den ganas de hacerlo entendible.


127.16.0.0/16
**00**000000.00000000
Le pasamos 2 bits a la porción de la red 2<sup>2</sup>
01000000.00000000
127.16.64.0 red 2
127.16.127.255 broadcast de red 2

127

**Ejercicio:**

Te dan la red 10.0.0.0/8. Debes crear 2000 subnets que deben ser distribuidas.
Cual debe ser el largo del prefijo?
cuantos hosts deben haber en cada subnet?

La red debe tomar 11 bits para crear 2000 subnets, específicamente se pueden crear 2048: 
- 2<sup>11</sup> = posibles subnets
00001010.**00000000.000**00000.00000000
Los 13 bits que le quedan al hostbit nos permite tener 8190 hosts:
- 2<sup>13</sup> - 2 =  posibles hosts

11111111.11111111.11100000.00000000
Nuestro prefijo será /19
255.255.224.0 es la subnetmask

PC1 tiene una IP de 10.217.182.223/11

1. Identificar la red
2. Broadcast
3. Firstusable address
4. Last usable address
5. Numero de hosts

Estamos prestando 3 bits >> 00001010.**110**11001.10110110.11011111
1. 
Para identificar red pasamos todos los hostbits a 0 >> 00001010.**110**00000.00000000.00000000
La red es 10.192.0.0/11
2. 
Para el broadcast pasamos todos los hostbits a 1 >> 00001010.**110**11111.11111111.11111111
10.223.255.255/11
3. 
First usable address es 10.192.0.1/11
4. 
Last usable address es 10.223.255.254/11
5. 
Numero de hosts 2<sup>21</sup> - 2 es 2.097.150

subnetmask es 11111111.11100000.0000000 >>255.224.0.0







Ignorar desde aquí para abajo

192.168.1.0

sub 1
red: 192.168.1.0/26
Broad:192.168.1.63/26
First usable: 192.168.1.1/26
Last usable: 192.168.1.62/26
Total: 62

sub 2
red: 192.168.1.64/26
Broad: 192.168.1.127/26
First usable:192.168.1.65
Last usable:192.168.1.126
Total: 62

sub 2
red: 192.168.1.128/26
Broad:192.168.1.191/26
First usable:192.168.1.129
Last usable:192.168.1.190/36
Total: 62

sub 2
red: 192.168.1.192/26
Broad:192.168.1.255/26
First usable:192.168.1.193
Last usable:192.168.1.254
Total:



**11011**001.11000000 
11011000.000000

01011011.**01**001110
01011011.01000000

01001101




00001010.00000000.00000000.**0001**0000
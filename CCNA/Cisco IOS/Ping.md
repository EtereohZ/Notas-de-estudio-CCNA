Network utility that is used to test reachability.
Measures round-trip time.

Usa 2 mensajes:
- ICMP Echo Request
- ICMP Echo Reply

No funciona como *broadcast*, por lo que hay que conocer la [[MAC Address|MAC]] del destinatario. Por lo que [[ARP]] tiene que usarse antes.
También podemos especificar el tamaño de cada ping

En [[Cisco CLI]], un "." significa que un ping fallo y un "!" significa que el ping fue exitoso.
¿Por qué falló el primer ping en la imagen?
- Porque como PC1 no conocía la dirección MAC de PC2, tuvo que usar ARP y en el tiempo que demoró en obtener su MAC, el primer ping falló.

###### Imagen de un ping en Cisco CLI:
![[cisco_cli_ping.png]]
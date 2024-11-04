It's a 6 byte physical address assigned to the device when it's made, it stands for *Media Access Control*.
The first 3 bytes are the OUI (Organizationally Unique Identifier), which is assigned to the company making the device, the last 3 bytes are unique to the device itself.

Está escrito como **12** carácteres hexadecimales.

Cuando el [[Switch]] aprende una Source MAC y la guarda en su *MAC address table* al recibir un frame, se llama ***Dynamic MAC Address***.

En *switches* Cisco , las direcciones MAC se eliminan de la *address table* luego de 5 minutos de inactividad. Se le llama *aging*.
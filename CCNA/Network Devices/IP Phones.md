
Estos teléfonos usan VoIP (Voice over IP) para permitir llamadas de teléfono a través de la red, como internet.
Se conectan al [[Switch|switch]], como otros dispositivos de internet y consiguen electricidad mediante [[PoE]]

estos teléfonos poseen 3 puertos:
1. El "uplink": Se conecta a un *switch* externo
2. El "Downlink": Se conecta al PC
3. Y el último puerto, que se conecta al teléfono mismo
Esto permite al PC y al teléfono user solo 1 puerto en el *switch*, ya que el tráfico desde el PC pasará por el teléfono hacia el *switch*.
Se recomienda separar el tráfico de voz del telefono y el tráfico de información del PC, separandolos en distintas [[VLAN|VLAN's]].


## Configuración

**Primero debemos entrar a la interfaz del *switch* y configurar el puerto**
1. **"interface </interface>"**
2. **"switchport mode access"**
3. **"switchport access vlan </numero>"**
4. **"switchport voice vlan </numero>**
	1. Este el el que usará el telefono
	2. Este tráfico será *tageado *
A pesar de que esta interfaz lleva tráfico de más de 1 VLAN, no se considera como una "trunk".









## Data Serialization

Antes de empezar con JSON en si, introduciremos "Data Serialization".

Es el proceso de convertir información en un formato estandarizado que puede ser almacenado o transmitido y reconstruido después. Esto permite que la información pueda ser comunicada efectivamente entre distintas aplicaciones.
Estos nos permite representan variables en texto.
***

JSON es un estándar abierto de formatos de archivos e intercambio de información que usa texto fácilmente legible para almacenar la información. En JSON los espacios vacíos no importan.

## Tipos de información

Tipos de datos no estructurados:
1. **String**
	1. Representa texto, siempre está rodeado por " "
2. **Number**
	1. Es un valor numérico
3. **Boolean**
	1. Representa "true" y "false"
4. **"Null**
	1. Representa la ausencia intencional de algún valor

Tipos de datos estructurados:
1. **Object**
	1. Una lista de variables "key-value" no ordenados
	2. Rodeados de { }
	3. La "key" es un "string"
	4. el "value" puede ser cualquier tipo de dato válido
	5. La "key" y el "value" se separan con un :
2. **Array**
	1. una serie de valores separados por comas
	2. No es necesario que sean el mismo tipo de dato


## XML

Usado como lenguaje para "data serialization".
- No es tan legible como JSON
- Espacios en blanco no importan
- Se representa igual que en HTML -> <key>value</key>


## YAML

ES usado por la herramienta de automatización de red "Ansible".
- Es muy facil de leer
- Los espacios en blanco si importan
	- Indentación es importante
- - Se usa para indicar una lista
- "Keys" y "values" se representan como "key:value"

Comparación entre JSON y YAML
![[JSON vs YAML.png]]
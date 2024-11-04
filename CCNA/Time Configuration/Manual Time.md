 
## Manual Time Configuration


1. Usamos el comando "clock set </hh:mm:ss> </dia> </mes> </año>"
	1. Se hace fuera de *config mode*
2. Con **"show clock detail"** vemos info sobre la fecha y quien la configuró


### Hardware Clock Configuration

El reloj del *hardware* se refiere como "calendar".
Se hacen fuera de *config mode*

1. Ingresamos comando **"calendar set </hh:mm:ss> </dia> </mes> </año>"**
2. Con **"clock update-calendar"** sincronizamos el calendario con el reloj
3. Con **"clock read-calendar"** sincronizamos el *clock* con el calendario

Podemos actualizar con [[Network Time Protocol|NTP]] el *calendar* con **"nt update-calendar"**

## Time Zone Configuration

1. Usamos el comando **"clock timezone </nombre> </offset_desde_UTC>"**
	1. Este se hace desde config mode
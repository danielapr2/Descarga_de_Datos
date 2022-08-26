# Descarga de Datos
# Con esta actividad practicamos la descarga de datos desde diferentes fuentes de internet para analizar sus estructuras y formatos.

Tras revisar diferentes fuentes de datos y sus contenidos, decidí que quiero enfocarme en la siguiente hipótesis:

El nivel de escolaridad, la tasa de desempleo y los salarios del género femenino impactan directa e indirectamente en el número de casos de violencia de género y feminicidios. 

Sé que son muchos los factores que influyen en la presencia de violencia contra la mujer y los actos feminicidas, sin embargo, me gustaría analizar cómo se relacionan la baja escolaridad y la desigualdad de oportunidades laborales y económicos hacia el género femenino en estas ocurrencias. 

Lamentablemente muchas mujeres tienen que soportar actos de violencia y vivir con sus agresores por ser su única opción; no cuentan con estabilidad económica o con la capacidad para poder independizarse. Creo que, si se da mayor visualización a la importancia de impulsar y apoyar la educación hacia la mujer, así como promover que sean económicamente independientes por medio de la igualdad de oportunidades laborales y salarios, podríamos causar un impacto en la disminución de estos actos en contra del genero femenino. 

# Seleccione la base de datos de crímenes de la página de Data Mexico, pidiendo que me diera la información agrupada por tipo de crimen, año y municipio:
```{r}
#Asignar el directorio donde estare trabajando:
setwd("C:\\Users\\PANA1\\Documents\\Maestría\\Ing Caracteristicas")
getwd()
```

```{r}
#Mandar llamar las librerías que se necesitaran para descargar nuestros datos:
library(httr)
library(jsonlite)
library(plotly)

```

```{r}
#Mandamos llamar el URL que se saca desde la pagina de Data Mexico 
crimenes.url = GET("https://api.datamexico.org/tesseract/cubes/sesnsp_crimes/aggregate.jsonrecords?captions%5B%5D=Type.Type.Crime+Type.Crime+Type+ES&captions%5B%5D=Geography.Geography.Municipality.Municipality+ES&drilldowns%5B%5D=Type.Type.Crime+Type&drilldowns%5B%5D=Date.Date.Year&drilldowns%5B%5D=Geography.Geography.Municipality&measures%5B%5D=Value&parents=false&sparse=false")

#Convertimos en “string”:
rawToChar(crimenes.url$content)

#Nuestros datos vienen en lenguaje JSON, por lo que debemos convertirlos para que sean mas manejables:
Datos = fromJSON(rawToChar(crimenes.url$content))
names(Datos)

"Pedimos que solo nos de la data de la variable "Datos" que declaramos anteriormente:
Datos<-Datos$data

```


Al haber pedido mis datos por municipios, fueron cerca de 750,000 registros, por lo que el programa tardo muchos minutos en correr:
![image](https://user-images.githubusercontent.com/111605081/186813470-7d851a5b-6173-4b29-b85f-d836ddc33438.png)

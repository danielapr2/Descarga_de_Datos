# Descarga de Datos crudos en RStudio
# Punto 1 de Asignación - Con esta actividad practicamos la descarga de datos desde diferentes fuentes de internet para analizar sus estructuras y formatos.

Tras revisar diferentes fuentes de datos y sus contenidos, decidí que quiero enfocarme en la siguiente hipótesis:

El nivel de escolaridad, la tasa de desempleo y los salarios del género femenino impactan directa e indirectamente en el número de casos de violencia de género y feminicidios. 

Sé que son muchos los factores que influyen en la presencia de violencia contra la mujer y los actos feminicidas, sin embargo, me gustaría analizar cómo se relacionan la baja escolaridad y la desigualdad de oportunidades laborales y económicos hacia el género femenino en estas ocurrencias. 

Lamentablemente muchas mujeres tienen que soportar actos de violencia y vivir con sus agresores por ser su única opción; no cuentan con estabilidad económica o con la capacidad para poder independizarse. Creo que, si se da mayor visualización a la importancia de impulsar y apoyar la educación hacia la mujer, así como promover que sean económicamente independientes por medio de la igualdad de oportunidades laborales y salarios, podríamos causar un impacto en la disminución de estos actos en contra del genero femenino. 

#Seleccione la base de datos de crímenes de la página de Data Mexico, pidiendo que me diera la información agrupada por tipo de crimen, año y municipio:
```{r}
#Asignar el directorio donde estare trabajando:
setwd("C:\\Users\\PANA1\\Documents\\Maestría\\Ing Caracteristicas")
getwd()
```

```{r}
#Mandar llamar las librerías que se necesitaran para descargar nuestros datos:
library(httr)
library(jsonlite)

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

print(Datos)
```


Al haber pedido mis datos por municipios, fueron cerca de 750,000 registros, por lo que el programa tardo muchos minutos en correr:
![image](https://user-images.githubusercontent.com/111605081/186813470-7d851a5b-6173-4b29-b85f-d836ddc33438.png)

Decidí hacer otro ejemplo filtrando solo por "Nación", en este caso México, por lo cual, el flujo corrió mucho mas rápido y me dio los siguientes resultados ya con mis datos de JSON convertidos:
![image](https://user-images.githubusercontent.com/111605081/186818203-da8dfb74-73e3-4b5a-91c9-4cb5f904d53d.png)


# Seguimos los mismos pasos que en el proceso anterior, solo que ahora descargamos la base de datos de empleabilidad por sector y sexo:

```{r}
empleabilidad.url = GET("https://api.datamexico.org/tesseract/cubes/inegi_economic_census_sex/aggregate.jsonrecords?drilldowns%5B%5D=Geography.Geography.State&drilldowns%5B%5D=Year.Year.Year&drilldowns%5B%5D=Sex.Sex.Sex&drilldowns%5B%5D=Industry.Industry.Sector&measures%5B%5D=Employed+workers&parents=false&sparse=false")

rawToChar(empleabilidad.url$content)

Datos2 = fromJSON(rawToChar(empleabilidad.url$content))
names(Datos2)

Datos2<-Datos2$data

print(Datos2)
```
Como "Datos2", nos arroja los siguientes resultados:
![image](https://user-images.githubusercontent.com/111605081/186915288-d19246d0-0051-46d1-a3f1-acde559c2b4a.png)


# Punto 2 de Asignación - Descarga desde otra fuente de datos.
```{r}
#Al tratar con datos en estructura JSON, se carga la librería pertinente:

library(jsonlite)
library(dplyr)

#Mandamos llamar nuestros datos desde el url, que en este caso es de las bases de datos del INEGI. (Para poder accesar es necesario solicitar un token):

json_INEGI <- "https://www.inegi.org.mx/app/api/indicadores/desarrolladores/jsonxml/INDICATOR/6200027788/es/07000026/true/BISE/2.0/8fbac81c-e270-2f88-47b0-a4b8aae60b0d?type=json"

#Convertimos nuestros datos de JSON a caracteres normales:

EducacionSuperior <- fromJSON(json_INEGI)

#El "identificador" viene en codigo, por lo que tendremos que busacr su traducción:

json_INEGI.Indicador <- "https://www.inegi.org.mx/app/api/indicadores/desarrolladores/jsonxml/CL_INDICATOR/6200027788/es/BISE/2.0/8fbac81c-e270-2f88-47b0-a4b8aae60b0d?type=json"


EducacionSuperiorIndicador <- fromJSON(json_INEGI.Indicador)
```

# Punto 3 de Asignación - Estructura de los datos que se descargaron sobre personas desaparecidas a nivel nacional.
## ¿Qué estructura encontraste en los datos?
## ¿Se pueden poner los datos en forma tabular para algún problema que nos interese en particular?
Los datos "Desaparecidos en corte nacional" se descargan en formato JSON, sin embargo, ya existen muchas ibrerías que pueden transformar cadenas de texto en JSON a caracteres string, lo que facilita su manejo.
La url de "Desaparecidos RNPDNO" descarga los datos en un archivo Zip, pero al igual que con los datos pasados, tambien existen librerias que descomprimen los datos para tenerlos de facil manejo. 

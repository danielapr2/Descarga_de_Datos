# Descarga_de_Datos
Con esta actividad practicamos la descarga de datos desde diferentes fuentes de internet para analizar sus estructuras y formatos.


from BeautifulSoup import BeautifulSoup
import csv
import requests

outputfile = "homosexualidad.csv"
csvfile = open(outputfile, 'wb')
fout = csv.writer(csvfile, delimiter=',', quotechar='"', quoting=csv.QUOTE_MINIMAL)

baseUrl = "https://www.inegi.org.mx/programas/enoe/15ymas/"
tablecount = -1

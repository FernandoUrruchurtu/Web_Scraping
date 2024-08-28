# Web Scraping Notes

Para realizar web scraping, se necesitan los paquetes designados en el texto: requirements.txt. Para ello se ejecuta el siguiente comando  
```bash
pip install -r requirements.txt
```
Esto instalara los paquetes de Beautifoul Soup y XPath los cuales sirven para acceder a las paginas y permitir extraer informacion.

Adicional es necesario saber que significan cada etiqueta de html, esa documentacion se encuentra en internet.

Este repo contiene:  
1. Scraping a pagina web de la DIAN para extraer el archivo del IPC
2. Scraping a una pagina de citas de libros, la cual fue hecha por la comunidad para poder practicar web scraping.

## Ejemplo de Web Scraping con XPath
### Primer Paso: Importar librerias
```python
import requests as rq #Importa el paquete de request para hacer peticiones a las web
from lxml import etree # lxml para realizar el parsing con xpath
from bs4 import BeautifulSoup # Importa beautifoulsoup para realizar el parsing
```
### Segundo paso: Realizar la peticion
```python
## Para este caso, usamos el metodo get a la pagina que siempre publica el ipc
resp = rq.get('https://www.dane.gov.co/index.php/estadisticas-por-tema/precios-y-costos/indice-de-precios-al-consumidor-ipc/ipc-informacion-tecnica')
# Con soup, hacemos el parsing al contenido
soup = BeautifulSoup(resp.content, 'html.parser')
# Con etree.HTML lo convertimos en un objeto que permita leer este html parseado
# con comandos de XPath
x = etree.HTML(str(soup))
# Luego, le decimos que se salte todos hasta los tags td con clase celda-tabla_docs,
# posteriormente a la etiqueta a y luego tome el atributo href
href_list = x.xpath('//td[@class="celda-tabla_docs"]/a/@href')
```

### Tercer paso, recorrer la lista href
```python
# El paso anterior nos devuelve un listado, ahora podemos buscar el archivo
# xlsx en todos los atributos de href
xlsx_list = [i for i in href_list if '.xlsx' in i]
# le hacemos un print para que nos muestre que encontro
link = 'https://www.dane.gov.co'+xlsx_list[0]
print(link)
```

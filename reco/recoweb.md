<a name="recoweb"></a>
 ## Reconocimiento Web
 
 Con whatweb, podremos obtener información de las tecnologias empleadas en una web, como su CMS, si lleva javascript, correos electronicos asociados...
 ```
 whatweb xiaomi.com
 ```
 Con gobuster se puede tambier hacer fuzzing de directorios en una web
 ```
 gobuster dir -u http://xiaomi.com/ -t 50 -w <dicionario>
 ```
 también con wfuzz
 ```
 wfuzz -w <diccionario> http://xiaomi.com/FUZZ --hc=403,404
 
 ```
o buscar ficheros de la misma manera pero terminando con la extensión a buscar
 ```
  wfuzz -w <diccionario> http://xiaomi.com/FUZZ.html --hc=403,404
 ```
 se podría buscar por rango, por ejemplo:
 ```
 wfuzz -t 200 -z range.1-20000 'https://mi.com/shop/buy/detail?product_id=FUZZ'
 
 Aquí lo que estariamos haciendo es buscar ID's de producto (para el ejemplo) ya que en la web, este id es el que identifica a cada uno de los productos de la tienda. 
 
 ```
 Se pueden filtrar los campos,  por ejemplo mostrar o excluir los resultados que incluyan una palabra con --hw o --sw:
 
  ```
 wfuzz -t 200 --hw=6515 -z range,1-20000 'https://mi.com/shop/buy/detail?product_id=FUZZ'
 wfuzz -t 200 --sw=6515 -z range,1-20000 'https://mi.com/shop/buy/detail?product_id=FUZZ'
 
 ```
 luego tenemos la herramienta FUFF, que es muy potente y además super rápida, además si el codigo de estado es 301, dice a donde redirecciona, por ejemplo:
 ```
fuff -t 200 -w <diccionario> -u https://xiaomi.com/FUZZ -v
fuff -t 200 -w <diccionario> -u https://xiaomi.com/FUZZ --mc 200 -v
 con esto le decimos que el codigo de estado sea 200 y en modo verbose
 ```
 Dirsearch  
 Tiene su propio diccionario y basta con ejecutar el siguiente comando:  
 
 ```
dirsearch -u <URL>
 ```
 * HTTPS: mirar certificado SSL (a veces hay cositas en Common Name)  
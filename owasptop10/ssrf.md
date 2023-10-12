## Server-Side Request Forgery
SSRF es una vulnerabilidad que nos permite consultar información privilegiada de a través de una solicitud interna que realice el servidor a si mismo. Como por ejemplo consultar una pagina privada gracias a una request que haga el servidor a una API.  
![imagen](https://github.com/glmbxecurity/eJPT2_eCCPT2_eWPT_Notes/assets/137443771/a41b0085-0e1a-45dd-a49f-6520d3fa8db8)  

### Ejemplo 1  (Simple)

Acceso a un portal de administracion, aprovechando una consulta de stock a través de una API  
![image](https://github.com/glmbxecurity/eJPT2_eCCPT2_eWPT_Notes/assets/137443771/ab989ac6-a867-4b38-8b2a-a4637941b049)  

### Ejemplo 2  (Bypass filtro)

En esta ocasion tenemos un filtro que no nos permite acceder a ``` localhost ``` ni a ```127.0.0.1```. Pero si podríamos acceder a ```127.1``` ya que sería otra manera diferente de referenciar al equipo local.  
![image](https://github.com/glmbxecurity/eJPT2_eCCPT2_eWPT_Notes/assets/137443771/6aad3066-e924-4318-a7b0-302c4e626a04)  

### Ejemplo 3

Awui tenemos un filtro que nos limita a utilizar si o si la direccion ```stock.webapi..``` . Si se modifica la URL como en los ejemplos anteriores nos da error. con lo que al fijarnos en la direccion del método ``` GET ``` vemos que tenemos lo siguiente:  
![image](https://github.com/glmbxecurity/eJPT2_eCCPT2_eWPT_Notes/assets/137443771/d173370d-6fc7-44bf-8e17-e5f96d70c56f)  
Así que podemos probar a incluir la variable path al final de la api para dirigirnos a donde queremos ir (y estariamos cumpliendo la restriccion de que la consulta debe ser a stock,webapi...   
![image](https://github.com/glmbxecurity/eJPT2_eCCPT2_eWPT_Notes/assets/137443771/d94d5b4d-e748-4c55-a2e3-f796146aeefb)  

**IMPORTANTE** Luego URLencodear la request, sino el ``` & ``` dará conflicto. 







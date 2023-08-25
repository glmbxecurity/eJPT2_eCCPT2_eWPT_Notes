Remote file inclusion es una vulnerabilidad que nos va a permitir "cargar" o leer mejor dicho e intrepretar un fichero ajeno a la máquina victima.

en el ejemplo de Gwolle plugin para wordpress, en su versión 1.5.3 es vulnerable a RFI. Para ello debe tener también habilitado el url include.

con searchsploit vemos las instrucciones:
<img src="https://raw.githubusercontent.com/glmbxecurity/eJPT2_eCCPT2_eWPT_Notes/main/images/rfi1.png" /> 

Tan sencillo como montar un servidor http y alojar un fichero php:
```
python3 -m http.server 80
```

El fichero PHP:
<img src="https://raw.githubusercontent.com/glmbxecurity/eJPT2_eCCPT2_eWPT_Notes/main/images/rfi2.png" /> 

Y al reiniciar el servidor web, ponernos a la escucha con netcat y recargar la página, recibimos la petición y ese codigo es interpretado del lado de la víctima. Lo que podría derivar a una reverse shell

<img src="https://raw.githubusercontent.com/glmbxecurity/eJPT2_eCCPT2_eWPT_Notes/main/images/rfi3.png" /> 


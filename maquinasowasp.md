## Máquinas OWASP Top 10

### secDevLabs (Github project)
Repositorio con varios dockers para probar técnicas del OWASP Top 10  
**Link** <a href="https://github.com/globocom/secDevLabs">secDevLabs</a>  

### MyExpense
* XSS
* SQL Injection

#### Configuraciones previas
1º Adaptador en modo puente  
(Si no deja por error 0xc0000094, ir a: <a href="https://www.youtube.com/watch?v=-Xqgl4DnKG0"> Aqui </a>  
2º Inciar con tecla "e" para editar el arranque y cambar el "ro" final por: ```rw init=/bin/bash```
3º editar /etc/network/interfaces la interfaz por la que aparezca en el comando ``` ip a``` (en los 2 campos)
4º editar los 4 ficheros del directorio /opt. editando la URL, dejandola así: ```https://IPdelamaquina/```

**Link** <a href="https://www.vulnhub.com/entry/myexpense-1,405/">MyExpense machine (Vulnhub)</a>  



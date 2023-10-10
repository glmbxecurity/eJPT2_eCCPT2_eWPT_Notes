## MyExpense (vulnhub)
-------------------
Técnicas:  
* XSS
* SQL Injection

#### Configuraciones previas
1º Adaptador en modo puente  
(Si no deja por error 0xc0000094, ir a: <a href="https://www.youtube.com/watch?v=-Xqgl4DnKG0"> Aqui </a>  
2º Inciar con tecla "e" para editar el arranque y cambar el "ro" final por: ```rw init=/bin/bash```  
3º editar /etc/network/interfaces la interfaz por la que aparezca en el comando ``` ip a``` (en los 2 campos)  
 * Si en modo dhcp no funciona, darle una ip y mascara dentro de la misma red del kali

  
4º editar los 4 ficheros del directorio /opt. editando la URL, dejandola así: ```https://IPdelamaquina/```

<a href="https://www.vulnhub.com/entry/myexpense-1,405/">MyExpense</a>  

  
## XXELab
----------------
Técnicas:  
* XXE

<a href="https://github.com/jbarone/xxelab">XXELab (Vulnhub)</a> 

## RFI
---------------
En este caso tenemos un docker, al que se le debe instalar además el plugin Gwolle
[DVWP](https://github.com/vavkamil/dvwp) (Docker)
[Gwolle PLugin for WP](https://es.wordpress.org/plugins/gwolle-gb/)

# CSRF
-------------
Tenemos una especie de red social con varios usuarios, en la que podremos tratar de explotar el CSRF

[Web_CSRF_Elgg](https://seedsecuritylabs.org/Labs_20.04/Files/Web_CSRF_Elgg/Labsetup.zip)

nota: 
```
Si a la hora de hacer el ‘**docker-compose up -d**‘, os salta un error de tipo: “**networks.net-10.9.0.0 value Additional properties are not allowed (‘name’ was unexpected)**“, lo que tenéis que hacer es en el archivo ‘**docker-compose.yml**‘, borrar la línea número 41, la que pone “**name: net-10.9.0.0**“.
```


## Enumeración
En este apartado veremos técnicas y herramientas de enumeración de vulnerabilidades en ciertos servicios, aplicaciones y demás.  

### Indice
- [Sistemas Operativos](#sis)
- [SQL injection](#sqlinjection)
- [XSS](#xss)

<a name="sis"></a>
## Sistemas Operativos

**LINUX**  
En linux, una buena herramienta de enumeración es LSE (Linux Smart Enumeration). Aunque esto queda mas para post-explotación, no está demás incluirlo en este post.  
Para utilizarlo bastaría con tener acceso a la máquina, compartir el fichero (por ejemplo a traves de un wget, teniendo el fichero en un serv web montado a propósito). y lanzarlo.  

<a href="https://github.com/diego-treitos/linux-smart-enumeration">LSE Download link</a>

**WINDOWS**  
Pdte desarrollo.  







<a name="sqlinjection"></a>
## SQL Injection
### Manera MANUAL
Manera de identificar que puede haber inyección, es que tenemos algunas tablas, o algun indicio que nos lleve a pensar que hay datos que se están consultando en una BBDD, como un inicio de sesión y en la URL veamos el tipico id=1 o similar.
se pueden probar las siguientes opciones:  
```
'ORDER BY 1-- -
'OR 2=1-- -
'sleep(5)-- -
```


### Con Herramientas
Pdte desarrollo.  






<a name="xss"></a>
## XSS
### Manera MANUAL
se puede probar a inyectar el siguiente código en cualquier formulario: 
```
<script>alert('XSS')</script>
```
Si aparece el cuadro de alerta, sería vulnerable en un principio.

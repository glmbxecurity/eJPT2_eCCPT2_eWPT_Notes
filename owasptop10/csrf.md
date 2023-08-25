  # CSRF
Es una vulnerabilidad en la que un atacante consigue que un usuario legítimo de un sitio web ejecute una acción sin su conocimiento/consentimiento.

## Ejemplo

Suponemos que estamos en un sitio web, cualquera, en la que tenemos usuarios registrados. Con datos de perfil, incluso puede ser un foro en el que haya envío de mensajes, etc. 

Para el ejemplo, haremos un cambio no autorizado de nombre de usuario a una víctima

Cuando se hace una petición legítima para un envío de datos en una web, lo normal es hacerlo mediante el método POST. 

Lo curioso es que si esa petición la realizamos por GET, se envía de una forma totalmente diferente, con la peculiaridad que se hace a través de una URL (en la url se definen los cambios, en este caso el de contraseña).
## Interceptación de la petición

Primero debemos tomar la manera en que esta web en concreto realiza esta petición. 

<img src="https://raw.githubusercontent.com/glmbxecurity/eJPT2_eCCPT2_eWPT_Notes/main/images/csrf1.png" />

Si cambiamos el método de envío a GET, queda de la siguiente manera:
<img src="https://raw.githubusercontent.com/glmbxecurity/eJPT2_eCCPT2_eWPT_Notes/main/images/csrf2.png" />
Si nos fijamos al final de todo, aparece un guid, que tiene pinta de ser el ID de usuario. Si conseguimos averiguar el ID de la víctima (visitando su perfil o intentando cualquier interacción, haciendo hovering con el ratón y viendo la URL, u otra manera). Podríamos modificar este GUID por el suyo propio e intentar hacer que la propia víctima ejecute esta acción. Ya que como es lógico solo el propio usuario debería ser capaz de cambiar sus propios datos.

## Envío de la petición

En este caso en concreto, vemos un formulario para envío de mensajes. cosa que podríamos aprovechar para inyectar este código en el mensaje (Ya que admite HTML). de manera que cuando la víctima abra el mensaje, por detrás y sin que se de cuenta está tramitando esta petición.

Lo vamos a camuflar en una imagen de la siguiente manera:
``` 
<img src="Enlace a inyectar" alt="Texto a mostrar para no levantar sospechas" height="1" width="1" />
```

El enlace a inyectar sería:

```
http://ip_web/action/profile....guid=56
```

finalmente queda:
```
<img src="http://ip_web/action/profile....guid=56" alt="Hola" height="1" width="1" />
```

# Extra
Hay campos que no son necesarios enviar en un POST/GET para que funcione la petición. Por ejemplo en un cambio de contraseña es posible que tengamos que poner contraseña actual y la nueva, pero igual si eliminamos esa parte del código de contraseña actual (si está mal programada la web), podríamos llegar a saltarnos esto y cambiar una contraseña.

## Authentication bypass
### Bruteforce  

Teniendo una lista de usuarios válidos [Explicación](https://github.com/glmbxecurity/eJPT2_eCCPT2_eWPT_Notes/blob/main/reco/recousersweb.md), podemos intentar fuerza bruta a las contraseñas.  
```ffuf -w valid_usernames.txt:W1,/usr/share/wordlists/SecLists/Passwords/Common-Credentials/10-million-password-list-top-100.txt:W2 -X POST -d "username=W1&password=W2" -H "Content-Type: application/x-www-form-urlencoded" -u http://10.10.144.67/customers/login -fc 200 ```
De la misma manera, se debe interceptar la peticion por burpsuite y analizar que campos debemos modificar para que funcione el ataque.  

### Fallos en el código

#### Ejemplo 1 (facil y poco probable)
Si para acceder al panel de admin, nos encontramos que el código tiene esto:   
``` if( url.substr(0,6) === '/admin') {
    # Code to check user is an admin
} else {
    # View Page
}
```

=== admin, significa que al ser escrito tal cual, no dejará entrar. sin embargo si escribimos /aDMin (por ejemplo) podriamos colarnos al panel de administración sin credenciales.  

#### Ejemplo 2 (Envío de reseteo de contraseña a un email tercero)  

En el campo de reseteo de contraseña, es posible que nos pida solamente el email, o el usuario, o ambas. pero si tenemos esta información, se puede intentar redirigir este email. Al introducir el email, vemos que hace una petición por GET para consultar la existencia del email. 
![imagen](https://github.com/glmbxecurity/eJPT2_eCCPT2_eWPT_Notes/assets/137443771/e00e5a02-20a6-4aec-bc6a-abd2900c19ff)
![imagen](https://github.com/glmbxecurity/eJPT2_eCCPT2_eWPT_Notes/assets/137443771/1ac1ed8f-f8a2-46cc-9f90-5690137ce5f5)

Al enviar la petición, nos pide el usuario asociado, y al interceptar vemos lo siguiente:  Pasa a ser una petición por POST, que podemos intentar trampear el email en la petición.
![imagen](https://github.com/glmbxecurity/eJPT2_eCCPT2_eWPT_Notes/assets/137443771/e71ae211-1701-4a51-b9a3-acec74b28b36)
![imagen](https://github.com/glmbxecurity/eJPT2_eCCPT2_eWPT_Notes/assets/137443771/c7b89486-1df7-4076-bf35-5ee97404d7c3)

El trampeo queda de la siguiente manera:  
![imagen](https://github.com/glmbxecurity/eJPT2_eCCPT2_eWPT_Notes/assets/137443771/03cb3dab-2a49-430a-a7ef-fd1f1190268c)  
![imagen](https://github.com/glmbxecurity/eJPT2_eCCPT2_eWPT_Notes/assets/137443771/6839ffe9-0fe8-4454-97d2-b64ac943723e)  











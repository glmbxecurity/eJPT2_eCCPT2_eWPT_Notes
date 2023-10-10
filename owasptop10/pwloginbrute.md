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

En el campo de reseteo de contraseña, es posible que nos pida solamente el email, o el usuario, o ambas. pero si tenemos esta información, se puede intentar redirigir este email.




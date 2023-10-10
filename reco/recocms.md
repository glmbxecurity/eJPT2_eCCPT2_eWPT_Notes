   <a name="recowp"></a>
## Reconocimiento Wordpress

 - Rutas importantes:  
 
   *wp-admin
   *wp-login
   *wp-content/plugins
   *xmlrpc.php
   
 
 Si en la página encontramos algún post, suele venir con el nombre del usuario que lo creó. ahí ya hay una pista
 
 Con wpscan, enumeramos version, plugins, etc
 ```
 wpscan --url 10.10.5.3 (scan basico)
 wpscan --url 10.10.5.3 -e vp,u (enumera plugins vulnerables y usuarios)
 wpscan -hh (ver todas las opciones)
 ```
 
 Con Wpscan API token se busca activamente en la web de WPScan. Basta con registrarse y obtener el token. luego:
 ```
 wpscan --url 10.12.43.5 -e vp,u --api-token <token>
 ```
 
 con curl, podemos intentar ver el codigo fuente en búsuqeda de plugins
 ```
 curl -s -X GET "url" | grep -oP 'plugins/\K.*' | sort -u
 
 se puede combinar con searchsploit
 ```
  <a name="recojoom"></a>
## Reconocimiento Joomla!
 ```
 perl joomscan.pl -u <url>
 ```
   <a name="recodru"></a>
## Reconocimiento Drupal
 ```
whatweb http://172.17.0.2
droopescan scan drupal --url 172.17.0.2
 
 ```
    <a name="recomag"></a>
## Reconocimiento Magento
 ```
php magescan.phar scan:all 172.17.0.2
```
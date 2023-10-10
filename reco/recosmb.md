   <a name="recosmb"></a>
## Reconocimiento SMB
Ver recursos compartidos:
 ```
 smbclient -L 172.17.0.2 -N
 smbmap -H 172.17.0.2
 enum4linux -a (escaneo simple pero completo) para mas opciones el manual
 ```
 * La opcion -N en smbclient se utiliza cuando no se tienen credenciales de acceso, es una sesión NULA
 
 
 Si nos encontramos con muchas carpetas, sería interesante hacer un tree. pero en la maquina victima no se puede hacer. con lo que instalamos cifs y montamos el directorio remoto en nuestro equipo y hacemos un tree de la siguiente manera:
 ```mount -t cifs //172.17.0.2/myshare /mnt/eddy```
 
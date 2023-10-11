# IDOR 
Esta es una vulnerabilidad que nos permite acceder a información en una web a la que a priori no deberiamos tener acceso. Por ejemplo a un perfil ajeno, o a datos de un perfil ajeno.  
El primer ejemplo sencillo para comprender esta vulnerabilidad es cuando en la URL vemos ```http://storeprueba.com/profile?id=1``` Nos percatamos de un ID de perfil. Si probamos otros id's como 2, 3, 4 o así. Es posible que podamos acceder a la visualización de este perfil.  

## Ejemplo 1
Tenemos nuestro perfil, donde nuestros datos vienen "rellenados" por defecto. Esta información la debe de coger de algún sitio. Investigamos las peticiones de red y vemos que hace una llamada a una API en JSON  
![imagen](https://github.com/glmbxecurity/eJPT2_eCCPT2_eWPT_Notes/assets/137443771/cd003534-ce2c-4e31-9cdb-3df7f2e7fa02)   
![imagen](https://github.com/glmbxecurity/eJPT2_eCCPT2_eWPT_Notes/assets/137443771/92cd2a40-1eeb-4411-91ff-e97835e2e023)  

Seguimos la URL y nos lleva a:  
![imagen](https://github.com/glmbxecurity/eJPT2_eCCPT2_eWPT_Notes/assets/137443771/8100f1dd-ef02-40f1-bf74-8f90f7c73166)  

Si cambiamos el ID en la URL ya estamos consultando datos privilegiados de un usuario:  
![imagen](https://github.com/glmbxecurity/eJPT2_eCCPT2_eWPT_Notes/assets/137443771/c1f0b5c1-cb22-4029-90a8-803ad6f21224)




## Nmap
<img align="center" src="https://raw.githubusercontent.com/glmbxecurity/assets/main/nmap.jpg" />
Herramienta básica y potente para realizar los escaneos. la sintaxis principal del progama es la siguiente:  

~~~
nmap <ip> <parametros>
~~~  

Los parámetros mas útiles son los siguientes:  
* -p1-100 (puertos del 1 al 100)
* -p- (todos los puertos)
* --top-ports 100 (Los 100 puertos mas comunes [en este ejemplo])
* -n (para evitar resolucion DNS)
* -T (del 0 al 5. Indica la velocidad, y por consiguiente la agresividad del escaneo)(un numero mayor puede ser detectado por firewalls)
* -sU (puertos UDP)
* -sV (version del servicio)
* -sn (detectar equipos en la red, mac, y marca)  
* --min-rate 5000 (agiliza bastante el escaneo, no se recomienda añadir mas de 5000)
<a name="evasion_nmap"></a>

## Evasion de Firewall con Nmap
Se puede intentar evitar el bloqueo por parte del firewall al intento de escaneo de puertos. para ello algo muy común es el hecho de fragmentar los paquetes, o indicar el tamaño maáximo de MTU (tamaño máximo de transimision de unidad), o falsear ciertos datos.  

* -f (fragmentar paquetes)
* --mtu <numero multiplo de 8>
* -D <ip> (falsificar ip de origen)
* --source port <puerto> (falsificar puerto de origen)
* --data-length <numero> (modificar tamaño de paquete, sumando ese numero al tamaño original del paquete)
* --spoof-mac <mac> (falsear la mac)  
   
 El falseo de IP y MAC, puede ser útil si se combina con el siguiente comando, que nos enumera los dispositivos con su IP y MAC que están conectados y activos en la red:  
 
 ~~~
 arp-scan -I <interfaz> --localnet
 ~~~  
   
 * -sS (tratar de no dejar evidencias en Firewall/IDS), explicación:  
   Al realizar un escaneo, el atacante envía un SYN, recibe un SYN/ACK, y vuelve a responder con un ACK. Aquí terminaría la conexión. En caso de estar el puerto cerrado o la conexion ser rechazada, recibiriamos un RST en lugar del SYN/ACK.  
 Pues bien, con "-sS", lo que hacemos es después de recibir el SYN/ACK, enviamos un RST. Ya que algunos Firewall, solo registran conexiones completas, y al no enviar el ACK, no lo registran en sus logs.
 
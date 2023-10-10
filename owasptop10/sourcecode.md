# Codigo fuente

En el código fuente se pueden investigar:  
* Comentarios, entre ```<!-- -->```
* Enlaces ocultos
* Contenido bloqueado por CSS
* Parar ejecución JS
* Inspeccionar Red

### Contenido bloqueado por CSS

Inspeccionanddo el codigo fuente, si nos damos con el caso de contenido bloqueado. Podríamos cambiar "block" por "none"
<img src="https://raw.githubusercontent.com/glmbxecurity/eJPT2_eCCPT2_eWPT_Notes/main/images/cssblock.jpg"/> 

## Parar ejecución código JS

Inspeccionando, en el modo depurador. si formateamos el código para que se vea línea a línea. Clicando en la línea que nos interese y luego recargando página, podemos cortar la ejecución de esa parte en javascript e intentar ver algo privilegiado en la web.  
<img src="https://raw.githubusercontent.com/glmbxecurity/eJPT2_eCCPT2_eWPT_Notes/main/images/jsblock.jpg"/>

## Inspeccion de red

Aunque esto se puede realizar a través de Burpsuite, también desde la pestaña Network. Por ejemplo al envío de un formulario, se ve la petición. A la cual podemos copiar la URL y ver hacia donde la está dirigiendo.
![imagen](https://github.com/glmbxecurity/eJPT2_eCCPT2_eWPT_Notes/assets/137443771/e1cad64c-bd16-49d9-b050-e79d06514f30)

![imagen](https://github.com/glmbxecurity/eJPT2_eCCPT2_eWPT_Notes/assets/137443771/49be1729-b7ba-4751-bfe2-0ab1e3e4b7e0)



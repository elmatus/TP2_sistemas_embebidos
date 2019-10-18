# TP2_sistemas_embebidos

- Inicialmente se modifica el archivo project.mk del proyecto firmware_v2.
  - Línea a modificar: ```PROJECT = projects/TP2/TP2_sistemas_embebidos```
  - comentar la línea que estaba descomentada.
  
# Punto 1:

- Copio el archivo "Blink.-sct" en la carpeta "gen" y lo renombro como "prefix.sct".
  - Aclaración: Tener un único archivo .sct en todo el proyecto.
- Modifico el archivo "prefix.sgen" contenido en la carpeta "gen"
  -	```targetFolder = "projects/TP2_sistemas_embebidos/gen"```
  - ```libraryTargetFolder = "projects/TP2_sistemas_embebidos/gen"```
- Click derecho en prefix.sgen -> Generate Code Artifacts
  - Si aparece el siguiente cartel de error hacer Clean Proyect, luego Build Proyect y realizar nuevamente el paso descripto arriba.
  
  ![](https://github.com/elmatus/TP2_sistemas_embebidos/blob/master/images/error_prefix_sgen.PNG)
  
  - Si se genera únicamente los archivos Prefix.h y sc_types.h cerrar el MCUXpresso y abrirlo nuevamente. Luego generar nuevamente los archivos
  
- Para correr el código correspondiente a Blinky.-sct en main.c se deberá tener definido lo siguiente: 

  ![](https://github.com/elmatus/TP2_sistemas_embebidos/blob/master/images/punto1_TimeEvents.PNG)
  
- Finalmente realizar Clean Project -> Build Project -> Debug (Como se explica en el TP1). 
  
- En el main primero se realizan inicializaciones, con funciones utilizadas en el TP1. Tener en cuenta que ```__USE_TIME_EVENTS == false```
  en este caso. 
  
  ![](https://github.com/elmatus/TP2_sistemas_embebidos/blob/master/images/punto1_main_init.PNG)
  
- Luego entra al bucle que se ejecuta siempre:

  ![](https://github.com/elmatus/TP2_sistemas_embebidos/blob/master/images/punto1_while.PNG)
  
- En este caso se ejecuta la línea ```prefixIface_raise_evTick(&statechart);```
  
  ![](https://github.com/elmatus/TP2_sistemas_embebidos/blob/master/images/punto1_raise_evTick.PNG)
  

- Luego se procede a usar Time Events, eliminando "prefix.sct", luego se copia "BlinkTimeEvent.-sct" en la carpeta "gen" y se lo
renombra, como se hizo anteriormente. Ahora hay que definir en el main-c ```#define __USE_TIME_EVENTS (true)```.

- Si se quiere cambiar de LED se modifica el archivo "prefix.sct". Si por ejemplo se quiere prender el led rojo del led rgb se deberá descomentar la línea ```const LEDR: integer = 0``` y se deberá modificar el primer argumento de la función opLED() por ```LEDR```. 

  ![](https://github.com/elmatus/TP2_sistemas_embebidos/blob/master/images/punto1_yakindu.PNG)

- Si hay problemas al ejecutar un programa utilizando Yakindu comprobar las prioridades. Para eso hacer click derecho en el cuadro donde se definen constantes, eventos, etc. -> Show Properties View -> Region Priority y ahí colocar las regiones en orden, de acuerdo como vaya ejecutandose el programa. 




# Punto 2


# Punto 3


# Punto 4


# Punto 5


# Punto 6


# Punto 7

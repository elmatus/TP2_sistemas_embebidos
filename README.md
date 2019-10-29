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
- El archivo "prefix.sgen" tiene lo que se muestra a continuación:
![](https://github.com/elmatus/TP2_sistemas_embebidos/blob/master/images/sgen.png)
- Click derecho en prefix.sgen -> Generate Code Artifacts como se muestra en la siguiente figura:
![](https://github.com/elmatus/TP2_sistemas_embebidos/blob/master/images/sgen-generateCode.png)
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

- Si aparece el siguiente cartel de error al hacer Build Project:

  ![](https://github.com/elmatus/TP2_sistemas_embebidos/blob/master/images/error_build_project.jpeg)

  Hacer Close Project y luego Build Project

# Punto 2


# Punto 3


# Punto 4


# Punto 5


# Punto 6


# Punto 7

- En este punto se simulo la maquina de estados de un microondas, donde se tiene 3 estados de cocción, un botón de arranque o terminar operación, un sensor donde se abrió la puerta y por ultimo un botón para cancelar el modo en que esta. En las siguientes imagenes se observa las maquinas de estados:

![](https://github.com/elmatus/TP2_sistemas_embebidos/blob/master/images/microondas/main-region.png)
![](https://github.com/elmatus/TP2_sistemas_embebidos/blob/master/images/microondas/arranque-terminar.png)
![](https://github.com/elmatus/TP2_sistemas_embebidos/blob/master/images/microondas/modos.png)
![](https://github.com/elmatus/TP2_sistemas_embebidos/blob/master/images/microondas/tecx.png)

- Otra cosa importante para el programa de Yakindu es el prefix que presenta las definiciones de las maquina de estado, como LED_ON o siModo. El prefix es el siguiente:

![](https://github.com/elmatus/TP2_sistemas_embebidos/blob/master/images/microondas/prefix.png)

- Mirando los diagramas nos damos cuenta que se puede parar el microondas cuando se comenzó la operación utilizando el botón de arranque/terminar,  por otro lado si se abrió la puerta automáticamente el microondas se detiene. También se observa que el diagrama de estados permite que el microondas arranque cuando no hay ningún modo.

- El funcionamiento del programa es simple de entender. En el LED RGB que presenta la EDU-CIAA se setea el modo de cocción a través de la tecla TEC1, luego se puede arrancar el microondas apretando la tecla TEC2 y con la misma tecla se puede parar el procedimiento. Para simular el sensor de que la puerta esta abierta se utilizo el botón TEC3 que cuando se aprieta indica que se abrió la puerta y termina el proceso de cocción establecido (si había uno), por otro lado, se configura la tecla TEC4 para borrar el modo de cocción.

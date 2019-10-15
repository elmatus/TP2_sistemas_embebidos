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
  
  ![](https://github.com/Hitalio/TP1/blob/master/images/error_prefix_sgen.PNG)
  
- Para correr el código correspondiente a Blinky.-sct en main.c se deberá tener definido lo siguiente: 

  ![](https://github.com/Hitalio/TP1/blob/master/images/punto1_TimeEvents.PNG)
  
- Finalmente realizar Clean Project -> Build Project -> Debug (Como se explica en el TP1). 
  
- En el main primero se realizan inicializaciones, con funciones utilizadas en el TP1. Tener en cuenta que ```__USE_TIME_EVENTS == false```
  en este caso. 
  
  ![](https://github.com/Hitalio/TP1/blob/master/images/punto1_main_init.PNG)
  
- Luego entra al bucle que se ejecuta siempre:

  ![](https://github.com/Hitalio/TP1/blob/master/images/punto1_while.PNG)
  
- En este caso se ejecuta la línea ```prefixIface_raise_evTick(&statechart);```
  
  ![](https://github.com/Hitalio/TP1/blob/master/images/punto1_raise_evTick.PNG)
  

- Luego se procede a usar Time Events, eliminando "prefix.sct", luego se copia "BlinkTimeEvent.-sct" en la carpeta "gen" y se lo
renombra, como se hizo anteriormente. 



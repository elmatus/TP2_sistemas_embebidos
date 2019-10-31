# TP2_sistemas_embebidos

El propósito del siguiente trabajo práctico es implementar una máquina de estados mediante la herramienta Yakindu, de modo que la codificación del programa se realice mediante bloques y no lineas de código.
Una ventaja del uso de Yakindu, es que permite simular los programas y observar como se van modificando las variables, permitiendo también observar errores o mejoras antes de implementarlo en la placa EDU-CIAA.

Por ello se implementó un único archivo .c y se realizaron distintas aplicaciones con máquinas de estado. Como por ejemplo, un generador de funciones, un portón, una escalera, entre otros.


- Inicialmente se modifica el archivo project.mk del proyecto firmware_v2.
  - Línea a modificar: ```PROJECT = projects/TP2/TP2_sistemas_embebidos```
  - comentar la línea que estaba descomentada.



Se debe modificar la macro TEST (#define TEST (SCT_X)) en main.c, dependiendo del ejercicio a resolver, de este modo, la maquina de estados operará con las distintas secciones del código

![](https://github.com/elmatus/TP2_sistemas_embebidos/blob/master/images/sct_.jpg)

  
# Punto 1:

El siguiente programa corresponde a hacer titilar un LED.

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
  
- Con la función prefix_init() se realiza la inicialización del diagrama de estados. A continuación se muestra la implementación de esta función, donde se inicializan los datos y los eventos:

  ![](https://github.com/elmatus/TP2_sistemas_embebidos/blob/master/images/punto1_prefix_init.PNG)

- Además con la función prefix_enter() se setea la secuancia del diagrama por defecto. 
  
  
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

Se probaron los ejemplos ya existentes realizando los pasos explicados en el punto 1 y corroboró su correcto funcionamiento tanto simuladolos mediante Yakindu como bajando los programas a la EDU-CIAA.

# Punto 3

 -  El siguiente programa se basa en simular un generador de funciones, con 3 formas de onda (cuadrada, senoidal, triangular o sin señal) con sus respectivas magnitudes (amplitud y frecuencia). El programa también considera cuando la persona quiere cambiar algunas de las magnitudes, es decir aumentar o disminuir la amplitud o la frecuencia. Los bloques del diagrama de estados se presentan a continuación:

  - Bloque para validar tecla presionada: \
 ![](https://github.com/elmatus/TP2_sistemas_embebidos/blob/master/images/generadorFunciones/tecx.png)

 - Bloque de espera (Idle): \
 ![](https://github.com/elmatus/TP2_sistemas_embebidos/blob/master/images/generadorFunciones/main-region.png)
 
 - Bloque para seleccionar la forma de onda: \
 ![](https://github.com/elmatus/TP2_sistemas_embebidos/blob/master/images/generadorFunciones/formaDeOnda.png)

- Bloque para seleccionar la magnitud de la forma de onda, es decir amplitud/tensión pico y frecuencia: \
 ![](https://github.com/elmatus/TP2_sistemas_embebidos/blob/master/images/generadorFunciones/magnitud.png)
 
 - Bloques para aumentar o disminuir la magnitud: \
 ![](https://github.com/elmatus/TP2_sistemas_embebidos/blob/master/images/generadorFunciones/down-magnitud.png)
 ![](https://github.com/elmatus/TP2_sistemas_embebidos/blob/master/images/generadorFunciones/up-magnitud.png)
 
- El prefix es el siguiente, es decir donde se declaran las constantes y eventos: \
  ![](https://github.com/elmatus/TP2_sistemas_embebidos/blob/master/images/generadorFunciones/prefix.png)
  
 - Una validación hecha es que si no selecciono ni magnitud o frecuencia no se puede aumentar o disminuir la misma.
  
- El funcionamiento de este programa consiste en setear el LED RGB como la forma de onda que uno quiere utilizar, esto se logra con la tecla TEC1. Luego, con la tecla TEC2 se puede configurar que se quiere setear (magnitud o fase, dependiendo de que quiera configurar el usuario) donde el usuario puede ver si se prende el LED1 o LED2 indicando lo que va a setear (LED1 para la amplitud y LED2 para la frecuencia). Por último, para disminuir el valor o aumentarlo se utilizan las teclas TEC3 y TEC4, respectivamente. Cuando el usuario subió o bajó el valor de amplitud o frecuencia se prenderá por un instante (500ms) el LED4. 


# Punto 4


# Punto 5

En el siguiente programa se implementa el control de un portón de cochera, el cuál cuenta con un motor con dos sentidos, control de apertura y cierre, fines de carrera y señalización luminosa. A continuación se presentan los bloques correspodientes al digrama de estados:

- Bloque correspondiente a la definición de constantes, eventos y operaciones: \ 
![](https://github.com/elmatus/TP2_sistemas_embebidos/blob/master/images/porton/prefix.PNG)

- Bloque para validar las teclas: \
![](https://github.com/elmatus/TP2_sistemas_embebidos/blob/master/images/porton/tecx.PNG)

- Bloque de espera del programa o idle: \
![](https://github.com/elmatus/TP2_sistemas_embebidos/blob/master/images/porton/idle.PNG)

- Bloque que indica la lógica del portón: \
![](https://github.com/elmatus/TP2_sistemas_embebidos/blob/master/images/porton/porton.PNG)

- Bloque para hacer parpadear un led: \
![](https://github.com/elmatus/TP2_sistemas_embebidos/blob/master/images/porton/led.PNG)

- El programa comenzará con el portón cerrado y luego estará esperando al evento de presionar una tecla. Si se presiona la tecla TEC1 se abrira el portón. Mientras se está abriendo titilará el LED3 hasta que sense el fin de carrera del portón abierto, representado por TEC 3. Luego se prenderá el LED1 para indicar que el portón se encuentra abierto. Si se presiona la tecla TEC2 se cerrará el portón y se realiza una secuencia similar al abrir el portón. Comenzará a titilar el LED3 hasta que se lo indique el fin de carrera de portón cerrado, representado por TEC4 y posteriormente se apagará el LED1 para indicar que el portón se encuentra cerrado. Cabe aclarar que si se presiona la tecla TEC2 al estar abriendo el portón, una vez que llegue al fin de carrera del portón cerrado, este volverá a la posición de cerrado. Análogamente ocurrirá una situación similar al estar cerrando el portón. 

# Punto 6

En el siguiente programa se implementa el control de una escalera mecánica, la misma presenta un motor con un único sentido, dos velocidades, sensores de ingreso  y egreso, tambíen tieneseñalización luminosa.

- Bloque correspondiente a la definición de constantes, eventos y operaciones: \ 
![](https://github.com/elmatus/TP2_sistemas_embebidos/blob/master/images/escalera/prefix.jpg)

- Bloque de espera del programa o idle: \
![](https://github.com/elmatus/TP2_sistemas_embebidos/blob/master/images/escalera/idle.jpg)

- Bloque que indica la lógica de la escalera\
![](https://github.com/elmatus/TP2_sistemas_embebidos/blob/master/images/escalera/escalera.jpg)

- Bloque para hacer parpadear un led con dos velocidades diferentes: \
![](https://github.com/elmatus/TP2_sistemas_embebidos/blob/master/images/escalera/titilar_LED.jpg)

- El programa comenzará con la escalera sin ocuapción, esto se indicará mediante un LED que titila a cierta velocidad de 1s.
Luego, mediante un sensor de ingreso, ubicado en el boton 1, se detecta que una persona ingresa a la escalera, visualmente esto se identifica con el mismo LED pero titilando a una velocidad mayor (250ms).
Finalmente, mediante el botón 2, se detecta el egreso de la persona, por lo que el LED vuelve a la posicón inicial, titilando a una velocidad de 1s.

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

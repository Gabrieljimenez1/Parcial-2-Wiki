# Parcial-2-Wiki

## Introducción
El presente proyecto tiene como objetivo automatizar el proceso de preparación de café en máquinas dispensadoras industriales, actualmente operadas de manera manual por la empresa Café & Máquinas S.A. Esta automatización busca mejorar la eficiencia operativa, garantizar un control preciso del inventario y permitir una experiencia más segura y confiable para el usuario final.

Para ello, se diseñó e implementó un sistema automático basado en tecnologías de control industrial, utilizando PLC y sensores, junto con una interfaz gráfica HMI desarrollada en CODESYS. El proceso fue validado a través de simulación por tiempo y a traves de sensores el cual fue programado en OpenPLC, que responde a eventos reales mediante sensores conectados a entradas digitales. 

## Levantamiento de requerimientos
Para el desarrollo de este proyecto, basados en el enunciado por el profesor, se levantan los siguientes items. <br>

**Requisitos funcionales:**
* Automizar el proceso de preparación de café, eliminando la necesiadad de poner el cafe y el agua en cada ciclo.
* Interfaz HMI para iniciar/parar el proceso.
* Sensores de nivel de café y agua.
* Sensores de presencia de vaso.
* Contabilización de vasos dispensados.
* Validación con prototipo físico utilizando OpenPLC. <br>

**Requisitos no funcionales:**
* Simulación en CODESYS.
* Validación del prototipo físico.


## Diseño de las solución

![Definicion_Variables](Imagenes_Videos/Variables.png)


## Desarrollo de la implementación

### Programación en CODESYS:
En la documentación del Codesys se explica cada una de las redes creadas y las variables utilizadas, este enfoque se centra en una aproximación de proceso controlado por tiempo.

![Codesys1](Imagenes_Videos/Codesys1.jpg)
![Codesys2](Imagenes_Videos/Codesys2.jpg)
![Codesys3](Imagenes_Videos/Codesys3.jpg)
![Codesys4](Imagenes_Videos/Codesys4.jpg)
![Codesys5](Imagenes_Videos/Codesys5.jpg)
![Codesys6](Imagenes_Videos/Codesys6.jpg)
![Codesys7](Imagenes_Videos/Codesys7.jpg)


### Programación en OPENPLC:
En este programa se implementó la simulación más cercana que se puede a lo que se espera que suceda en el mundo real, es por esta razón que se buscó diseñar el programa de tal forma que ya tuviera todas las implementaciones físicas que son particulares de la solución, es decir, lo ya mencionado anteriormente como lo son sensores, sensor infrarrojo, entre otros componentes. Esto mediante principalmente de la capacidad de integrar código en C++ con la programación Ladder del programa OPENCL, de esta manera, se implementó código buscando el funcionamiento adecuado del proceso implementado, el ejemplo más claro de este diseño fue la implementación de código para poder mostrar el dato de la cantidad de cafés servidos en un Display LCD, el cual necesita de una configuración en código para poder funcionar de manera adecuada. Este código se aplico y se trato de aplicar, sin embargo, debido a un mal funcionamiento con las librerias que se trataron de implementar. Entonces para este punto se dejo una salida simple, donde se buscara colocar un led que indique cada vez que llegue una nueva señal indicando que se conto un cafe nuevo.

Además de este caso donde se necesita usar código, el diseño es básicamente el mismo que se puede observar en CODESYS, utilizando el mismo tipo de programación visual, basada en contactos y bobinas, y también agregando los bloques de funciones estándar como lo son los TON, y el CTU utilizados en el desarrollo del proceso, ahora, se demostraran las variables de entrada y salida que se usaron para crear el proceso dentro del programa:

![Variables OPEN](Imagenes_Videos/Distribucion_Ent_Sal_OPEN.png)

Luego de tener definidas estas variables, y como se puede observar en la imagen, se le asigno un valor para su ubicación lógica en OPENPLC, lo que significa que esa variable tiene un valor especifico en los pines del Arduino al momento de programar el Arduino con este programa creado, de igual manera, se puede observar a que pin físico corresponde cada entrada o salida de este proceso. 

Despues de haber definido estas variables principales se continuo realizando el resto de la programación del proceso en logica ladder, como ya se habia comentado en la seccion de CODESYS, para finalmente dar con la arquitectura presentada a continuacion, la cual ya tiene incluido los timers que se usarán y de igual manera el contador encargado de .

![Arq_OPEN1](Imagenes_Videos/Arquitectura1.png)
![Arq_OPEN2](Imagenes_Videos/Arquitectura2.png)

## Validación de funcionamiento



Como primer acercamiento a validar el correcto funcionamiento del programa, usando OPENPLC, se comenzaron a hacer pruebas de simulación desde la visualización, ya que se quería comprobar que lo implementado en CODESYS funcionára de manera correcta. Esta simulación cumple un papel fundamental en el proceso de diseño ingenieril, ya que permite evaluar el comportamiento de cada etapa del proceso bajo condiciones controladas, sin necesidad de contar inicialmente con todos los componentes físicos. Para esto se utilizaron botones y leds que utilizaban las variables que ya habiamos asignado, además de negar algunas para poder utilizarlo con función de invisible, que lo mantiene mientras este en TRUE. 

En las primeras pruebas, aun sin usar el código para convertir la señal de la salida del contador a algo entendible por el Display LCD, se encontró un error en la programación, el cual consiste en el pin "PV" del CTU, ya que originalmente se tenía un valor de 1, para que el CTU lo contara como satisfactorio y avanzara con la lógica, para así poder activar el reinicio de "Valve_Out", sin embargo, esta disposición no lograba permitir que el CTU contara más allá de 1, por lo cual se decidió por dar un valor más alto, casi imposible para una cafetera de este tipo, para así evitar el problema ya mencionado, el valor elegido fue 1000.

Otra de las validaciones que se realizaron y produjeron cambios en la forma en la que se simulaba el proceso, sucedió cuando al simular, se hizo evidente que la variable "Sensor_Cup" iniciaba con un valor de "True" lo cual no permitía iniciar el proceso debido a la lógica de programación Ladder que se siguió, ya que esta variable es de naturaleza cerrada, por lo cual, se tuvo que hacer necesario que cada vez que se iniciara una simulación se forzara el estado en falso de esa variable.

También cabe destacar, que las primeras veces simulo el proceso después de haber implementado el código en C++ para el funcionamiento de un Display LCD externo para reportar el conteo de días, este código, alojado en su archivo separado de texto se borraba cada vez que se simulaba, o se sale del programa, esta complicación después dejo de presentarse cuando se dejo el comentario incial con el que cuenta ese archivo, sin embargo, este arreglo no sirvio de mucho, porque como ya se menciono antes, el Display LCD se decidio por no colocar en el montaje final, ya que se presenta un problema con la libreria a importar, el cual no se pudo corregir para un funcionamiento satisfactorio. 


*Hablar del montaje y funcionamiento en fisico, cuando ya exista*

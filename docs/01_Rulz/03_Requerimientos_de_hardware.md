## 1 - Introducción.
En este documento se encuentran explicados los requisitos básicos recomendados para correr los diferentes módulos de LuziaRulz. También se encontrarán algunas de las distintas pruebas que se realizaron sobre los módulos.

## 2- Descripción de hardware mínimo para Frontend y Backend de Reglas.
Para un máximo de 15 usuarios simultáneos operando sobre reglas, simulaciones, entidades, etc dentro de la aplicación ARG2 se requiere.

Procesador: Intel® Xeon® Processor E3-1220L o equivalente.
Memoria Ram: 4GB.
Almacenamiento: 250GB*.

*  Nota: 250GB Es más que suficiente para un universo de reglas acotado. Si se requiriese más espacio es probable que se requiera mejorar el procesador y la memoria

## 3- Descripción de hardware mínimo para Runtime / Motor de reglas.
Según las pruebas realizadas determinamos que existen dos posibles configuraciones de hardware:
### 3.1. Evaluación en batch.
Para un máximo de 320000 facts/objetos de datos divididos en hasta 8 requests (40 mil facts por request) se requiere la siguiente especificación de hardware:

Procesador: Intel® Xeon® Processor E5-1630 v3 o equivalente. *
Memoria Ram: 16GB DDR4. **
Almacenamiento local: 80GB***.

- *En esta especificación es importante que el rendimiento por nucleo del procesador sea bueno, en este caso el procesador elegido a modo de ejemplo tiene 4 núcleos físicos corriendo a una frecuencia de hasta 3.8Ghz, con una litografía de 22nm con un tdp máximo de 140W. 
- **También es recomendable que la memoria ram sea rápida, se recomienda, mínimamente, un esquema "Dual-Channel" para evitar cuellos de botella (tambien se puede usar triple y quad channel).
- ***Es imprescindible tener al menos 80 GB de almacenamiento local (no storage).

### 3.2. Evaluación de tipo "online" / Secuencial.
En este caso se prioriza el tiempo de respuesta, para que este sea menor a tres segundos se recomienda no superar el umbral de los 300 requests por segundo con un solo fact por request. Para este caso se recomienda un hardware mínimo como por ejemplo:

Procesador: Intel® Xeon® Processor E5-2650L v3 o equivalente. *
Memoria Ram: 16GB DDR4. **
Almacenamiento local: 80GB***.

- *En esta especificación es importante que el procesador tenga buena cantidad de nucleos disponibles y no tanto el poder de fuerza bruta en procesamiento de cada uno de estos nucleos, en este caso el procesador elegido a modo de ejemplo tiene 12 núcleos físicos (24 con HT) corriendo a una frecuencia de hasta 2.5Ghz, con una litografía de 22nm con un tdp máximo de 65W (este es un procesador de la familia low power). 
- **También es recomendable que la memoria ram sea rápida, se recomienda un esquema al menos "Dual-Channel" para evitar cuellos de botella (tambien se puede usar triple y quad channel).
- ***Es imprescindible tener al menos 80 GB de almacenamiento local (no storage).
## 4- Escalabilidad.
### 4.1 - Mejora de tiempos de respuesta
Si se quiere mejorar el tiempo de respuesta se necesita mejorar el poder de procesamiento por núcleo, por ejemplo, (hablando siempre de la misma familia de procesadores) pasar de un procesador cuya frecuencia base es 1.8Ghz a uno de 3.6Ghz la velocidad de procesamiento es técnicamente el doble, con esto se recortarían a la mitad los tiempos de respuesta para la misma cantidad de requests.

### 4.2 - Aumentar cantidad de facts por requests.
Si lo que se desea por otro lado es aumentar la cantidad de facts por request, se recomienda aumentar 512MB de ram por cada 10000 facts mas ingresados al sistema de evaluación.
### 4.3 - Aumentar cantidad de requests simultáneos.
El procesamiento de cada request es ejecutado exclusivamente por un solo hilo, debido a esto si se requiere aumentar la cantidad de requests paralelos sin ser penalizado en los tiempos de respuesta se requiere aumentar la cantidad de núcleos físicos de procesador, siendo un requisito exclusivo para estos una buena velocidad de procesamiento. 

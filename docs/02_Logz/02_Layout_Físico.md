# Layout Físico General 
----------------------------------

A continuación se describe el layout físico recomendado de la solución para alcanzar los siguientes objetivos.

* La solución debe soportar un tráfico de al menos 500 Eventos por segundo (E/S en lo sucesivo) 
* La latencia entre eventos no debe superar los 2 milisegundos
* Que los eventos son leidos de múltiples fuentes y que deben ser procesados en forma simultánea 
* Que de acuerdo a las ultimas mediciones de performance se procesan aproximadamente 1.8Gb de información por día 
* La solución representa una frontera de seguridad importante en donde es vital contar con el análisis de eventos en tiempo real por lo cual se debe garantizar un UP Time de 24hs X 7 días a la semana por 365 días al año

## Diseño de alto nivel 

La plataforma debe contar con al menos dos componentes: Un componente para alojar los servidores de aplicaciones y otro componente para el respositorio de datos. 

>NOTA: Al referirnos a componentes no hacemos referencia tácita a un solo servidor sino a un conjunto de servidores que actuan como un solo prestador de servicios. Luego se realizara un detalle pormenorizado de la arquitectura física recomendada 

## Servidor de aplicaciones 

Este componente alojará dos servidores de aplicaciones: 

1. JBoss EAP 6.2 
2. Apache Webserver. 

Un factor importante en los servidores de Aplicaciónes es la memoria RAM, ésta debe ser la suficiente para manejar el caudal de eventos teniendo en cuenta las reglas que se darán de alta para análisis en tiempo real y simultáneamente manejar situaciones de mantenimiento/contingencia del cluster de servidores de datos. Este servidor debe garantizar alta velocidad entre el procesador y la memoria. 

>NOTA:Sobre el almacenamiento físico cabe destacar que las prestaciones de este server no requieren gran capacidad de almacenamiento. 

## Cluster/Repositorio de datos

Cuando nos referimos al repositorio de datos no solo hacemos referencia a la BBDD sino también a un repositorio de archivos donde se alojan los resultados del syslog. 

Este repositorio es recomendable que no sea un único componente físico sino que sea un conjunto de servidores para lograr conseguir: 

* Mejor balanceo y distribución de carga 
* Tolerancia a prueba de fallos con redundancia. 

Además es importante considerar que para lograr una mejor performance es recomendable que el acceso a Disco sea el mas rápido disponible. 
Los discos de estado sólido son la mejor opción actualmente.


## Requerimientos 

A continuación se menciona una lista de recomendaciones para lograr un UPTIME de 24x7x365 en base a las características mencionadas.

### Servidor de aplicaciones 

Este servidor debe poseer al menos un procesador de 2.5+ Ghz con 4 núcleos y 32Gb de RAM. En cuanto al SSD con 256Gb o superior es recomendado. 

Para lograr el UPTIME propuesto es recomendable que sea al menos dos servidores.

### Repositorio de datos 

>TIP: El tamaño total recomendado del cluster es será directamente proporcional al tamaño promedio de los eventos. Vamos a establecer un que un evento promedio tiene 1KB, teniendo en cuenta un caudal de 500 eventos por segundo nos da un promedio de 500KB por segundo, a éso debemos sumarle la redundancia de datos, lo cual nos suma la misma cifra, teniendo 1MB por segundo y 85 GB por día y 2,6TB cada 30 días. 

El repositorio de datos se divide en los siguientes nodos:

### **MASTER**
Nodo que maneja todas las transacciones que solicitan datos, distribuye y balancea los datos que se almacenan en los nodos DATA. Necesita más capacidad de procesamiento que memoria RAM o capacidad de almacenamiento.
### **DATA**
Nodo que almacena datos y responde a requests de búsqueda de datos que realiza el nodo MASTER. También realiza todos los cacheos necesarios para disminuir el tiempo de respuesta. Necesita más memoria RAM y capacidad de almacenamiento que capacidad de procesamiento.

>TIP: Un nodo MASTER/DATA necesitará mucha RAM y buena capacidad de procesamiento.

Cuando un nodo MASTER envía tareas de mantenimiento a los nodos DATA, estos suelen desconectarse del cluster si es que no pueden manejar simultáneamente dichas tareas y las peticiones usuales de datos. Por éste motivo, es escencial que haya una cantidad de nodos DATA suficiente para el caudal de datos planteado, una salida de un nodo DATA entre 5 del cluster no tendrá tanto impacto como en un cluster de 3 nodos.


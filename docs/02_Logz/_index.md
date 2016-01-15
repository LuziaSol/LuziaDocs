
Esta documentación se divide en 2 secciones: 

1. **Configuración del entorno, compilación y ejecución de logz** <br>
Explica como levantar el entorno en el que se encuentra contenido logz, las herramientas necesarias para compilarlo y ejecutarlo correctamente
2. **Configuración de los módulos de Logz:**<br>
Explica como configurar correctamente Logz en los distintos escenarios que se puedan plantear.


>TIP: Si ya tenemos levantado el entorno se puede ver directamente la configuración de los módulos de la plataforma en la sección: [Documentación de los módulos de Logz](#doc_mod).

## Índice de contenido

- [Requisitos](#requisitos)
- [Estructura interna de la plataforma](#estr_interna)
- [Instalación del entorno](./Logz/Instalación_de_entorno#instalacion)
	- [JDK 1.7](./Logz/Instalación_de_entorno#jdk)
	- [Elasticsearch 1.7.3](./Logz/Instalación_de_entorno#elasticsearch)
	- [Jboss EAP 6.2.1](./Logz/Instalación_de_entorno#jboss)
	- [Maven 3.x](./Logz/Instalación_de_entorno#maven)
	- [Git 1.9.1](./Logz/Instalación_de_entorno#git)
- [Layout Físico General recomendado de la plataforma](#layout)
- [Documentación de los módulos de Logz](#doc_mod)


<a name="requisitos"></a>
## Requisitos 

A continuación se indican los componentes necesarios para poder ejecutar la aplicación:

* JDK 1.7.x
* JBoss 7.1.1 
* Maven 3.0.4
* Elasticsearch 1.7.3
* Git 1.9.1

<a name="estr_interna"></a>
## Estructura interna de la plataforma

A continuación se detalla la estructura interna de los archivos contenidos en el repositorio

|Nombre|Descripción|
|------|-------|
|**api**|Módulo librería que contiene todas las interfaces, entidades, POJOs, DTOs y DVOs utilizados por Logz|
|**services**|Módulo librería que contiene todas las implementaciones de las interfaces declaradas en el módulo *api*|
|**reader**|Módulo Standalone del Agente Logz que lee un archivo de logs de eventos y los envía al servicio REST de logz para su posterior procesado|
|**processor-engine**|Módulo del motor de procesamiento JavasCript|
|**web**|Módulo WEB de logz|
|**analyzer-cep**|Módulo de Análisis de eventos|
|**etc**|Directorio que contiene toda la documentación, librerías ad-hoc, y configuraciones adicionales de la Plataforma|
|**pom.xml**|Descriptor MAVEN padre que agrupa todos los submódulos del proyecto|


Aplicativo Interceptor y Analizador de Eventos 

Logz es un aplicativo inicialmente pensado para recibir instancias de eventos que suceden, almacenarlos y procesarlos aplicando una lógica establecida por el usuario mediante una interfaz simple de codificación JavaScript.
Los eventos se reciben vía una interfaz de servicios Web REST, y se almacenan en una base de datos NoSql para luego ser procesados. 
Pueden consultarse los eventos mediante la misma interfaz de servicios WEB y generarse opcionalmente archivos de ontologías en base a las búsquedas realizadas para aplicar posterior lógica de procesamiento. 

<a name="layout"></a>
## Layout Físico General recomendado de la plataforma
* [Arquitectura física](./Logz/Layout_Físico)


<a name="doc_mod"></a>
## Documentación de los módulos de Logz

* [Módulo Web](./Logz/Módulo_Web)
* [Módulo Analizador](./Logz/Módulo_de_Análisis)
* [Módulo Lector](./Logz/Módulo_de_Lectura)

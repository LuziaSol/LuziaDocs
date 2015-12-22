## Configración Módulo READER de Logz
--------------------------------------------

El lector de Logz es el encargado de leer el archivo específico donde se almacenan los eventos en el equipo de orígen. También es el encargado de asegurarse que el evento no sea leído múltiples veces y de llevar el seguimiento del archivo de log en caso de que el mismo sea alterado (ej, rotación de tamaño)

>TIP: Los requisitos para la ejecución son los siguientes<br>
>
>* JDK 1.7.x
>
>Para poder configurar estos requisitos debería remitirse al [documento Principal](./Instalación_de_entorno#instalacion) de configuración


### Índice de contenido

- [Configuración del Agente Lector](#configuracion_reader)
- [Ejecución del Agente Lector](#configuracion_app)
 - [Parámetros posibles para configurar el agente por comando](#params)
 - [Ejecución en misma máquina](#ejecucion_misma)
 - [Ejecución en diferente máquina](#ejecucion_diferente)
 - [Puesta en marcha de una datasource](#ejecucion_gral)


<a name="configuracion_reader"></a>
## Configuración del Agente Lector

El lector se *auto configura* si se va a ejecutar en la misma máquina donde corre la aplicación, pero en general esto se hace en máquinas donde se almacenan los archivos de eventos que se leerán y enviarán a Logz.

### Archivo de configuración del Lector

El archivo de configuración se puede crear a mano antes de ejecutar el lector, si no se crea previamente se crea automáticamente al ejecutarlo por primera vez en el directorio de ejecución configurado. <br>
El directorio de ejecución se configura por parámetro de ejecución al correr el comando para levantar el agente, _más adelante se revisará este punto_.

 * Crear el archivo `config.json` en un directorio seleccionado, para este caso vamos a utilizar `/opt/agente` (puede ser cualquiera), y lo tendremos en cuenta para cuando vayamos a inicializar el agente mas adelante.

 * Agregar la siguiente información
 
```
	configuration.logz.url.host=127.0.0.1 #(El host del server donde corre Pahtfinder)
	configuration.logz.url.port=8180 #(El puerto donde corre Logz)
	configuration.logz.url.protocol=http #(El protocolo de comunicación con Logz)
	configuration.logz.url.context=/logz #(El contexto sobre el que corre Pathfidner)
	configuration.health.active=true #(Flag para determinar si el agente envía datos al health externo)
	configuration.health.url.host=127.0.0.1 #(Host donde corre Logz)
	configuration.health.url.port=8180 #(El puerto donde corre el health externo a logz)
	configuration.health.url.context=/logz-health #(Contexto sobre el que corre el health externo de Logz)
```

<a name="configuracion_app"></a>
## Ejecución del Agente Lector
Como vimos anteriormente el agente puede ejecutarse directamente sin crear configuraciones si el mismo se ejecuta en el mismo servidor de Logz. Vamos a proceder a detallar los dos métodos de ejecución.

<a name="params"></a>
### Parámetros posibles para configurar el agente por comando

El agente recibe distintos parámetros de configuración por línea de comando, a continuación se detallan:

|Parámetro|Detalle|
|--------|--------|
|**-w**|Directorio de ejecución, *debe tener los permisos necesarios que el usuario que corre el agente pueda crear archivos en él*|
|**-h**|Host de Logz, Se matchea con la propiedad `configuration.logz.url.host`|
|**-p**|Puerto de Logz, Se matchea con la propiedad `configuration.logz.url.port`|
|**-c**|Contexto de Logz, Se matchea con la propiedad `configuration.logz.url.context`|
|**-hh**|Host del Health Logz, Se matchea con la propiedad `configuration.health.url.host`|
|**-hp**|Puerto del Health Logz, Se matchea con la propiedad `configuration.health.url.port`|
|**-hc**|Contexto del Health Logz, Se matchea con la propiedad `configuration.health.url.context`|

<a name="ejecucion_misma"></a>
### Ejecución en misma máquina
Si el agente lector corre sobre el mismo servidor donde corre Logz el comando para ejecutarlo es el siguiente (se utilizará el directorio de trabajo establecido en la sección "<<configuracion_reader>>"):

``` 
 $ java -jar logz-agent.jar -w /opt/agente 
```

<a name="ejecucion_diferente"></a>
### Ejecución en diferente máquina
Si el agente lector corre sobre el mismo servidor donde corre Logz el comando para ejecutarlo es el siguiente:

```
 $ java -jar logz-agent.jar -w /opt/agente -h[HOST_PATHFINDER] -p[PORT_PATHFINDER] -c[CONTEXT_PATHFINDER] -hh[HOST_HEALTH_PATHFINDER] -hp[PORT_HEALTH_PATHFINDER] -hc[CONTEXT_HEALTH_PATHFINDER]
```

<a name="ejecucion_gral"></a>
### Puesta en marcha de una datasource
Iniciar el agente solo hace que el mismo registre en la Plataforma pero no comienza a enviar eventos. <br>
Para que los eventos comiencen a ser enviados a Logz es necesario ingresar a Logz a la sección de *Agentes* y registrar en el Agente registrado una nueva datasource, donde se especificará el path completo al archivo de eventos. Habiendo hecho esto, aparecerá el botón On/Off para poder comenzar el envío de eventos

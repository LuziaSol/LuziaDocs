# Configración Módulo de Análisis de Logz
------------------------------------------------

El analizador de Logz es una aplicación standalone que se comunica via servicios WEB REST. Es el encargado de consumir las reglas de correlación de eventos y generar un entorno de ejecución que en tiempo real recibe paquetes de eventos enviados luego de que se procesan en Logz, ejecuta las reglas y devuelve los disparos resultantes.

>TIP: Los requisitos para la ejecución son los siguientes::
>
>* JDK 1.7.x
>* JBoss 6.2
>* Maven 3.x
>
>Para poder configurar estos requisitos debería remitirse al [documento Principal](./Instalación_de_entorno#instalacion) de configuración

### Índice de contenido

- [Configuración JBoss](#conf_an_jboss)
- [Configuración Logz](#conf_an_app)

<a name="conf_an_jboss"></a>
## Configuración JBoss
Es necesario agregar parámetros al archivo de configuración de JBoss `standalone.xml` ubicado dentro del directorio `{JBOSS_INSTALL_DIR}/standalone/configuration` 

* Ubicar el segmento "<hornetq-server>" y asegurarse que contenga el siguiente bloque de código

```XML
<hornetq-server>
...
	<jms-destinations>
	...
		<jms-queue name="queue/Alerts">
			<entry name="java:/jms/queue/Alerts"/>
			<durable>true</durable>
		</jms-queue>
		<jms-queue name="queue/NormalAnalisis">
			<entry name="java:/jms/queue/NormalAnalisis"/>
			<durable>true</durable>
		</jms-queue>
		<jms-queue name="queue/CepAnalisis">
			<entry name="java:/jms/queue/CepAnalisis"/>
			<durable>true</durable>
		</jms-queue>
		<jms-topic name="topic/Logz">
			<entry name="topic/Logz"/>
			<entry name="java:/jms/topic/Logz"/>
		</jms-topic>
	...
	</jms-destinations>
</hornetq-server>
```

<a name="conf_an_app"></a>
## Configuración Logz
Crear el archivo de configuración `logz_analyzer_cep_properties` ubicado en el directorio `{JBOSS_INSTALL_DIR}/standalone/configuration` y agregar lo siguiente

```
#activacion CEP
kb.rules.stream=true #(Flag de configuración para análisis en tiempo real)
#no utilizada si kb.rules.stream=false
kb.rules.stream.entrypoint=logz #(Nombre de punto de entrada de eventos)

logz.remote=true #(Flag de activación de análisis remoto de eventos)
logz.host=127.0.0.1:8180 #(host y puerto de ejecución de logz)
logz.context=/logz #(Contexto de ejecución de logz)
```






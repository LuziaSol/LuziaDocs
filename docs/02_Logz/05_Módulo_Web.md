# Configración Módulo WEB de Logz
-------------------------------------------

El módulo web es la interfaz principal para la interacción con el la plataforma, a través de ella se realizan búsquedas de eventos, se cargan reglas, parsers para los eventos,etc. Este módulo también contiene la interfaz Web REST, a la que llegan los eventos enviados por el lector. <br>
A continuación se explican las configuraciones para poder levantar y ejecutar correctamente el módulo web.

>TIP: Los requisitos para la ejecución son los siguientes::
>
>* JDK 1.7.x
>* Elasticsearch 1.7.3
>* JBoss 6.2
>* Maven 3+
>* GIT 3+
>
>Para poder configurar estos requisitos debería remitirse al [documento Principal](./Instalación_de_entorno#instalacion) de configuración


### Índice de contenido

- [Clonado de Proyecto](#clone)
- [Configuración JBoss](#conf_web_jboss)
- [Configuración Logz](#conf_web_app)


<a name="clone"></a>
# Clonado de Proyecto
A continuación se detallan los pasos para descargar y clonar el repositorio con los fuentes para compilar el proyecto completo

	$ git clone https://github.com/LuziaSol/Logz

<a name="conf_web_jboss"></a>
# Configuración JBoss
A continuación se detallan las configuraciones del Jboss básicas para la correcta ejecución del aplicativo

* Agregar la configuración de persistencia al archivo standalone.xml<br>
Buscar el bloque `<datasources>` y modificar la siguiente línea :

```XML
<datasource jndi-name="java:jboss/datasources/ExampleDS" pool-name="ExampleDS" enabled="true" use-java-context="true">
```
por la siguiente:

```XML
<datasource jta="true" jndi-name="java:jboss/datasources/logzDS" pool-name="logzDS" enabled="true" use-java-context="true" use-ccm="true">
```
agregándose de que quede de la siguiente forma :

```XML
<datasources>
...
	<datasource jta="true" jndi-name="java:jboss/datasources/logzDS" pool-name="logzDS" enabled="true" use-java-context="true" use-ccm="true">
        <connection-url>jdbc:h2:mem:test;DB_CLOSE_DELAY=-1</connection-url>
        <driver>h2</driver>
        <security>
            <user-name>sa</user-name>
            <password>sa</password>
        </security>
    </datasource>
    <drivers>
        <driver name="h2" module="com.h2database.h2">
            <xa-datasource-class>org.h2.jdbcx.JdbcDataSource</xa-datasource-class>
        </driver>
    </drivers>
...
</datasources>
```

* Configuración de la Queue receptora de eventos<br>
Ubicar el segmento `<hornetq-server>`, y dentro del mismo, el bloque `<jms-destinations>`, luego asegurarse que contenga la información necesaria como se ejemplifica a continuación:

```XML
<hornetq-server>
...
	<jms-destinations>
	...	        
	    <jms-queue name="queue/Logz">
	            <entry name="queue/Logz"/>
	            <entry name="java:/jms/queue/Logz"/>
	    </jms-queue>
	    <jms-queue name="queue/Reprocess">
               <entry name="java:/jms/queue/Reprocess"/>
               <durable>false</durable>
        </jms-queue>
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

* Habilitar el Jboss para que escuche en una interfaz pública y se pueda acceder desde otro equipo que no sea el mismo server donde se está instalando
Ubicar el segmento `<interfaces>` y dentro del mismo modificar el que tiene el nombre _"public"_ dejando todo configurado de la siguiente forma:

```XML
<interfaces>
    ...
    <interface name="public">
        <inet-address value="${jboss.bind.address:0.0.0.0}"/>
    </interface>
    ...
</interfaces>
```

* Habilitar el sistema de mails de Logz con las credenciales del servidor SMTP correspondiente, para esto es necesario ubicar el bloque `<subsystem xmlns="urn:jboss:domain:mail:1.1">` y agregar la información pertinente, a continuación se ejemplifica un servidor SMTP de gmail y uno local abierto

```XML
<profile>
...
	<subsystem xmlns="urn:jboss:domain:mail:1.1">
	...
		<!-- ELEGIR SOLO UNA OPCION -->
		<!-- SERVIDOR SMTP LOCAL ABIERTO
		<mail-session jndi-name="java:jboss/mail/Logz">
		    <smtp-server outbound-socket-binding-ref="mail-smtp"/>
		</mail-session>
		-->
		<!-- SERVIDOR SMTP GMAIL
		<mail-session jndi-name="java:jboss/mail/Logz">
		    <smtp-server ssl="true" outbound-socket-binding-ref="gmail-smtp">
		        <login name="user@gmail.com" password="pass"/>
		    </smtp-server>
		</mail-session>
	...
	</subsystem>
...
	<socket-binding-group name="standard-sockets" default-interface="unsecure" port-offset="${jboss.socket.binding.port-offset:0}">
	...
	<outbound-socket-binding name="mail-smtp">
        <remote-destination host="localhost" port="25"/>
    </outbound-socket-binding>
    <outbound-socket-binding name="gmail-smtp">
        <remote-destination host="smtp.gmail.com" port="465"/>
    </outbound-socket-binding>
	...
	</socket-binding-group>

</profile>

```

<a name="conf_web_app"></a>
# Configuración Logz

* Crear el archivo `logz_config.properties` en el mismo directorio que el archivo `standalone.xml` y agregarle editar las siguientes configuraciones:

```
logz.host=127.0.0.1 (IP público del host donde está Logz)
logz.port=8180 (Puerto donde corre el JBoss)
logz.context=/logz (Contexto de ejecución de Logz)
logz.metrics=true (Activar Métricas)
logz.instance=node-1 (Nombre de la instancia)

logz.user=admin (Usuario inicial del sistema)
logz.pass=admin (Password inicial del sistema)

logz.health.active=true (Flag para habilitar el envío de datos de Salud a un Health externo)
logz.health.url.host=localhost (IP público del Health Externo)
logz.health.url.port=8180 (Puerto donde corre el JBoss del Health Externo)
#logz.health.url.context=/logz-health
logz.kibana.url=127.0.0.1 (IP Público donde corre Kibana)
#logz.backup=/tmp/logz/backups (Directorio de trabajo para realizar backups, *debe estar creado*)
logz.drl.active=true ()

elasticsearch.host=127.0.0.1 (IP Público donde corre un nodo master de ES, valores separados por coma cuando son múltiples nodo master)
elasticsearch.port=9301 (Puerto donde corre el/los nodo/s master de ES)
#elasticsearch.index.rotation=true (Flag para rotación de índices de los eventos)
#elasticsearch.ping=1 (Tiempo en segundos para pingear los nodos de ES)
#elasticsearch.index.persist=logz-index (Índice de Persistencia en ES para los eventos recibidos)
#elasticsearch.user=admin (Usuario de ES si es que se ha configurado SHIELD)
#elasticsearch.pass=admin (Password de ES si es que se ha configurado SHIELD)
#elasticsearch.cluster=logz (Nombre del cluster de ES)

analyzer.active=true (Flag para enviar eventos a análisis)
analyzer.host=127.0.0.1 (IP Pública del JBoss donde se encuentra desplegado el analizador)
analyzer.port=8180 (Puerto donde corre el JBoss donde se encuentra desplegado el analizador)
#analyzer.context=/analyzer-cep (Contexto de ejecución del analizador)

ldap.auth=false (Flag de autorización ldap/active directory)
ldap.host=192.168.1.1:10389 (Host del server de LDAP)
ldap.protocol=ldap (Protocolo de comunicación)
#ldap.referrals.ignore=true (Flag para ignorar las referencias de las unidades organizacionales)
#ldap.security.auth = simple (Tipo de autentificación con el servidor LDAP)
#ldap.security.scheme = native (Esquema de comunicación del servidor LDAP native/active-directory)
#ldap.security.principal.construct = true (Flag para construír el nombre de usuario a partir del nombre de dominio base)
#ldap.security.principal.suffix = uid (Sufijo del Nombre de dominio)
ldap.security.principal.attachBaseDN = true (Flag para atachear el nombre de dominio base al usuario)
#ldap.security.users.class = person #its default
ldap.security.users.baseDN = ou\=users,ou\=logz,dc\=cuyum,dc\=com (Nombre de dominio para los usuarios)
ldap.security.roles.baseDN = ou\=roles,ou\=logz,dc\=cuyum,dc\=com (Nombre de dominio para los roles)
#ldap.security.roles.admin = "Admin" (Mapeo de rol de administración)
#ldap.security.roles.user = "User" (Mapeo de rol de usuario)
#ldap.security.domain = false (Flag de especificación de seguridad de dominio)
#ldap.security.domain.name = cuyum.com (Nombre de dominio base)
```

> ADVERTENCIA: __**Configurar el nombre del cluster en ElasticSearch en su archivo de configuración correspondiente**__ (`{ELASTICSEARCH_INSTALL_DIR}/elasticsearch.yml`) o configurar el archivo `logz_config.properties` con el correspondiente




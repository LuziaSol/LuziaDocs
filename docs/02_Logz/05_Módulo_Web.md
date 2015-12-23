# Configración Módulo WEB de Logz
-------------------------------------------

El módulo web es la interfaz principal para la interacción con el la plataforma, a través de ella se realizan búsquedas de eventos, se cargan reglas, parsers para los eventos,etc. Este módulo también contiene la interfaz Web REST, a la que llegan los eventos enviados por el lector. <br>
A continuación se explican las configuraciones para poder levantar y ejecutar correctamente el módulo web.

>TIP: Los requisitos para la ejecución son los siguientes::
>
>* JDK 1.7.x
>* Elasticsearch 1.7.3
>* JBoss 6.2
>* Maven 3.x
>
>Para poder configurar estos requisitos debería remitirse al [documento Principal](./Instalación_de_entorno#instalacion) de configuración


### Índice de contenido

- [Configuración JBoss](#conf_web_jboss)
- [Configuración Logz](#conf_web_app)

<a name="conf_web_jboss"></a>
# Configuración JBoss
A continuación se detallan las configuraciones del Jboss básicas para la correcta ejecución del aplicativo

* Agregar la configuración de persistencia al archivo standalone.xml<br>
Buscar el bloque `<datasources>` y agregar asegurarse que quede como se encuentra detallado:

```XML
<datasources>
...
	<datasource jta="true" jndi-name="java:jboss/datasources/logzDS" pool-name="logzDS" enabled="true" use-java-context="true" use-ccm="true">
        <connection-url>jdbc:postgresql://localhost:5432/[NOMBRE_DE_DB]</connection-url>
        <driver-class>org.postgresql.Driver</driver-class>
        <driver>postgresql</driver>
        <security>
            <user-name>[DB_USER]</user-name>
            <password>[DB_PASS]</password>
        </security>
    </datasource>
...
</datasources>
```

* Configuración de la Queue receptora de eventos<br>
Ubicar el segmento "<hornetq-server>" y dentro del mismo el bloque "<jms-destinations>" y asegurarse que contenga la información necesaria como se ejemplifica a continuación

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
	...
	</jms-destinations>
</hornetq-server>
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




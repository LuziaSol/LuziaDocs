# Entorno de ejecución
------------------------------------------

El entorno de ejecución solo comprende lo necesario para poder ejecutar la plataforma.

<a name="instalacion"></a>
## Instalación del entorno
A continuación se detallan los pasos de instalación teniendo como base un sistema linux.

<a name="jdk"></a>
### JDK 1.7
El modulo web corre sobre Java Runtime Environment 7, y para compilar los binarios es necesario instalar Java Development Kit.

### Ubuntu (.deb y Derivados)

* Asegurarse que no se tiene openjdk instalado, en caso de tenerlo, es necesario removerlo

```
$ sudo apt-get update && apt-get remove openjdk-6-jre
```

* Instalar el repositorio que contiene los paquetes necesarios para levantar Oracle Java 7

```	
$ sudo add-apt-repository ppa:webupd8team/java
$ sudo apt-get update
```

* Proceder con la instalacion de oracle-java7-installer

```
$ sudo apt-get install oracle-java7-installer
```

* Confirmar que la instalación ha sido exitosa

```
$ java --version

Output:
java version "1.7.0_XX"
Java(TM) SE Runtime Environment (build 1.7.0_XX-ZXX)
Java HotSpot(TM) Server VM (build XX.XX-ZXX, mixed mode)
```

### Red-Hat (.rpm y Dervivados)

* 64 Bits

```
 $ cd /opt/
 $ wget --no-cookies --no-check-certificate --header "Cookie: gpw_e24=http%3A%2F%2Fwww.oracle.com%2F; oraclelicense=accept-securebackup-cookie" "http://download.oracle.com/otn-pub/java/jdk/7u79-b15/jdk-7u79-linux-x64.tar.gz"
 $ tar -xvzf jdk-7u79-linux-x64.tar.gz
```

* 32 Bits

```
 $ cd /opt/
 $ wget --no-cookies --no-check-certificate --header "Cookie: gpw_e24=http%3A%2F%2Fwww.oracle.com%2F; oraclelicense=accept-securebackup-cookie" "http://download.oracle.com/otn-pub/java/jdk/7u79-b15/jdk-7u79-linux-i586.tar.gz"
 $ tar xzf jdk-7u79-linux-i586.tar.gz
```

* Instalar Java

```
 $ cd /opt/jdk1.7.0_79/
 $ alternatives --install /usr/bin/java java /opt/jdk1.7.0_79/bin/java 2
 $ alternatives --config java
```

* Activar Java correspondiente

```
 There are 3 programs which provide 'java'.
   Selection    Command
 -----------------------------------------------
 *  1           /opt/jdk1.7.0_60/bin/java
  + 2           /opt/jdk1.7.0_72/bin/java
    3           /opt/jdk1.7.0_79/bin/java
 Enter to keep the current selection[+], or type selection number: 3 [Press Enter]
```

* Activar "alternatives"

```
 $ alternatives --install /usr/bin/jar jar /opt/jdk1.7.0_79/bin/jar 2
 $ alternatives --install /usr/bin/javac javac /opt/jdk1.7.0_79/bin/javac 2
 $ alternatives --set jar /opt/jdk1.7.0_79/bin/jar
 $ alternatives --set javac /opt/jdk1.7.0_79/bin/javac 
```

* Confirmar que la instalación ha sido exitosa

```
 $ java --version
 Output:
 java version "1.7.0_XX"
 Java(TM) SE Runtime Environment (build 1.7.0_XX-ZXX)
 Java HotSpot(TM) Server VM (build XX.XX-ZXX, mixed mode)
```

<a name="elasticsearch"></a>
### Elasticsearch 1.7.3
El motor de busquedas Elasticsearch almacena todos los datos procesados por logz

> ADVERTENCIA: Se debe tener JDK 7 

### Ubuntu (.deb y derivados)

* Descargar e instalar la "Public Signing Key": 

```
$ wget -qO - https://packages.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
```

* Agregar el repositorio a `/etc/apt/sources.list.d/elasticsearch-1.7.list`

```
 $ echo "deb http://packages.elastic.co/elasticsearch/1.7/debian stable main" | sudo tee -a /etc/apt/sources.list.d/elasticsearch-1.7.list
```

* Actualizar apt-get e instalar elasticsearch 

```
$ sudo apt-get update && sudo apt-get install elasticsearch
```

* Configurar como servicio
	* Para sistemas con SysV init
	```
	 $ sudo update-rc.d elasticsearch defaults 95 10
	```
	* Para sistemas si SysV init
	```
	 $ sudo /bin/systemctl daemon-reload
	 $ sudo /bin/systemctl enable elasticsearch.service
	```


>## ADVERTENCIA
>Use el comando "echo" para agregar el repositorio de Elasticsearch. Elasticsearch no provee un repositorio de fuentes *no* utilice add-apt-repository ya que deberá ingresar una entrada para deb-src también.
>Si lo hace verá el siguiente error
>```
> Unable to find expected entry 'main/source/Sources' in Release file (Wrong sources.list entry or malformed file)
> Just delete the deb-src entry from the /etc/apt/sources.list file and the installation should work as expected.
>```


### Red-Hat (.rpm y derivados)::

* Descargar e instalar la "Public Signing Key" 

```
 $ rpm --import https://packages.elastic.co/GPG-KEY-elasticsearch
```

* Agregar lo siguiente al archivo `elasticsearch.repo` en el directorio `/etc/yum.repos.d/`

```
 [elasticsearch-1.7]
 name=Elasticsearch repository for 1.7.x packages
 baseurl=http://packages.elastic.co/elasticsearch/1.7/centos
 gpgcheck=1
 gpgkey=http://packages.elastic.co/GPG-KEY-elasticsearch
 enabled=1 
```
* Instalar elasticsearch

```
 $ yum install elasticsearch
```
* Verificar que la instalación ha sido exitosa<br>
Acceder desde el navegador a http://localhost:9200 y deberia retornar algo similar a:	

```JSON
		{
			"status" : 200,
			"name" : "Malice",
			"version" : {
				"number" : "1.0.1",
				"build_hash" : "5c03844e1978e5cc924dab2a423dc63ce881c42b",
				"build_timestamp" : "2014-02-25T15:52:53Z",
				"build_snapshot" : false,
				"lucene_version" : "4.6"
			},
			"tagline" : "You Know, for Search"
		}
```

>Links de Referencia::			
>
>http://www.elasticsearch.org/guide/en/elasticsearch/reference/current/setup-service.html
>http://www.elasticsearch.org/guide/en/elasticsearch/reference/current/setup.html


<a name="jboss"></a>
### Jboss EAP 6.2.1
El Servidor de aplicaciones certificado jee7 de redhat es el contenedor del módulo web

* Descargar Jboss EAP 6.2 del siguiente link

http://www.jboss.org/download-manager/file/jboss-eap-6.2.0.GA.zip

* Renombrar el archivo standalone.xml por standalone-bkp.xml y standalone-full-ha.xml por standalone.xml. 
(Ambos archivos se encuentran localizados en <JBOSS_DIRECTORY>/standalone/configuration)

>ADVERTENCIA: Necesita tener una cuenta registrada en el portal de Red Hat con el producto habilitado para la descarga

<a name="maven"></a>
### Maven 3.x
Apache Maven es el encargado de administrar todas las dependencias externas del proyecto para poder compilar los binarios.

>ADVERTENCIA: Es necesario tener JDK 7 instalado

* Descargar los binarios de Maven de la siguiente URL

```
http://maven.apache.org/download.cgi (Se debe escoger un mirror y descargar los "Binaries")
```

* Descomprimir los binarios

```
$ sudo tar -xvzf apache-maven-3.X.X-bin.tar.gz
$ sudo mv apache-maven-3.X.X-bin /usr/local/apache-maven/
$ sudo ln -s /usr/local/apache-maven/apache-maven-3.X.X-bin /usr/local/apache-maven/apache-default
```

* Configurar las variables de entorno para encontrar los ejecutables correctos

```
$ export M2_HOME=/usr/local/apache-maven/maven-default
$ export M2=$M2_HOME/bin
$ export PATH=$M2:$PATH
```

* Verificar que se encuentran configurados correctamente los ejecutables

``` 
$ mvn --version

Output:
Apache Maven 3.X.X ([HASH]; TIMESTAMP)
Maven home: /usr/local/apache-maven/apache-default
Java version: 1.7.0_XX, vendor: Oracle Corporation
Java home: /usr/lib/jvm/java-7-oracle/jre
Default locale: es_AR, platform encoding: UTF-8
OS name: "linux", version: "[VERSION]", arch: "[ARCH]", family: "unix"
```

* Una vez instalado maven se deben agrear las librerías necesarias para poder compilar el proyecto

```
$ mvn install:install-file -Dfile={CLONED_LOGZ_REPO_DIR}/etc/libs/commons-io-2.5-SNAPSHOT.jar -DgroupId=commons-io -DartifactId=commons-io -Dversion=2.5-SNAPSHOT -Dpackaging=jar
```

<a name="layout"></a>
## Layout Físico General recomendado de la plataforma
* [Arquitectura física](./Layout_Físico)


<a name="doc_mod"></a>
## Documentación de los módulos de Logz

* [Módulo Web](./Módulo_Web)
* [Módulo Analizador](./Módulo_de_Análisis)
* [Módulo Lector](./Módulo_de_Lectura)

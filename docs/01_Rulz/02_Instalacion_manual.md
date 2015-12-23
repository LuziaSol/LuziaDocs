# Instalar Luzia Rulz
---------------------------------------------------------------------
Este documento contempla la instalación de Luzia Rulz desde 0 a partir de los requerimientos basicos del sistema.

## Indice
1. [Requisitos Previos](#id-req-prev)
  * [Python 2.7.x](#id-rp-python)
  * [MongoDB](#id-rp-mongo)
  * [Python DEV TOOLS](#id-rp-python-dev)
  * [REDIS SERVER](#id-rp-redis)

2. [Instalar](#id-inst)



<div id='id-req-prev'/>
## 1 Requisitos previos:

<div id='id-rp-python'/>
### 1.1 Python 2.7.x

  Antes de proceder con la instalación debe asegurarse que la versión de python que se está utilizando en el sistema sea mayor a la 2.7.1 y menor a la 3.0. Este chequeo puede realizarlo mediante la terminal del sistema ejecutando el siguiente comando:

  ```sh
  $ python --version
  ```

  Este comando es válido tanto para sistemas Linux basados en Ubuntu como en RHEL.

<div id='id-rp-mongo'/>
### 1.2 MongoDB > 2.6

  Si su sistema no posee instalada la base de datos “MongoDB”, deberá realizar la instalación de la misma para poder hacer uso de la aplicación. Usted puede comprobar si se encuentra instalada ejecutando el siguiente comando:

  ```sh
  $ mongo --version
  ```

  En caso de recibir el siguiente mensaje:

  ```sh
  Failed global initialization: BadValue Invalid or no user locale set. Please ensure LANG and/or LC_* environment variables are set correctly.
  ```

  Ejecute el siguiente comando y vuelva a intentarlo:

  ```sh
  $ export LC_ALL=C
  ```

  De no encontrarse instalada, puede seguir los siguientes pasos:

** En Debian/Ubuntu **

* Importar la clave publica

  ```sh
  $ sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 7F0CEB10
  ```

* Crear la lista de archivos de MongoDB

  ```sh
  $ echo 'deb http://downloads-distro.mongodb.org/repo/ubuntu-upstart dist 10gen' | sudo tee /etc/apt/sources.list.d/mongodb.list
  ```

* Actualizar los paquetes

  ```sh
  $ sudo apt-get update
  ```

* Instalar los paquetes de MongoDB

  ```sh
  $ sudo apt-get install -y mongodb-org
  ```

** En Centos/fedora/rhel **

* Crear el archivo /etc/yum.repos.d/mongodb.repo

  ```sh
  $ vi /etc/yum.repos.d/mongodb.repo
  ```

    ```sh
  [mongodb]
  name=MongoDB Repository
  baseurl=http://downloads-distro.mongodb.org/repo/redhat/os/x86_64/
  gpgcheck=0
  enabled=1
  ```

* Instalar los paquetes de MongoDB

  ```sh
  $ sudo yum install -y mongodb-org
  ```

* Configurar SELinux

  ```sh
  $ vi /etc/selinux/config
  ```

  ```sh
  SELINUX=permissive
  ```

* Reinicie el servidor

* Iniciar el servicio de Mongo

  ```sh
  $ sudo service mongod start
  $ sudo chkconfig mongod on
  ```

<div id='id-212'/>
### 1.3 Python-dev tools

** En Debian/Ubuntu **

  ```sh
  $ sudo apt-get install -y python-dev libldap2-dev libsasl2-dev libssl-dev
  ```

** En Centos/fedora/rhel **

  ```sh
  $ sudo yum install python-devel openldap-devel cyrus-sasl-devel libopenssl-devel
  ```


<div id='id-rp-redis'/>
### 1.4 Redis-Server

  Es necesario que posee el servidor Redis instalado en su sistema. Realice el chequeo de esto mediante el siguiente comando:

  ```sh
  $ sudo service redis-server status
  ```

  De no encontrarse instalado puede realizar la instalación del mismo de la siguiente manera:

** En Debian/Ubuntu **

  ```sh
  $ sudo apt-get install redis-server
  ```

** En Centos/fedora/rhel **

* Instalar EPEL Repo

  ```sh
  wget -r --no-parent -A 'epel-release-*.rpm' http://dl.fedoraproject.org/pub/epel/7/x86_64/e/

  rpm -Uvh dl.fedoraproject.org/pub/epel/7/x86_64/e/epel-release-*.rpm
  ```

* Instalar server

  ```sh
  $ yum install redis
  ```

<div id='id-inst'/>
## 2 Instalar:

* Copiar los archivos fuente a /opt/LuziaRulz


* Crear un ambiente virtual para la aplicación en /opt

  ```sh
  $ virtualenv rulz_env
  ```

* Iniciar virtualenv

  ```sh
  $ source rulz_env/bin/activate
  ```
* Ingresar a la carpeta de archivos fuente

  ```sh
  $ cd /opt/LuziaRulz
  ```

* Instalar los pquetes necesarios

  ```sh
  $ python setup.py install
  ```

* Instalar la aplicación y seguir todos los pasos para generar el archivo de configuración y todas las carpetas de datos

  ```sh
  $ python install.py install
  ```

  De aqui en adelante se usaran los valores por defecto del instalador, de haber utilizado otros debera reemplazarlos donde corresponda

* Crear el archivo app.wsgi en la carpeta wsgis

  ```sh
  $ vi /opt/Rulz/wsgis/app-rulz.wsgi
  ```

* Agregar el siguiente contenido:

  ```py
  import sys
  import os
  import site
  from os import environ, getcwd
  import logging, sys

  sys.path.append('/opt/LuziaRulz/src')
  os.environ['PYTHON_EGG_CACHE']="/opt/rulz_env/lib/python2.7/site-packages"
  os.environ['ARG_SETTINGS']="/op/Rulz/conf/luzia-rlz.cfg"
  site.addsitedir('/opt/rulz_env/lib/python2.7/site-packages')

  from initserver import app as application
  logging.basicConfig(stream=sys.stderr, level=logging.DEBUG)

  LOG = logging.getLogger(__name__)
  LOG.debug('Current path: {0}'.format(getcwd()))

  # Application config
  application.debug = True

  with application.app_context():
          pass
  ```

* Crear el archivo rulz.domain.conf en la carpeta de configuraciones de apache

  En Ubuntu
  ```sh
  $ vi /etc/apache2/sites-enabled/rulz.domain.conf
  ```

  En centos
  ```sh
  $ vi /etc/httpd/conf.d/rulz.domain.conf
  ```

* Agregar el siguiente contenido (reemplazar rulz.domain por el verdadero dominio a utilizar):

  ```conf
  <VirtualHost *:80>
    ServerName rulz.domain

    <Directory /opt/LuziaRulz>
      Require all granted
      Options +FollowSymLinks -Indexes
    </Directory>

    <Directory "/opt/Rulz">
      Options -Indexes +FollowSymLinks
      AllowOverride All
      Require all granted
    </Directory>

    # Possible values include: debug, info, notice, warn, error, crit,
    # alert, emerg.
    LogLevel debug

    WSGIDaemonProcess rulz_demon python-path=/opt/Rulz:/opt/rulz_env/lib/python2.7/site-packages user=apache group=apache threads=5
    WSGIScriptAlias / /opt/Rulz/wsgis/app-rulz.wsgi

    <Location />
      WSGIProcessGroup rulz_demon
    </Location>

    WSGIApplicationGroup %{GLOBAL}
    WSGIPassAuthorization On
    WSGIScriptReloading On

    ErrorLog /var/log/httpd/error-rulz.domain.com.log
    CustomLog /var/log/httpd/access-rulz.domain.com.log combined
  </VirtualHost>
  ```

* Re iniciar apache

  En ubuntu
  ```sh
  $ service apache2 restart
  ```

  En Centos
  ```sh
  $ service httpd restart
  ```

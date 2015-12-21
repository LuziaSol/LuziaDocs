Instalar Luzia Rulz usando docker


# 1- Consideraciones para la instalación

* Es necesario que los puertos 80 y 8080 estén disponibles para el despliegue de la aplicación. 

* Solo puede ser instalado en sistemas operativos 64 Bits.

* Tener instalado wget.

# 2- Instalar contenedor en linux

### 2.1 - Instalación de Rulz

Correr el siguiente comando en una terminal.
'''
$ wget -O - http://docker.luziasol.com:8080/install.mac.sh | sh
'''
### 2.2 - Iniciar Detener

#### **Para iniciarlo:**
'''
$ sudo docker stop LuziaRulz12182015
'''
#### 
**Para detenerlo**
'''
$ sudo docker start LuziaRulz12182015
'''
# 3- Instalar contenedor en mac

### 3.1 - Instalar wget, correr el comando en una terminal
'''
$ brew install wget
'''
### 3.2 - Instalar Docker Toolbox 

(solo compatible con mac en versiones superiores a OS X 10.8) visitar el siguiente enlace para obtener el pkg instalable.

[https://github.com/docker/toolbox/releases/download/v1.9.1e/DockerToolbox-1.9.1e.pkg](https://github.com/docker/toolbox/releases/download/v1.9.1e/DockerToolbox-1.9.1e.pkg)

### 3.3 - Instalación de Rulz

Correr el siguiente comando en una "**Docker Quickstart Terminal**"
'''
wget -O - http://docker.luziasol.com:8080/install.mac.sh | sh
'''
### 3.4 - Iniciar / Detener

Para iniciar o detener el contenedor de LuziaRulz se pueden utilizar los siguientes comandos en una "**Docker Quickstart Terminal"**

#### **Para iniciarlo:**
'''
$ docker stop LuziaRulz12182015
'''
#### 
**Para detenerlo:**
'''
$ docker start LuziaRulz12182015
'''

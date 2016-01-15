# Instalar Luzia Logz usando docker
----------------------------------
Este documento contempla la instalación de toda la plataforma de Logz en un mismo contenedor, está concebido para que su instalación sea lo mas sencilla posible a modo de demostración. __No es recomendable usar esta instalación para futuros ambientes productivos__.


## 1- Consideraciones para la instalación

* Es necesario disponer de 2GB de RAM en la máquina donde se instalará la Virtual Machine de docker.

* Es necesario que los puertos 80 y 8080 estén disponibles para el despliegue de la aplicación. 

* Solo puede ser instalado en sistemas operativos 64 Bits.

* Tener instalado wget.

## 2- Instalar contenedor en linux

### 2.1 - Instalación de Logz

Correr el siguiente comando en una terminal.
```
wget -O - http://docker.luziasol.com:8080/logz/install.linux.sh | sh
```
### 2.2 - Iniciar Detener

#### **Para iniciarlo:**
```
sudo docker start Logz
```
#### 
**Para detenerlo**
```
sudo docker stop Logz
```
3- Instalar contenedor en mac
======


### 3.1 - Instalar wget, correr el comando en una terminal
```
$ brew install wget
```
### 3.2 - Instalar Docker Toolbox 

(solo compatible con mac en versiones superiores a OS X 10.8) visitar el siguiente enlace para obtener el pkg instalable.

[https://github.com/docker/toolbox/releases/download/v1.9.1e/DockerToolbox-1.9.1e.pkg](https://github.com/docker/toolbox/releases/download/v1.9.1e/DockerToolbox-1.9.1e.pkg)

### 3.3 - Instalación de Logz

Correr el siguiente comando en una "**Docker Quickstart Terminal**"
```
$ wget -O - http://docker.luziasol.com:8080/logz/install.mac.sh | sh
```
### 3.4 - Iniciar / Detener

Para iniciar o detener el contenedor de LuziaRulz se pueden utilizar los siguientes comandos en una "**Docker Quickstart Terminal"**

#### **Para iniciarlo:**
```
$ docker start Logz
```
#### **Para detenerlo:**
```
$ docker stop Logz
```
#### **Para acceder:**

Acceder mediante un navegador a [{SERVER_IP}:8080/logz] e ingresar las credenciales admin/admin (siendo SERVER_IP la ip del servidor donde se ha instalado)

#### **Para enviar eventos:**

Descargar agente de [http://docker.luziasol.com:8080/logz/logz-agent.jar] y remitirse a la documentación del Agente lector




## Requerimientos
- Oracle Java 6 o superior
- JBoss EAP 6.1 o superior

## Compilar
1) Descargar de https://github.com/cuyum/selectividad.git la aplicación
```
>git clone https://github.com/cuyum/selectividad.git
```

2) Crear el archivo **selectividad.war**
```
>cd selectividad
>mvn clean install
```

>Se deberia haber creado el archivo **target/selectividad.war**

## Configuración (Opcional)

Por default esta aplicación necesita tener creado el directorio **$HOME/.m2/repository** para almacenar los drl instalados (unidad de deploy).
Por ejemplo si el EAP se levanta con el usuario "jboss" deberia existir el directorio **/home/jboss/.m2/repository** para que se pueden instalar los DRL.

En caso de necesitar guardar las unidadades de deploy en otro directorio se deberá realizar lo siguiente:

1) Detener el servidor

2) Modificar el archivo [EAP_HOME]/standalone/configuration/standalone.xml y agregar la siguiente propiedad del sistema

```
<system-properties>
       ...
  <property name="kie.maven.settings.custom" value="${jboss.server.config.dir}/selectividad-settings.xml" />
       ...
</system-properties>
```

3) Copiar el archivo adjunto etc/selectividad-settings.xml dentro del directorio [EAP_HOME]/standalone/configuration/

4) Editar el archivo copiado y setear el directorio donde se guardaran las unidades de deploy:

```
<localRepository>/u02/jboss/data/selectividad</localRepository>
```

5) Crear el directorio ingresado:

```
mkdir /u02/jboss/data/selectividad
```

## Desplegar

1) copiar el war generado en el directorio de deploy de JBoss

```
>cp target/selectividad.war [JBOSS_EAP_HOME]/standalone/deployments
```

2) Iniciar el JBoss EAP

## Prueba

1) Test de funcionamiento. Ejecutar con metodo GET o POST el siguiente url

```
http://localhost:8080/selectividad/rest/rule_engine/test
```
> La respuesta deberia ser: **TODO OK**

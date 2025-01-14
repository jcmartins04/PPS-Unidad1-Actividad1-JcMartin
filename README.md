# PPS-Unidad1-Actividad1-JcMartin
Actividad 1 de la Unidad 1 de Puesta en Producción Segura. Tabajaremos con los Entornos de Desarrollo

Tenemos varios objetivos:

> [Crear un entorno de desarrollo Eclipse con docker](#Eclipse-Docker)

> [Instalar extensiones en un IDE](#Instalar-extensiones)

> [Probar los entornos de Desarrollo](#Prueba-entornos) 
---
## Eclipse Docker

En [este enlace](https://hub.docker.com/r/dockeruc/eclipse) puedes encontrar podemos crear un contenedor docker con un entorno IDE Eclipse

Lee bien las instrucciones y ten en cuenta que tienes que hacer varias operaciones. Las que tienes a continuación son de un entorno Linux:

1. Crear las carpetas necesarias:
~~~
sudo mkdir -p  $HOME/docker/eclipse/datos
sudo chown -R $(whoami) $HOME/docker/eclipse
sudo chgrp -R $(whoami) $HOME/docker/eclipse
~~~

2. Configurar el entorno gráfico 

~~~
# Al arrancar el contenedor obteniamos el siguiente error: library initialization failed - unable to allocate file descriptor table - out of memoryºº
# En Kali linux no existe limite para ulimit y docker se vuelve un poco loco con este contenedo, modificando el siguiente archivo
# hacemos posible que el contenedor arranque
#
# Modificar /etc/systemd/system/multi-user.target.wants/docker.service
# Añadir a ExecStart --> ExecStart=/usr/bin/dockerd ... --default-ulimit nofile=65536:65536 ...
# sudo systemctl daemon-reload
# sudo systemctl restart docker
#
# La variable de entorno artifactory_host no se que hace, no se lanza nada en ese puerto que yo haya podido ver.
#
#       -e artifactory_host='127.0.0.1:8080'\

export DISPLAY=:0.0
xhost +local:tcpserver
~~~

3. Lanzar el contenedor

~~~

docker run -ti --rm \
        -e DISPLAY=$DISPLAY \
        --name eclipse \
        -v /tmp/.X11-unix:/tmp/.X11-unix \
        -v ./workspace:/workspace \
        -v ./eclipse/datos:/home/developer \
        dockeruc/eclipse

~~~
 

> __Explica el comando docker que has utilizado__

## Instalar extensiones

Las extensiones de un IDE nos van a facilitar la labor de programar, hacer más flexible nuestro IDE, además de hacer nuestros código más seguro.
Tenemos muchas extensiones, tanto para lenguajes de programación específicos como para el IDE.

__Las siguientes operaciones las puedes hacer desde el entorno Eclipse que hemos creado o puedes utilizar el IDE que prefieras en tu equipo__
>__Busca cuáles son las mejores extensiones de eclipse para programadores y las añades desde la tienda de tu IDE__
>__ Busca y escribe para qué sirven estos plugins: Checkstyle, Sonar Lint.__
>__Instala los plugins y complementos que has encontrado. Además busca e instala los plugins Checkstyle y Sonar Lint.__


## Prueba entornos

El entorno de desarrollo nos sirve para crear nuestras aplicaciones y además podemos comprobar los errores que tienen, problemas de seguridad, etc. por lo que desde allí vamos a poder corregirlos.
>__Descarga el código fuente de un proyecto java o python: compila, enlaza y ejecutaló. Tienes algunos ejemplos en la carpeta Sources de este repositorio__
>__Utiliza las herramientas de depuración de Eclipse o Netbeans para depurar el proyecto, y las diferentes extensiones para ver información, problemas, etc.__

---
## ENTREGA
>__Crea un repositorio  con nombre PPS-Unidad1Actividad1-Tu nombre que contenga las respuestas a las preguntas y las evidencias de que has realizado las operaciones indicadas.__

>__Sube a la plataforma, tanto el repositorio comprimido como la dirección a tu repositorio de Github.__

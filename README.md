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
 

> __Explicación del comando docker que he utilizado__

Este comando, del apartado 3, una vez inicializado el entorno gráfico, lanza un contenedor Docker con la imagen `dockeruc/eclipse`, configurado
para ejecutar Eclipse con soporte gráfico utilizando el entorno X11 del host (`-e DISPLAY=$DISPLAY` y `-v /tmp/.X11-unix:/tmp/.X11-unix`).
Los directorios locales `./workspace` y `./eclipse/datos`  se montan en el contenedor para almacenar los proyectos y datos del usuario,
respectivamente, asegurando persistencia entre ejecuciones.
El contenedor se ejecuta de forma interactiva (`-ti`) y se elimina automáticamente al terminar (`--rm`), evitando residuos en el sistema.

![Lanzando eclipse](./images/script-clipse.png)

![Lanzando eclipse](./images/script-clipse2.png)

![Lanzando eclipse](./images/script-clipse3.png)

![Lanzando eclipse](./images/script-clipse4.png)


## Instalar extensiones

Las extensiones de un IDE nos van a facilitar la labor de programar, hacer más flexible nuestro IDE, además de hacer nuestros código más seguro.
Tenemos muchas extensiones, tanto para lenguajes de programación específicos como para el IDE.

>__Las siguientes operaciones las puedes hacer desde el entorno Eclipse que hemos creado o puedes utilizar el IDE que prefieras en tu equipo__
>__Busca cuáles son las mejores extensiones de eclipse para programadores y las añades desde la tienda de tu IDE__
>__Busca y escribe para qué sirven estos plugins: Checkstyle, Sonar Lint.__

Eclipse es un entorno de desarrollo integrado (IDE) ampliamente utilizado que permite ampliar sus funcionalidades mediante diversos complementos.
A continuación, se presentan algunas de las extensiones más recomendadas para programadores que buscan mejorar su productividad y eficiencia:

**EGit**  
Proporciona integración con Git, permitiendo gestionar repositorios directamente desde Eclipse. 

**Spring Tools**  
Facilita el desarrollo de aplicaciones con el framework Spring, ofreciendo soporte para Spring Boot y herramientas de configuración. 

**Maven Integration for Eclipse (M2E)**  
Permite gestionar proyectos Maven dentro de Eclipse, facilitando la configuración y administración de dependencias. 

**Mylyn**  
Ofrece una interfaz centrada en tareas que integra herramientas de gestión del ciclo de vida de aplicaciones, mejorando la productividad al reducir la sobrecarga de información. 

**Checkstyle**  
Ayuda a mantener un estilo de código consistente al señalar desviaciones de las reglas de codificación definidas, mejorando la calidad del código. 

**FindBugs**  
Analiza programas Java compilados para detectar posibles errores, contribuyendo a la identificación temprana de problemas en el código. 

**PMD**  
Detecta código mal escrito y posibles errores, ayudando a mejorar la calidad y mantenimiento del código fuente. 

**AnyEdit Tools**  
Agrega funcionalidades adicionales al editor, como la eliminación de espacios en blanco innecesarios y la conversión de caracteres especiales, mejorando la legibilidad del código. 

**Eclim**  
Integra las funcionalidades de Eclipse con editores de texto como Vim, permitiendo a los desarrolladores escribir código en diferentes lenguajes con soporte avanzado. 

**Subclipse**  
Proporciona soporte para Subversion (SVN) dentro de Eclipse, facilitando el control de versiones y la colaboración en proyectos. 

Estas extensiones amplían las capacidades de Eclipse, adaptándolo a las necesidades específicas de cada desarrollador y mejorando la eficiencia en el desarrollo de software. 

>__Instala los plugins y complementos que has encontrado. Además busca e instala los plugins Checkstyle y Sonar Lint.__

**Checkstyle**  
Ayuda a mantener un estilo de código consistente al señalar desviaciones de las reglas de codificación definidas, mejorando la calidad del código.

**SonarLint**
Es una herramienta que ayuda a mejorar la calidad del código y prevenir errores. Algunas de sus principales funciones son:  

1. **Análisis de código en tiempo real:** Detecta errores, vulnerabilidades y malas prácticas mientras se escribe el código, sin necesidad de ejecutar compilaciones o tests.  
   
2. **Soporte para múltiples lenguajes:** Compatible con lenguajes como Java, JavaScript, Python, C, C++, entre otros.  

3. **Reglas configurables:** Permite personalizar las reglas de análisis de acuerdo con las necesidades del proyecto.  

4. **Integración con SonarQube y SonarCloud:** Si se conecta a estas plataformas, puede sincronizar las reglas y obtener información adicional del análisis del proyecto.  

5. **Feedback inmediato:** Muestra problemas directamente en el editor de Eclipse, lo que permite corregirlos de manera rápida y eficiente.  

En resumen, SonarLint actúa como un "asistente de calidad" que mejora la productividad al mantener el código limpio y libre de errores desde el inicio.

## Prueba entornos

El entorno de desarrollo nos sirve para crear nuestras aplicaciones y además podemos comprobar los errores que tienen, problemas de seguridad, etc. por lo que desde allí vamos a poder corregirlos.
>__Descarga el código fuente de un proyecto java o python: compila, enlaza y ejecutaló. Tienes algunos ejemplos en la carpeta Sources de este repositorio__
>__Utiliza las herramientas de depuración de Eclipse o Netbeans para depurar el proyecto, y las diferentes extensiones para ver información, problemas, etc.__

---
## ENTREGA
>__Crea un repositorio  con nombre PPS-Unidad1Actividad1-Tu nombre que contenga las respuestas a las preguntas y las evidencias de que has realizado las operaciones indicadas.__

>__Sube a la plataforma, tanto el repositorio comprimido como la dirección a tu repositorio de Github.__

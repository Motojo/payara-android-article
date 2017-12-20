
#Payara Micro en Android (Root no requerido)

¿Te has preguntado que tan pequeño es Payara Micro?

Al inicio comencé con un divertido experimento instalando una aplicación en mi teléfono para poder ejecutar algunos comandos Linux en una terminal, pero una cosa llevó a la otra y terminé ejecutando una instancia de Payara Micro. A continuación te mostraré como.

Para tu comodidad, quizas quieras relizar este experimento desde tu computadora [conectandóla vía SSH con tu dispositivo Android](https://termux.com/ssh.html).

## Instalando Termux y una Distribución Linux.

La mejor applicación para emular una terminal que pude encontrar es [Termux](https://play.google.com/store/apps/details?id=com.termux), la cual es una aplicación de codigo abierto que no solo emula una terminal android, sino que provee un entorno Linux por igual. Esto significa que es posible instalar paquetes, ejecutar los comandos más comunes de Linux e incluso programar procesos para que se ejecuten al encender tu dispositivo. Una de las cosas más geniales que se pueden hacer es instalar algunas de las distribuciones de linux más comunes como Arch, Debia, Fedora, Kali, Ubuntu, etc. gracias a las imagenes hechas por la comunidad.

Sin embargo, aún con Termux no es posible instalar un JDK. Despues de multiples intentos, instalé [Fedora 27 en Termux](https://wiki.termux.com/wiki/Fedora) ya que en mi caso, encontré que es la distribución más ligera para nuestro requerimiento. Para instalarla, solo debemos de escribir los siguientes comandos (utiliza **f27_arm** o **f27_arm64** dependiendo de tu dispositivo).

    pkg install wget
    wget https://raw.githubusercontent.com/nmilosev/termux-fedora/master/termux-fedora.sh
    bash termux-fedora.sh f27_arm

Esta operación tardará de 10 a 30 minutos dependiendo de tu dispositivo, asi que hay que ser paciente. Una vez terminada la instalación podemos iniciar Fedora con el comando `startfedora`:

![alt text](http://guate-jug.net/payara-android/2.png "instaling fedora 1")
![alt text](http://guate-jug.net/payara-android/4.png "instaling fedora 2")

## Instalando OpenJDK 8

Ya dentro de nuestro entorno de **Fedora 27**, procederemos a instalar OpenJDK versión 8 mediante el administrador de paquetes YUM:

    yum install java-1.0.8-openjdk

![alt text](http://guate-jug.net/payara-android/5.png "instaling JDK")

Es posible que Fedora necesite actualizar algunas dependencias antes de instalar el JDK, asi que el proceso puede tardar un poco mas de lo esperado. Es posible que al terminar el proceso quieras verificar la instalación del JDK con el comando `java -v`, por lo que te darás cuenta de que probablemente falle; pero no te preocupes, esto es "normal" y no afecta nuestra meta de ejecutar una instancia de Payara Micro.

## Descargando Payara Micro y realizando el despliegue de una aplicación.

Ahora, descarguemos el archivo JAR de Payara Micro. Para hacer esto, copiaremos el enalce desde [el sitio web de Payara](https://www.payara.fish/downloads) y lo utilizaremos con el siguiente comando:

    wget "<URL_DESCARGA_PAYARA_MICRO>" -O payara-micro.jar

También es recomendable que descarguemos un archivo WAR [como este](https://www.dropbox.com/s/w573h7lajd9405w/Application.war?dl=1) para probar nuestro servidor.

Este archivo WAR es una modificación de mi [Proyecto inicial de Java EE7 / Gradle](https://github.com/Motojo/Java-EE7-Starter-Project) que solo contiene una página `index.html` y 3 servicios web RESTful de pruebaÑ


    <host>:<port>/api
    <host>:<port>/api/list
    <host>:<port>/api/query?name=&age=

Ahora que tenemos instalado un JDK, el ejecutable de Payara Micro y un archivo WAR de prueba, ¡Es hora de desplegar!

    java -jar payara-micro.jar --deploy Application.war

![alt text](http://guate-jug.net/payara-android/6.png "all needed files")

![alt text](http://guate-jug.net/payara-android/8.png "all needed files")

Dependiendo de tu dispositivo, el despliegue puede tardar varios minutos e incluso lanzar algunas excepciones (en algunos casos), pero la aplicación será desplegada sin lugar a duda.

En este escenario, el servidor se encontrará escuchando en el puerto **8080**, asi que podemos dirigirnos al navegador web del dispositivo y abrir el sitio web http://localhost:8080:

![alt text](http://guate-jug.net/payara-android/9.png "all needed files")
![alt text](http://guate-jug.net/payara-android/10.png "all needed files")
![alt text](http://guate-jug.net/payara-android/11.png "all needed files")

## Alcanzando nuestro servidor Payara Micro fuera de la red local

En el caso que desees presumir con tus amigos tu nuevo servidor Payara portable, puedes instalar un servicio llamado [Ngrok](https://ngrok.com).

Para hacer esto, necesitas descargar la [version ARM](https://ngrok.com/download) e instalarla en tu terminal de Termux. Una vez instalado, puedes exponer tu servidor con el siguiente comando:

    ./ngrok http 8080

El cual te dará una URL por medio de la cual se puede acceder al servidor en la Internet.
¿No es esto genial?

![alt text](http://guate-jug.net/payara-android/12.png "ngrok")

## Notas Finales

En este momento, los pasos para lograr todo esto son un tanto complicados, pero quizás en el futuro el proceso completo pueda ser simplificado para hacer más facil ejecutar una instancia de Payara Micro en tu dispositivo.

Recuerda que todo esto depende de las especificaciones técnicas de tu dispositivo, lo que significa que el servidor puede resultar muy lento o incluso no responder en su ejecución. Utilizando mi **Nexus 5X**, tomó 5 minutos realizar el cargue de la pagina de bienvenida del servidor, pero en una tablet aparte con mucha mas memoria RAM dicha pagina cargaba más rapido.

Cada año mejoran los procesadores y el rendimiento de los dispositivos móviles, asi que ¿Podemos pensar a futuro en una flota móvil distribuida de servidores Payara Micro que se comunica con multiples dispositivos IoT a tráves de microservicios? ¿¡Pueden imaginarse eso!?

¿O quizas dispositivos cercanos compartiendo datos en tiempo real mediante Java EE y/ó MicroProfile? Las posibilidades son infinitas...

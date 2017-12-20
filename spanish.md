
#Payara Micro en Android (Root no requerido)

¿Te has preguntado que tan pequeño es Payara Micro?

Al inicio comencé con un divertido experimento instalando una aplicación en mi teléfono para poder ejecutar algunos comandos Linux en una terminal, pero una cosa llevó a la otra y terminé ejecutando una instancia de Payara Micro. A continuación te mostraré como.

Para tu comodidad, quizas quieras relizar este experimento desde tu computadora [conectandóla vía SSH con tu dispositivo Android](https://termux.com/ssh.html).

## Instalando Termux y una Distribución Linux.

La mejor applicación para emular una terminal que pude encontrar es [Termux](https://play.google.com/store/apps/details?id=com.termux), la cual es una aplicación de codigo abierto que no solo emula una terminal android, sino que provee un entorno Linux por igual.

En Termux puedes instalar paquetes, ejecutar los comandos más comunes de linux o incluso progamar procesos para que se ejecuten al encender tu dispositivo, pero una de las cosas más geniales es que puedes instalar dentro de Termux algunas de las distribuciones de linux más comunes como Arch, Debia, Fedora, Kali, Ubuntu, etc gracias a las imagenes hechas por la comunidad.

Pero solo con Termux no podemos instalar un JDK (aún), asi que instalé un [fedora 27 en Termux](https://wiki.termux.com/wiki/Fedora) ya que en mi caso, encontré que es la distribución más ligera. Para instlarlo solo debemos de escribir los siguientes comandos ( usa **f27_arm** o **f27_arm64** dependiendo de tu dispositivo).



```
pkg install wget
wget https://raw.githubusercontent.com/nmilosev/termux-fedora/master/termux-fedora.sh
bash termux-fedora.sh f27_arm
```

El proceso tarda de 10 a 30 minutos dependiendo de tu dispositivo, asi que hay que ser paciente. Una vez terminada la instalación podemos iniciar fedora con el comando **startfedora**

![alt text](http://guate-jug.net/payara-android/2.png "instaling fedora 1")
![alt text](http://guate-jug.net/payara-android/4.png "instaling fedora 2")

## Instalando OpenJDK 8

Ya dentro de nuestro entorno de Fedora, es tiempo de instalar el JDK haciendo uso del gestor de paquetes yum.

```
yum install java-1.0.8-openjdk
```
![alt text](http://guate-jug.net/payara-android/5.png "instaling JDK")

Es posible que Fedora necesite actualizar algunas dependencias antes de instalar el JDK, asi que el proceso puede tardar un poco. Es posible que al terminar el proceso deseemos probar la instalación del  JDK con el comando _java -v_ y probablemente falle, pero no te preocupes, esto es "normal" y no afecta nuestra meta de ejecutar una instancia de Payara Micro.

## Descargando Payara Micro y deployando una aplicación.
Descarguemos el archivo jar de Payara Micro, sólo copiemos el link desde [el sitio web de Payara](https://www.payara.fish/downloads) y peguemoslo en siguiente comando.
```
wget "<Pegar el link dentro de las comillas>" -O payara-micro.jar
```
Tambien es recomendable que descarguemos un archivo WAR como [este](https://www.dropbox.com/s/w573h7lajd9405w/Application.war?dl=1) para probar nuestro servidor
Este Archivo WAR es una modificación de mi [Proyecto inicial de Java EE7 / Gradle](https://github.com/Motojo/Java-EE7-Starter-Project) que solo contiene un index.html y 3 Web Services Restful.

```
<host>:<port>/api
<host>:<port>/api/list
<host>:<port>/api/query?name=&age=
```

Ya tenemos instalado un JDK, ya tenemos el ejecutable de Payara Micro y un Archivo WAR, asi que ¡Es hora del Deploy!

```
java -jar payara-micro.jar --deploy Application.war
```

![alt text](http://guate-jug.net/payara-android/6.png "all needed files")

![alt text](http://guate-jug.net/payara-android/8.png "all needed files")
Dependiendo de tu dispositivo, puede tardar varios minutos e incluso lanzar algunas excepciones (no en todos los dispositivos), pero finalmente la aplicación será deployada. En este caso lo hice en el puerto  8080, asi que podemos dirigirnos al navegador web del dispositivo para probar.

```
http://localhost:8080
```
![alt text](http://guate-jug.net/payara-android/9.png "all needed files")
![alt text](http://guate-jug.net/payara-android/10.png "all needed files")
![alt text](http://guate-jug.net/payara-android/11.png "all needed files")

## Exponiendo nuestro Servidor fuera de la red local
En el caso que desees presumir con tus amigos tu nuevo servidor JavaEE portable , puedes instalar un servicio llamado  [Ngrok](https://ngrok.com) .

Unicamente necesitas descargar la [version ARM](https://ngrok.com/download) e instalarla en tu terminal de Termux.
Una vez instalado, puedes exponer tu servidor con el comando

```
./ngrok http 8080
```
Este te dará una URL en la cual puedes acceder a tu servidor. ¿Genial no?

![alt text](http://guate-jug.net/payara-android/12.png "ngrok")



## Notas Finales
En este momento, el proceso para lograr ejecutar una instancia de Payara Micro en nuestro dispositivo Android es complicado, pero quizás en el futuro podamos empacar todo en un instalador para que sea más facil ejecutar un servidor JavaEE en tu dispositivo.

Dependiendo de las especificaciones técnicas de tu dispositivo, el servidor puede ser muy lento o incluso no responder, en mi caso con un Nexus 5x le tomaba hasta 5 minutos responder una petición, pero en mi tablet que tiene más Memoria RAM el servidor respondía mucho mejor.

Cada año mejoran los procesadores y el rendimiento de los dispositivos móviles, asi que ¿Podemos pensar a futuro en una flota distribuida de  servidores JavaEE comunicandose con distpositivos IoT con microservicios? ¡Sería Genial!

¿O quizas dispositivos cercanos compartiendo datos en tiempo real usando todos los beneficios de JavaEE? Las posibilidades son infinitas.

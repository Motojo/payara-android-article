
# How to Run Payara Micro on Android (No Root required)

Do you ever wondered how micro is Payara micro?

At first, this was a funny experiment installing a terminal app on my android phone and playing around with some linux commands.
One thing lead to another and ended running a Payara Micro instance on my device. I gonna show you how to do that.

For convenience you may want to do all this proccess [using ssh into your device](https://termux.com/ssh.html) and your computer keyboard.

## Installing Termux and a Linux distro.
The best terminal app I found is [Termux](https://play.google.com/store/apps/details?id=com.termux). wich is an open source terminal emulator and linux enviroment.
You can install packages, run most of the common linux commands, or even set processes when you boot up your phone, but one of the most amazing things is that you can install in your Termux enviroment some community made images of Linux's distros like Arch, Debian, Fedora, Kali, Ubuntu, etc. 

Just with Termux you can't install a JDK (yet) so after several tests, I installed [fedora 27 at Termux](https://wiki.termux.com/wiki/Fedora) because I found that is one of the Lightweight distros for this case. To achieve this just type this commands in Termux ( use **f27_arm** or **f27_arm64** depending your device)

```
pkg install wget
wget https://raw.githubusercontent.com/nmilosev/termux-fedora/master/termux-fedora.sh
bash termux-fedora.sh f27_arm
```

This will take from 10 to 30 minutes depending on your device, so be patient. After the instalation is complete start the fedora enviroment with **startfedora**

![alt text](http://guate-jug.net/payara-android/2.png "instaling fedora 1")
![alt text](http://guate-jug.net/payara-android/4.png "instaling fedora 2")
## Installing OpenJDK 8

Now we are inside our fedora 27 enviroment, so we are going to install the JDK8 from the openJDK using yum package manager.

```
yum install java-1.0.8-openjdk
```
![alt text](http://guate-jug.net/payara-android/5.png "instaling JDK")

Fedora might want to update some dependencies before installing the JDK, so this will take a while. 
If you check java version with _java-v_ after installing the JDK it gonna fail so don’t worry about it, doesn't affect our goal to run an instance of Payara Micro.

## Downloading Payara Micro and and deploying an application.
Let’s download the Payara Micro jar, just copy the link from [Payara website](https://www.payara.fish/downloads) and paste it into this command.
```
wget "<Paste link between quotes>" -O payara-micro.jar
```

Is recommended to also download a WAR file like [this](https://www.dropbox.com/s/w573h7lajd9405w/Application.war?dl=1) to test our server.
This WAR is a modified project of my [JavaEE 7 / Gradle starter project](https://github.com/Motojo/Java-EE7-Starter-Project) with just an index.html and 3 Restful WebServices 

```
<host>:<port>/api
<host>:<port>/api/list
<host>:<port>/api/query?name=&age=
```

Now we have a JDK installed, the payara-micro jar file and a WAR file to deploy, so is deploy time!

```
java -jar payara-micro.jar --deploy Application.war
```

![alt text](http://guate-jug.net/payara-android/6.png "all needed files")

![alt text](http://guate-jug.net/payara-android/8.png "all needed files")
Depending your device this will take several minutes, and throw some exceptions (not on every device) and finally your application will be deployed.
In this case, I did it on the port 8080, so go to the device web browser and type

```
http://localhost:8080
```
![alt text](http://guate-jug.net/payara-android/9.png "all needed files")
![alt text](http://guate-jug.net/payara-android/10.png "all needed files")
![alt text](http://guate-jug.net/payara-android/11.png "all needed files")

## Exposing your server outside your local Network
In the caseyou want to show your new portable JavaEE server to your friends you can install a service called [Ngrok](https://ngrok.com) 

You need to download the [ARM version](https://ngrok.com/download) and install it on your termux terminal.
Once installed you can expose your server with the command

```
./ngrok http 8080
```
and this will give you an URL to see your server from the internet. isn't that cool?

![alt text](http://guate-jug.net/payara-android/12.png "ngrok")



## Final Notes
At this time the procces to achieve this is still too complicated, maybe in the future this can be packed in an easy way to install and run a JavaEE server in your phone.
Of course that depending your device specs, the server can be too slow or even unresponsive, in my case with Nexus 5x took up to 5 minutes to answer, but on my tablet with more memory it runs faster.

With increasing performance in mobile processors each year maybe we can think about a distribuited / mobile fleet of JavaEE servers comunicating with IoT devices using microservices?  Imagine that!
Nearby devices sharing real time data powered by JavaEE? the posibillities are unlimited...

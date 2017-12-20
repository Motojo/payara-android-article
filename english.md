
# How to Run Payara Micro on Android (No Root required)

Do you ever wondered how micro is Payara micro?

At first, this was a fun experiment installing a terminal app on my Android phone and playing around with some Linux commands. One thing lead to another and I ended running a Payara Micro instance on my device. I'm gonna show you how to do that.

For convenience's sake you may want to run this experiment from the comfort of your PC by [connecting it via SSH to your device](https://termux.com/ssh.html).

## Installing Termux and a Linux distribution.

The best terminal app I found is [Termux](https://play.google.com/store/apps/details?id=com.termux), which is an open source terminal emulator that provides a Linux environment. This means that you can install packages, run common Linux commands, and even configure startup processes when you boot up your phone. One of the most amazing things you can do with it is install community-made images of Linux's distributions like Arch, Debian, Fedora, Kali, Ubuntu, etc.

However, even with Termux you can't install a JDK yet. After several tests, I installed [Fedora 27 in Termux](https://wiki.termux.com/wiki/Fedora) since I realized that is the most lightweight distribution for this requirement. To install it, just type the following commands in Termux ( use **f27_arm** or **f27_arm64** depending your device):

    pkg install wget
    wget https://raw.githubusercontent.com/nmilosev/termux-fedora/master/termux-fedora.sh
    bash termux-fedora.sh f27_arm

This operation will take from 10 to 30 minutes depending on your device, so be patient. After the installation is complete start the Fedora environment with the `startfedora` command:

![alt text](http://guate-jug.net/payara-android/2.png "instaling fedora 1")
![alt text](http://guate-jug.net/payara-android/4.png "instaling fedora 2")

## Installing OpenJDK 8

Now that we are inside of our **Fedora 27** environment, we are going to install OpenJDK v8 using the YUM package manager:

    yum install java-1.0.8-openjdk

![alt text](http://guate-jug.net/payara-android/5.png "instaling JDK")

Fedora might want to update some dependencies before installing the JDK, so this might take a while.
If you check the Java version with `java -v` after installing the JDK, it will fail; so don’t worry about it since it doesn't affect our goal to run an instance of Payara Micro.

## Downloading Payara Micro and deploying an application.

Let’s download the Payara Micro jar. To do this, just copy the distribution link from the [Payara website](https://www.payara.fish/downloads) and use it with the following command:

    wget "<PAYARA_MICRO_DOWNLOAD_URL>" -O payara-micro.jar

Is recommended to also download a WAR file like [this one](https://www.dropbox.com/s/w573h7lajd9405w/Application.war?dl=1) to test the server.

This WAR is a modified project of my [JavaEE 7 / Gradle starter project](https://github.com/Motojo/Java-EE7-Starter-Project) with just an _index.html_ page and 3 sample RESTful web services:

    <host>:<port>/api
    <host>:<port>/api/list
    <host>:<port>/api/query?name=&age=

Now that we have a JDK installed, the _payara-micro_ JAR file and a sample WAR artifact, it is deployment time!

    java -jar payara-micro.jar --deploy Application.war

![alt text](http://guate-jug.net/payara-android/6.png "all needed files")

![alt text](http://guate-jug.net/payara-android/8.png "all needed files")

Depending on your device this will take several minutes, and may throw some exceptions, but your application will be deployed nonetheless.

In this case, the server is listening on port **8080** by default, so go to the device web browser and launch the http://localhost:8080 website:

![alt text](http://guate-jug.net/payara-android/9.png "all needed files")
![alt text](http://guate-jug.net/payara-android/10.png "all needed files")
![alt text](http://guate-jug.net/payara-android/11.png "all needed files")

## Open the Payara Micro server outside your local Network

In the case you want to show your new portable Payara server to your friends you can install a service called [Ngrok](https://ngrok.com).

To do this, you need to download the [ARM version](https://ngrok.com/download) and install it on your Termux terminal. Once installed you can open your server with the following command:


    ./ngrok http 8080

Which will give you an URL to see your server from the Internet. Isn't it cool?

![alt text](http://guate-jug.net/payara-android/12.png "ngrok")

## Final Notes

At this time the steps to achieve all of this are a bit convoluted, so maybe in the future the entire process can be simplified to ease running a Payara server in your Phone.

Remember that all of this depends on your device specifications, which means that the server can run too slow or become unresponsive. Using my **Nexus 5X** smartphone, it took up to 5 minutes for the Payara Server's homepage to load, but on a separate tablet with more RAM the page loaded faster.

With an ever increasing performance in mobile processors each year, maybe we can start to think about creating a distributed-mobile fleet of Payara Micro Servers that talks to several **IoT** devices using an array of micro-services?  Can you imagine that!?

Nearby devices sharing real time data powered by Java EE and/or MicroProfile? The possibilities are unlimited...

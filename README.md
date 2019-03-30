# Java 8 (1.8.0_202) JDK + Maven 3.6 + Python 3.5/2.7 + pip 19.0 + node 11.11 + npm 6.7 + Gradle 5.2 + noVNC/VNC (as Cluster Container Desktop)
[![](https://images.microbadger.com/badges/image/openkbs/jdk-mvn-py3-vnc.svg)](https://microbadger.com/images/openkbs/jdk-mvn-py3-vnc "Get your own image badge on microbadger.com") [![](https://images.microbadger.com/badges/version/openkbs/jdk-mvn-py3-vnc.svg)](https://microbadger.com/images/openkbs/jdk-mvn-py3-vnc "Get your own version badge on microbadger.com")

# License Agreement
By using this image, you agree the [Oracle Java JDK License](http://www.oracle.com/technetwork/java/javase/terms/license/index.html).
This image contains [Oracle JDK 8](http://www.oracle.com/technetwork/java/javase/downloads/index.html). You must accept the [Oracle Binary Code License Agreement for Java SE](http://www.oracle.com/technetwork/java/javase/terms/license/index.html) to use this image.

# Components
* VNC/noVNC for both browswer-based VNC and VNC-client to connect to use Desktop GUI from Docker container.
* java version "1.8.0_202"
  Java(TM) SE Runtime Environment (build 1.8.0_202-b08)
  Java HotSpot(TM) 64-Bit Server VM (build 25.202-b08, mixed mode)
* Apache Maven 3.6.0
* Python 3.5.2 / Python 2.7.12 + pip 19.0.2
* Node v11.9.0 + npm 6.5.0 (from NodeSource official Node Distribution)
* Gradle 5.1* Other tools: git wget unzip vim python python-setuptools python-dev python-numpy 
* VNC/noVNC for remote Desktop over Container Platform (Openshift, Kubernetes, etc.) 
* Google-Chrome 73.0 + Firefox 66.0.2
* 
# Note:
This project mainly adopt the [ConSol docker-headless-vnc-container](https://github.com/ConSol/docker-headless-vnc-container) implementation.

# Run (recommended for easy-start)
It's highly recommended to change vnc password to prevent others using the default password to get into your container, modify the file "**./docker.env**" as below and save the file before you hit, "**./run.sh**":
```
(./docker.env) file:

#### ---- VNC Password: (default is vncpassword) ----
VNC_PW=VeryXtrongPasswordHere!

#### ---- VNC Resolution (1280x800, 1920x1080, etc.): ----
VNC_RESOLUTION=1920x1080

```
* Once the above update is done, you can now run the command below.
```
./run.sh
```
## Connect to VNC Viewer/Client or noVNC (Browser-based VNC)
* connect via VNC viewer localhost:5901, default password: vncpassword
* connect via noVNC HTML5 full client: http://localhost:6901/vnc.html, default password: vncpassword
* connect via noVNC HTML5 lite client: http://localhost:6901/?password=vncpassword

Once it is up, the default password is "'''vncpassword'''" to access with your web browser:
```
http://<ip_address>:6901/vnc.html,
e.g.
=> Standalone Docker: http://localhost:6901/vnc.html
=> Openshift Container Platform: http://<route-from-openshift>/vnc.html
=> similarly for Kubernetes Container Platform: (similar to the Openshift above!)
```
# Run - Override VNC environment variables 
The following VNC environment variables can be overwritten at the docker run phase to customize your desktop environment inside the container. You can change those variables using configurations CLI or Web-GUI with OpenShift, Kubernetes, DC/OS, etc.
```
VNC_COL_DEPTH, default is 24 , e.g., change to 16,
    -e VNC_COL_DEPTH=16
VNC_RESOLUTION, default: 1920x1080 , e.g., change to 1280x1024
    -e VNC_RESOLUTION=1280x1024
VNC_PW, default: vncpassword , e.g., change to MySpecial!(Password%)
    -e VNC_PW=MySpecial!(Password%)
```

# Screen (Desktop) Resolution
Two ways to change Screen resolutions.

## 1.) Modify ./docker-run.env file
```
#VNC_RESOLUTION="1280x1024"
VNC_RESOLUTION=1920x1280
```

## 2.) Customize Openshift or Kubernetes container run envionrment
```
Set up, say, VNC_RESOLUTION with value 1920x1280
```

# Base the image to build add-on components

```Dockerfile
FROM openkbs/jdk-mvn-py3-vnc
```

# Run the image

Then, you're ready to run:
- make sure you create your work directory, e.g., ./data

```bash
mkdir ./data
docker run -d --name my-jdk-mvn-py3-vnc -v $PWD/data:/data -i -t openkbs/jdk-mvn-py3-vnc
```

# Build and Run your own image
Say, you will build the image "my/jdk-mvn-py3-vnc".

```bash
docker build -t my/jdk-mvn-py3-vnc .
```

To run your own image, say, with some-jdk-mvn-py3-vnc:

```bash
mkdir ./data
docker run -d --name some-jdk-mvn-py3-vnc -v $PWD/data:/data -i -t my/jdk-mvn-py3
```

## Shell into the Docker instance

```bash
docker exec -it some-jdk-mvn-py3-vnc /bin/bash
```

# Corporate Proxy Root and Intemediate Certificates setup for System and Web Browsers (FireFox, Chrome, etc)
1. Save your corporate's Certificates in the currnet GIT directory, `./certificates`
2. During Docker run command, 
```
   -v `pwd`/certificates:/certificates ... (the rest parameters)
```
If you want to map to different directory for certificates, e.g., /home/developer/certificates, then
```
   -v `pwd`/certificates:/home/developer/certificates -e SOURCE_CERTIFICATES_DIR=/home/developer/certificates ... (the rest parameters)
```
3. And, inside the Docker startup script to invoke the `~/scripts/setup_system_certificates.sh`. Note that the script assumes the certficates are in `/certificates` directory.
4. The script `~/scripts/setup_system_certificates.sh` will automatic copy to target directory and setup certificates for both System commands (wget, curl, etc) to use and Web Browsers'.

# Reference
* [VNC / NoVNC](https://github.com/novnc/noVNC)
* [ConSol docker-headless-vnc-container](https://github.com/ConSol/docker-headless-vnc-container)
* [Running GUI apps in Docker containers using VNC](http://blog.fx.lv/2017/08/running-gui-apps-in-docker-containers-using-vnc/)
* [Docker-headless-VNC-Container](https://github.com/DrSnowbird/docker-headless-vnc-container)
* [NoVNC in CentOS](https://www.server-world.info/en/note?os=CentOS_7&p=x&f=10)

# See also X11 and VNC/noVNC docker-based IDE collections
* [openkbs/docker-atom-editor](https://hub.docker.com/r/openkbs/docker-atom-editor/)
* [openkbs/eclipse-oxygen-docker](https://hub.docker.com/r/openkbs/eclipse-oxygen-docker/)
* [openkbs/eclipse-phonto-vnc-docker](https://cloud.docker.com/u/openkbs/repository/docker/openkbs/eclipse-photon-vnc-docker)
* [openkbs/eclipse-photon-docker](https://hub.docker.com/r/openkbs/eclipse-photon-docker/)
* [openkbs/eclipse-photon-vnc-docker](https://hub.docker.com/r/openkbs/eclipse-photon-vnc-docker/)
* [openkbs/intellj-docker](https://hub.docker.com/r/openkbs/intellij-docker/)
* [openkbs/intellj-vnc-docker](https://hub.docker.com/r/openkbs/intellij-vnc-docker/)
* [openkbs/knime-vnc-docker](https://cloud.docker.com/u/openkbs/repository/docker/openkbs/knime-vnc-docker)
* [openkbs/mysql-workbench-vnc-docker](https://cloud.docker.com/u/openkbs/repository/docker/openkbs/mysql-workbench-vnc-docker)
* [openkbs/netbeans10-docker](https://hub.docker.com/r/openkbs/netbeans10-docker/)
* [openkbs/netbeans](https://hub.docker.com/r/openkbs/netbeans/)
* [openkbs/papyrus-sysml-docker](https://hub.docker.com/r/openkbs/papyrus-sysml-docker/)
* [openkbs/pycharm-docker](https://hub.docker.com/r/openkbs/pycharm-docker/)
* [openkbs/pycharm-vnc-docker](https://cloud.docker.com/u/openkbs/repository/docker/openkbs/pycharm-vnc-docker)
* [openkbs/rapidminer-docker](https://cloud.docker.com/u/openkbs/repository/docker/openkbs/rapidminer-docker)
* [openkbs/scala-ide-docker](https://hub.docker.com/r/openkbs/scala-ide-docker/)
* [openkbs/sublime-docker](https://hub.docker.com/r/openkbs/sublime-docker/)
* [openkbs/webstorm-docker](https://hub.docker.com/r/openkbs/webstorm-docker/)
* [openkbs/webstorm-vnc-docker](https://cloud.docker.com/u/openkbs/repository/docker/openkbs/webstorm-vnc-docker)
* [openkbs/rest-dev-vnc-docker](https://cloud.docker.com/u/openkbs/repository/docker/openkbs/rest-dev-vnc-docker)

# See Also - Docker-based SQL GUI
* [Sqlectron SQL GUI at openkbs/sqlectron-docker](https://hub.docker.com/r/openkbs/sqlectron-docker/)
* [Mysql-Workbench at openkbs/mysql-workbench](https://hub.docker.com/r/openkbs/mysql-workbench/)
* [PgAdmin4 for PostgreSQL at openkbs/pgadmin-docker](https://hub.docker.com/r/openkbs/pgadmin-docker/)

# Releases information
```
developer@cf5bc1c3dd9f:~$ /usr/printVersions.sh 
+ echo JAVA_HOME=/usr/java
JAVA_HOME=/usr/java
+ java -version
java version "1.8.0_202"
Java(TM) SE Runtime Environment (build 1.8.0_202-b08)
Java HotSpot(TM) 64-Bit Server VM (build 25.202-b08, mixed mode)
+ mvn --version
Apache Maven 3.6.0 (97c98ec64a1fdfee7767ce5ffb20918da4f719f3; 2018-10-24T18:41:47Z)
Maven home: /usr/apache-maven-3.6.0
Java version: 1.8.0_202, vendor: Oracle Corporation, runtime: /usr/jdk1.8.0_202/jre
Default locale: en_US, platform encoding: UTF-8
OS name: "linux", version: "4.15.0-46-generic", arch: "amd64", family: "unix"
+ python -V
Python 2.7.12
+ python3 -V
Python 3.5.2
+ pip --version
pip 19.0.3 from /usr/local/lib/python3.5/dist-packages/pip (python 3.5)
+ pip3 --version
pip 19.0.3 from /usr/local/lib/python3.5/dist-packages/pip (python 3.5)
+ gradle --version

Welcome to Gradle 5.2.1!

Here are the highlights of this release:
 - Define sets of dependencies that work together with Java Platform plugin
 - New C++ plugins with dependency management built-in
 - New C++ project types for gradle init
 - Service injection into plugins and project extensions

For more details see https://docs.gradle.org/5.2.1/release-notes.html


------------------------------------------------------------
Gradle 5.2.1
------------------------------------------------------------

Build time:   2019-02-08 19:00:10 UTC
Revision:     f02764e074c32ee8851a4e1877dd1fea8ffb7183

Kotlin DSL:   1.1.3
Kotlin:       1.3.20
Groovy:       2.5.4
Ant:          Apache Ant(TM) version 1.9.13 compiled on July 10 2018
JVM:          1.8.0_202 (Oracle Corporation 25.202-b08)
OS:           Linux 4.15.0-46-generic amd64

+ npm -v
6.7.0
+ node -v
v11.11.0
+ cat /etc/lsb-release /etc/os-release
DISTRIB_ID=Ubuntu
DISTRIB_RELEASE=16.04
DISTRIB_CODENAME=xenial
DISTRIB_DESCRIPTION="Ubuntu 16.04.3 LTS"
NAME="Ubuntu"
VERSION="16.04.3 LTS (Xenial Xerus)"
ID=ubuntu
ID_LIKE=debian
PRETTY_NAME="Ubuntu 16.04.3 LTS"
VERSION_ID="16.04"
HOME_URL="http://www.ubuntu.com/"
SUPPORT_URL="http://help.ubuntu.com/"
BUG_REPORT_URL="http://bugs.launchpad.net/ubuntu/"
VERSION_CODENAME=xenial
UBUNTU_CODENAME=xenial
```


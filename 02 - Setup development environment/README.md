# Setup development environment
The goal is to develop a Talend connector taht will read / write from/to _Redis_.


## Installing tools
### Talend connectors use Java
The _TCK_ (_Talend Component Kit_) is a Java framework. Please install java on your environment. I will not explain how to install it since it depends what operating system you're using.

I use a Java Oracle implementation but should be ok with openJdk:
- https://www.oracle.com/fr/java/technologies/javase/javase8u211-later-archive-downloads.html
- https://openjdk.java.net/install/

```
$ java -version
java version "1.8.0_211"
Java(TM) SE Runtime Environment (build 1.8.0_211-b12)
Java HotSpot(TM) 64-Bit Server VM (build 25.211-b12, mixed mode)
```

### A new maven project
We will use maven to describe and build the project:
- https://maven.apache.org/

I will not explain how to install it since it depends what operating system you're using.

```
$ mvn --version
Apache Maven 3.8.1 (05c21c65bdfed0f71a2f2ada8b84da59348c4c5d)
Maven home: /opt/apache-maven/apache-maven-3.8.1
Java version: 1.8.0_211, vendor: Oracle Corporation, runtime: /opt/java/oracle/jdk1.8.0_211/jre
Default locale: en_US, platform encoding: UTF-8
OS name: "linux", version: "5.4.0-80-generic", arch: "amd64", family: "unix"
```

### Do I need an IDE ?
On mys side I use Intellij :
- https://www.jetbrains.com/idea/

```
IntelliJ IDEA 2019.1.1 (Community Edition)
Build #IC-191.6707.61, built on April 16, 2019
JRE: 1.8.0_202-release-1483-b44 amd64
JVM: OpenJDK 64-Bit Server VM by JetBrains s.r.o
Linux 5.4.0-80-generic
```
But it is not needed for this training. A simple text editor will be ok, you have choices:
- https://atom.io/
- https://www.sublimetext.com/
- https://www.vim.org/download.php
- ...

All maven commands will be executed in a shell.

## Creating the project

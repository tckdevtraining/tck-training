# Setup development environment
The goal is to develop a Talend connector taht will read / write from/to _Redis_.


## Installing tools
### Talend connectors use Java
The _TCK_ (_Talend Component Kit_) is a Java framework. Please install java on your environment. I will not explain how to install it since it depends what operating system you're using.

I use a Java Oracle implementation but should be ok with openJdk:
- https://www.oracle.com/fr/java/technologies/javase/javase8u211-later-archive-downloads.html
- https://openjdk.java.net/install/

```bash
$ java -version
java version "1.8.0_211"
Java(TM) SE Runtime Environment (build 1.8.0_211-b12)
Java HotSpot(TM) 64-Bit Server VM (build 25.211-b12, mixed mode)
```

### A new maven project
We will use maven to describe and build the project:
- https://maven.apache.org/

I will not explain how to install it since it depends what operating system you're using.

```bash
$ mvn --version
Apache Maven 3.8.1 (05c21c65bdfed0f71a2f2ada8b84da59348c4c5d)
Maven home: /opt/apache-maven/apache-maven-3.8.1
Java version: 1.8.0_211, vendor: Oracle Corporation, runtime: /opt/java/oracle/jdk1.8.0_211/jre
Default locale: en_US, platform encoding: UTF-8
OS name: "linux", version: "5.4.0-80-generic", arch: "amd64", family: "unix"
```

### Do I need an IDE ?
On my side I use Intellij :
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
First, I create a folder that will contain the development of the _Redis_ connector:
```bash
$ mkdir -p ~/dev/training/tck/redis
$ cd ~/dev/training/tck/redis
$ # We then create maven structure
$ touch pom.xml
$ mkdir -p src/main/java/org/tckdev/training/redis
$ mkdir -p src/main/resources/org/tckdev/training/redis
$ mkdir -p src/test/java/org/tckdev/training/redis
```
Edit the _pom.xml_ file and copy/past the following content:
```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="           http://maven.apache.org/POM/4.0.0           http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <groupId>org.tckdev.training</groupId>
  <artifactId>redis</artifactId>
  <version>1.0.0-SNAPSHOT</version>
  <packaging>jar</packaging>
  <name>Redis TCK Connector</name>
  <description>A Talend connector to deal with Redis.</description>
  <properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <!-- Talend Component Kit current version is 1.35.1. -->
    <tck.version>1.35.1</tck.version>
    <junit5.version>5.7.0</junit5.version>
    <!-- Configuration of Talend component maven plugin -->
    <!-- I deactivate some of default validations at first -->
    <talend.validation.dataset>false</talend.validation.dataset>
    <talend.validation.svg>false</talend.validation.svg>
  </properties>
  <dependencies>
    <dependency>
      <!-- This is the Talend Component Kit API dependency we will use to write the Redis connector. -->
      <groupId>org.talend.sdk.component</groupId>
      <artifactId>component-api</artifactId>
      <version>${tck.version}</version>
      <scope>provided</scope>
    </dependency>
    <dependency>
      <!-- Talend Component Kit also provided facilities for unit tests. -->
      <groupId>org.talend.sdk.component</groupId>
      <artifactId>component-runtime-junit</artifactId>
      <version>${tck.version}</version>
      <scope>test</scope>
    </dependency>
    <dependency>
      <groupId>org.junit.jupiter</groupId>
      <artifactId>junit-jupiter</artifactId>
      <version>${junit5.version}</version>
      <scope>test</scope>
    </dependency>
  </dependencies>
  <build>
    <extensions>
      <extension>
        <!-- Talend Component Kit provides a maven extension to validate development and generate some stuff. -->
        <groupId>org.talend.sdk.component</groupId>
        <artifactId>talend-component-maven-plugin</artifactId>
        <version>${tck.version}</version>
      </extension>
    </extensions>
    <plugins>
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-compiler-plugin</artifactId>
        <version>3.8.1</version>
        <configuration>
          <!-- We will develop in Java 8. -->
          <source>1.8</source>
          <target>1.8</target>
          <forceJavacCompilerUse>true</forceJavacCompilerUse>
          <compilerId>javac</compilerId>
          <fork>true</fork>
          <compilerArgs>
            <arg>-parameters</arg>
          </compilerArgs>
        </configuration>
      </plugin>
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-surefire-plugin</artifactId>
        <version>3.0.0-M4</version>
        <configuration>
          <trimStackTrace>false</trimStackTrace>
          <runOrder>alphabetical</runOrder>
        </configuration>
      </plugin>
    </plugins>
  </build>
</project>
```
We currently can't test the build of the project since there is no source yet.

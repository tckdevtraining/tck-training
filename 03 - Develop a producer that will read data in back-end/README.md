# Develop a producer that will read data in back-end
The development of the TCK producer will be done in several step. First, we will create a simple TCK emitter that will connect to _Redis_ and a pre-defined key. It will be tested within a unit test. It will be like POC to access _Redis_. Once it is successful, we will complete the emitter with it configuration (_@Datastore_, _@Dataset_), some services etc...

## A simple emitter as a POC
Reading the TCK documentation, we understand that to read data in a backend and generate records, we have to develop a _mapper_ taht will create _emitter_. It is very efficient if your backend is clusterized. Indeed each _emitter_ will be able to deal with on cluster, and then, parallelization become possible:
- https://talend.github.io/component-runtime/main/1.35.1/component-define-input.html

For a first POC, we will not try to do such optimization. In that case, TCK framework has a shortcut to only develop the emitter:
- https://talend.github.io/component-runtime/main/1.35.1/component-registering.html#_component_type_and_name

### Which java client will we use ?
_Redis_ web site list a variety of clients, and so for Java language :
- https://redis.io/clients#java

The _Lettuce_ seems to be very advanced and well maintened client:
- https://lettuce.io/

Here is its _Getting started_ page:
- https://github.com/lettuce-io/lettuce-core/wiki/Getting-started

#### Adding Lettuce in our _pom.xml_
So, open the _pom.xml_ and in _<dependencies>_ section add :
```
<dependency>
    <groupId>io.lettuce</groupId>
    <artifactId>lettuce-core</artifactId>
    <version>6.1.0.RELEASE</version>
</dependency>
```

###
First, we will create the emitter class:
```
$ mkdir -p src/main/java/org/tckdev/training/redis/emitter
$ touch src/main/java/org/tckdev/training/redis/emitter/RedisEmitter.java
```
Open _RedisEmitter.java_ file and copy/past following code:

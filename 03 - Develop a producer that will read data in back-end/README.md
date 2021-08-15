# Develop a producer that will read data in back-end
The development of the TCK producer will be done in several steps. First, we will create a simple TCK emitter that will connect to _Redis_ and a pre-defined key. It will be tested within a unit test. It will be like a POC to access _Redis_. Once it is successful, we will complete the emitter with it configuration (_@Datastore_, _@Dataset_), some services etc...

## Follow each steps using git
The project is hosted in `github`. Please clone it locally:
```bash
$ git clone https://github.com/tckdevtraining/tck-redis.git
```
Each step is tagged. So it will be easy to follow the training, just checkout the desired tag, for instance:
```bash
$ git checkout 03-develop-emitter_POC
```

## A simple emitter as a POC
Get the code for this part:
```bash
$ git checkout 03-develop-emitter_POC
```

Reading the TCK documentation, we understand that to read data in a backend and to generate records, we have to develop a _mapper_ that will create _emitter_. It is very efficient if your backend is clusterized. Indeed each _emitter_ will be able to deal with on cluster, and then, parallelization become possible:
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
So, open the _pom.xml_ file. You should see in `<dependencies>` section the reference to _lettuce_ library:
```xml
<dependency>
    <groupId>io.lettuce</groupId>
    <artifactId>lettuce-core</artifactId>
    <version>${lettuce.version}</version>
</dependency>
```
In `<properties>` section we set the _lettuce_ version and we also disabled some _talend-component-maven-plugin_ validations that would block us at that time:
```xml
<properties>
    [...]
    <lettuce.version>6.1.0.RELEASE</lettuce.version>
    <!-- Configuration of Talend component maven plugin -->
    <!-- I deactivate some of default validations at first -->
    <talend.validation.dataset>false</talend.validation.dataset>
    <talend.validation.svg>false</talend.validation.svg>
</properties>
```

### First, we must define `package-info.java` file
This file is mandatory and is used to register the `container` (_connectors, processors, services, etc..._) in the components manager.
```bash
$ mkdir -p src/main/java/org/tckdev/training/redis/
$ touch src/main/java/org/tckdev/training/redis/package-info.java
```
This file is short, so I show it completely. For other files, I will only include extracts. Here is its content:
```java
@Components(family = "Redis", categories = "Databases NoSQL")
@Icon(value = Icon.IconType.CUSTOM, custom = "redis-logo")
[...]
```
We define the family, categories and the icon we want to use. The icon must be copied into `resources/icons`.

### Then, we will create the emitter class:
The emitter class is `org.tckdev.training.redis.emitter.RedisEmitter`. It is just a quick test to access the redis database. The class ha sthe `@Emitter` annotation giving it a name. TCK doesn't use java interfaces to define life cycle but annotate method. So you can see a `@PostConstruct` and a `@Producer`. The first one is called each time the emitter is deserialized in a runtime environment (_for instance a Spark node_) and the second one is called until it return `null` (_`batch` mode behavior_).

Here is the code used to connect to _redis_:
```java
RedisClient redisClient = RedisClient.create("redis://default:aze123_@localhost:6379/0");
StatefulRedisConnection<String, String> connection = redisClient.connect();
syncCommands = connection.sync();
```
In the `@Producer` we read a value from _redis_ and then, we create a tck record that we return:
```java
final String my_key_value = syncCommands.get("my_key");

final Builder builder = recordBuilderFactory.newRecordBuilder();
builder.withString("my_key", my_key_value);

done = true;

return builder.build();
```

We use the `RecordBuilderFactory` to get the record builder. It is a service injected by the framework in the constructor:
```java
public RedisEmitter(final RecordBuilderFactory recordBuilderFactory){
    this.recordBuilderFactory = recordBuilderFactory;
}
```
TCK proposes several built-in service that we can use to help our development.

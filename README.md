# Zookeeper tutorial

## Environment
1) Install idea Community edition (or Ultimate - 30 days eval. period)
    - https://www.jetbrains.com/ru-ru/idea/download/
1) Setup Zookeper: 
    - Download and unzip: http://zookeeper.apache.org/releases.html#download
    - Create folder "data" in root of unzip directory.
    - Rename __"zoo_sample.cfg"__ to __"zoo.cfg"__.
    - Find __"dataDir"__ variable in file __"zoo.cfg"__ and change to path of created __"data"__ folder.
    - Open folder "bin".
    - Execute __zkserver__.
1) Create Spring project
    - https://start.spring.io/#!type=maven-project&language=java&platformVersion=2.2.6.RELEASE&packaging=jar&jvmVersion=1.8&groupId=com.example&artifactId=demo&name=demo&description=Demo%20project%20for%20Spring%20Boot&packageName=com.example.demo&dependencies=actuator,cloud-starter-zookeeper-discovery,cloud-starter-zookeeper-config,web
1) Install Idea Plugin
    - __Zoolytic__ (Zookeeper Client)
    - Restart Idea
    - Choose View -> Tool Windows -> Zoolytic

## Example

### Create Node: 
    /config/<application_name>/<valiable_name>

## Project

### MyEndpoint    
  ```kotlin
  @RestController
  @RefreshScope
  class MyEndpoint {

      @Value("\${<valiable_name>}")
      lateinit var springVariable: String

      @GetMapping("/")
      fun home(): String {
          return springVariable
      }
  }
  ```

### application.yml
```yml
server:
    port: 8081
spring:
  application:
    name: <application_name>
  cloud:
    zookeeper:
      connect-string: localhost:2181
```

## Analogs: 
 - Consul and Valut (Hashicorp)
 - Eureka (Netflix)
 - ETCD

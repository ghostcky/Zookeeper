# Zookeeper tutorial

## Environment
1) Install idea Community edition (or Ultimate - 30 days eval. period)
    - https://www.jetbrains.com/ru-ru/idea/download/
1) Install Idea Plugin
    - __Zoolytic__ (Zookeeper Client)
    - Restart Idea
    - Choose View -> Tool Windows -> Zoolytic
1) Setup Zookeper (https://medium.com/@shaaslam/installing-apache-zookeeper-on-windows-45eda303e835): 
    - Download and unzip: http://zookeeper.apache.org/releases.html#download
    - Create folder "data" in root of unzip directory.
    - Rename __"zoo_sample.cfg"__ to __"zoo.cfg"__.
    - Find __"dataDir"__ variable in file __"zoo.cfg"__ and change to path of created __"data"__ folder.
    - Open folder "bin".
    - Execute __zkserver__.
    
------

## Examples:
## ZNode

### Create First Spring project
https://start.spring.io/#!type=maven-project&language=kotlin&platformVersion=2.2.6.RELEASE&packaging=jar&jvmVersion=1.8&groupId=ru.liberteam.ghostcky&artifactId=Service1&name=Service1&description=&packageName=ru.liberteam.ghostcky.Service1&dependencies=actuator,cloud-starter-zookeeper-discovery,cloud-starter-zookeeper-config,web

### Create Node
    /config/micro_service_1/<valiable_name>

## Project

### MyEndpoint    
  ```kotlin
  @RestController
  @RefreshScope
  class MyEndpoint {

      @Value("\${<valiable_name>}")
      lateinit var springVariable: String

      @GetMapping("/")
      fun showVariable(): String {
          return "It`s first micro service. Znode: $springVariable"
      }
  }
  ```

### application.yml
```yml
server:
    port: 8081
spring:
  application:
    name: microservice1
  cloud:
    zookeeper:
      connect-string: localhost:2181
```

### CHECK call URL http://localhost:8081

-------

## Discovery service

### Create Second Spring project
https://start.spring.io/#!type=maven-project&language=kotlin&platformVersion=2.2.6.RELEASE&packaging=jar&jvmVersion=1.8&groupId=ru.liberteam.ghostcky&artifactId=Service2&name=Service2&description=&packageName=ru.liberteam.ghostcky.Service2&dependencies=actuator,cloud-starter-zookeeper-discovery,cloud-starter-zookeeper-config,web

    Add annotation @EnableDiscoveryClient on Service2Application class (on starter class)

### pom.xml

    Add dependancy: 
    
    ```xml
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-starter-feign</artifactId>
        <version>1.4.7.RELEASE</version>
    </dependency>
    ```

### MyDiscoveryEndpoint
  ```kotlin
  @RestController
  class MyDiscoveryEndpoint {

    @Autowired
    lateinit var firstServiceClient: FirstServiceClient;

    @GetMapping("/discovery-example")
    fun discoveryExample(): String {
        return "It`s second service. Znode from First Service: ${firstServiceClient.show()}";
    }
  }
  ```

### FirstServiceClient
  ```kotlin
  @Configuration
  @EnableFeignClients
  @EnableDiscoveryClient
  class FirstServiceClient {

    @Autowired
    lateinit var theClient: TheClient;

    @FeignClient(name = "microservice1")
    interface TheClient {

        @RequestMapping(path = arrayOf("/"), method = arrayOf(RequestMethod.GET))
        @ResponseBody
        fun home(): String;
    }
    
    fun show(): String {
        return theClient.home();
    }
  }
  ```

### application.yml
```yml
server:
    port: 8082
spring:
  application:
    name: microservice2
  cloud:
    zookeeper:
      connect-string: localhost:2181
      discovery:
        enabled: true
```

### Edit SERVICE_1 application.yml 
- Activate discovery in properties with adding __spring.cloud.zookeeper.discovery.enabled: true__ and __management.security.enabled: false__ like you can see below:
    ```yml
    server:
        port: 8081
    spring:
      application:
        name: microservice1
      cloud:
        zookeeper:
          connect-string: localhost:2181
          discovery:
            enabled: true
    
    management:
      security:
        enabled: false
    ```

- Add annotation __@EnableDiscoveryClient__ on Service1Application class (on starter class)

### CHECK call URL http://localhost:8082/discovery-example

## Analogs: 
 - Consul and Valut (Hashicorp)
 - Eureka (Netflix)
 - ETCD

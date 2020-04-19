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
https://start.spring.io/#!type=maven-project&language=kotlin&platformVersion=2.2.6.RELEASE&packaging=jar&jvmVersion=1.8&groupId=com.example&artifactId=demo&name=demo&description=Demo%20project%20for%20Spring%20Boot&packageName=com.example.demo&dependencies=actuator,cloud-starter-zookeeper-discovery,cloud-starter-zookeeper-config,web

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
    name: micro_service_1
  cloud:
    zookeeper:
      connect-string: localhost:2181
```

-------

## Discovery service

### Create Second Spring project
https://start.spring.io/#!type=maven-project&language=kotlin&platformVersion=2.2.6.RELEASE&packaging=jar&jvmVersion=1.8&groupId=com.example&artifactId=demo&name=demo&description=Demo%20project%20for%20Spring%20Boot&packageName=com.example.demo&dependencies=actuator,cloud-starter-zookeeper-discovery,cloud-starter-zookeeper-config,web

### MyDiscoveryEndpoint
  ```kotlin
  @RestController
  class MyDiscoveryEndpoint {

    @Autowired
    lateinit var firstServiceClient: FirstServiceClient;

    @GetMapping("/discovery-example")
    fun discoveryExample(): String {
        return "It`s second service. Znode from firstservice: ${firstServiceClient.show()}";
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

    @FeignClient(name = "micro_service_1")
    interface TheClient {

        @RequestMapping(path = "/", method = RequestMethod.GET)
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
    name: micro_service_2
  cloud:
    zookeeper:
      connect-string: localhost:2181
```

## Analogs: 
 - Consul and Valut (Hashicorp)
 - Eureka (Netflix)
 - ETCD

# Zookeeper tutorial

## Environment
1) Install idea Community edition (or Ultimate - 30 days eval. period)
    - https://www.jetbrains.com/ru-ru/idea/download/
1) Generate Zookeeper Cluster: https://bit.ly/2xF8pDY
    - Sign in with Docker ID (Registration needed)
1) Create Spring project
    - https://start.spring.io/#!type=maven-project&language=java&platformVersion=2.2.6.RELEASE&packaging=jar&jvmVersion=1.8&groupId=com.example&artifactId=demo&name=demo&description=Demo%20project%20for%20Spring%20Boot&packageName=com.example.demo&dependencies=actuator,cloud-starter-zookeeper-discovery,cloud-starter-zookeeper-config,web
1) Install Idea Plugin
    - __Zoolytic__ (Zookeeper Client)

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

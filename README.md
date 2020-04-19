# Zookeeper tutorial

## Environment
1) Install idea Community edition (or Ultimate - 30 days eval. period)
https://www.jetbrains.com/ru-ru/idea/download/
1) Generate Zookeeper Cluster: https://bit.ly/2xF8pDY
    - Sign in with Docker ID (Registration needed)
1) Create Spring project
https://start.spring.io/
Get generated: https://bit.ly/2xGqJfN
<details>
  <summary>Or generate by:</summary>  
  <p>
    
### Dependencies:
- - Spring Boot Actuator
- - Apache Zookeeper Configuration
- - Apache Zookeeper Discovery
- - Spring Web
- Project: Maven Project 
- Language: Kotlin
- Spring Boot: 2.3.0 M4
- Packaging: jar
- Java: 8
    
  </p>
</details>

4) Install Idea Plugin: __Zoolytic__ (Zookeeper Client)

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

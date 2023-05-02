[Back to Home](https://github.com/springboot-microservices-project/)

# Search Master Service

- [Design](#design)
- [Tech Spesification](#tech-specification)
- [Pom.xml](#pomxml)
- [Application.yml](#applicationyml)
- [cors](#cors)

## Design
![alt text](https://github.com/springboot-microservices-project/.github/blob/main/profile/page/master-service/master-service-design.png?raw=true)


## Tech Specification
| Name  |
|----|
| Springboot 2.7^  |
| Java 11 |
| liquibase|
| eureka-client|
| rabbit-mq|
| mysql|
| spring-jpa|



## Pom.xml
```
 <dependencies>

        <!-- discovery service-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
        </dependency>


        <!-- rabbit mq-->
        <dependency>
            <groupId>org.springframework.amqp</groupId>
            <artifactId>spring-rabbit</artifactId>
            <version>2.4.6</version>
        </dependency>



        <!--        elastic search-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-elasticsearch</artifactId>
            <version>2.7.1</version>
        </dependency>
        <dependency>
            <groupId>jakarta.json</groupId>
            <artifactId>jakarta.json-api</artifactId>
            <version>2.1.0</version>
        </dependency>



        <!--        jpa-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-jpa</artifactId>
        </dependency>



        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>


        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>

        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <version>1.18.24</version>
            <scope>provided</scope>
        </dependency>

        <!-- json -->
        <dependency>
            <groupId>com.google.code.gson</groupId>
            <artifactId>gson</artifactId>
            <version>2.9.1</version>
        </dependency>

        <dependency>
            <groupId>org.json</groupId>
            <artifactId>json</artifactId>
            <version>20220924</version>
        </dependency>


        <!--Security-->
        <dependency>
            <groupId>org.springframework.security</groupId>
            <artifactId>spring-security-core</artifactId>
            <version>5.7.3</version>
        </dependency>


        <!-- base 64 -->
        <dependency>
            <groupId>commons-codec</groupId>
            <artifactId>commons-codec</artifactId>
            <version>1.15</version>
        </dependency>


        <!-- jwt -->
        <dependency>
            <groupId>com.auth0</groupId>
            <artifactId>java-jwt</artifactId>
            <version>3.4.1</version>
        </dependency>



    </dependencies>
```

## Application.yml
```

#server port http
server:
  port: 8686
  
   
 
# custom properties for elastic
application:
  elastic:
   host: localhost
   port: 9200
   username: user
   password: password

  
spring:

  # profile
  profiles:
    active: docker

  # app name
  application:
    name: schmaster-service

  # used on rabbit mq config, for allowing multiple  @bean function
  main:
    allow-bean-definition-overriding: true


  # rabbit mq
  rabbitmq:
    host: 127.0.0.1
    port: 5672
    username: guest
    password: guest
  
  
  # discovery client
eureka:
  client:
    register-with-eureka: true
    fetch-registry: true
    service-url:
      defaultZone: http://localhost:8761/eureka
    instance:
      hostname: localhost
  
  
  # CORS ORIGIN
custom:
  cors:
  Access-Control-Allow-Origin: http://localhost:8181


```


## Cors
- MainApp.java
```
    // - Implement to all microservices
    // - Http Request : Enable CORS for all  domain (*)
    @Value("${custom.cors.Access-Control-Allow-Origin}")
    String accessControlAllowOrigin;

    @Bean
    CorsConfigurationSource corsConfigurationSource() {
        CorsConfiguration configuration = new CorsConfiguration();
        // HTTP client request headers:
        //configuration.addAllowedOrigin("x-requested-with", "content-type", "Authorization", "Account-Key", "Api-Key");

        // HTTP server response headers:
        configuration.setAllowedOrigins(Arrays.asList(accessControlAllowOrigin));
        configuration.setAllowedMethods(Arrays.asList("POST", "PUT", "GET", "OPTIONS", "DELETE"));
        configuration.setAllowedHeaders(Arrays.asList("x-requested-with", "content-type", "Authorization", "Account-Key", "Api-Key"));
        UrlBasedCorsConfigurationSource source = new UrlBasedCorsConfigurationSource();
        source.registerCorsConfiguration("/**", configuration);
        return source;
    }

```



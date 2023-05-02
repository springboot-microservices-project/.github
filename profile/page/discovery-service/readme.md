[Back to Home](https://github.com/springboot-microservices-project/)

# Discovery Service

- [Design](#design)
- [Tech Spesification](#tech-specification)
- [Pom.xml](#pomxml)
- [Application.yml](#applicationyml)

## Design
![alt text](https://github.com/springboot-microservices-project/.github/blob/main/profile/page/discovery-service/discovery-service-up.png?raw=true)


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




		<!-- spring actuator metric point-->
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-actuator</artifactId>
		</dependency>

		<!--prometheus metric end point-->
		<dependency>
			<groupId>io.micrometer</groupId>
			<artifactId>micrometer-registry-prometheus</artifactId>
			<scope>runtime</scope>
		</dependency>


		<!-- eureka client -->
		<dependency>
			<groupId>org.springframework.cloud</groupId>
			<artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>
		</dependency>

		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
			<scope>test</scope>
		</dependency>
	</dependencies>
```

## Application.yml
```
#server port http
server:
  port: 8761
  
  
spring:

  # profile
  profiles:
    active: docker

    # app name
  application:
    name: discovery-service

    # used on rabbit mq config, for allowing multiple  @bean function
  main:
    allow-bean-definition-overriding: true  
  

eureka:
  client:
    register-with-eureka: false
    fetch-registry: false
```


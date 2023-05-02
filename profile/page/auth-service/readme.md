[Back to Home](https://github.com/springboot-microservices-project/)

# Auth Service

## 1. Design
![alt text](https://github.com/springboot-microservices-project/.github/blob/main/profile/page/auth-service/auth-service-architecture.jpg?raw=true)

## 2. System Spec

### 2.1 Tech Spec
| Name  |
|----|
| Springboot 2.7^  |
| Java 11 |
| jwt| 
| OAuth0|
| liquibase|
| redis| 
| mysql| 
| rabbitmq| 
| eureka-client| 


### 2.2 Librarries

| Name  | Version | 
|----|----|
| liquibase-core | latest|
| spring-boot-starter-cache | latest|
| spring-boot-starter-data-redis| latest|
| spring-data-redis| latest|
| jedis| latest|
| spring-cloud-starter-netflix-eureka-client| latest|
| commons-codec| latest|
| spring-boot-starter-security| latest|
| java-jwt| latest|
| mysql-connector-java| latest|
| spring-boot-starter-data-jpa| latest|
| spring-boot-starter-web| latest|
| spring-boot-starter-test| latest|
| lombok| latest|
| gson| latest|
| spring-security-config| latest|

### 2.3 Pom.xml
```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.7.3</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>
    <groupId>com.deni.microservices</groupId>
    <artifactId>auth</artifactId>
    <version>0.1</version>
    <name>auth-service</name>
    <description>Auth Service</description>
    <properties>
        <java.version>1.8</java.version>

        <spring-cloud.version>2021.0.5</spring-cloud.version>
    </properties>


    <!-- spring cloud gateway import-->
    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>org.springframework.cloud</groupId>
                <artifactId>spring-cloud-dependencies</artifactId>
                <version>${spring-cloud.version}</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
        </dependencies>
    </dependencyManagement>

    <dependencies>

        <!-- liquibase -->
        <dependency>
            <groupId>org.liquibase</groupId>
            <artifactId>liquibase-core</artifactId>
        </dependency>


        <!-- redis-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-cache</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-redis</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.data</groupId>
            <artifactId>spring-data-redis</artifactId>
        </dependency>

        <dependency>
            <groupId>redis.clients</groupId>
            <artifactId>jedis</artifactId>
        </dependency>


        <!-- discovery service-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
        </dependency>


        <!-- encrypt Key-Api, Account-Key -->
        <dependency>
            <groupId>commons-codec</groupId>
            <artifactId>commons-codec</artifactId>
            <version>1.15</version>
        </dependency>

        <!-- spring secuity oauth 0 basic-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-security</artifactId>
        </dependency>

        <dependency>
            <groupId>com.auth0</groupId>
            <artifactId>java-jwt</artifactId>
            <version>3.4.1</version>
        </dependency>


        <!--  mysql-->

        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <scope>runtime</scope>
        </dependency>


        <!-- jpa-->
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
        <dependency>
            <groupId>com.google.code.gson</groupId>
            <artifactId>gson</artifactId>
            <version>2.9.1</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.security</groupId>
            <artifactId>spring-security-config</artifactId>
            <version>5.7.3</version>
        </dependency>


    </dependencies>


    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>


    <profiles>
        <profile>
            <id>dev</id>
            <properties>
                <activeProperties>dev</activeProperties>
            </properties>
            <activation>
                <activeByDefault>false</activeByDefault>
            </activation>
        </profile>

        <profile>
            <id>docker</id>
            <properties>
                <activeProperties>docker</activeProperties>
            </properties>
            <activation>
                <activeByDefault>true</activeByDefault>
            </activation>

        </profile>


    </profiles>

</project>

```

### 2.4 application.yml
```
#server port http
server:
  port: 8282

spring:

# profile
  profiles:
    active: dev
    
# app name
  application:
    name: auth-service


# liquibase
  liquibase:
   change-log: classpath:changelog/changelog-master.yml


  # redis with auth basic
  redis:
    host: localhost
    port: 6379
    password: password123
    database: 0
    timeout: 10

  # jpa
  jpa:
    hibernate:
      ddl-auto: update
    show-sql: true
    properties:
      hibernate:
        format_sql: false

        dialect: org.hibernate.dialect.MySQL5InnoDBDialect

  # mysql
  datasource:
    url: jdbc:mysql://localhost:3310/db_auth
    username: user
    password: password
    driver-class-name: com.mysql.cj.jdbc.Driver

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

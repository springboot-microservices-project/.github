[Home](https://github.com/springboot-microservices-project/) /
[Gateway](https://github.com/springboot-microservices-project/.github/blob/main/profile/page/gateway-service/readme.md)

# Metric Spring actuator and Prometheus

### 1.1. Spring Actuator & Prometheus on SpringCloud Gateway
![alt text](https://github.com/springboot-microservices-project/.github/blob/main/profile/page/gateway-service/image/gateway-springactuator-prometheus.png?raw=false)

### 1.2. Spring Actuator & Prometheus configuration on SpringCloud Gateway
- pom.xml
```
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
```

- application.yml
```
# promotheus
# spring actuator config, for show all metric api
# access all metric api : http://localhost:[port]/actuator
# access promotheus metric api : http://localhost:[port]/actuator/prometheus
# access promotheus web : http://localhost:9090/graph
management:
  endpoint:
    prometheus:
      enabled: true
  endpoints:
    web:
      exposure:
        include: prometheus, metrics, info, health, shutdown, beans
  metrics:
    export:
      prometheus:
        enabled: true
```






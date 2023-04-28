[Home](https://github.com/springboot-microservices-project/) /
[Gateway](https://github.com/springboot-microservices-project/.github/blob/main/profile/page/gateway-service/readme.md)

# Spring actuator and Prometheus Metric

### 1.1. Flow of Spring Actuator & Prometheus Metric on SpringCloud Gateway
![alt text](https://github.com/springboot-microservices-project/.github/blob/main/profile/page/gateway-service/image/gw-actuator-prometheus-grafana-flow.png?raw=false)

### 1.2. List of metric APIs
```
{
    "_links": {
        "self": {
            "href": "http://localhost:8181/actuator",
            "templated": false
        },
        "prometheus": {
            "href": "http://localhost:8181/actuator/prometheus",
            "templated": false
        },
        "beans": {
            "href": "http://localhost:8181/actuator/beans",
            "templated": false
        },
        "health": {
            "href": "http://localhost:8181/actuator/health",
            "templated": false
        },
        "health-path": {
            "href": "http://localhost:8181/actuator/health/{*path}",
            "templated": true
        },
        "info": {
            "href": "http://localhost:8181/actuator/info",
            "templated": false
        },
        "metrics-requiredMetricName": {
            "href": "http://localhost:8181/actuator/metrics/{requiredMetricName}",
            "templated": true
        },
        "metrics": {
            "href": "http://localhost:8181/actuator/metrics",
            "templated": false
        }
    }
}
```

### 1.2. Example value of prometheus Metric
![alt text](https://github.com/springboot-microservices-project/.github/blob/main/profile/page/gateway-service/image/gw-metric.png?raw=true)

### 1.4. Configuration 
- **pom.xml**
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

- **application.yml**
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






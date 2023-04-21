[Back to Home](https://github.com/springboot-microservices-project/)

# Gateway Service

## 1. Features
- Gateway can manage [**Authorization Token**](https://github.com/springboot-microservices-project/.github/blob/main/profile/page/gateway-service/page/gateway-routing.md) 
- Gateway can [**Routing**](https://github.com/springboot-microservices-project/.github/blob/main/profile/page/gateway-service/page/gateway-routing.md) all api to microservices
- Gateway can configure the [**CORS**](https://github.com/springboot-microservices-project/.github/blob/main/profile/page/gateway-service/page/gateway-cors.md) (Cross Origin Resource Sharing)
- Gateway can configure the [**Ratelimmiter**](https://github.com/springboot-microservices-project/.github/blob/main/profile/page/gateway-service/page/gateway-ratelimmiter.md) and will saving data limit into redis
- Gateway can configure the [**Circuit Breaker**](https://github.com/springboot-microservices-project/.github/blob/main/profile/page/gateway-service/page/gateway-circuitbreaker.md) for spesifiect API and services
- Gateway can configure the [**Circuit Breaker Fallback**](https://github.com/springboot-microservices-project/.github/blob/main/profile/page/gateway-service/page/gateway-circuitbreaker-fallback.md) when circuit breaker is failed
- Gateway can modified the [**request header**](https://github.com/springboot-microservices-project/.github/blob/main/profile/page/gateway-service/page/gateway-modified-reqheader.md) for access all microservices



## 2. System Spec

### 2.1 Tech Spec
| Name  |
|----|
| Springboot 2.7^  |
| Java 11 |
| Spring Cloud Gateway |
|  |


### 2.2 Librarries

| Name  | Version | 
|----|----|
| spring-cloud-starter-netflix-eureka-client | latest  |
| spring-boot-starter-actuator | latest |
| micrometer-registry-prometheus | latest |
| spring-cloud-starter-gateway | latest |
| spring-cloud-starter-circuitbreaker-reactor-resilience4j | 3.0.0 |
| spring-boot-starter-data-redis-reactive | latest |
| spring-boot-starter-cache | latest |
| spring-boot-starter-data-redis | latest |
| spring-data-redis | latest |
| lombok | latest |
| gson | 2.9.1 |
| json | 20220924 |
| common-codec | 1.15 |





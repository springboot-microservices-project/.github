[Back to Home](https://github.com/springboot-microservices-project/)

# Gateway Service


tetw
## 1. Features
- Gateway can **Routing** all api to microservices
- Gateway can configure the **CORS** (Cross Origin Resource Server)
- Gateway can configure the **Ratelimmiter** and will saving data limit into redis
- Gateway can configure the **Circuit Breaker** for spesifiect API and services
- Gateway can configure the **Fallback** when circuit breaker is failed
- Gateway can modified the **request header** for access all microservices
- Gateway can save data token Authorization into redis, when login to auth-service is success


## 1.1. Gateway login flow
![alt text](https://github.com/springboot-microservices-project/.github/blob/main/profile/page/gateway-service/gateway-login-get-token-flow.png?raw=false)



## 1.1. Gateway verify token and authorization flow
![alt text](https://github.com/springboot-microservices-project/.github/blob/main/profile/page/gateway-service/gateway-verify-token-and-authorize-flow.png?raw=false)









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





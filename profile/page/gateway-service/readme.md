[Back to Home](https://github.com/springboot-microservices-project/)

# Gateway Service




## 1. System Spec

### 1.1 Tech Spec
`Springboot 2.7^`
`Java 11`
`Spring Cloud Gateway`

### 1.2 Librarries
`spring-cloud-starter-gateway`
`Resilence4j`
`spring-data-redis`
`spring-actuator`
`prometheus`
`spring-cloud-starter-netflix-eureka-client`

### 1.3 Features
- Configurable the Routing all api to microservices
- Configurable the CORS (Cross Origin Resource Server) configuration
- Configurable the Ratelimmiter save data limit into redis
- Configurable the Circuit Breaker for spesifiect API and services
- Configurable the Fallback when circuit breaker is failed
- Gateway can modified the request header for access all microservices
- Gateway can save data token Authorization into redis, when login to auth-service is success





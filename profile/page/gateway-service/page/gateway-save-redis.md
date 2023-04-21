[Home](https://github.com/springboot-microservices-project/) /
[Gateway](https://github.com/springboot-microservices-project/.github/blob/main/profile/page/gateway-service/readme.md)

# Save Redis


## 1. Features
- Gateway can **Routing** all api to microservices
- Gateway can configure the **CORS** (Cross Origin Resource Server)
- Gateway can configure the **Ratelimmiter** and will saving data limit into redis
- Gateway can configure the **Circuit Breaker** for spesifiect API and services
- Gateway can configure the **Fallback** when circuit breaker is failed
- Gateway can modified the **request header** for access all microservices
- Gateway can save data token Authorization into redis, when login to auth-service is success


## 1.1. Flow of get Authorization Token 
![alt text](https://github.com/springboot-microservices-project/.github/blob/main/profile/page/gateway-service/gateway-login-get-token-flow.png?raw=false)

- **1)** `client` call the `login api` with valid (username & password) on request body
- **2)** `api gateway` will routing to `auth-service`
- **3)** `auth-service` will check the username and password, is valid or not
- **4)** `if user is valid` : auth service will save the JWT token to redis, and send the response to `api-gateway`
- **4)** `if user is not valid` : `auth service` will send the response to `api-gateway`
-  **5)** `api gateway` willl send the response to the client


## 1.2. Flow of verify token and routing to microservices

![alt text](https://github.com/springboot-microservices-project/.github/blob/main/profile/page/gateway-service/gateway-verify-token-and-authorize-flow.png?raw=false)


- **1)** `client` call API on `A service` or `B service` with valid (Authorization JWT) on request header
- **2)** `api gateway` will run `filters verify token` for checking the `token verified` on redis is already exist?
- **3b)** `if token verified is not exist` : so `api gateway` will routing to  `auth service` for processing the verify token function, and will send back the response body `Authorization,Account-Key,Api-Key` to `api gateway`
- **3a)** `if token verified is  exist` : `api gateway` will run  `filter authorization key` `filter Account-Key` and `API-Key`
- **4)** `api gateway` will write `response header` (Authorization, Account-Key, Api-Key)
- **5)** `api gateway` will routing to `microservice` 
- **6)** `microservices` will running the `filter decode key` for extract the information and validation, that all token is valid or not.
- **7)** if all token is not valid `microservices` will block and send back to `api gateway` , if all token is valid `microservices` will send back to `microservice` with data what client needs
- **8)** `api gateway` will send the response body to `client`








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





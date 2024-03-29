# Springboot Microservices Project
This is an prototype of backend server with Microservice architecture made using Springboot and frontend made using react. this project just a template and we can customize what bussines needed, and than all services will run on docker containers for simplify develop, deploy and testing

## Already implements
- Implement [**Microservices Architectures**](https://github.com/springboot-microservices-project/.github/blob/main/profile/page/architectures/readme.md) on Springboot Projects as a backend services
- Implement [**Discovery services**](https://github.com/springboot-microservices-project/.github/blob/main/profile/page/discovery-service/readme.md) using **Eureka Server** for automatic registration all services and automatic find ip and hostname
- Implement [**Gateway services**](https://github.com/springboot-microservices-project/.github/blob/main/profile/page/gateway-service/readme.md) using **Springcloud gateway** for routing every end point on microservices
- Implement [**CORS**](https://github.com/springboot-microservices-project/.github/blob/main/profile/page/gateway-service/page/gateway-cors.md) (Control-Origin Resource Sharing), used for control and allowing the **domain, http method, http header, etc** for access the API on backend 
- Implement  [**Auth services**](https://github.com/springboot-microservices-project/.github/blob/main/profile/page/auth-service/readme.md)  with basic authentication, for handling the authentication & authorization, for all microservices
- Implement **Redis** for memory cache and saving the authentication token
- Implement [**Master Service**](https://github.com/springboot-microservices-project/.github/blob/main/profile/page/master-service/readme.md) for handling all master datas
- Implement [**Transaction Service**](https://github.com/springboot-microservices-project/.github/blob/main/profile/page/trans-service/readme.md) for handling all transaction datas
- Implement [**Inventory Service**](https://github.com/springboot-microservices-project/.github/blob/main/profile/page/inventory-service/readme.md) for handling all inventory datas
- Implement [**schmaster service**](https://github.com/springboot-microservices-project/.github/blob/main/profile/page/schmaster-service/readme.md) for handling search data master using elasticsearch
- Implement [**schtransaction service**](https://github.com/springboot-microservices-project/.github/blob/main/profile/page/schtransaction-service/readme.md) for handling search data transaction using elasticsearch
- Implement **CQRS** (Command Query Responsibile Segregation), used for split (insert, update, delete) to SQL and (select) to NoSQL
- Implement **Message broker** using RabbitMQ for Asyncronous task used when all microservices will communicate with publish-consume message
- Implement [**Email Service**](https://github.com/springboot-microservices-project/.github/blob/main/profile/page/email-service/readme.md) for handling email sending, data will receive from rabbitMQ
- Implement [**Report Service**](https://github.com/springboot-microservices-project/.github/blob/main/profile/page/report-service/readme.md) for handling report Generator, data will receive from rabbitMQ
- Implement **Logging** using **logback**
- Implement [**Spring actuator & prometheus metric**](https://github.com/springboot-microservices-project/.github/blob/main/profile/page/gateway-service/page/gateway-spring-actuator-prometheus-metric.md) on gateway service 
- Implement **resilence4j** on gawateway service, for handling [**ratelimiter**](https://github.com/springboot-microservices-project/.github/blob/main/profile/page/gateway-service/page/gateway-ratelimmiter.md) and [**circuit breaker**](https://github.com/springboot-microservices-project/.github/blob/main/profile/page/gateway-service/page/gateway-circuitbreaker.md) on every end point
- Implement **Monitoring API** using [**Prometheus and Grafana**](https://github.com/springboot-microservices-project/.github/blob/main/profile/page/gateway-service/page/gateway-prometheus-grafana-monitoring.md)
- Implementat a **Dockerfile** to all services for creating the docker images 
- Implement push docker image to **dockerhub** [https://hub.docker.com/repositories/denitiawan](https://hub.docker.com/search?q=denitiawan)
- Implement **docker-compose** for configure, setup and running the all services
- Implement **database migration** on every microservices using **liquibase**



## Backend Technologies
`Springboot 2.7^`
`Java 11`
`Spring Security`
`JWT `
`Oauth0`
`Mysql`
`liquibase (database migration)`
`cors`
`swagger`
`resillent4j`
`springActuator`
`elasticSearch`
`MySql`
`Redis`
`RabbitMQ`
`Springcloud Gateway`
`Eureka Discovery Service`
`Rate Limmiter`
`Circuit Breaker`
`SMPTGmail`
`reportPDF`
`reportExcel`
`logback`
`Spring JPA`
`JPA Audit`
`Docker`
`Docker-compose`
`microserivices`


### Backend Repositories
| Project Name     | Visibility  | URL Repository                                                          | 
|------------------|--------------|-------------------------------------------------------------------------|
| discovery-service | private | [...](https://github.com/denitiawan/springboot-microservices-discovery) |
| gateway-service | private | [...](https://github.com/denitiawan/springboot-microservices-gateway) |
| auth-service | private | [...](https://github.com/denitiawan/springboot-microservices-auth) |
| master-service | private | [...](https://github.com/denitiawan/springboot-microservices-master) |
| transaction-service | private | [...](https://github.com/denitiawan/springboot-microservices-transaction) |
| schmaster-service | private | [...](https://github.com/denitiawan/springboot-microservices-schmaster) |
| schtransaction-service | private | [...](https://github.com/denitiawan/springboot-microservices-schtransaction) |
| inventory-service | private | [...](https://github.com/denitiawan/springboot-microservices-inventory) |
| mail-service | private | [...](https://github.com/denitiawan/springboot-microservices-email) |
| report-service | private | [...](https://github.com/denitiawan/springboot-microservices-report) |



## Frontend Technologies
`React.js`
`Redux Multi Store`
`i18n Multi Resource`
`Router`
`Secure Routing`
`Error Boundaries`
`Variable Environtment .env`
`React Hook`
`Functional Component`
`Loggin`
`axios`
`Docker`
`Docker-compose`


### Frontend Repositories
| Project Name     | Visibility     | Description  | URL Repository                                                          | 
|------------------|--------------|--------------|-------------------------------------------------------------------------|
| Web C-Panel | private | Web project | [...](https://github.com/denitiawan/springboot-microservices-web) |


## Documentation Types
`FSD`
`TSD`
`Diagram and Arsitektur (drawio)`
`ERD (plan uml)`
`Figma prototype`
`Api Collection postman`
`Product Backlog`
`docker-compose.yml`
`all technical notes`


### Document repository
| Project Name     | Visibility  | URL Repository                                                          | 
|------------------|--------------|-------------------------------------------------------------------------|
| documentation-service | private | [...](https://github.com/denitiawan/springboot-microservices-documentation) |



## How to run the application? you can try it using Docker Compose
- clone `setup` repository on this link https://github.com/springboot-microservices-project/setup
- after clone, open folder `./docker-compose/spring_microservices` in `terminal` or `cmd`
- on `terminal` or `cmd` type this commands `docker-compose up -d`
- wait until all aplication running well

## Testing
| Name     | URL      | client access |
|--------|--------------|--------------|
| Web C-Panel | http://localhost:3006 | Browser |
| Discovery Server  | http://localhost:8761 | Browser |
| Grafana Server  | http://localhost:3000 | Browser |
| Prometheus Server  | http://localhost:9090 | Browser |

## Syntax for Redis CLI
- open cmd
- type `docker container exec -it spring_microservices_redis  redis-cli -h 127.0.0.1 -p 6379 -a "password123"` enter
- see all keys `keys *`
- get by key `get <keyname>`
- set key `set <keyname> <value>`
- see TTL `ttl <keyname>`

## Api Collection (Postman)
- see on this link [Springboot-Microservices.postman_collection.json](https://github.com/denitiawan/springboot-microservices-documentation/blob/main/environtment/api-collections/collection/Springboot-Microservices_v0.0.5.postman_collection.json)

| Name | URL | Method | body |
|--------|--------|--------|--------|
| Login | http://localhost:8181/login  | POST |{"username":"admin","password":"admin"} |
| . . . | . . . | . . . | . . . |



# About Me

My Name is **Deni Setiawan**, I am **Backend & System Analyst** at http://nexsoft.co.id/ from Indonesia.
**Loves writing and coding enthusiast** who always **keeps learning** about software engineering skills for make me **keep an update and strenghts skills** 🚀

| Profile     | URL                                                          | 
|------------------|--------------|
| Github | https://github.com/denitiawan |
| Medium | https://deni-setiawan.medium.com/ |


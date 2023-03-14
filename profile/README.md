# Welcome 

My Name is **Deni Setiawan**, I am **Backend Dev & System Analyst** at http://nexsoft.co.id/ from Indonesia.
**Loves writing and coding enthusiast** who always **keeps learning** about software engineering skills for make me **keep an update and strenghts skills** 🚀


### About Me
| Profile     | URL                                                          | 
|------------------|--------------|
| Github | https://github.com/denitiawan |
| Medium | https://deni-setiawan.medium.com/ |

# 1. what is 'Springboot Microservices Project'
Here we will learning how to create backend service with microservices architecture using springboot framework, and all services will run on docker containers, for simplify developement, deployment and testing

### 1.1. Overviews
- Implement Microservices Architectures on Springboot Projects as a backend services
- Implement Discovery services for automatic registration all services and automatic find ip and hostname
- Implement Gateway services for routing every end point on microservices
- Implement Auth services, with basic authentication, for handling the authentication & authorization, for all microservices
- Implement Redis for memory cache and saving the authentication token
- Implement Master Service for handling all master datas
- Implement Transaction Service for handling all transaction datas
- Implement Inventory Service for handling all inventory datas
- Implement schmaster service for handling search data master using elasticsearch
- Implement schtransaction service for handling search data transaction using elasticsearch
- Implement CQRS (Command Query Responsibile Segregation), used for split (insert, update, delete) to SQL and (select) to NoSQL
- Implement Message broker using RabbitMQ for Async task used when microservices will sending data and receive data
- Implement Email Service for handling email sending, data will receive from rabbitMQ
- Implement Report Service for handling report Generator, data will receive from rabbitMQ
- Implement Logging using logback
- Implement spring actuator on gateway service, for monitoring and tracking every endpoint using metric 
- Implement resilence4j on gawateway service, for handling ratelimiter and circuit breaker on every end point
- Implement Monitoring API using Prometheus and Grafana
- Implementat a Dockerfile to all services for creating the docker images 
- Implement push docker image to dockerhub [https://hub.docker.com/repositories/denitiawan](https://hub.docker.com/repositories/denitiawan)
- Implement docker-compose, for configure, setup and running the all services
- Implement database migration on every microservices using liquibase


### 1.2. Backend
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


#### 1.2.1. Repositories
| Project Name     | Visibility  | URL Repository                                                          | 
|------------------|--------------|-------------------------------------------------------------------------|
| discovery-service | private | https://github.com/denitiawan/springboot-microservices-discovery                                     |
| gateway-service | private | https://github.com/denitiawan/springboot-microservices-gateway                                     |
| auth-service | private | https://github.com/denitiawan/springboot-microservices-auth                                     |
| master-service | private | https://github.com/denitiawan/springboot-microservices-master                                     |
| transaction-service | private | https://github.com/denitiawan/springboot-microservices-transaction                                     |
| schmaster-service | private | https://github.com/denitiawan/springboot-microservices-schmaster                                     |
| schtransaction-service | private | https://github.com/denitiawan/springboot-microservices-schtransaction                                     |
| inventory-service | private | https://github.com/denitiawan/springboot-microservices-inventory                                     |
| mail-service | private | https://github.com/denitiawan/springboot-microservices-email                                     |
| report-service | private | https://github.com/denitiawan/springboot-microservices-report                                     |


### 1.3. Documentation
- FSD
- TSD
- Diagram and Arsitektur (drawio)
- ERD (plan uml)
- Figma prototype
- Api Collection postman
- Product Backlog


### 1.3.1 repository
| Project Name     | Visibility  | URL Repository                                                          | 
|------------------|--------------|-------------------------------------------------------------------------|
| documentation-service | private | https://github.com/denitiawan/springboot-microservices-documentation                                     |


# 


# 2. Run application using Docker Compose
- clone `setup` repository on this link https://github.com/springboot-microservices-project/setup
- after clone, open folder `./docker-compose/spring_microservices` in `terminal` or `cmd`
- on `terminal` or `cmd` type this commands `docker-compose up -d`
- wait until all aplication running well

## 2.1.  Test Backend
| container     | URL      | client access |
|--------|--------------|--------------|
| discovery-service  | http://localhost:8761 | Browser |


### 2.2.1. Redis CLI
- open cmd
- type `docker container exec -it spring_microservices_redis  redis-cli -h 127.0.0.1 -p 6379 -a "password123"` enter
- see all keys `keys *`
- get by key `get <keyname>`
- set key `set <keyname> <value>`
- see TTL `ttl <keyname>`

### 2.2.3.   Api Collection (Postman)
- see on this link [DRP.postman_collection.json](https://github.com/dockerize-react-project/postman/DRP.postman_collection.json)

| Name | URL | Method | body |
|--------|--------|--------|--------|
| Login | http://localhost:8181/login  | POST |{"username":"admin","password":"admin"} |
| . . . | . . . | . . . | . . . |




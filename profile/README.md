# Springboot 2 Microservices
This is an prototype of backend server with Microservice architecture made using Springboot and frontend made using react. this project just a template and we can customize what bussines needed, and than all services will run on docker containers for simplify develop, deploy and testing

## Implements
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
- Implement **Message broker** using RabbitMQ for Asyncronous task used when all microservices will communicate with publish-consume message
- Implement [**Email Service**](https://github.com/springboot-microservices-project/.github/blob/main/profile/page/email-service/readme.md) for handling email sending, data will receive from rabbitMQ
- Implement [**Report Service**](https://github.com/springboot-microservices-project/.github/blob/main/profile/page/report-service/readme.md) for handling report Generator, data will receive from rabbitMQ
- Implement **Logging** using **logback**
- Implement [**Spring actuator & prometheus metric**](https://github.com/springboot-microservices-project/.github/blob/main/profile/page/gateway-service/page/gateway-spring-actuator-prometheus-metric.md) on gateway service 
- Implement **resilence4j** on gawateway service, for handling [**ratelimiter**](https://github.com/springboot-microservices-project/.github/blob/main/profile/page/gateway-service/page/gateway-ratelimmiter.md) and [**circuit breaker**](https://github.com/springboot-microservices-project/.github/blob/main/profile/page/gateway-service/page/gateway-circuitbreaker.md) on every end point
- Implement **Monitoring API** using [**Prometheus and Grafana**](https://github.com/springboot-microservices-project/.github/blob/main/profile/page/gateway-service/page/gateway-prometheus-grafana-monitoring.md)

## Technologies
- **backend** : `Springboot 2.7^` `Java 11` `Spring Security` `JWT ` `Oauth0` `Mysql` `liquibase (database migration)` `cors` `swagger` `resillent4j` `springActuator` `elasticSearch` `MySql` `Redis` `RabbitMQ` `Springcloud Gateway` `Eureka Discovery Service` `Rate Limmiter` `Circuit Breaker` `SMPTGmail` `reportPDF` `reportExcel` `logback` `Spring JPA` `JPA Audit` `Docker` `Docker-compose` `microserivices`
- **frontend** : `React.js` `Redux Multi Store` `i18n Multi Resource` `Router` `Secure Routing` `Error Boundaries` `Variable Environtment .env` `React Hook` `Functional Component` `Loggin` `axios` `Docker` `Docker-compose`
- **Database** : `mysql` `redis`
- **message broker** : `rabbit mq`
- **Monitoring** : `spring actuator` `prometheus` `grafana`
- **devops** : `ci/cd github action` `docker` `kubernetes` `argocd` `git ops`
- **documentation** : `FSD` `Api Spec` `Postman Collection`

## Private Repository
<img src="https://github.com/user-attachments/assets/67905c5e-c86c-4c74-b8b3-708030712c5d" width="400">
<img src="https://github.com/user-attachments/assets/326b455d-f3e7-41ba-bb8e-11e74877a61d" width="400">

## Contact
Contact me [Deni Setiawan](https://www.linkedin.com/in/deni-setiawan-a2328967/) if you are interested in this project




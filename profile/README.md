# Springboot 2 Microservices
Building microservice using springboot 2 as backend and react js as frontend. apps will running on kubernetes with argocd as tools.
And this project just a template and we can customize what bussines needed.

## Projects Informations
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

## User Interface
### Service Discovery
<img src="https://github.com/user-attachments/assets/74709e73-7af3-4d30-9da5-7d15554170c0" width="700">

### Frontend 
<img src="https://github.com/user-attachments/assets/2e4e2c1d-cc68-4489-ad68-8fb23ed7dff5" width="300">
<img src="https://github.com/user-attachments/assets/29a6bf0f-8eac-4196-a88c-7afc685b956c" width="600">

### Rabbitmq
<img src="https://github.com/user-attachments/assets/d3f2c2fd-b19e-4ebb-8f05-af8f30f2e225" width="700">

### Prometheus & Grafana
<img src="https://github.com/user-attachments/assets/e6cbe86e-d9df-4313-b8e1-2faf13a4a82d" width="400">
<img src="https://github.com/user-attachments/assets/58fca8a0-9b03-4392-80a9-e790b1ec04b3" width="400">


## Deployment
### Kubernetes
<img src="https://github.com/user-attachments/assets/e97954b6-2eee-4ea3-a61c-ed5b33958ec8" width="400">

### Argo CD
<img src="https://github.com/user-attachments/assets/de4edfeb-c5fb-438e-b7b1-e05bcc09d084" width="400">

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




[Home](https://github.com/springboot-microservices-project/) /
[Gateway](https://github.com/springboot-microservices-project/.github/blob/main/profile/page/gateway-service/readme.md)

# CORS (Cross-origin Resource Sharing)

### 1.1. Flow of CORS on SpringCloud Gateway
![alt text](https://github.com/springboot-microservices-project/.github/blob/main/profile/page/gateway-service/image/gateway-gateway-cors-flow.png?raw=false)


### 1.2. CORS configuration on SpringCloud Gateway
- application.yml
```

  # rules of enable CORS in Springboot microservices architecture:
  # - on api gateway-service, implement global cors for accepting the url : front end
  # - on every microservices, implement global cors for accepting the url : api-gateway-service
  # - on every microservices, dont use @CorsOrigin on every endpoint, because that annotation will generate duplicate key on http response header, thats will make the browser cannot accept the http response
  cloud:
    gateway:
      globalcors:
        corsConfigurations:
          '[/**]':
            allowedOrigins:
              - "http://localhost:3006"
              - "http://localhost:9090"
            allowedHeaders:
              - content-type
              - x-requested-with
              - Authorization
              - Account-Key
              - Api-Key
            allowedMethods:
              - GET
              - POST
              - DELETE
              - PUT
              - OPTIONS
            allow-credentials: true

```

``` 
"http://localhost:3006"  this is an URL of frontend
``` 

```
"http://localhost:9090"  this is an URL of prometheus
``` 





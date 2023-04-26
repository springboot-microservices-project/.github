[Home](https://github.com/springboot-microservices-project/) /
[Gateway](https://github.com/springboot-microservices-project/.github/blob/main/profile/page/gateway-service/readme.md)

# Routing

### 1.1. Flow of Routing APIs to microservices on SpringCloud Gateway
![alt text](https://github.com/springboot-microservices-project/.github/blob/main/profile/page/gateway-service/image/gateway-routing-architecture.png?raw=true)


### 1.2. Routing configuration on SpringCloud Gateway

- application.yml
```
# routing server uri
custom:
  auth-service:
    uri: http://localhost:8282
  master-service:
    uri: http://localhost:8383
  transaction-service:
    uri: http://localhost:8484
  inventory-service:
    uri: http://localhost:8585
  schmaster-service:
    uri: http://localhost:8686
  schtransaction-service:
    uri: http://localhost:8787
  report-service:
    uri: http://localhost:8888
  email-service:
    uri: http://localhost:8989  
```

- GatewayPredicatesRoutingConfig
```
@Configuration
public class GatewayPredicatesRoutingConfig {

 ....
 ....
 ....

 @Value("${custom.master-service.uri}")
 String masterServiceUri;

 @Value("${custom.transaction-service.uri}")
 String transactionServiceUri;

 ....
 ....
 ....


 @Bean
    public RouteLocator myRoutes(RouteLocatorBuilder routeLocatorBuilder) {
        BaseFilterRoutes baseFilterRoutes = new BaseFilterRoutes(redisRepo, restTemplate, authServiceUri, rateLimmiterConfig.redisRateLimiter());
        return routeLocatorBuilder.routes()
        
        ....
        ....
        ....
        
         // Master Service
         .route(p -> p.path("/api/master/**")                        
                        .uri(masterServiceUri))
        
        // Transaction Service
        .route(p -> p.path("/api/transaction/**")                        
                        .uri(transactionServiceUri))
                        
        ....
        ....
        ....
        
        
     }

}
```



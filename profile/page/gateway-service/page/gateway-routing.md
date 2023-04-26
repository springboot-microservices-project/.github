[Home](https://github.com/springboot-microservices-project/) /
[Gateway](https://github.com/springboot-microservices-project/.github/blob/main/profile/page/gateway-service/readme.md)

# Routing

### 1.1. Flow of Routing APIs to microservies on SpringCloud Gateway
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

 @Bean
    public RouteLocator myRoutes(RouteLocatorBuilder routeLocatorBuilder) {
        BaseFilterRoutes baseFilterRoutes = new BaseFilterRoutes(redisRepo, restTemplate, authServiceUri, rateLimmiterConfig.redisRateLimiter());
        return routeLocatorBuilder.routes()
        
        ....
        ....
        ....
        
         // Master Service
                .route(p -> p.path("/api/master/**")
                        .filters(baseFilterRoutes.FILTER_ROUTE_GLOBAL(CB_MASTER_SERVICES, ROLE_ALL))
                        .uri(masterServiceUri))
        
        // Transaction Service
                .route(p -> p.path("/api/transaction/**")
                        .filters(baseFilterRoutes.FILTER_ROUTE_GLOBAL(CB_TRANSACTION_SERVICES, ROLE_ALL))
                        .uri(transactionServiceUri))
                        
        ....
        ....
        ....
        
        
     }

}
```



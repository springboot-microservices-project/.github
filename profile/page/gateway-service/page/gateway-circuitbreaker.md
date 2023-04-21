[Home](https://github.com/springboot-microservices-project/) /
[Gateway](https://github.com/springboot-microservices-project/.github/blob/main/profile/page/gateway-service/readme.md)

# Circuit Breaker

### 1.1. Flow of Circuit Breaker on SpringCloud Gateway
![alt text](https://github.com/springboot-microservices-project/.github/blob/main/profile/page/gateway-service/image/gateway-circuitbreaker-squence.png?raw=true)


### 1.2. Circuit Breaker configuration on SpringCloud Gateway
- **1)** application.yml
```
# custom properties : circuit breaker
custom:
  circuit-breaker:
    timeoutduration: 3
```
- **2)** CircuitBreakerConfig.java

```
package com.deni.microservices.gateway.gateway.webhandler.prefilter.circuitbreaker;

import io.github.resilience4j.timelimiter.TimeLimiterConfig;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.cloud.circuitbreaker.resilience4j.ReactiveResilience4JCircuitBreakerFactory;
import org.springframework.cloud.circuitbreaker.resilience4j.Resilience4JConfigBuilder;
import org.springframework.cloud.client.circuitbreaker.Customizer;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

import java.time.Duration;

@Configuration
public class CircuitBreakerConfig {


    /**
     * CIRCUIT BREAKER CONFIG :
     * - Circuit Breaker Config
     * - Time limiter Config
     * - Timeout Duration = untuk seting berapa lama proses api dianggap lama (seting per detik)?
     */

    @Value("${custom.circuit-breaker.timeoutduration}")
    int timeoutDurationInSecond;

    @Bean
    public Customizer<ReactiveResilience4JCircuitBreakerFactory> defaultCustomizer() {
        return factory -> factory.configureDefault(id -> new Resilience4JConfigBuilder(id)
                .circuitBreakerConfig(io.github.resilience4j.circuitbreaker.CircuitBreakerConfig.ofDefaults())
                .timeLimiterConfig(TimeLimiterConfig.custom()
                        .timeoutDuration(Duration.ofSeconds(timeoutDurationInSecond))
                        .build())
                .build());
    }

}

```


- **3)** setup CircuitBreaker on RouteLocator
```
@Bean
    public RouteLocator myRoutes(RouteLocatorBuilder routeLocatorBuilder) {
        BaseFilterRoutes baseFilterRoutes = new BaseFilterRoutes(redisRepo, restTemplate, authServiceUri, rateLimmiterConfig.redisRateLimiter());
        return routeLocatorBuilder.routes()
        
        // Master Service
        .route(p -> p.path("/api/master/**")
             .filters(baseFilterRoutes.FILTER_ROUTE_GLOBAL(CB_MASTER_SERVICES, ROLE_ALL))
             .uri(masterServiceUri))
         .build();
```


- **4)** setup circuitBreaker on filters 
```
   public Function<GatewayFilterSpec, UriSpec> FILTER_ROUTE_GLOBAL(
        String circuitBreakerName, String role) {
        return f -> f.preserveHostHeader()
                     .filter(this::filteRequestVerifyTokenAPI_ALL)
                     .filter(this::filterRequestHeaderAcountKey)
                     .filter(this::filterRequestHeaderApiKey)
                     .requestRateLimiter().configure(c -> c.setRateLimiter(redisRateLimiter))
                     .circuitBreaker(c -> c.setName(circuitBreakerName).setFallbackUri(GatewayFallbackController.FALLBACK_CIRCUITBREAKER));                        
            }            
```





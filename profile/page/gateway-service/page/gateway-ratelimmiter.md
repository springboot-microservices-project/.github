[Home](https://github.com/springboot-microservices-project/) /
[Gateway](https://github.com/springboot-microservices-project/.github/blob/main/profile/page/gateway-service/readme.md)

# Rate Limmiter

## 1.1. Flow of Ratelimmiter
![alt text](https://github.com/springboot-microservices-project/.github/blob/main/profile/page/gateway-service/image/gateway-ratelimmiter-flow.png?raw=false)

- **1)** `client` call the `login api` with valid (username & password) on request body
- **2)** `api gateway` will routing to `auth-service`
- **3)** `auth-service` will check the username and password, is valid or not
- **4)** `if user is valid` : auth service will save the JWT token to redis, and send the response to `api-gateway`
- **4)** `if user is not valid` : `auth service` will send the response to `api-gateway`
- **5)** `api gateway` willl send the response to the client


## 1.2. Rate Limmiter Configuration on SpringCloud Gateway


- **1)** application.yml
```
# custom properties : rate limiter
custom:  
  redis-rate-limiter:
    replenishRate: 1
    burstCapacity: 3
```


- **2)** create ratelimmiter configuration on RateLimmiterConfig file
```
package com.deni.microservices.gateway.gateway.webhandler.prefilter.ratelimiter;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.cloud.gateway.filter.ratelimit.KeyResolver;
import org.springframework.cloud.gateway.filter.ratelimit.RedisRateLimiter;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import reactor.core.publisher.Mono;

@Configuration
public class RateLimmiterConfig {

    public RateLimmiterConfig() {

    }

    /**
     * Rate Limmiter Config:
     * - defaultReplenishRate : for configure the minimum number of request per second
     * example value = 1, that means client allowed request more than 1 time in 1 seconds
     * <p>
     * - defaultBurstCapacity : for configure the maximum number of request per second
     * example value = 2, that means client only allowed request less than 2 time in second
     */
    @Value("${custom.redis-rate-limiter.replenishRate}")
    int defaultReplenishRate;
    @Value("${custom.redis-rate-limiter.burstCapacity}")
    int defaultBurstCapacity;

    int requestedToken = 1;

    @Bean
    public RedisRateLimiter redisRateLimiter() {
        RedisRateLimiter redisRateLimiter = new RedisRateLimiter(defaultReplenishRate, defaultBurstCapacity);
        //redisRateLimiter.setRe

        return redisRateLimiter;
    }


    /**
     * User key Resolver Config: for rate limiter
     */
    @Bean
    KeyResolver userKeyResolver() {

        // for development only
        //return exchange -> Mono.just("1");
        return exchange ->
              Mono.just(exchange.getRequest().getRemoteAddress().getHostName());
    }

}

```


- **3)** setup routing on RouteLocator
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


- **4)** setup requestRateLimiter 
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


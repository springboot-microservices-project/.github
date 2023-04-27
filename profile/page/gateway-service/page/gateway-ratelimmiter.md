[Home](https://github.com/springboot-microservices-project/) /
[Gateway](https://github.com/springboot-microservices-project/.github/blob/main/profile/page/gateway-service/readme.md)

# Rate Limmiter

## 1.1. Flow of Ratelimmiter
![alt text](https://github.com/springboot-microservices-project/.github/blob/main/profile/page/gateway-service/image/gateway-ratelimmiter-flow.png?raw=true)

- gateway detecting five request in 1 second
- gateway just accepting 3 request, 
- gateway block 2 request, because we already set just allowing 3 request in one second


## 1.2. Sequence of Ratelimmiter (client - gateway - redis)
![alt text](https://github.com/springboot-microservices-project/.github/blob/main/profile/page/gateway-service/image/gateway-ratelimmiter-sequence-redis.png?raw=true)
- plan uml
```
@startuml
' component
actor       client as client
participant gateway as gateway
database    redis    as redis


' flow
client -> gateway : request 1
gateway -> redis : request_rate_limmiter_tokens:2
gateway -> client : 200 OK

client -> gateway : request 2
gateway -> redis : request_rate_limmiter_tokens:1
gateway -> client : 200 OK

client -> gateway : request 3
gateway -> redis : request_rate_limmiter_tokens:0
gateway -> client : 200 OK

client -> gateway : request 4
gateway -> client : 429 Too Many Request

client -> gateway : request 5
gateway -> client : 429 Too Many Request

@enduml
```
## 1.2. Sequence of Ratelimmiter (client - gateway - microservices)
![alt text](https://github.com/springboot-microservices-project/.github/blob/main/profile/page/gateway-service/image/gateway-ratelimmiter-squence.png?raw=true)


## 1.4. Rate Limmiter Configuration on SpringCloud Gateway

- **1)** pom.xml
```

        <!--  spring cloud gateway : rate limmiter-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-circuitbreaker-reactor-resilience4j</artifactId>
            <version>3.0.0</version>
        </dependency>

        <!--  spring cloud gateway : save rate limmiter into redis -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-redis-reactive</artifactId>
        </dependency>

```


- **2)** application.yml
```
# redis
spring:
  redis:
    host: localhost
    port: 6379
    password: password123
    database: 0
    timeout: 10

# custom properties : rate limiter
custom:  
  redis-rate-limiter:
    replenishRate: 1
    burstCapacity: 3
```
- **3)** create redis configuration file **RedisConfig.java**
```
package com.deni.microservices.gateway.adapter.redis.config;

import org.springframework.beans.factory.annotation.Value;
import org.springframework.cache.annotation.EnableCaching;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.data.redis.connection.RedisStandaloneConfiguration;
import org.springframework.data.redis.connection.jedis.JedisClientConfiguration;
import org.springframework.data.redis.connection.jedis.JedisConnectionFactory;
import org.springframework.data.redis.core.RedisTemplate;
import org.springframework.data.redis.repository.configuration.EnableRedisRepositories;
import org.springframework.data.redis.serializer.GenericToStringSerializer;
import org.springframework.data.redis.serializer.JdkSerializationRedisSerializer;
import org.springframework.data.redis.serializer.StringRedisSerializer;

@Configuration
@EnableCaching
@EnableRedisRepositories
public class RedisConfig {

    @Value("${spring.redis.host}")
    private String redisHostname;

    @Value("${spring.redis.port}")
    private int redisPort;

    @Value("${spring.redis.password}")
    private String redisPassword;


    @Bean
    public RedisTemplate<String, String> redisTemplate() {

        // redis template
        RedisTemplate<String, String> redisTemplate = new RedisTemplate<String, String>();
        redisTemplate.setKeySerializer(new StringRedisSerializer());
        redisTemplate.setHashKeySerializer(new GenericToStringSerializer<String>(String.class));
        redisTemplate.setHashKeySerializer(new JdkSerializationRedisSerializer());
        redisTemplate.setValueSerializer(new JdkSerializationRedisSerializer());

        // redis config (host, port, password)
        RedisStandaloneConfiguration redisConfig = new RedisStandaloneConfiguration(redisHostname, redisPort);
        redisConfig.setPassword(redisPassword);

        // redis template
        JedisClientConfiguration jedisClientConfig = JedisClientConfiguration.builder().build();

        // redis connection
        JedisConnectionFactory redisConnection = new JedisConnectionFactory(redisConfig, jedisClientConfig);
        redisConnection.afterPropertiesSet();

        redisTemplate.setConnectionFactory(redisConnection);

        return redisTemplate;
    }

}

```

- **4)** create ratelimmiter configuration file **RateLimmiterConfig.java**
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


- **5)** setup routing on RouteLocator
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


- **6)** setup requestRateLimiter 
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


[Home](https://github.com/springboot-microservices-project/) /
[Gateway](https://github.com/springboot-microservices-project/.github/blob/main/profile/page/gateway-service/readme.md)

# Filters

### 1.1. Flow of Filterizations process when routing APIs to microservies on SpringCloud Gateway
![alt text](https://github.com/springboot-microservices-project/.github/blob/main/profile/page/gateway-service/image/gateway-routing-architecture.png?raw=false)


### 1.2. Filters configuration on SpringCloud Gateway
- **application.yml**

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

- GatewayPredicatesRoutingConfig.java
```
@Configuration
public class GatewayPredicatesRoutingConfig {
..
..

@Value("${custom.master-service.uri}")
String masterServiceUri;

...
...

    @Bean
    public RouteLocator myRoutes(RouteLocatorBuilder routeLocatorBuilder) {
        BaseFilterRoutes baseFilterRoutes = new BaseFilterRoutes(redisRepo, restTemplate, authServiceUri, rateLimmiterConfig.redisRateLimiter());
        return routeLocatorBuilder.routes()
        
        ...
        ...
        
        // Master Service
        .route(p -> p.path("/api/master/**")
                        .filters(baseFilterRoutes.FILTER_ROUTE_GLOBAL(CB_MASTER_SERVICES, ROLE_ALL))
                        .uri(masterServiceUri)
                        
        ...
        ...
                        
   }
   
   
   

}
```

- **BaseFilterRoutes.java**
```
public class BaseFilterRoutes {

    public Function<GatewayFilterSpec, UriSpec> FILTER_ROUTE_GLOBAL(String circuitBreakerName, String role) {                
         return f -> f.preserveHostHeader()
                        .filter(this::filteRequestVerifyTokenAPI_ALL)
                        .filter(this::filterRequestHeaderAcountKey)
                        .filter(this::filterRequestHeaderApiKey)
                        .requestRateLimiter().configure(c -> c.setRateLimiter(redisRateLimiter))
                        .circuitBreaker(c -> c.setName(circuitBreakerName).setFallbackUri(GatewayFallbackController.FALLBACK_CIRCUITBREAKER));
        
    }   
       
    private Mono<Void> filteRequestVerifyTokenAPI_ALL(ServerWebExchange exchange, GatewayFilterChain chain) throws UnsupportedOperationException {
        exchange = new FilteRequestVerifyTokenAPI(redisRepo, restTemplate, authServiceUri)
                .verifyToken(exchange, "/api/verify");
        return chain.filter(exchange);
    }  
    
    private Mono<Void> filterRequestHeaderAcountKey(ServerWebExchange exchange, GatewayFilterChain chain) throws UnsupportedOperationException {
        return FilterRequestHeaderAccountKey.filterAcountKey(exchange, chain);
    }
    
    public Mono<Void> filterRequestHeaderApiKey(ServerWebExchange exchange, GatewayFilterChain chain) throws UnsupportedOperationException {
        return FilterRequestHeaderApiKey.filterApiKey(exchange, chain);
    }

}
```

- **FilteRequestVerifyTokenAPI.java**
```
package com.deni.microservices.gateway.gateway.webhandler.prefilter.requestfilter;

import com.deni.microservices.gateway.adapter.redis.dto.RedisDto;
import com.deni.microservices.gateway.adapter.redis.repo.RedisRepo;
import com.deni.microservices.gateway.gateway.webhandler.prefilter.requestfilter.dto.AccountKeyResponseDto;
import com.deni.microservices.gateway.gateway.GatewayConstants;
import com.google.gson.Gson;
import org.json.JSONObject;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.http.*;
import org.springframework.http.server.reactive.ServerHttpRequest;
import org.springframework.util.CollectionUtils;
import org.springframework.util.concurrent.ListenableFuture;
import org.springframework.web.client.AsyncRestTemplate;
import org.springframework.web.client.RestTemplate;
import org.springframework.web.server.ResponseStatusException;
import org.springframework.web.server.ServerWebExchange;

import java.util.List;

public class FilteRequestVerifyTokenAPI {

    RedisRepo redisRepo;
    RestTemplate restTemplate;

    AsyncRestTemplate asyncRestTemplate;

    String authServiceUri;

    public FilteRequestVerifyTokenAPI(RedisRepo redisRepo, RestTemplate restTemplate, String authServiceUri) {
        this.redisRepo = redisRepo;
        this.restTemplate = restTemplate;
        this.authServiceUri = authServiceUri;
        this.asyncRestTemplate = new AsyncRestTemplate();
    }

    private final Logger log = LoggerFactory.getLogger(FilteRequestVerifyTokenAPI.class);

    public ServerWebExchange verifyToken(ServerWebExchange exchange, String endPointAPI) {
        log.info("filter Verify Token starting");

        // #1 check Authorization is exist on req header
        List<String> tokenHeader = exchange.getRequest().getHeaders().get(GatewayConstants.HttpHeaderKeys.HEAD_AUTHORIZATION);
        if (CollectionUtils.isEmpty(tokenHeader)) {
            throw new ResponseStatusException(HttpStatus.UNAUTHORIZED);
        }
        String token = tokenHeader.get(0);

        // #2
        // get data acount-key to redis
        // verify token to auth-service
        AccountKeyResponseDto verifyToken = doVerifyToken(token, authServiceUri
                , endPointAPI);
        if (verifyToken == null) {
            throw new ResponseStatusException(HttpStatus.UNAUTHORIZED);
        }

        // #3 add Acount-Key to request header
        ServerHttpRequest request = exchange.getRequest().mutate()
                .header(GatewayConstants.HttpHeaderKeys.HEAD_ACOUNT_KEY, verifyToken.getAccountKey())
                .build();
        exchange.mutate().request(request).build();


        // #4 add Api-Key to request header for accessing microservices

        request = exchange.getRequest().mutate()
                .header(GatewayConstants.HttpHeaderKeys.HEAD_API_KEY, verifyToken.getApiKey())
                .build();
        exchange.mutate().request(request).build();


        // #4 add Keys to response header
        exchange = AddHttpHeader.addRequestHeader(exchange, GatewayConstants.HttpHeaderKeys.HEAD_AUTHORIZATION, token);
        exchange = AddHttpHeader.addRequestHeader(exchange, GatewayConstants.HttpHeaderKeys.HEAD_ACOUNT_KEY, verifyToken.getAccountKey());
        exchange = AddHttpHeader.addRequestHeader(exchange, GatewayConstants.HttpHeaderKeys.HEAD_API_KEY, verifyToken.getApiKey());

        return exchange;
    }


    private AccountKeyResponseDto doVerifyToken(String token, String authServiceUri, String endPointAPI) {
        AccountKeyResponseDto result = getHeadersKeyFromRedis(token);
        if (result != null) {
            return result;
        } else {
            // call api verify token
            String verifyToken = callApiVerifyToken(token, authServiceUri, endPointAPI);
            if (verifyToken == null) {
                return null;
            }
            JSONObject jsonObject = new JSONObject(verifyToken);

            // save to redis
            AccountKeyResponseDto accountKey = new Gson().fromJson(jsonObject.get("data").toString(), AccountKeyResponseDto.class);
            redisRepo.saveOrUpdate(accountKey.getAuthorization(), accountKey);

            // return account key
            return accountKey;
        }
    }

    private String callApiVerifyToken(String token, String authServiceUri, String endPointApi) {
        String url = authServiceUri + endPointApi;

        // create request body
        JSONObject request = new JSONObject();

        // set headers
        HttpHeaders headers = new HttpHeaders();
        headers.setContentType(MediaType.APPLICATION_JSON);
        headers.add(GatewayConstants.HttpHeaderKeys.HEAD_AUTHORIZATION, token);
        HttpEntity<String> entity = new HttpEntity<String>(request.toString(), headers);
        try {

            // Wajib pakai asyncRestTemplate, karena api gateway ingin pakai non blocking proses
            //
            ListenableFuture<ResponseEntity<String>> response = asyncRestTemplate.postForEntity(url, entity, String.class);

            //String response =  postForObject(url, entity, String.class);
            //if (!response.isEmpty()) {

            ResponseEntity<String> stringResponseEntity = response.get();
            if (stringResponseEntity != null) {
                return stringResponseEntity.getBody();
            } else {
                return null;
            }
        } catch (Exception e) {
            return null;
        }
    }


    private AccountKeyResponseDto getHeadersKeyFromRedis(String token) {
        RedisDto redis = redisRepo.getDataByHaskeyForEntity(token, AccountKeyResponseDto.class);
        if (redis != null) {
            AccountKeyResponseDto dto = (AccountKeyResponseDto) redis.getValue();
            // save update to redis
            redisRepo.saveOrUpdate(dto.getAuthorization(), dto);
            return dto;
        }

        return null;
    }
}

```

- **FilterRequestHeaderAccountKey.java**
```
package com.deni.microservices.gateway.gateway.webhandler.prefilter.requestfilter;

import com.deni.microservices.gateway.gateway.GatewayConstants;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.cloud.gateway.filter.GatewayFilterChain;
import org.springframework.http.HttpStatus;
import org.springframework.util.CollectionUtils;
import org.springframework.web.server.ResponseStatusException;
import org.springframework.web.server.ServerWebExchange;
import reactor.core.publisher.Mono;

import java.util.List;

public class FilterRequestHeaderAccountKey {
    private static final Logger log = LoggerFactory.getLogger(FilterRequestHeaderAccountKey.class);

    public static Mono<Void> filterAcountKey(ServerWebExchange exchange, GatewayFilterChain chain) throws UnsupportedOperationException {

        log.info("filter Acount-Key starting");

        // #1 check Acount-Key is exist on req header
        List<String> tokenHeader = exchange.getRequest().getHeaders().get(GatewayConstants.HttpHeaderKeys.HEAD_ACOUNT_KEY);
        if (CollectionUtils.isEmpty(tokenHeader)) {
            throw new ResponseStatusException(HttpStatus.UNAUTHORIZED);
        }

        return chain.filter(exchange);
    }
}

```

- **FilterRequestHeaderApiKey.java**
```
package com.deni.microservices.gateway.gateway.webhandler.prefilter.requestfilter;

import com.deni.microservices.gateway.gateway.GatewayConstants;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.cloud.gateway.filter.GatewayFilterChain;
import org.springframework.http.HttpStatus;
import org.springframework.util.CollectionUtils;
import org.springframework.web.server.ResponseStatusException;
import org.springframework.web.server.ServerWebExchange;
import reactor.core.publisher.Mono;

import java.util.List;

public class FilterRequestHeaderApiKey {
    private static final Logger log = LoggerFactory.getLogger(FilterRequestHeaderApiKey.class);
    public static Mono<Void> filterApiKey(ServerWebExchange exchange, GatewayFilterChain chain) throws UnsupportedOperationException {
        log.info("filter Api-Key filter stating");

        // #1 check Api-Key is exist?
        List<String> apiKeyHeader = exchange.getRequest().getHeaders().get(GatewayConstants.HttpHeaderKeys.HEAD_API_KEY);
        if (CollectionUtils.isEmpty(apiKeyHeader)) {
            throw new ResponseStatusException(HttpStatus.UNAUTHORIZED);
        }

        return chain.filter(exchange);
    }
}

```






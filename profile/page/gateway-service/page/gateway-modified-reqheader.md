[Home](https://github.com/springboot-microservices-project/) /
[Gateway](https://github.com/springboot-microservices-project/.github/blob/main/profile/page/gateway-service/readme.md)

# Gateway Modified Request Header

### 1.1. Flow of Modified Request Header on SpringCloud Gateway
![alt text](https://github.com/springboot-microservices-project/.github/blob/main/profile/page/gateway-service/image/gateway-modifiy-reqheader-architectures.png?raw=false)


### 1.2. Example code for Modified Request Header on SpringCloud Gateway

- AddHttpHeader.java
```
package com.deni.microservices.gateway.gateway.webhandler.prefilter.requestfilter;

import org.springframework.http.HttpHeaders;
import org.springframework.util.MultiValueMap;
import org.springframework.web.server.ServerWebExchange;

public class AddHttpHeader {

    public static ServerWebExchange addRequestHeader(ServerWebExchange exchange, String headerKey, String headerValue) {
        MultiValueMap<String, String> map = new HttpHeaders();
        map.add(headerKey, headerValue);
        exchange.getResponse().getHeaders().addAll(map);

        return exchange;
    }

}

```
- example implementation of AddHttpHeader class
```
 ServerWebExchange exchange;   

 // #4 add Keys to response header
 exchange = AddHttpHeader.addRequestHeader(exchange, GatewayConstants.HttpHeaderKeys.HEAD_AUTHORIZATION, token);
 exchange = AddHttpHeader.addRequestHeader(exchange, GatewayConstants.HttpHeaderKeys.HEAD_ACOUNT_KEY, verifyToken.getAccountKey());
 exchange = AddHttpHeader.addRequestHeader(exchange, GatewayConstants.HttpHeaderKeys.HEAD_API_KEY, verifyToken.getApiKey());

```



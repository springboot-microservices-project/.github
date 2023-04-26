[Home](https://github.com/springboot-microservices-project/) /
[Gateway](https://github.com/springboot-microservices-project/.github/blob/main/profile/page/gateway-service/readme.md)

# Routing

### 1.1. Flow of Routing APIs to microservies on SpringCloud Gateway
![alt text](https://github.com/springboot-microservices-project/.github/blob/main/profile/page/gateway-service/image/gateway-routing-architecture.png?raw=true)


### 1.2. Routing configuration on SpringCloud Gateway




```
  .filter(this::filteRequestVerifyTokenAPI_ALL)
  
  .filter(this::filterRequestHeaderAcountKey)
  
  .filter(this::filterRequestHeaderApiKey)
  
  .requestRateLimiter()
  
  .circuitBreaker();
```




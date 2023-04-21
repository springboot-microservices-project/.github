[Home](https://github.com/springboot-microservices-project/) /
[Gateway](https://github.com/springboot-microservices-project/.github/blob/main/profile/page/gateway-service/readme.md)

# CORS (Cross-origin Resource Sharing)

### 1.1. Flow of CORS on SpringCloud Gateway
![alt text](https://github.com/springboot-microservices-project/.github/blob/main/profile/page/gateway-service/image/gateway-gateway-cors-flow.png?raw=false)


### 1.2. CORS configuration on SpringCloud Gateway
- application.yml
```
# rules of enable CORS in Springboot microservices architecture:
# - on api gateway-service, implement global cors for accepting the url : front end & prometheus
# - on every microservices, implement global cors for accepting the url : api-gateway-service
# - on every microservices, dont use @CorsOrigin on every endpoint, because that annotation will generate duplicate key on http response header, thats will make the browser cannot accept the http response
spring:  
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


### 1.3. CORS configuration on every microservices
- application.yml
```
# Cors for allowing api gateway can consume all APIs on every microservices
custom:
  cors:
    Access-Control-Allow-Origin: http://localhost:8181
```

- MainApp.java
```
package com.deni.microservices.master;

import com.deni.microservices.master.common.audit.AuditAwareImpl;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.client.discovery.EnableDiscoveryClient;
import org.springframework.cloud.netflix.eureka.EnableEurekaClient;
import org.springframework.context.annotation.Bean;
import org.springframework.data.domain.AuditorAware;
import org.springframework.data.jpa.repository.config.EnableJpaAuditing;
import org.springframework.web.cors.CorsConfiguration;
import org.springframework.web.cors.CorsConfigurationSource;
import org.springframework.web.cors.UrlBasedCorsConfigurationSource;
import org.springframework.web.servlet.config.annotation.CorsRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

import java.util.Arrays;


@SpringBootApplication
@EnableJpaAuditing(auditorAwareRef = "auditorAware") // config for jpa audit entity in package audit
@EnableEurekaClient
@EnableDiscoveryClient
public class MainApp {


    public static void main(String[] args) {
        SpringApplication.run(MainApp.class, args);
    }


    // for jpa audit field
    @Bean
    public AuditorAware<String> auditorAware() {
        return new AuditAwareImpl();
    }


    // - Implement to all microservices
    // - Http Request : Enable CORS for all  domain (*)
    @Value("${custom.cors.Access-Control-Allow-Origin}")
    String accessControlAllowOrigin;

    @Bean
    CorsConfigurationSource corsConfigurationSource() {
        CorsConfiguration configuration = new CorsConfiguration();
        // HTTP client request headers:
        //configuration.addAllowedOrigin("x-requested-with", "content-type", "Authorization", "Account-Key", "Api-Key");

        // HTTP server response headers:
        configuration.setAllowedOrigins(Arrays.asList(accessControlAllowOrigin));
        configuration.setAllowedMethods(Arrays.asList("POST", "PUT", "GET", "OPTIONS", "DELETE"));
        configuration.setAllowedHeaders(Arrays.asList("x-requested-with", "content-type", "Authorization", "Account-Key", "Api-Key"));
        UrlBasedCorsConfigurationSource source = new UrlBasedCorsConfigurationSource();
        source.registerCorsConfiguration("/**", configuration);
        return source;
    }


}

```

### 1.4. on every microservices, dont use @CorsOrigin on every endpoint, because that annotation will generate duplicate key on http response header, thats will make the browser cannot accept the http response

- every controllers
```
@RestController
@RequestMapping(BaseController.basePrefixApi + "/product")
public class ProductController extends BaseController {
    Logger log = LoggerFactory.getLogger(ProductController.class);


    @Autowired
    ProductService productService;

    @Autowired
    ProductPageSortService productPageSortService;

    ResponseEntity responseEntity;

    public ProductController(ProductService productService) {
        this.productService = productService;
    }
    
    
    
    //@CrossOrigin //*** DONT USE THIS ***
    
    @Secured({"ROLE_ADMIN", "ROLE_USER"})
    @RequestMapping(value = "/list", method = {RequestMethod.OPTIONS, RequestMethod.GET})
    public ResponseEntity<Response> getAll(
            @RequestHeader(name = authorizationHeader, required = false) String authorization,
            @RequestHeader(name = apiKeyHeader, required = false) String apiKey,
            @RequestHeader(name = accountKeyHeader, required = false) String accountKey
    ) {
        log.debug("Request {}", "list()");
        headerIsValid(authorization, apiKey, accountKey, new BaseControllerContract() {
            @Override
            public void headerIsValid(AccountKeyValueDto accountKey) {
                ResponseService response = productService.getAll();
                log.debug("Response {}", response.getHttpStatus());
                responseEntity = ResponseHandler.createHttpResponse(response.getMessage(), response.getData(), response.getHttpStatus());
            }

            @Override
            public void headerIsNotValid(String message) {
                log.debug("Response {}", HttpStatus.UNAUTHORIZED);
                responseEntity = ResponseHandler.createHttpResponse(message, "", HttpStatus.UNAUTHORIZED);
            }
        });
        return responseEntity;

    }
}    

```




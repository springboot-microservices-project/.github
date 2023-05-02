[Back to Home](https://github.com/springboot-microservices-project/)

# Auth Service

- [Design](#design)
- [Tech Spesification](#tech-Specification)
- [Librarries](#librarries)
- [Pom.xml](#pomxml)
- [Application.yml](#applicationyml)
- [SecurityConfiguration](#securityconfiguration)
- [Jwtauthenticationfilter](#jwtauthenticationfilter)
- [Jwtauthorizationfilter](#jwtauthorizationfilter)

## Design
![alt text](https://github.com/springboot-microservices-project/.github/blob/main/profile/page/auth-service/auth-service-architecture.jpg?raw=true)


## Tech Specification
| Name  |
|----|
| Springboot 2.7^  |
| Java 11 |
| jwt| 
| OAuth0|
| liquibase|
| redis| 
| mysql| 
| rabbitmq| 
| eureka-client| 


## Librarries

| Name  | Version | 
|----|----|
| liquibase-core | latest|
| spring-boot-starter-cache | latest|
| spring-boot-starter-data-redis| latest|
| spring-data-redis| latest|
| jedis| latest|
| spring-cloud-starter-netflix-eureka-client| latest|
| commons-codec| latest|
| spring-boot-starter-security| latest|
| java-jwt| latest|
| mysql-connector-java| latest|
| spring-boot-starter-data-jpa| latest|
| spring-boot-starter-web| latest|
| spring-boot-starter-test| latest|
| lombok| latest|
| gson| latest|
| spring-security-config| latest|

## Pom.xml
```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.7.3</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>
    <groupId>com.deni.microservices</groupId>
    <artifactId>auth</artifactId>
    <version>0.1</version>
    <name>auth-service</name>
    <description>Auth Service</description>
    <properties>
        <java.version>1.8</java.version>

        <spring-cloud.version>2021.0.5</spring-cloud.version>
    </properties>


    <!-- spring cloud gateway import-->
    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>org.springframework.cloud</groupId>
                <artifactId>spring-cloud-dependencies</artifactId>
                <version>${spring-cloud.version}</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
        </dependencies>
    </dependencyManagement>

    <dependencies>

        <!-- liquibase -->
        <dependency>
            <groupId>org.liquibase</groupId>
            <artifactId>liquibase-core</artifactId>
        </dependency>


        <!-- redis-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-cache</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-redis</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.data</groupId>
            <artifactId>spring-data-redis</artifactId>
        </dependency>

        <dependency>
            <groupId>redis.clients</groupId>
            <artifactId>jedis</artifactId>
        </dependency>


        <!-- discovery service-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
        </dependency>


        <!-- encrypt Key-Api, Account-Key -->
        <dependency>
            <groupId>commons-codec</groupId>
            <artifactId>commons-codec</artifactId>
            <version>1.15</version>
        </dependency>

        <!-- spring secuity oauth 0 basic-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-security</artifactId>
        </dependency>

        <dependency>
            <groupId>com.auth0</groupId>
            <artifactId>java-jwt</artifactId>
            <version>3.4.1</version>
        </dependency>


        <!--  mysql-->

        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <scope>runtime</scope>
        </dependency>


        <!-- jpa-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-jpa</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>


        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>

        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <version>1.18.24</version>
            <scope>provided</scope>
        </dependency>
        <dependency>
            <groupId>com.google.code.gson</groupId>
            <artifactId>gson</artifactId>
            <version>2.9.1</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.security</groupId>
            <artifactId>spring-security-config</artifactId>
            <version>5.7.3</version>
        </dependency>


    </dependencies>


    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>


    <profiles>
        <profile>
            <id>dev</id>
            <properties>
                <activeProperties>dev</activeProperties>
            </properties>
            <activation>
                <activeByDefault>false</activeByDefault>
            </activation>
        </profile>

        <profile>
            <id>docker</id>
            <properties>
                <activeProperties>docker</activeProperties>
            </properties>
            <activation>
                <activeByDefault>true</activeByDefault>
            </activation>

        </profile>


    </profiles>

</project>

```

## application.yml
```
#server port http
server:
  port: 8282

spring:

# profile
  profiles:
    active: dev
    
# app name
  application:
    name: auth-service


# liquibase
  liquibase:
   change-log: classpath:changelog/changelog-master.yml


  # redis with auth basic
  redis:
    host: localhost
    port: 6379
    password: password123
    database: 0
    timeout: 10

  # jpa
  jpa:
    hibernate:
      ddl-auto: update
    show-sql: true
    properties:
      hibernate:
        format_sql: false

        dialect: org.hibernate.dialect.MySQL5InnoDBDialect

  # mysql
  datasource:
    url: jdbc:mysql://localhost:3310/db_auth
    username: user
    password: password
    driver-class-name: com.mysql.cj.jdbc.Driver

# discovery client
eureka:
  client:
    register-with-eureka: true
    fetch-registry: true
    service-url:
      defaultZone: http://localhost:8761/eureka
    instance:
      hostname: localhost

# CORS ORIGIN
custom:
  cors:
    Access-Control-Allow-Origin: http://localhost:8181


```

## SecurityConfiguration
```
package com.deni.microservices.auth.security.config;

import com.deni.microservices.auth.adapter.redis.repo.RedisRepo;
import com.deni.microservices.auth.module.user.repo.UserRepo;
import com.deni.microservices.auth.security.filter.JwtAuthenticationFilter;
import com.deni.microservices.auth.security.filter.JwtAuthorizationFilter;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.authentication.dao.DaoAuthenticationProvider;
import org.springframework.security.config.annotation.authentication.builders.AuthenticationManagerBuilder;
import org.springframework.security.config.annotation.method.configuration.EnableGlobalMethodSecurity;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;
import org.springframework.security.config.http.SessionCreationPolicy;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
import org.springframework.security.crypto.password.PasswordEncoder;

/**
 * Created by denitiawan on 18/08/2022.
 */
@Configuration
@EnableWebSecurity
@EnableGlobalMethodSecurity(securedEnabled = true) // this setting for enable the annotation @Secured
public class SecurityConfiguration extends WebSecurityConfigurerAdapter {

    @Autowired
    RedisRepo redisRepo;
    private UserPrincipalDetailsService userPrincipalDetailsService;
    private UserRepo userRepo;

    public SecurityConfiguration(UserPrincipalDetailsService service, UserRepo userRepo) {
        this.userPrincipalDetailsService = service;
        this.userRepo = userRepo;
    }

    @Override
    protected void configure(AuthenticationManagerBuilder auth) throws Exception {
        // setting auth dari database  table user
        auth.authenticationProvider(authenticationProvider());

    }

    // registrasikan jwt disini
    @Override
    protected void configure(HttpSecurity http) throws Exception {

        // remove csrf and state in session because in jwt we do not need them
        http.csrf().disable();
        http.sessionManagement().sessionCreationPolicy(SessionCreationPolicy.STATELESS);

        // add jwt filters (1. authentication, 2. authorization)
        http.addFilter(new JwtAuthenticationFilter(authenticationManager(), redisRepo));
        http.addFilter(new JwtAuthorizationFilter(authenticationManager(), userRepo));
        http.authorizeRequests();

        // login
        http.authorizeRequests().antMatchers("/login").permitAll();
        http.authorizeRequests().antMatchers("/api/user/register").permitAll();
                
        // any endpoint authenticated
        http.authorizeRequests().anyRequest().authenticated();

    }


    // function untuk ambil data user dari table user untuk auth
    @Bean
    DaoAuthenticationProvider authenticationProvider() {
        DaoAuthenticationProvider daoAuthenticationProvider = new DaoAuthenticationProvider();
        daoAuthenticationProvider.setPasswordEncoder(passwordEncoder());
        daoAuthenticationProvider.setUserDetailsService(this.userPrincipalDetailsService);

        return daoAuthenticationProvider;
    }

    // untuk password encoder yg akan dipakai oleh configure AuthenticationManagerBuilder
    @Bean
    PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder();
    }
}

```

## JWTAuthenticationFilter
```
package com.deni.microservices.auth.security.filter;

import com.auth0.jwt.JWT;
import com.deni.microservices.auth.adapter.redis.repo.RedisRepo;
import com.deni.microservices.auth.common.controller.Response;
import com.deni.microservices.auth.security.config.UserPrincipal;
import com.deni.microservices.auth.module.auth.dto.LoginDTO;
import com.deni.microservices.auth.module.auth.dto.Token;
import com.fasterxml.jackson.databind.ObjectMapper;
import com.google.gson.Gson;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.security.authentication.AuthenticationManager;
import org.springframework.security.authentication.UsernamePasswordAuthenticationToken;
import org.springframework.security.core.Authentication;
import org.springframework.security.core.AuthenticationException;
import org.springframework.security.web.authentication.UsernamePasswordAuthenticationFilter;
import org.springframework.web.bind.annotation.CrossOrigin;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;

import javax.servlet.FilterChain;
import javax.servlet.ServletException;
import javax.servlet.http.Cookie;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.util.ArrayList;
import java.util.Date;

import static com.auth0.jwt.algorithms.Algorithm.HMAC512;

public class JwtAuthenticationFilter extends UsernamePasswordAuthenticationFilter {
    Logger log = LoggerFactory.getLogger(JwtAuthenticationFilter.class);

    RedisRepo redisRepo;

    private AuthenticationManager authenticationManager;

    public JwtAuthenticationFilter(AuthenticationManager authenticationManager,RedisRepo redisRepository) {
        this.authenticationManager = authenticationManager;
        this.redisRepo = redisRepository;
    }


    // @PostMapping(value = "localhost:8080/login", produces = MediaType.APPLICATION_JSON_UTF8_VALUE, consumes = MediaType.APPLICATION_JSON_UTF8_VALUE)
    // @ReqBody {"username":"den", "password":"den123"}
    @RequestMapping( method = {RequestMethod.OPTIONS, RequestMethod.POST})
    @Override
    public Authentication attemptAuthentication(HttpServletRequest request, HttpServletResponse response) throws AuthenticationException {
        log.debug("Request login {}", "login & authentication filter");

        // Grab credentials and map them to login viewmodel
        LoginDTO credentials = null;
        try {
            credentials = new ObjectMapper().readValue(request.getInputStream(), LoginDTO.class);
        } catch (IOException e) {
            e.printStackTrace();
        }

        // Create login token
        UsernamePasswordAuthenticationToken authenticationToken = new UsernamePasswordAuthenticationToken(
                credentials.getUsername(),
                credentials.getPassword(),
                new ArrayList<>());

        // Authenticate user
        Authentication auth = null;
        try {
            auth = authenticationManager.authenticate(authenticationToken);
        } catch (Exception e) {
            unsuccessfulLogin(request, response);
        }


        return auth;
    }

    // login success and then generating token using JWT
    // format token : X-AUTH-TOKEN.xxxx.yyyyy
    @Override
    protected void successfulAuthentication(HttpServletRequest request, HttpServletResponse response, FilterChain chain, Authentication authResult) throws IOException, ServletException {
        // Grab principal
        UserPrincipal principal = (UserPrincipal) authResult.getPrincipal();

        // Create JWT Token
        String token = JWT.create()
                .withSubject(principal.getUsername())
                .withExpiresAt(new Date(System.currentTimeMillis() + JwtProperties.TOKEN_EXPIRED_TIME))
                .sign(HMAC512(JwtProperties.TOKEN_SECRET.getBytes()));

        String finalToken = JwtProperties.TOKEN_PREFIX + token;
        // Add token in response header
        response.addHeader(JwtProperties.TOKEN_X_KEY, finalToken);

        // add token in response cookie
        Cookie cookie = new Cookie(JwtProperties.TOKEN_X_KEY, finalToken);
        response.addCookie(cookie);


        // save token to redis
        redisRepo.saveOrUpdate(finalToken, finalToken);



        // add body
        Response statusResponse = Response.builder()
                .status(HttpStatus.OK.toString())
                .message("Login Success")
                .data(new Token(JwtProperties.TOKEN_X_KEY, finalToken))
                .build();
        Object body = new ResponseEntity<>(statusResponse, HttpStatus.OK);

        response.setCharacterEncoding("UTF-8");
        response.setContentType("application/json");


        response.setStatus(HttpServletResponse.SC_OK);
        response.getWriter().write(new Gson().toJson(body));
        response.getWriter().flush();

        log.debug("Response Login {}", HttpStatus.OK);
    }


    protected void unsuccessfulLogin(HttpServletRequest request, HttpServletResponse response) {
        try {
            // add body
            Response statusResponse = Response.builder()
                    .status(HttpStatus.FORBIDDEN.toString())
                    .message("Login Failed")
                    .data("Username & Password is not Match")
                    .build();
            Object body = new ResponseEntity<>(statusResponse, HttpStatus.FORBIDDEN);
            response.setContentType("application/json");
            response.setStatus(HttpServletResponse.SC_FORBIDDEN);
            response.getWriter().write(new Gson().toJson(body));
            response.getWriter().flush();

            log.debug("Response Login {}", HttpStatus.FORBIDDEN.toString());
        } catch (Exception e) {
            log.debug("Exception Login {}", HttpStatus.INTERNAL_SERVER_ERROR.toString());
            log.debug("Exception Login {}", e.getMessage());
            log.debug("Exception Login {}", e.getCause());
        }
    }

    // for 500 user not found

    protected void userNotFound(HttpServletRequest request, HttpServletResponse response) {
        try {
            // add body
            Response statusResponse = Response.builder()
                    .status(HttpStatus.FORBIDDEN.toString())
                    .message("Login Failed")
                    .data("Username is not found")
                    .build();
            Object body = new ResponseEntity<>(statusResponse, HttpStatus.FORBIDDEN);
            response.setContentType("application/json");
            response.setStatus(HttpServletResponse.SC_FORBIDDEN);
            response.getWriter().write(new Gson().toJson(body));
            response.getWriter().flush();

            log.debug("Response Login {}", HttpStatus.FORBIDDEN.toString());
        } catch (Exception e) {
            log.debug("Exception Login {}", HttpStatus.INTERNAL_SERVER_ERROR.toString());
            log.debug("Exception Login {}", e.getMessage());
            log.debug("Exception Login {}", e.getCause());
        }
    }
}

```

## JWTAuthorizationFilter
```
package com.deni.microservices.auth.security.filter;

import com.auth0.jwt.JWT;
import com.deni.microservices.auth.common.controller.Response;
import com.deni.microservices.auth.security.config.UserPrincipal;
import com.deni.microservices.auth.module.user.entity.User;
import com.deni.microservices.auth.module.user.repo.UserRepo;
import com.google.gson.Gson;
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.security.authentication.AuthenticationManager;
import org.springframework.security.authentication.UsernamePasswordAuthenticationToken;
import org.springframework.security.core.Authentication;
import org.springframework.security.core.AuthenticationException;
import org.springframework.security.core.context.SecurityContextHolder;
import org.springframework.security.web.authentication.www.BasicAuthenticationFilter;
import org.springframework.web.bind.annotation.CrossOrigin;

import javax.servlet.FilterChain;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

import static com.auth0.jwt.algorithms.Algorithm.HMAC512;

public class JwtAuthorizationFilter extends BasicAuthenticationFilter {
    private UserRepo userRepository;

    public JwtAuthorizationFilter(AuthenticationManager authenticationManager, UserRepo userRepository) {
        super(authenticationManager);
        this.userRepository = userRepository;
    }

    @Override // api public akan di filter di function ini, apakah header sudah sesuai dengan yg diminta oleh security
    protected void doFilterInternal(HttpServletRequest request, HttpServletResponse response, FilterChain chain) throws IOException, ServletException {
        // Read the Authorization header, where the JWT token should be
        String header = request.getHeader(JwtProperties.TOKEN_X_KEY);

        // If header does not contain X-AUTH-TOKEN or is null delegate to Spring impl and exit
        if (header == null || !header.startsWith(JwtProperties.TOKEN_PREFIX)) {
            chain.doFilter(request, response);
            return;
        }

        // If header is present, try grab user principal from database and perform authorization
        Authentication authentication = getUsernamePasswordAuthentication(request);
        SecurityContextHolder.getContext().setAuthentication(authentication);

        // Continue filter execution
        chain.doFilter(request, response);
    }

    private Authentication getUsernamePasswordAuthentication(HttpServletRequest request) {
        // test get data dari redis
        //String redisToken = (String) redisRepo.getValueRaw(JwtProperties.TOKEN_X_KEY);

        // get token from header client
        String token = request.getHeader(JwtProperties.TOKEN_X_KEY)
                .replace(JwtProperties.TOKEN_PREFIX, "");

        if (token != null) {
            // parse the token and validate it
            String userName = JWT.require(HMAC512(JwtProperties.TOKEN_SECRET.getBytes()))
                    .build()
                    .verify(token)
                    .getSubject();

            // Search in the DB if we find the user by token subject (username)
            // If so, then grab user details and create spring auth token using username, pass, authorities/roles
            if (userName != null) {
                User user = userRepository.findByUsername(userName);
                UserPrincipal principal = new UserPrincipal(user);
                UsernamePasswordAuthenticationToken auth = new UsernamePasswordAuthenticationToken(userName, null, principal.getAuthorities());
                return auth;
            }
            return null;
        }
        return null;
    }


    protected void onUnsuccessfulAuthentication(HttpServletRequest request, HttpServletResponse response, AuthenticationException failed) throws IOException
    {
        // add body
        Response statusResponse = Response.builder()
                .status(HttpStatus.FORBIDDEN.toString())
                .message("Cannot akses")
                .data("Cannot akses")
                .build();
        Object body = new ResponseEntity<>(statusResponse, HttpStatus.FORBIDDEN);
        response.setContentType("application/json");
        response.setStatus(HttpServletResponse.SC_FORBIDDEN);
        response.getWriter().write(new Gson().toJson(body));
        response.getWriter().flush();

    }

}

```

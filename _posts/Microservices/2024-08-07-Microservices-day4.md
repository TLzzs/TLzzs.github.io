---
title: "Build A MicroService | Day 4 Add FeignClient to simplify the http call to external service"
date: 2024-08-06 00:00:00
categories: [Microservices]
tags: [Microservices]
---

### Overview
FeignClient is a declarative HTTP client provided by the Spring Cloud Netflix library. It simplifies the process of 
making HTTP calls to external services by abstracting the complexities involved in creating and managing HTTP requests. 
Here's a brief overview of FeignClient and how it can be used:

#### Key Features
- **Declarative HTTP Calls:** You can define the API endpoints as Java interfaces and specify the HTTP methods (GET, POST,
etc.) and their mappings using annotations.
- **Integration with Spring Cloud:** FeignClient integrates seamlessly with other Spring Cloud components like Ribbon 
(for client-side load balancing) and Eureka (for service discovery).
- **Simplified Exception Handling:** You can define custom error decoders to handle HTTP errors in a more readable and
maintainable way.


[Day 4 Github Commit History](https://github.com/TLzzs/microservices/commit/f862636307891649356ca112aaefa8729b2b60d8#diff-e1752feb8aeae799db63a59973ac2029cdd1e75ab72c3e4061af5afd5d3f9ef9)


### Add openfeign dependency to parent project

```xml
<dependency>
  <groupId>org.springframework.cloud</groupId>
  <artifactId>spring-cloud-starter-openfeign</artifactId>
</dependency>
```

### Enable Feign Clients in the main application

```java
@SpringBootApplication
@EnableEurekaClient
@EnableFeignClients(
        basePackages = "com.ludistudy.clients"
)
public class CustomerApplication {
    public static void main(String[] args) {
        SpringApplication.run(CustomerApplication.class, args);
    }
}
```


### Define the Feign Client

```java

@FeignClient(
        value = "fraud",
        path = "api/v1/fraud-check"
)
public interface FraudClient {
    // INSTEAD OF USING REST TEMPLATE ALL CLIENTS USING THIS INTERFACE to call api
    @GetMapping("{customerId}")
    FraudCheckResponse isFraudster(@PathVariable("customerId") Integer customerId);
}
```

### Calling the api via OpenFeign

```java
//        FraudCheckResponse fraudCheckResponse = restTemplate.getForObject(
//                "http://FRAUD/api/v1/fraud-check/{customerId}",
//                FraudCheckResponse.class,
//                customer.getId()
//        );
        //after enable the open feign
        private final FraudClient fraudClient;
        FraudCheckResponse fraudCheckResponse = fraudClient.isFraudster(customer.getId());
```

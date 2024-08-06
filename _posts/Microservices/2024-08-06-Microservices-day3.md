---
title: "Build A MicroService | Day 3 Register service into Eureka Server, Server as loadBalancer"
date: 2024-08-06 00:00:00
categories: [Microservices]
tags: [Microservices]
---

### Overview
In this setup, we have successfully created two services, Customer and Fraud. When the Customer API is called, the 
Customer service makes a REST call using RestTemplate to the Fraud service. To handle a high volume of incoming requests, 
we need multiple instances of the Fraud service running. While Kubernetes can be used for orchestration, we will use 
Eureka Server for service management in this example.

[Day 3 Github Commit History](https://github.com/TLzzs/microservices/commit/52059be7435bd8582d0a6df925d9eda179cc2bac)

### Key Functions of Eureka Server
1. Service Registry:
   - Registration: Microservices register themselves with the Eureka server at startup. The server maintains a registry 
     of all available service instances. Discovery: Microservices can query the Eureka server to find instances of other 
     services, enabling them to locate and communicate with each other dynamically.

2. Service Discovery:
   - Client-side Discovery: Services query the Eureka server to get the addresses of available service instances. 
     This information is then used to make requests directly to these instances.
   - Health Checks: Eureka server periodically checks the health of registered services. Services that fail health checks 
     are removed from the registry to ensure that only healthy instances are discoverable.

3. Load Balancing:
   - Client-side Load Balancing: When a service requests another service, Eureka can provide multiple instances of the 
     requested service. The client (with the help of a load balancer like Ribbon) can distribute requests across these 
     instances, balancing the load.
<div style="display: flex; justify-content: space-between;">
    <img width="874" alt="Screenshot 2024-08-06 at 16 47 24" src="https://github.com/user-attachments/assets/9decafb0-3d65-4f9a-87bd-bf72e78e5405" style="width: 45%; margin-right: 10px;">
    <img width="1123" alt="Screenshot 2024-08-06 at 16 47 57" src="https://github.com/user-attachments/assets/982e82fe-9704-4d58-98af-df86afd9d8b4" style="width: 50%;">
</div>
### Add Spring Cloud dependencies
Since Eureka is part of Spring Cloud, we need to include the relevant dependencies in each project.
Use the `DependencyManagement` section to manage the versions.
```xml
<dependency>
  <groupId>org.springframework.cloud</groupId>
  <artifactId>spring-cloud-dependencies</artifactId>
  <version>${spring-cloud.version}</version>
  <type>pom</type>
  <scope>import</scope>
</dependency>
```

Add Eureka Client Dependency into every individual service
```xml
<dependency>
  <groupId>org.springframework.cloud</groupId>
  <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
</dependency>
```

Create a Eureka Server Service and add Server Dependency into it
```xml
<dependency>
  <groupId>org.springframework.cloud</groupId>
  <artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>
</dependency>
```


### Configure Application.yml for Eureka Server and Client Services
Add the following configuration to the Eureka server's application.yml. Set fetch-registry and 
register-with-eureka to false as it will not act as a client.
```yaml
server:
  port: 8761
eureka:
  client:
    fetch-registry: false
    register-with-eureka: false
```

Add the following configuration to the application.yml files of both Customer and Fraud services to register with the
Eureka server.
```yaml
eureka:
  client:
    service-url:
      defaultZone: http://localhost:8761/eureka
```

### @EnableEurekaServer And @EnableEurekaClient
The `@EnableEurekaServer` annotation is used to make your Spring Boot application act as a Eureka server. This server 
becomes a service registry that other microservices can register with and discover each other through.

```java
@SpringBootApplication
@EnableEurekaServer
public class EurekaServerApplication {
    public static void main(String[] args) {
        SpringApplication.run(EurekaServerApplication.class, args);
    }
}
```

The `@EnableEurekaClient` annotation is used to make your Spring Boot application act as a Eureka client. This client 
registers itself with a Eureka server and can discover other services registered with the same server. 
Add below to Customer and Fraud Services

```java
@SpringBootApplication
@EnableEurekaClient
public class CustomerApplication {
    public static void main(String[] args) {
        SpringApplication.run(CustomerApplication.class, args);
    }
}
```

### Using @LoadBalanced with RestTemplate

using the `@LoadBalanced` annotation is necessary to enable client-side load balancing with Ribbon when using a 
RestTemplate or other HTTP clients in Spring Cloud. This annotation instructs Spring Cloud to use Ribbon to resolve the
service instance and perform load balancing.

If you are using Feign (see next day), load balancing is automatically enabled when you use the `@FeignClient` 
annotation, as Feign integrates with Ribbon out of the box.

```java
public class CustomerConfig {

    @Bean
    @LoadBalanced
    public RestTemplate restTemplate() {
        return new RestTemplate();
    }
}
```
### Eureka DashBoard
Spring Cloud Eureka provide a dashboard UI , by default you can access via link `http://localhost:8761/`

<u> Dashboard Features </u>:

- Service List: The dashboard displays a list of all registered services, including their instances and statuses.
- Instance Information: You can view detailed information about each instance, such as IP address, port, status, and metadata.
- Health Checks: The dashboard shows the health status of each service instance, helping you monitor the overall health of your microservices ecosystem.
- Registration and Deregistration: As services register and deregister with the Eureka server, the dashboard dynamically updates to reflect these changes.
### Conclusion
By following these steps, you ensure that your Customer and Fraud services can register with the Eureka server and 
discover each other for load-balanced communication. This setup helps manage service instances efficiently, ensuring 
high availability and fault tolerance.



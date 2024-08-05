---
title: "Build A MicroService | Day 1 Getting Started"
date: 2024-08-02 00:00:00
categories: [Microservices]
tags: [Microservices]
---

## Introduction

I have embarked on a journey to deepen my understanding of microservices architecture using 
Spring Boot. This blog serves as a living document where I will chronicle my daily learning 
experiences, challenges, and milestones. By sharing these insights, I hope to not only enhance 
my own comprehension but also assist others who might be navigating similar paths. This educational
adventure is driven by a commitment to mastering the intricacies of building scalable, 
efficient, and robust software architectures.


## System Design
The system design illustrated in the diagram is a comprehensive microservices architecture, encapsulated within a 
private network, and is a prime example of modern software engineering. Here’s a breakdown of its components and their 
interactions:

- **Load Balancer:** At the gateway, the Load Balancer manages incoming traffic, ensuring even distribution across 
- various services and improving the system's resilience and scalability.

- **Microservices (Fraud, Customer, Notification):**
  - The 'Fraud' service detects potentially fraudulent activities.
  - The 'Customer' service manages customer-related operations.
  - The 'Notification' service deals with sending alerts and notifications.

- **Data Storage:**
  - Each service interacts with its own MongoDB database, demonstrating a pattern where microservices are paired with 
  - separate databases to ensure loose coupling and data integrity.

- **Message Queues (Kafka, RabbitMQ):**
  - Kafka handles high-throughput message processing.
  - RabbitMQ is used for lighter, more specific tasks like sending notifications.

- **Eureka Server:**
  - Acts as a service registry that helps in the discovery of microservices. It allows services to find and communicate 
  - with each other without hard-coded locations, which enhances the flexibility and scalability of the system.

- **Config Server:**
  - Centralizes external configuration management across all microservices, enabling them to adapt to new configurations
  - in real-time without downtime.

- **Distributed Tracing (Zipkin):**
  - Integrated through Sleuth, Zipkin provides insights into the request flow across microservices, aiding in debugging 
  - and monitoring.

- **Private Docker Registry:**
  - Facilitates secure management and deployment of Docker images that are used to run the microservices, ensuring that 
  - all components are up-to-date and consistent across different environments.

This design exemplifies a robust, scalable microservices architecture capable of supporting complex business applications.
It leverages modern tools and practices to ensure that each component functions efficiently while maintaining the system's
overall resilience and responsiveness.
![](https://cdn.fs.teachablecdn.com/vy03BC0TiCQ9NcnC1agV)



## Parent Maven Project

### Declaring Dependencies in Parent Maven Project
Those dependencies are included in the build of the parent project itself. They will also be inherited by all child 
projects, making those dependencies available to the child projects without needing to explicitly define them in the 
child POMs
```xml
 <dependencies>
    <dependency>
      <groupId>org.projectlombok</groupId>
      <artifactId>lombok</artifactId>
    </dependency>
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-test</artifactId>
    </dependency>
  </dependencies>
```

### Declaring Dependencies Management and Dependencies in Parent Maven Project
The purpose of this section is to specify the versions of the dependencies that the child modules should use, without 
actually including these dependencies in the parent project's build. The child modules then inherit these versions and 
can include the dependencies in their own builds without specifying the versions again.
```xml
<dependencyManagement>
    <dependencies>
      <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-dependencies</artifactId>
        <version>${spring.boot.dependencies.version}</version> 
        <scope>import</scope>
        <type>pom</type>
      </dependency>
    </dependencies>
  </dependencyManagement>
```

## Customer Service 
#### [Github Link Commit History Link](https://github.com/TLzzs/microservices/commit/84b01deef78aa655b9de4c8eb29deb622e95cb07)
MileStone: Want to create a customer service , where provide an api so that user can use this api to register a customer
in to our system, the customer will be saved into our Database 

### Define Customer Entity
```java
@Data
@Builder
@AllArgsConstructor
@NoArgsConstructor
@Entity
public class Customer {

    @Id
    @SequenceGenerator(
            name = "customer_id_sequence",
            sequenceName = "customer_id_sequence"
    )
    @GeneratedValue(
            strategy = GenerationType.SEQUENCE,
            generator = "customer_id_sequence"
    )
    private Integer id;
    private String firstName;
    private String lastName;
    private String email;
}
```
### Define Repository 
```java
@Repository
public interface CustomerRepository extends JpaRepository<Customer, Integer> {
  // A Customer Repository
}
```

### Define Dto of incoming Request
```java
public record CustomerRegistrationRequest(
        String firstName,
        String lastName,
        String email) {
}
```

### Define Controller
```java
@Slf4j
@RestController
@RequestMapping("api/v1/customers")
public record CustomerController(CustomerService customerService) {

    @PostMapping
    public void registerCustomer(@RequestBody CustomerRegistrationRequest customerRegistrationRequest) {
        log.info("new customer registration {}", customerRegistrationRequest);
        customerService.registerCustomer(customerRegistrationRequest);
    }
}
```

### Define Service
```java
@Service
public record CustomerService(CustomerRepository customerRepository) {
    public void registerCustomer(CustomerRegistrationRequest request) {
        Customer customer = Customer.builder()
                .firstName(request.firstName())
                .lastName(request.lastName())
                .email(request.email())
                .build();
        // todo: check if email valid
        // todo: check if email not taken
        customerRepository.save(customer);

    }
}
```

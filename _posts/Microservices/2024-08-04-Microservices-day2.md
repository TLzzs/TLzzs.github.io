---
title: "Build A MicroService | Day 2 Build Fraud Service,  Enable Communication Between Services"
date: 2024-08-04 00:00:00
categories: [Microservices]
tags: [Microservices]
---

### Overview
I will build a Fraud Service, it provides an api, where client can use this api to confirm if a customer is a fraudulent or not 
The Api will accept a incoming variable customerId and return a type of FraudCheckResponse

### define a Dto of FraudCheckResponse
```java
public record FraudCheckResponse (Boolean isFraudster) {
}
```

### define an entity of FraudCheckHistory 
```java
@Data
@Builder
@AllArgsConstructor
@NoArgsConstructor
@Entity
public class FraudCheckHistory {
    @Id
    @SequenceGenerator(
            name = "fraud_id_sequence",
            sequenceName = "fraud_id_sequence"
    )
    @GeneratedValue(
            strategy = GenerationType.SEQUENCE,
            generator = "fraud_id_sequence"
    )
    private Integer id;
    private Integer customerId;
    private Boolean isFraudster;
    private LocalDateTime createdAt;

}
```

### define a repository of FraudCheckHistory
```java
public interface FraudCheckHistoryRepository
        extends JpaRepository<FraudCheckHistory, Integer> {
}
```

### define a Controller 
```java
@RestController
@RequestMapping("api/v1/fraud-check")
@AllArgsConstructor
@Slf4j
public class FraudController {

    private final FraudCheckService fraudCheckService;

    @GetMapping(path = "{customerId}")
    public FraudCheckResponse isFraudster(
            @PathVariable("customerId") Integer customerID) {
        boolean isFraudulentCustomer = fraudCheckService.
                isFraudulentCustomer(customerID);
        log.info("fraud check request for customer {}", customerID);

        return new FraudCheckResponse(isFraudulentCustomer);
    }
}
```


### define Service Layer
```java
@Service
@AllArgsConstructor
public class FraudCheckService {

    private final FraudCheckHistoryRepository fraudCheckHistoryRepository;

    public boolean isFraudulentCustomer(Integer customerId) {
        fraudCheckHistoryRepository.save(
                FraudCheckHistory.builder()
                        .customerId(customerId)
                        .isFraudster(false)
                        .createdAt(LocalDateTime.now())
                        .build()
        );
        return false;
    }

}
```


### modify the service layer in customer 
```java
@Service
@AllArgsConstructor
public class CustomerService {

  private final CustomerRepository customerRepository;
  private final RestTemplate restTemplate;

  public void registerCustomer(CustomerRegistrationRequest request) {
    Customer customer = Customer.builder()
      .firstName(request.firstName())
      .lastName(request.lastName())
      .email(request.email())
      .build();
    // todo: check if email valid
    // todo: check if email not taken
    customerRepository.saveAndFlush(customer);
    // todo: check if fraudster
    FraudCheckResponse fraudCheckResponse = restTemplate.getForObject(
      "http://localhost:8081/api/v1/fraud-check/{customerId}",
      FraudCheckResponse.class,
      customer.getId()
    );

    if (fraudCheckResponse.isFraudster()) {
      throw new IllegalStateException("fraudster");
    }
  }
}
```

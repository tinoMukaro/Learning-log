Java Microservices Notes

1. What Are Microservices?

Microservices architecture is a way of building applications as a collection of small independent services.

Each service:

Handles one business function
Runs independently
Has its own database (sometimes)
Communicates over network APIs

Example:

User Service
Product Service
Order Service
Payment Service

Instead of one huge application (monolith), the system is split into smaller apps.

2. Monolith vs Microservices
   Monolith

Everything is inside one application.

Example:

Single App
├── Users
├── Products
├── Orders
└── Payments
Problems
Hard to scale
Hard to maintain
One bug can crash entire system
Slow deployments
Microservices

Each feature is separated.

User Service
Product Service
Order Service
Payment Service
Advantages
Independent deployment
Easier scaling
Easier maintenance
Teams can work separately
Disadvantages
More complex setup
Network communication issues
Distributed debugging
Requires DevOps knowledge 3. Main Components in Java Microservices

Typical Spring Boot microservices ecosystem:

Client
↓
API Gateway
↓
Microservices
↓
Database

Additional infrastructure:

Eureka Server
Kafka
Redis
Config Server
Docker
Monitoring tools 4. Spring Boot

Most Java microservices use:

Spring Boot
Spring Cloud

Spring Boot helps create standalone applications quickly.

Example:

@SpringBootApplication
public class UserServiceApplication {
public static void main(String[] args) {
SpringApplication.run(UserServiceApplication.class, args);
}
} 5. REST APIs

Services communicate using HTTP APIs.

Example:

GET /users/1
POST /products
PUT /orders/3
DELETE /cart/2

Controller example:

@RestController
@RequestMapping("/users")
public class UserController {

    @GetMapping("/{id}")
    public String getUser(@PathVariable Long id) {
        return "User " + id;
    }

} 6. DTO (Data Transfer Object)

DTOs transfer data between layers/services.

Purpose:

Hide internal entities
Reduce unnecessary data
Improve security

Example:

public class UserDTO {
private String name;
private String email;
}

Instead of returning entity directly:

return userDTO; 7. Entity

Represents database table.

Example:

@Entity
public class User {

    @Id
    @GeneratedValue
    private Long id;

    private String username;

} 8. Repository Layer

Handles database operations.

Example:

public interface UserRepository
extends JpaRepository<User, Long> {
}

Spring automatically creates CRUD methods.

9. Service Layer

Contains business logic.

Example:

@Service
public class UserService {

    public User createUser(User user){
        return repository.save(user);
    }

} 10. Maven

Build tool for Java projects.

Used for:

Dependency management
Building applications
Packaging JAR files

Main file:

pom.xml

Build project:

mvn clean install
What it does
Cleans old builds
Downloads dependencies
Compiles code
Runs tests
Creates JAR 11. Multi-Module Projects

Large systems often have shared libraries.

Example:

commons-library
user-service
product-service
order-service

Shared code goes into commons.

Example shared items:

DTOs
Utility classes
Exception handlers
Security configs 12. API Gateway

Single entry point for all requests.

Example:

Client → API Gateway → Services

Responsibilities:

Routing
Authentication
Rate limiting
Logging
Security

Popular gateways:

Spring Cloud Gateway
NGINX
Kong 13. NGINX

Can act as:

Reverse proxy
Load balancer
API gateway

Example:

Client → NGINX → User Service

Used heavily in production.

14. Service Discovery (Eureka)

Services register themselves dynamically.

Example:

User Service → Eureka
Order Service → Eureka

Other services can discover them.

Without Eureka:

Hardcoded URLs

With Eureka:

Service names

Example:

@FeignClient(name = "USER-SERVICE") 15. Eureka Server

Acts like a phonebook for services.

Example:

@EnableEurekaServer

Services register automatically.

16. Feign Client

Simplifies service-to-service communication.

Without Feign:

RestTemplate
WebClient

With Feign:

@FeignClient(name = "USER-SERVICE")
public interface UserClient {

    @GetMapping("/users/{id}")
    UserDTO getUser(@PathVariable Long id);

} 17. Kafka

Distributed event streaming platform.

Used for:

Messaging
Event-driven systems
Async communication

Example:

Order Created Event
↓
Kafka
↓
Notification Service
Inventory Service
Payment Service 18. Kafka Producer

Sends messages.

Example:

kafkaTemplate.send("orders", order); 19. Kafka Consumer

Receives messages.

Example:

@KafkaListener(topics = "orders")
public void consume(Order order){
System.out.println(order);
} 20. Why Kafka Is Used

Benefits:

Loose coupling
Async processing
Better scalability
Faster systems

Example:
Instead of waiting for email service:

Order Service → Kafka → Email Service 21. Redis

In-memory database.

Very fast.

Used for:

Caching
Sessions
Temporary data
Rate limiting

Example:

Database query result → Redis cache

Benefits:

Reduces DB load
Faster responses 22. Config Server

Centralized configuration management.

Instead of:

Each service has configs

Use:

One config server

Stores:

Database URLs
Secrets
Environment configs 23. Profiles

Different configs for environments.

Example:

application-dev.properties
application-prod.properties

Run:

-Dspring.profiles.active=dev 24. Docker

Containerization platform.

Packages app with:

Java
Dependencies
Runtime

Example:

FROM openjdk:17
COPY target/app.jar app.jar
ENTRYPOINT ["java","-jar","app.jar"]

Build:

docker build -t user-service .

Run:

docker run -p 8080:8080 user-service 25. Docker Compose

Runs multiple containers together.

Example:

services:
kafka:
redis:
mysql:
user-service:

Useful in local development.

26. Databases in Microservices

Options:

Shared database
Database per service

Preferred:

One database per service

Advantages:

Loose coupling
Independent scaling 27. Communication Types
Synchronous

Waits for response.

Example:

REST API
Feign Client
Asynchronous

Does not wait.

Example:

Kafka
RabbitMQ 28. Load Balancing

Distributes traffic.

Example:

Client
↓
Load Balancer
↓
3 User Service Instances

Improves:

Scalability
Availability 29. Circuit Breaker

Prevents cascading failures.

If service is down:

Stop repeated calls

Popular tool:

Resilience4j 30. Logging

Microservices generate massive logs.

Common tools:

ELK Stack
Grafana
Loki

Basic logging:

log.info("User created"); 31. Reading Java Logs

Focus on:

ERROR
WARN
Exception name
Stack trace start

Example:

NullPointerException

Usually first meaningful error matters most.

32. Common Errors
    Port already in use
    8080 already occupied

Fix:

server.port=8081
Bean creation error

Usually dependency/config issue.

Kafka connection error

Kafka container not running.

Database connection failed

Wrong credentials or DB offline.

33. Common Ports
    Service Port
    Spring Boot 8080
    Eureka 8761
    Kafka 9092
    MySQL 3306
    Redis 6379
    MinIO 9000
34. MinIO

Object storage system.

Used for:

File uploads
Images
PDFs
Videos

Works like AWS S3.

Example:

Profile picture uploads

Bucket = storage folder/container.

35. Authentication

Common methods:

JWT Tokens
OAuth2

Flow:

Login → Token → Access APIs 36. JWT

JSON Web Token.

Contains:

User ID
Roles
Expiry

Example:

Authorization: Bearer token 37. Spring Security

Handles:

Authentication
Authorization
Roles
Protected endpoints 38. Microservices Deployment Flow

Typical workflow:

Code
↓
GitHub
↓
CI/CD Pipeline
↓
Docker Build
↓
Deploy Server/Kubernetes 39. CI/CD

Continuous Integration / Deployment.

Tools:

GitHub Actions
Jenkins
GitLab CI

Purpose:

Automatic testing
Automatic deployment 40. Kubernetes (K8s)

Container orchestration platform.

Manages:

Scaling
Deployments
Failures
Networking

Mostly used in large production systems.

41. Health Checks

Spring Boot Actuator:

/actuator/health

Checks if service is alive.

42. Common Spring Annotations
    Controller
    @RestController
    Service
    @Service
    Repository
    @Repository
    Dependency Injection
    @Autowired
43. Dependency Injection

Spring automatically provides objects.

Example:

@Autowired
private UserService userService; 44. Environment Variables

Used for secrets/configs.

Example:

DB_PASSWORD=${DB_PASSWORD} 45. Why Microservices Are Hard

Challenges:

Distributed systems
Debugging across services
Network failures
Monitoring
Deployment complexity 46. Typical Real-World Architecture
Frontend
↓
NGINX / API Gateway
↓
Microservices
├── User Service
├── Product Service
├── Order Service
└── Payment Service
↓
Kafka / Redis / Databases 47. What Juniors Should Focus On

Learn in this order:

Spring Boot basics
REST APIs
JPA/Hibernate
DTOs
Maven
Microservices basics
API Gateway
Kafka
Docker
Security 48. Important Commands
Maven
mvn clean install
Run Spring Boot
mvn spring-boot:run
Docker
docker ps
docker images
docker logs container_id 49. Typical Folder Structure
src/main/java
├── controller
├── service
├── repository
├── dto
├── entity
├── config
├── exception
└── security 50. Final Big Picture

Microservices are:

Many small Spring Boot apps
Communicating through APIs/events
Managed using infrastructure tools

Core technologies:

Spring Boot
Maven
Kafka
Docker
Redis
Eureka
API Gateway
Databases

Goal:

Scalable + Maintainable + Independent systems

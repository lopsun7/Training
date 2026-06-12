# HW12

This homework is a Spring Boot interview question bank written in my own words.

It is based partly on my current project:
[student-management-system](https://github.com/lopsun7/student-management-system)

My current project version notes:

- Spring Boot: `3.5.0`
- Spring Framework: `6.2.7`
- Java: `21`

Source page: [HW 12 on Notion](https://app.notion.com/p/37dc5511f06081faa875eeac184df5ec)

## 1. Introduce Spring Framework

Spring Framework is a Java framework for building enterprise applications.

It provides:

- IoC container
- dependency injection
- AOP
- transaction management
- Spring MVC
- validation
- testing support
- integration with database, messaging, security, and cloud tools

My own summary:
Spring Framework gives developers a structured way to build maintainable Java applications instead of wiring everything manually.

## 2. Why Spring? Why Spring Boot? Spring Boot advantages

Spring is popular because it is modular, mature, and widely used in enterprise systems.

Spring Boot is built on top of Spring and makes development faster.

Main Spring Boot advantages:

- auto-configuration
- starter dependencies
- embedded server like Tomcat
- easy external configuration
- production-ready tools like Actuator
- less XML and less boilerplate
- easier testing

My own summary:
If Spring Framework gives me the building blocks, Spring Boot gives me a faster and more opinionated way to assemble them.

## 3. Give me an example where you use Spring Boot

I used Spring Boot in my student management project.

I built:

- REST controller endpoints
- service layer
- JPA repository
- PostgreSQL integration
- validation
- global exception handling
- AOP logging
- Actuator
- simple async processing

That project is a good example of using Spring Boot to build a backend CRUD API from frontend request to database persistence.

## 4. What Spring version and Spring Boot version did you use?

In my current project, I used:

- Spring Boot `3.5.0`
- Spring Framework `6.2.7`
- Java `21`

This is based on my current `pom.xml` and dependency tree as of June 12, 2026.

## 5. What Java version can we use with Spring Boot 3?

Spring Boot 3 started with Java 17 as the minimum requirement.

My practical answer:

- Spring Boot 3 requires at least Java 17
- my own project uses Java 21

So in interviews I would say:
For Spring Boot 3, Java 17+ is the safe answer, and many teams now use Java 17 or Java 21.

## 6. New features from Spring Boot 3

Important Spring Boot 3 level changes include:

- based on Spring Framework 6
- uses `jakarta.*` instead of `javax.*`
- improved AOT and native image support
- better observability and metrics integration
- better support for modern Java versions
- support for Problem Details style error responses in modern Spring web apps

My own summary:
Spring Boot 3 modernized the stack, especially around Java 17+, Jakarta migration, observability, and native image support.

## 7. What is Spring Boot feature set?

Common Spring Boot features:

- stand-alone application startup
- embedded web server
- auto-configuration
- starter dependencies
- externalized configuration
- profiles
- Actuator
- integration with Spring Data, Security, Validation, AOP, and testing

## 8. What is `@EnableAutoConfiguration`? What is auto-configuration?

Auto-configuration means Spring Boot automatically configures beans based on:

- dependencies on the classpath
- application properties
- existing beans

For example:

- if `spring-boot-starter-data-jpa` is present, Spring Boot can configure JPA basics
- if `spring-boot-starter-web` is present, it can configure Spring MVC and embedded Tomcat

`@SpringBootApplication` already includes `@EnableAutoConfiguration`.

My own summary:
Auto-configuration reduces manual setup by letting Spring Boot make smart default decisions.

## 9. How to stop auto-configuration in Spring Boot

There are several ways:

1. Exclude it on the main class:

```java
@SpringBootApplication(exclude = DataSourceAutoConfiguration.class)
```

2. Exclude it in properties:

```properties
spring.autoconfigure.exclude=org.springframework.boot.autoconfigure.jdbc.DataSourceAutoConfiguration
```

3. Avoid adding the starter dependency that triggers it

## 10. What is profiling in Spring Boot?

Profiling in Spring Boot usually means using Spring Profiles to load different configuration for different environments.

For example:

- `dev`
- `test`
- `prod`

A profile lets me change behavior without changing the code.

## 11. How do you define a profile? Where do you define it?

A profile can be defined in two common places:

1. In configuration files
2. On beans or configuration classes with `@Profile`

Examples:

```properties
spring.profiles.active=dev
```

```java
@Configuration
@Profile("prod")
public class ProdConfig {
}
```

## 12. Where and how can you load profiles in Spring Boot?

Common ways to load profiles:

1. In `application.properties` or `application.yml`
2. In profile-specific files like `application-dev.properties`
3. By environment variable:

```text
SPRING_PROFILES_ACTIVE=prod
```

4. By JVM argument:

```text
-Dspring.profiles.active=prod
```

5. By command line argument:

```text
--spring.profiles.active=prod
```

6. In IDE run configuration

My summary:
Profiles can be defined in files or annotations, and activated by properties, environment variables, JVM args, command line, or IDE settings.

## 13. How can you use profile?

I can use profiles to:

- switch database connection between local and production
- enable mock beans in development
- load different third-party API keys
- change logging level
- enable or disable certain beans

## 14. How Spring IoC works

IoC means Inversion of Control.

Instead of me manually creating all objects with `new`, Spring creates and manages them in the container.

Basic flow:

1. Spring scans configuration and annotations
2. It creates bean definitions
3. It creates bean instances
4. It resolves dependencies
5. It injects dependencies
6. It manages bean lifecycle

Dependency Injection is the main way Spring implements IoC.

## 15. Spring IoC annotations, bean annotations, and Spring Boot annotations

Important annotations I use or should know:

Core stereotype annotations:

- `@Component`
- `@Service`
- `@Repository`
- `@Controller`
- `@RestController`

Configuration annotations:

- `@Configuration`
- `@Bean`
- `@ComponentScan`
- `@Profile`
- `@SpringBootApplication`
- `@EnableAutoConfiguration`

DI annotations:

- `@Autowired`
- `@Qualifier`
- `@Resource`

Web annotations:

- `@RequestMapping`
- `@GetMapping`
- `@PostMapping`
- `@PutMapping`
- `@DeleteMapping`
- `@PathVariable`
- `@RequestParam`
- `@RequestBody`
- `@ResponseBody`

Validation annotations:

- `@Valid`
- `@NotBlank`
- `@NotNull`
- `@Size`
- `@Email`

Transaction annotations:

- `@Transactional`

AOP annotations:

- `@Aspect`
- `@Pointcut`
- `@Before`
- `@After`
- `@Around`

Testing annotations:

- `@SpringBootTest`
- `@AutoConfigureMockMvc`
- `@Test`

Actuator customization annotations:

- `@Endpoint`
- `@ReadOperation`
- `@WriteOperation`
- `@DeleteOperation`

## 16. How many ways to inject bean? Injection types

Common ways:

1. Constructor injection
2. Setter injection
3. Field injection

Example of constructor injection:

```java
public StudentController(StudentService studentService) {
    this.studentService = studentService;
}
```

This is the one I use most.

## 17. Why constructor injection?

I prefer constructor injection because:

- dependency is explicit
- easier to test
- supports immutable fields with `final`
- prevents partially initialized objects
- better for required dependencies

My own summary:
Constructor injection makes the class design cleaner and safer.

## 18. By name vs by type injection

By type means Spring injects based on the Java type.

By name means Spring injects based on the bean name.

### By type

This is the default behavior of `@Autowired`.

If only one bean of that type exists, Spring injects it directly.

### By name

This is used when multiple beans have the same type.

Common ways:

- `@Qualifier("beanName")`
- `@Resource(name = "beanName")`

Example:

```java
public NotificationService(@Qualifier("emailService") MessageService messageService) {
    this.messageService = messageService;
}
```

## 19. How to inject bean with same type

If there are multiple beans of the same type, I can:

- use `@Qualifier`
- use `@Resource(name = "...")`
- mark one bean as `@Primary`

This avoids ambiguity.

## 20. Bean scope and bean types

Common bean scopes:

- `singleton`
- `prototype`
- `request`
- `session`
- `application`
- `websocket`

In Spring Boot, `singleton` is the default scope.

In my project, service and repository beans are singleton by default.

## 21. `@Controller` vs `@RestController`

`@Controller` is mainly for MVC pages and views.

`@RestController` is for REST APIs and returns JSON directly.

`@RestController` is basically:

```java
@Controller + @ResponseBody
```

## 22. What is controller? How do you implement controller?

A controller receives HTTP requests and returns responses.

In Spring Boot, I usually implement a controller with:

- `@RestController`
- `@RequestMapping`
- request mapping methods like `@GetMapping` and `@PostMapping`

Example from my project:

```java
@RestController
@RequestMapping("/api/v1/students")
public class StudentController {
}
```

Controller responsibility:

- receive request
- parse input
- call service layer
- return response

## 23. How do you write REST API in Spring Boot?

Typical steps:

1. Create entity
2. Create repository
3. Create service
4. Create controller
5. Add validation
6. Add exception handling
7. Test with Postman or MockMvc

Common annotations:

- `@GetMapping`
- `@PostMapping`
- `@PutMapping`
- `@DeleteMapping`
- `@RequestBody`
- `@PathVariable`
- `@RequestParam`

## 24. How to write Spring Boot from frontend to backend and save data to database

Typical flow:

1. Frontend sends HTTP request
2. Controller receives the request
3. Request body is converted into a Java object
4. Validation runs
5. Service handles business logic
6. Repository saves data to database
7. Response is returned to frontend

Example flow in my project:

- frontend sends student JSON
- backend `StudentController` receives it
- `StudentServiceImpl` processes it
- `StudentRepository` saves it into PostgreSQL

## 25. How do you connect the database in Spring Boot?

In my project, I connected PostgreSQL through:

- `spring.datasource.url`
- `spring.datasource.username`
- `spring.datasource.password`

I also used Spring Data JPA and Hibernate.

Example:

```properties
spring.datasource.url=jdbc:postgresql://localhost:5432/student_management_system
spring.datasource.username=postgres
spring.datasource.password=postgres
```

## 26. What is DispatcherServlet? Describe Spring MVC

`DispatcherServlet` is the front controller in Spring MVC.

It is the central entry point for web requests.

Spring MVC is the Servlet-based web framework in Spring.

### How Spring MVC works

1. Client sends HTTP request
2. `DispatcherServlet` receives it
3. It finds the matching controller method
4. Controller handles the request
5. Service layer runs business logic
6. Repository or DAO accesses database
7. Response is returned to client

My summary:
`DispatcherServlet` coordinates the request flow in Spring MVC.

## 27. What is WebFlux? Have you used it?

Spring WebFlux is the reactive web stack in Spring.

It is:

- non-blocking
- asynchronous
- based on reactive programming
- often used with Reactor types like `Mono` and `Flux`

Difference from Spring MVC:

- Spring MVC is Servlet-based and usually blocking
- WebFlux is reactive and non-blocking

Honest answer:
I have basic understanding of WebFlux, but I did not use WebFlux in my current student management project. My project uses Spring MVC.

## 28. What is AOP? Why AOP?

AOP means Aspect-Oriented Programming.

It is used to separate cross-cutting concerns from business logic.

Examples:

- logging
- transaction management
- security checks
- performance monitoring

Why use AOP:

- less repeated code
- cleaner business logic
- centralized technical logic

## 29. What is advice?

Advice is the action that runs at a matched join point.

Common advice types:

- `@Before`
- `@After`
- `@AfterReturning`
- `@AfterThrowing`
- `@Around`

## 30. Where did you use AOP in your application?

I used Spring AOP in my student management project for service-layer logging.

I put a pointcut on the `serviceimpl` package and added:

- `@Before`
- `@After`
- `@Around`

That lets me print method signature logs without putting logging code in every service method.

## 31. Could AOP apply to private method?

In normal Spring proxy-based AOP, private methods are not intercepted.

Also, self-invocation inside the same class does not go through the proxy again.

That is exactly what I demonstrated in my project:

- external service method call was intercepted
- internal helper call was not intercepted
- private helper method was not intercepted

If someone asks deeper:
AspectJ weaving can handle more cases than normal proxy-based Spring AOP.

## 32. How do you handle global exception in Spring Boot?

I usually use:

- `@ControllerAdvice` for general MVC
- `@RestControllerAdvice` for REST API

In my project I used `@RestControllerAdvice`.

That lets me centralize exception handling in one place.

## 33. How do you handle exception in Spring Boot? How do you implement exception?

I normally:

1. create custom exception class
2. throw it in service layer
3. catch it globally with `@RestControllerAdvice`
4. return structured JSON error response

In my project:

- I created `ResourceNotFoundException`
- I handled it in `GlobalExceptionHandler`

## 34. How do you handle exceptions and validation in Spring?

Validation flow:

- annotate fields with validation annotations
- use `@Valid` in controller method
- catch `MethodArgumentNotValidException` globally

Exception flow:

- service throws business exception
- global exception handler converts it into response JSON

This keeps controller code cleaner and gives consistent error format.

## 35. How do you validate input data in Spring Boot?

I use:

- `@Valid`
- Bean Validation annotations such as `@NotBlank`, `@Email`, and `@Size`

Example from my project:

- `firstName` uses `@NotBlank`
- `email` uses `@Email`
- `course` uses `@Size`

## 36. What does `@Transactional` do? How do you use it to manage request?

`@Transactional` tells Spring to run the method inside a database transaction.

This helps:

- maintain data consistency
- rollback on failure
- group multiple DB operations as one unit

In my project, I used it on the service layer.

Typical idea:

- controller receives request
- service method with `@Transactional` handles business logic
- if one step fails, transaction can roll back

I usually put `@Transactional` on service methods, not controller methods.

## 37. How do you handle security of REST endpoint?

Common approaches:

- authentication
- authorization
- token validation
- HTTPS
- input validation
- exception handling
- CORS configuration

With Spring Security, typical protections are:

- JWT or session authentication
- role-based access control
- endpoint restrictions

## 38. Authentication process

Basic authentication flow:

1. user sends credentials
2. backend verifies identity
3. backend creates authenticated context or token
4. later requests include token or session
5. backend checks whether the request is authenticated and authorized

In JWT style flow:

1. login
2. server returns token
3. frontend sends token in header
4. server validates token on each protected request

## 39. How did you use Spring Security? Spring Security experience

Honest answer:
I have basic understanding of Spring Security, but I did not add Spring Security into my current student management project yet.

If I add it, I would typically use it for:

- login authentication
- role-based authorization
- protecting REST endpoints
- password encoding
- JWT or session-based security

## 40. Have you used Spring AOP, Spring Security, and Spring Batch?

My honest answer:

- Spring AOP: yes, I used it in my current project for service-layer logging
- Spring Security: basic understanding, not used in this current project
- Spring Batch: I know it is for batch jobs and chunk processing, but I have not used it in this project

## 41. Have you used Spring Cloud?

I have basic understanding of Spring Cloud concepts, but I did not use Spring Cloud in this student management project.

Spring Cloud is commonly used in microservice environments for:

- config server
- discovery service
- gateway
- circuit breaker
- distributed tracing

## 42. What is discovery service, and why use it?

A discovery service keeps track of microservice instances.

Why use it:

- services can find each other dynamically
- easier scaling
- avoids hardcoding service addresses
- useful in cloud and microservice environments

## 43. What discovery service implementation did you use before?

Honest answer:
I did not use discovery service in my current project.

My basic understanding is that common choices include:

- Eureka
- Consul
- Nacos
- Kubernetes service discovery

## 44. What annotations and configuration are used in Eureka?

Basic Eureka server setup usually includes:

- `@EnableEurekaServer` on the server
- `@EnableDiscoveryClient` or Spring Cloud discovery support on clients

Common config examples:

```properties
eureka.client.service-url.defaultZone=http://localhost:8761/eureka
spring.application.name=user-service
```

Honest note:
I know the basic concept, but I did not configure Eureka in my current project.

## 45. What is CQRS pattern? Have you used it?

CQRS means Command Query Responsibility Segregation.

It separates:

- command side for write operations
- query side for read operations

Benefits:

- clearer separation
- possible independent scaling
- optimized read/write models

Honest answer:
I did not use CQRS in my current project. My student management project uses a simpler standard CRUD architecture.

## 46. What is Spring Boot Actuator?

Spring Boot Actuator provides production-ready monitoring and management endpoints.

Examples:

- `/actuator/health`
- `/actuator/info`
- `/actuator/metrics`

I added Actuator to my current project.

## 47. What dependency is needed for Actuator?

The dependency is:

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```

## 48. What annotations do we use for customized Actuator?

For custom actuator endpoints, common annotations are:

- `@Endpoint`
- `@ReadOperation`
- `@WriteOperation`
- `@DeleteOperation`

Usually the custom endpoint class is also managed as a Spring bean.

## 49. What test frameworks did you use?

In my current project I used:

- JUnit 5
- Spring Boot Test
- MockMvc
- AssertJ
- H2 for test database
- Mockito support from `spring-boot-starter-test`

## 50. CompletableFuture / multithreading in Spring Framework

In Spring projects, async and multithreading can be handled through:

- `@Async`
- thread pools
- `CompletableFuture`
- scheduled tasks

`CompletableFuture` is useful when:

- chaining async tasks
- combining multiple async results
- avoiding blocking the main thread

In my project, I added a simple `@Async` logging example after student creation.

## 51. How many ways to load profile in Spring Boot?

Short answer:

- profile-specific config files
- `spring.profiles.active` in properties or yaml
- environment variables
- JVM arguments
- command line arguments
- IDE run configuration
- `@Profile` for bean-level activation

## 52. How do you handle by-name injection?

I usually handle by-name injection with:

- `@Qualifier("beanName")`
- `@Resource(name = "beanName")`

This is important when there are multiple beans of the same type.

## 53. How do you handle security of REST endpoint in practical terms?

Practical checklist:

- protect endpoints with Spring Security
- authenticate the user
- authorize by roles or permissions
- validate request body
- sanitize input where necessary
- return proper status codes like `401` and `403`
- use HTTPS
- avoid exposing internal exception details

## 54. How to code a Spring Boot project to fetch URL from 3rd party API

High-level steps:

1. create Spring Boot project
2. add web dependency
3. create service class for external API call
4. use `RestClient`, `WebClient`, or `RestTemplate`
5. expose controller endpoint

Simple example with modern style:

```java
@Service
public class WeatherService {

    private final RestClient restClient;

    public WeatherService(RestClient.Builder builder) {
        this.restClient = builder.baseUrl("https://api.example.com").build();
    }

    public String getWeather() {
        return restClient.get()
                .uri("/weather")
                .retrieve()
                .body(String.class);
    }
}
```

## 55. How to write Spring Boot to call frontend, backend, and save data

If the question means full request flow, my answer is:

1. frontend sends JSON
2. controller receives request
3. `@RequestBody` converts JSON to object
4. `@Valid` validates data
5. service handles business logic
6. repository saves to database
7. backend returns JSON response

That is exactly the pattern I used in my student management project.

## 56. Have you used Spring annotations? What annotations did you use?

Yes, in my current project I used annotations such as:

- `@SpringBootApplication`
- `@RestController`
- `@RequestMapping`
- `@GetMapping`
- `@PostMapping`
- `@PutMapping`
- `@DeleteMapping`
- `@PathVariable`
- `@RequestParam`
- `@RequestBody`
- `@Service`
- `@Transactional`
- `@Entity`
- `@Table`
- `@Id`
- `@GeneratedValue`
- `@Column`
- `@Valid`
- `@NotBlank`
- `@Email`
- `@Size`
- `@RestControllerAdvice`
- `@ExceptionHandler`
- `@Aspect`
- `@Pointcut`
- `@Before`
- `@After`
- `@Around`
- `@EnableAsync`
- `@Async`

## Final summary

- Spring Framework provides IoC, DI, MVC, AOP, transactions, validation, and testing support.
- Spring Boot makes Spring development faster with auto-configuration, starters, and embedded server support.
- My current project uses Spring Boot `3.5.0`, Spring Framework `6.2.7`, and Java `21`.
- I used Spring MVC, JPA, validation, exception handling, AOP, Actuator, async, and PostgreSQL in my project.
- I have direct project experience with Spring Boot and Spring AOP, while Spring Security, WebFlux, Spring Cloud, Eureka, Spring Batch, and CQRS are areas where I currently have conceptual understanding rather than project usage.

## Version note

For the version-related answers above, I matched them against:

- my local project `pom.xml`
- my local dependency tree
- official Spring Boot and Spring Framework docs as checked on June 12, 2026

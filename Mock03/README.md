# Mock 03

This entry records the third mock interview submission video and the related Spring interview scripts.

## Video Submission

- Recording date: `2026-06-15`
- Storage: AWS S3
- Video link: [Open the third mock interview recording](https://amzn-s3-shykid7-bucket.s3.us-east-2.amazonaws.com/Mocking%2003.mov?response-content-disposition=inline&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJ7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLWVhc3QtMiJHMEUCIGkA9s6Z6d07nQmdM3qGZvsmveyvsk2Iy6m98wSofKZCAiEAn5kShyjRq1Q4blF%2B%2B81YOCxQevHOyjyFC%2Ftt3Mw5VHAquQMIZxAAGgw0MTc4MzUwMTQxNDUiDMXGPAqh%2FG7AIIkDeiqWAxKjjpeZ08aiO3uY6B0jnnCrJR5JyAaYiI%2BEYLka%2BhuDksR5SqLNZ8gMsDug0vMHHd1ckgT5nztEvC%2FzlKcsSUt0%2B3%2BIsUO4a3Pg6dqkLLamPJjXQkUOchmlXquS1Y%2FPRHI6GHx7xgYl9Gy1AWZK5TUJhKJEN%2B573%2F4tdSEGv2x3UiFbEPA%2FSb61b8ueSLNZCdEJveoO3Zdc%2B0X3JT7MEJlDeqj0dH%2BFRqZhDjjxn2vrjl%2FtNhzo0dFGkff61q03kEKvFDgnCIbwczw7t2p1kXTkMuP49NPQxNDAHgOxiX51tx%2FSJp4Mn3QyH4DduLvEuBz%2FjjP57HyxfwdsdCYiUtRgcdR6X32HceOqMuI%2F4okROJeR324S%2BPbyblrgNmYW4lMaLa8jKxjxo7Ol5d0L6TTCA060bv7%2BWnLD2JgfGZ7Q3%2FTtGXhFXUhe92s8yDekq5zVovQ2tZBEihv2gy7UAaC8nXfcI0sos%2FkxjMJ%2FmngUN%2B%2FxV58KRU7Tsixwlc9BUjpWI4xDcZGioJN53yjvutxS3qwm14owo%2BvB0QY63gI78fikQzaViHKCJ9G%2BbNcH4vzCaNYpxFEBGzfNrMZH4it7LUcaJ0a50g4qfB7w0X5208DQmpEDHRzrSd%2FEFtx7vc41cGwEnfS46GpDgIlX6n1xqv9pV%2BNhP%2FrschLQU7xAk1h%2Fm%2BPuNT6w3Ky13AUZSdE23eV05jynfDoG0GhDvEGVXXrSGbwwlmM9ze8wzBjBpui68AaulfCQZKMo6fa8ojVu4I2gS1vhgB2YVD4Vw5fCIJIPPk423DpSrxjZ7s%2BKDzeOtndVuTVSJgsfk0hG%2B8%2B4biDWsZsn%2BvXe9uOx7shkIS1bv4Mo%2BaELWRoVLXh3MJIqa4nfSHsdBbZdiWbFp6ar69nlV1h3Y2WWm39Kw0N8FpVOJDs17ybOQ9y2pITLq0Sm2tmthlWU2LlQ5lPRkFTdNk3aAEwf%2B8OLFVZK1KnGSJ%2BZCOtndFTU5qNG5l9BBYj13LooQXHL7SS96A%3D%3D&X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=ASIAWCSHILAAZ2JO4UPG%2F20260615%2Fus-east-2%2Fs3%2Faws4_request&X-Amz-Date=20260615T220653Z&X-Amz-Expires=43200&X-Amz-SignedHeaders=host&X-Amz-Signature=282a2ebc8af18324da42e2ddcbd5e4526dae8e9b4f1bfc250b7bb963d0d59abc)

## Mock Scripts

### 1. Introduce what is Spring Framework

Spring Framework is a Java framework used to build enterprise applications. Its core feature is IoC, which means Spring manages object creation and dependency injection for us. It also provides modules like Spring MVC, AOP, transaction management, data access, security, and integration. In real projects, I mainly use Spring Boot on top of Spring to build REST APIs and microservices faster.

### 2. Spring Boot version you used

I mainly used Spring Boot 3.x in recent projects, with Java 17 or Java 21. I also have experience with Spring Boot 2.x. In Spring Boot 3, one important change is that it uses Jakarta EE packages, so `javax` changed to `jakarta`.

### 3. How do you define profile

In Spring Boot, profile is used to separate configurations for different environments, like `dev`, `test`, and `prod`. For example, I can have `application-dev.yml` and `application-prod.yml`, then activate one with `spring.profiles.active=dev`. This helps me use different database URLs, Redis configs, logging levels, and external service settings.

### 4. What discovery service implementation you used before

I used Eureka before. In a microservice system, each service registers itself to Eureka Server, and other services can discover it by service name instead of hardcoding IP and port. I used it with Spring Cloud and Feign clients for service-to-service communication.

### 5. What is AOP

AOP means Aspect-Oriented Programming. It is used to handle cross-cutting concerns like logging, authentication, transaction, metrics, or auditing. Instead of writing the same logic in every business method, we define an aspect and apply it through pointcuts. In Spring, AOP is usually implemented by proxy.

### 6. How to write Spring Boot to call from frontend to backend and save data to database

Normally, the frontend sends an HTTP request to the backend controller. The controller receives the request body using `@RequestBody`, validates it, then calls the service layer. The service layer handles business logic and calls the repository layer. The repository uses Spring Data JPA or MyBatis to save data into the database. The flow is: frontend -> controller -> service -> repository -> database.

### 7. Describe Spring MVC

Spring MVC is a web framework based on the Model-View-Controller pattern. The controller handles requests, the model contains data, and the view renders the response. In REST API projects, we usually use `@RestController`, so the response is JSON instead of a traditional HTML view.

### 8. How do you validate input data in Spring Boot

I usually use Bean Validation with annotations like `@NotNull`, `@NotBlank`, `@Size`, `@Email`, and `@Min`. In the controller, I add `@Valid` before the request DTO. If validation fails, Spring throws an exception, and I handle it globally with `@RestControllerAdvice`.

### 9. Spring Boot Actuator

Spring Boot Actuator provides production-ready monitoring endpoints. For example, `/actuator/health` checks application health, `/actuator/metrics` exposes metrics, and `/actuator/info` shows app information. In real projects, it can be integrated with Prometheus and Grafana for monitoring.

### 10. How does Spring MVC work

When a request comes in, it first reaches `DispatcherServlet`. Then `DispatcherServlet` finds the correct controller method through handler mapping. The controller processes the request and returns data. Then Spring uses message converters, like Jackson, to convert the Java object into JSON response.

### 11. What is controller, how you use controller, how you implement controller

Controller is the entry point of the backend API. I use `@RestController` to define a REST controller, and use annotations like `@GetMapping`, `@PostMapping`, `@PutMapping`, and `@DeleteMapping` to handle different HTTP methods. The controller should be thin. It only receives request, validates input, calls service, and returns response.

### 12. What is WebFlux? Have you used it in your project?

WebFlux is Spring's reactive web framework. It is non-blocking and event-driven, usually used for high-concurrency scenarios, streaming, or when we need to call many external APIs. I have basic experience with it, but most of my production experience is with Spring MVC because it is simpler and fits normal REST API services well.

### 13. How do you connect the database in Spring Boot

First, I add the database dependency, like PostgreSQL or MySQL driver, and Spring Data JPA dependency. Then I configure database URL, username, password, and driver in `application.yml`. After that, I create entity classes and repository interfaces. Spring Boot auto-configures the `DataSource` and connection pool.

### 14. How do you handle global exception in Spring Boot

I use `@RestControllerAdvice` with `@ExceptionHandler`. For example, I can handle validation exceptions, business exceptions, and unexpected system exceptions in one place. Then I return a consistent error response with error code, message, and timestamp.

### 15. Spring Boot annotation

Common annotations include `@SpringBootApplication`, `@RestController`, `@Service`, `@Repository`, `@Component`, `@Autowired`, `@Configuration`, `@Bean`, `@Value`, `@Transactional`, `@Valid`, `@RequestBody`, `@PathVariable`, and `@RequestParam`. These annotations help Spring manage beans, expose APIs, inject dependencies, validate data, and manage transactions.

### 16. How Spring IoC works, annotations, injection, bean types

Spring IoC means the Spring container creates and manages objects for us. It reads metadata from annotations, configuration classes, or XML, then creates beans and injects dependencies. Common annotations are `@Component`, `@Service`, `@Repository`, `@Controller`, `@Configuration`, and `@Bean`. Bean scopes include `singleton`, `prototype`, `request`, `session`, and `application`. The default scope is `singleton`.

### 17. How many ways to inject bean in Spring and which one we use most

There are three common ways: constructor injection, setter injection, and field injection. In real projects, constructor injection is recommended because it makes dependencies explicit, supports immutability, and is easier for testing. Field injection is simple but not recommended for production code.

### 18. By name vs by type

By type means Spring injects a bean based on its class or interface type. By name means Spring injects a bean based on the bean name. Usually Spring uses by type first. If there are multiple beans of the same type, we can use `@Qualifier` to specify the bean name, or use `@Primary` to define the default bean.

### 19. Why constructor injection

Constructor injection is recommended because the dependency is required when the object is created. It makes the class easier to test, avoids null dependencies, and allows fields to be `final`. It also makes circular dependencies easier to detect during startup.

### 20. What Java version can we use with Spring Boot 3

Spring Boot 3 requires Java 17 or higher. In recent projects, I usually use Java 17 or Java 21. Java 17 is very common because it is an LTS version.

### 21. What is DispatcherServlet

DispatcherServlet is the front controller in Spring MVC. All HTTP requests first go to `DispatcherServlet`. It finds the correct controller method, calls it, handles the return value, and sends the response back to the client. It is the core component of Spring MVC request processing.

### 22. RESTful endpoint design from scratch

RESTful endpoint design means I design backend APIs based on resources, HTTP methods, clear URL naming, request and response format, status codes, validation, and security.

For example, if I design an order system, I first identify the resources, like users, products, orders, and payments.

Then I design endpoints using nouns, not verbs.

For example:

- `GET /api/v1/orders` means get order list
- `GET /api/v1/orders/{orderId}` means get one order by ID
- `POST /api/v1/orders` means create a new order
- `PUT /api/v1/orders/{orderId}` means replace or update the whole order
- `PATCH /api/v1/orders/{orderId}` means partially update the order
- `DELETE /api/v1/orders/{orderId}` means delete or cancel the order

For relationships, I can design endpoints like:

- `GET /api/v1/users/{userId}/orders`

This means get all orders for one user.

For request body, I usually use DTOs instead of exposing entity classes directly. For example, create order request may contain product ID, quantity, address, and payment method.

For response, I return a consistent JSON structure, like data, message, error code, and timestamp.

I also use proper HTTP status codes. For example, `200` for success, `201` for created, `400` for bad request, `401` for unauthorized, `403` for forbidden, `404` for not found, and `500` for internal server error.

For list APIs, I add pagination, sorting, and filtering. For example:

- `GET /api/v1/orders?page=0&size=20&sort=createdAt,desc&status=PAID`

In Spring Boot, I implement this with `@RestController`, `@RequestMapping`, `@GetMapping`, `@PostMapping`, `@RequestBody`, `@PathVariable`, and `@RequestParam`.

The common flow is: controller receives the request, validates the DTO, calls service layer for business logic, service calls repository layer, then returns response DTO to frontend.

I also consider validation, exception handling, authentication, authorization, idempotency, and API versioning. For example, payment or order creation should support idempotency key to avoid duplicate orders.

So overall, my RESTful design principle is: resource-based URL, correct HTTP method, clean request and response DTO, proper status code, pagination for list APIs, global exception handling, and clear separation between controller, service, and repository.

## Notes

- This link was provided as an S3 signed URL.
- Signed URLs may expire or become inaccessible after their validity window ends.

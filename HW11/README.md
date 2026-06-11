# HW11

This homework is split into two parts:

- Part 1 focuses on how the Spring Boot Project 1 can continue to grow.
- Part 2 answers the Spring Boot interview-style questions in my own words.

The Spring Boot project used for Part 1 is my previous project, `New Project 4`, which is published here:
[student-management-system](https://github.com/lopsun7/student-management-system)

Source page: [HW 11 on Notion](https://app.notion.com/p/37cc5511f0608127aa62f9cfd521f70f)

## Part 1. Keep working on Spring Boot Project 1

For Part 1, I continued working from my previous student management Spring Boot app, which I built in `New Project 4`. That project already has controller, service, repository, exception handling, validation, PostgreSQL, and basic AOP logging. For this homework, I used that same project as the base and extended it with a simple `@RequestParam` search endpoint, `@Transactional`, Actuator configuration, and a lightweight `@Async` example.

### 1. `@PathVariable`, `@RequestParam`, and `@RequestBody`

These three annotations are all used to get request data, but they come from different places.

#### `@PathVariable`

`@PathVariable` reads a value directly from the URL path.

Example:

```java
@GetMapping("/{id}")
public ResponseEntity<Student> getStudentById(@PathVariable Long id) {
    return ResponseEntity.ok(studentService.getStudentById(id));
}
```

Example request:

```text
GET /api/v1/students/1
```

Use case:
- fetching one resource by id
- updating one specific resource
- deleting one specific resource

#### `@RequestParam`

`@RequestParam` reads a value from the query string.

Example:

```java
@GetMapping("/search")
public ResponseEntity<List<Student>> searchStudents(
        @RequestParam String course) {
    return ResponseEntity.ok(studentService.findByCourse(course));
}
```

Example request:

```text
GET /api/v1/students/search?course=Java
```

Use case:
- filtering
- sorting
- pagination
- optional search conditions

#### `@RequestBody`

`@RequestBody` reads JSON from the request body and converts it into a Java object.

Example:

```java
@PostMapping
public ResponseEntity<Student> createStudent(@Valid @RequestBody Student student) {
    return new ResponseEntity<>(studentService.createStudent(student), HttpStatus.CREATED);
}
```

Use case:
- create a new object
- update an existing object
- receive structured JSON data

My summary:
- `@PathVariable` = value from URL path
- `@RequestParam` = value from query string
- `@RequestBody` = object from request body JSON

### 2. Exception handler

In Spring Boot, exception handling should be centralized instead of writing try-catch in every controller method.

The common way is to use `@RestControllerAdvice` with `@ExceptionHandler`.

Example:

```java
@RestControllerAdvice
public class GlobalExceptionHandler {

    @ExceptionHandler(ResourceNotFoundException.class)
    public ResponseEntity<Map<String, String>> handleNotFound(ResourceNotFoundException ex) {
        Map<String, String> body = new HashMap<>();
        body.put("message", ex.getMessage());
        return new ResponseEntity<>(body, HttpStatus.NOT_FOUND);
    }
}
```

Benefits:
- cleaner controller code
- consistent error responses
- easier maintenance
- one place to manage business exceptions and validation exceptions

In Project 1, this is useful when:
- student id does not exist
- email already exists
- request payload is invalid

### 3. Validation

Spring Boot validation is usually done with Bean Validation annotations and `@Valid`.

Example entity:

```java
public class Student {

    @NotBlank(message = "First name is required")
    private String firstName;

    @NotBlank(message = "Last name is required")
    private String lastName;

    @Email(message = "Email format is invalid")
    @NotBlank(message = "Email is required")
    private String email;
}
```

Example controller:

```java
@PostMapping
public ResponseEntity<Student> createStudent(@Valid @RequestBody Student student) {
    return new ResponseEntity<>(studentService.createStudent(student), HttpStatus.CREATED);
}
```

Why validation matters:
- stops bad data before it reaches business logic
- improves API reliability
- gives the client clear error messages

### 4. `@Transactional`

The homework prompt says `@Trancational`, but the correct Spring annotation is `@Transactional`.

`@Transactional` tells Spring to run the method inside a database transaction.

Typical usage:

```java
@Transactional
public Student updateStudent(Long id, Student updatedStudent) {
    Student student = studentRepository.findById(id)
            .orElseThrow(() -> new ResourceNotFoundException("Student not found"));

    student.setFirstName(updatedStudent.getFirstName());
    student.setLastName(updatedStudent.getLastName());
    student.setEmail(updatedStudent.getEmail());

    return studentRepository.save(student);
}
```

#### Isolation level

Isolation level controls how one transaction sees data from another transaction.

Common levels:
- `READ_UNCOMMITTED`: allows dirty read, usually not recommended
- `READ_COMMITTED`: prevents dirty read, common default in many systems
- `REPEATABLE_READ`: same row read multiple times stays consistent in one transaction
- `SERIALIZABLE`: strongest consistency, but lowest concurrency

Practical idea:
- stronger isolation means safer data
- stronger isolation also means more locking and lower performance

#### Propagation level

Propagation controls how a method joins or creates transactions.

Common levels:
- `REQUIRED`: join existing transaction, or create a new one if none exists
- `REQUIRES_NEW`: always create a brand-new transaction
- `SUPPORTS`: join if one exists, otherwise run without transaction
- `MANDATORY`: must already have a transaction
- `NOT_SUPPORTED`: suspend transaction and run without one
- `NEVER`: fail if a transaction exists
- `NESTED`: nested transaction with savepoint behavior

Most common practical answer:
- `REQUIRED` is the default and most common
- `REQUIRES_NEW` is useful when a subtask must commit independently

### 5. Actuator

Spring Boot Actuator provides built-in monitoring and management endpoints.

Common endpoints:
- `/actuator/health`
- `/actuator/info`
- `/actuator/metrics`
- `/actuator/env`
- `/actuator/beans`

Why it is useful:
- check whether the app is alive
- inspect metrics
- help with debugging and operations
- support monitoring tools

Example dependency:

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```

My summary:
Actuator is mainly for observability. It helps developers and operations teams understand application health, metrics, and runtime details.

### 6. Bonus: cache, log, async

#### Cache

Cache stores frequently used data so the application does not need to fetch it from the database every time.

Example idea:
- cache the student list
- cache a student by id

Benefits:
- faster response
- reduced database load

#### Log

Logging records what the application is doing.

For example:
- request received
- student created
- exception happened
- how long a method took

In Project 1, I already added a simple AOP logging example on the service layer. That is a good teaching example of how logging can be separated from business logic.

#### Async

Async means a task can run in another thread without blocking the current request thread.

Example use cases:
- send email after registration
- write audit logs
- call a slow external API

Simple example:

```java
@Async
public void sendWelcomeEmail(Student student) {
    // send email in background
}
```

To use async, Spring Boot normally needs `@EnableAsync`.

## Part 2. Spring Boot interview questions

## 1. Why Spring Boot? Pros and cons

Spring Boot helps developers build Spring applications faster with less configuration.

### Pros

- auto-configuration reduces setup work
- embedded server makes it easy to run locally
- starter dependencies simplify dependency management
- good production features like Actuator
- strong ecosystem with Spring MVC, Spring Data, Spring Security, and more

### Cons

- beginners may rely too much on magic and not understand what is happening underneath
- auto-configuration can feel heavy if the application is very small
- startup and memory usage may be larger than lightweight frameworks
- too many abstractions can make debugging harder for new developers

My summary:
Spring Boot is excellent for productivity and enterprise development, but developers still need to understand core Spring concepts instead of depending only on defaults.

## 2. How to start a Spring Boot project from scratch

My normal steps are:

1. Go to Spring Initializr.
2. Choose project type, language, Spring Boot version, group, and artifact.
3. Add dependencies such as Spring Web, Spring Data JPA, Validation, PostgreSQL, Actuator, and Lombok if needed.
4. Download and open the project in IntelliJ IDEA.
5. Configure `application.properties`.
6. Create package structure such as controller, service, repository, entity, exception, and config.
7. Create entity and repository.
8. Create service layer.
9. Create controller layer.
10. Run the app and test with Postman.

## 3. `@Controller` vs `@RestController`

`@Controller` is mainly used in Spring MVC applications that return view names such as HTML pages.

`@RestController` is used for REST APIs and returns JSON directly.

`@RestController` is basically:

```java
@Controller + @ResponseBody
```

Use rule:
- returning page view -> `@Controller`
- returning JSON API -> `@RestController`

## 4. `@PathVariable` vs `@RequestParam`

`@PathVariable` is part of the URL path and usually identifies a specific resource.

Example:

```text
GET /students/5
```

`@RequestParam` is in the query string and is usually used for filters or optional inputs.

Example:

```text
GET /students?course=Java&page=1
```

My summary:
- `@PathVariable` = resource identity
- `@RequestParam` = filtering or extra request options

## 5. `@RequestBody` vs `@ResponseBody`

`@RequestBody` converts request JSON into a Java object.

`@ResponseBody` converts a Java object into the HTTP response body, usually JSON.

Example:

```java
@PostMapping
@ResponseBody
public Student createStudent(@RequestBody Student student) {
    return studentService.createStudent(student);
}
```

In practice, if a class uses `@RestController`, then `@ResponseBody` is already implied for all methods.

## 6. How to use `GetMapping`, `PutMapping`, `PostMapping`, `DeleteMapping`, and `RequestMapping`

These annotations map HTTP requests to controller methods.

- `@GetMapping` for reading data
- `@PostMapping` for creating data
- `@PutMapping` for replacing or updating data
- `@DeleteMapping` for deleting data
- `@RequestMapping` for common base path or for general request mapping

Example:

```java
@RestController
@RequestMapping("/api/v1/students")
public class StudentController {

    @GetMapping
    public List<Student> getAllStudents() { ... }

    @GetMapping("/{id}")
    public Student getStudentById(@PathVariable Long id) { ... }

    @PostMapping
    public Student createStudent(@RequestBody Student student) { ... }

    @PutMapping("/{id}")
    public Student updateStudent(@PathVariable Long id, @RequestBody Student student) { ... }

    @DeleteMapping("/{id}")
    public ResponseEntity<Void> deleteStudent(@PathVariable Long id) { ... }
}
```

## 7. What is Spring Actuator?

Spring Actuator is a Spring Boot module that exposes operational endpoints for health, metrics, and app information.

It is very useful for:
- monitoring
- debugging
- deployment checks
- production support

If someone asks me in an interview, I would say:
Actuator helps me observe the running application without writing those monitoring endpoints manually.

## 8. How to achieve async in a Spring Boot application

The normal steps are:

1. Add `@EnableAsync` on a configuration class or the main application class.
2. Add `@Async` on the method that should run asynchronously.
3. Make sure the async method is called through a Spring bean.

Example:

```java
@SpringBootApplication
@EnableAsync
public class DemoApplication {
}
```

```java
@Service
public class NotificationService {

    @Async
    public void sendEmail() {
        // background work
    }
}
```

Important note:
Like Spring AOP, async also depends on proxy behavior, so self-invocation inside the same class may not trigger it.

## 9. How does Spring handle exception?

Spring can handle exceptions locally or globally.

Common approaches:
- `try-catch` inside code for special cases
- `@ExceptionHandler` inside controller
- `@ControllerAdvice` or `@RestControllerAdvice` for global handling

For REST API projects, global exception handling is cleaner because it gives consistent error format.

## 10. How does Spring validate data?

Spring usually validates data with:
- Bean Validation annotations like `@NotNull`, `@NotBlank`, `@Size`, `@Email`
- `@Valid` in controller method parameters
- global exception handling for validation errors

This is important because it blocks bad input early and keeps service logic cleaner.

## 11. How does Spring do logging?

Spring Boot usually uses SLF4J with Logback by default.

Developers can write logs like:

```java
private static final Logger LOGGER = LoggerFactory.getLogger(StudentServiceImpl.class);
```

```java
LOGGER.info("Creating student: {}", student.getEmail());
```

Common log levels:
- `TRACE`
- `DEBUG`
- `INFO`
- `WARN`
- `ERROR`

Spring can also combine logging with AOP so repeated log behavior can be applied around service methods.

## 12. Cache hit vs cache miss

A cache hit means the requested data is already in cache, so the system returns it quickly.

A cache miss means the data is not in cache, so the system has to load it from the database or another source first.

Simple example:
- first request for student id 1 -> cache miss
- second request for student id 1 -> cache hit

Cache hits improve speed. Too many cache misses reduce the value of having a cache.

## 13. Basic Redis research

Redis is an in-memory data store. It is very fast because it keeps data mainly in memory instead of reading from disk each time.

Common Redis use cases:
- caching
- session storage
- distributed lock
- rate limiting
- pub/sub messaging

Why Redis is popular:
- very fast
- simple key-value model
- supports expiration time
- supports data structures like string, list, set, hash, and sorted set

My basic understanding:
In a Spring Boot project, Redis is often used as a cache layer between the application and the database. If data is found in Redis, the app can avoid hitting the database and respond faster.

## 14. `@RestControllerAdvice` instead of `@ControllerAdvice`

`@ControllerAdvice` is a general global advice component for controllers.

`@RestControllerAdvice` is more convenient for REST APIs because it behaves like:

```java
@ControllerAdvice + @ResponseBody
```

That means exception results are returned directly as JSON.

For API projects, I usually prefer `@RestControllerAdvice` because:
- less boilerplate
- response body is automatic
- better fit for JSON APIs

## Final summary

- Spring Boot improves development speed with auto-configuration and starter dependencies.
- `@PathVariable`, `@RequestParam`, and `@RequestBody` solve different request input problems.
- `@RestControllerAdvice` is a clean way to centralize REST API exception handling.
- Validation and transactions improve correctness and data safety.
- Actuator, logging, cache, and async improve observability and system capability.
- Redis is commonly used in Spring Boot as a fast cache layer.

## Mock Practice 03

### Recording

- Mock Practice 03 video: [Mock Practice 03.mov](https://amzn-s3-shykid7-bucket.s3.us-east-2.amazonaws.com/Mock%20Practice%2003.mov?response-content-disposition=inline&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Security-Token=IQoJb3JpZ2luX2VjED4aCXVzLWVhc3QtMiJGMEQCIAZlZre3jnBQKr9H1AHcZ6uUasrcvaVIBM9XlkzKS4i5AiB%2B%2BvNHyvlOWZ2ePj2%2BKp80S2yYp487Mhx8DYQoZgfDuyq5AwgHEAAaDDQxNzgzNTAxNDE0NSIMciXamNBYSD0K7MEYKpYDi1rSmQzdO%2BLRSaOmvX6RCBmBvhs%2FL9Kn2j0KTNNlud24%2Be11SGVhL9N%2B1ik8lTOqA26K12R2uMA%2FAgFvgvJ0RYkswKc9A3jzFQDTgcek%2Fcq961L4ZFW9iI4LTfgg2FZTGIAO9a51GBpM8Z2CPD5HX%2FIE%2BfOQ4RIY6lKdVxc4ugVB%2FY54IKLs1zzqScaxCTIp6NxTWaI0EZT3z7KZJL6j44iopPrb45mDETz2ujXSc2bcteJbl2KAxpr%2FCZ9GBLsLOOg244W1GE7VHIMK5yrLTAebQGE06eQfeuLKVlKzBsx95BAlC7P20n6b2ruLt%2B9oX%2FzrWYq8XCwEIqtYK0nV8eEJ7sPYF6o6sKr066THSNMh18DUFFUbVPJGCpY42QLe7bd%2FAxRP1HZOuu%2FvsSwrSAeaC7ZwHplNZdHbnlCyinG0PqWkNO6PxYx2%2B32xFIvO3q4ygwpozQBdZGV5PbYThAXT%2BMe5oJaj5jSTD70%2FJUNPBxdVIH3fji1khECMf8DZxASHaOnzQnsQ12GJ1wxaootaucDbGTCt1qzRBjrfAi7N1XcMpyoOuVZdkhCoUUZ3NF2lP%2BIgQ5wQHg8jlYld%2BZ7YibHP6Rx6hNjUqbW4KaC4CF%2BHIGtIAGoTUbdOyah0zGeahn8%2BreJ5u2CFYC2brrd6cZk6jCDRlHsPe6%2FyABgivkekonzIR3TDpRtG42F%2BQ%2BedsEytBHrLdK482%2FDWDFZjjYk8BtygNmhztBns0OmeDUpe3CSZDdb5%2FgpBBRjEyOnD0Bm65ynqICCzUU8wE%2FXHOyr6wYlvTJaHpOFgxmhDx2vvk2Vl1S0KI8SBo2kv8TuuSoLa5IphR44KZ1taGdkA35a447ZJ8%2BhzsL3nF4NkW%2BoMSC1Z%2Bize3MK8cNm2EZeNh0BgpPnFofoeGVPDWz%2B70Gtvli%2FyMcMyLaK9kKfwxcur03ONayNXixjT8mJ13mOWZAyufOvKwXoEnUQwXbNBOL%2B1GGmBrH%2Brnb1RuNvemYXHyLeYX2kh4EXwVA%3D%3D&X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=ASIAWCSHILAATD2TOKXX%2F20260611%2Fus-east-2%2Fs3%2Faws4_request&X-Amz-Date=20260611T214511Z&X-Amz-Expires=43200&X-Amz-SignedHeaders=host&X-Amz-Signature=57c99603d4282e6822e7e3687687fd014fce01bc03e416ce778ec6693ed92920)

### Script 1. New features in Java 17

Java 17 is an LTS version. Some important features are sealed classes, records, pattern matching for `instanceof`, switch improvements, and better JVM performance. In real projects, I mostly care about records for DTOs, pattern matching to reduce boilerplate, and sealed classes to control inheritance.

### Script 2. Where did you use Singleton in your project?

I used Singleton for shared service components, like configuration manager, cache manager, or utility clients. In Spring Boot, most beans are Singleton by default, so one service instance is managed by the Spring IoC container and reused across requests.

### Script 3. How to use Stream to filter people younger than 30?

I would use Java Stream with `filter`. For example, `people.stream().filter(p -> p.getAge() < 30).collect(Collectors.toList())`. The `filter` method takes a `Predicate`, which means one input and returns a boolean.

### Script 4. How do you design RESTful APIs to get/create/update an object?

I usually design APIs around resources. For example, for users: `GET /users/{id}` to get one user, `POST /users` to create, `PUT /users/{id}` to fully update, `PATCH /users/{id}` to partially update, and `DELETE /users/{id}` to delete. I also use proper status codes like `200`, `201`, `400`, `404`, and `500`.

### Script 5. New features in Java 11

Java 11 is also an LTS version. Important features include the new HTTP Client API, local variable syntax for lambda parameters, new String methods like `isBlank`, `lines`, `strip`, and `repeat`, and performance improvements. In projects, String utility methods and the HTTP Client are very practical.

### Script 6. Write REST API for User and TodoItem many-to-many CRUD

I would create two entities: `User` and `TodoItem`, with a many-to-many relationship using a join table like `user_todo_item`. Then I would create repository, service, and controller layers. APIs could be `POST /users`, `POST /todo-items`, `GET /users/{id}/todo-items`, `POST /users/{userId}/todo-items/{todoId}` to assign, and `DELETE /users/{userId}/todo-items/{todoId}` to remove the relationship.

### Script 7. What GC parameters do you know? How to change GC parameters?

GC parameters are JVM arguments used to control memory and garbage collection. Common ones are `-Xms` for initial heap size, `-Xmx` for max heap size, `-XX:+UseG1GC`, `-XX:+UseZGC`, and `-Xlog:gc` for GC logs. We can change them in JVM startup options, Docker command, Kubernetes deployment, or application server configuration.

### Script 8. What is SOLID principle?

SOLID is five object-oriented design principles. Single Responsibility means one class has one main responsibility. Open-Closed means open for extension but closed for modification. Liskov Substitution means child classes should replace parent classes safely. Interface Segregation means small specific interfaces are better. Dependency Inversion means depend on abstraction, not concrete classes.

### Script 9. JVM tuning

For JVM tuning, I first check metrics like heap usage, GC frequency, GC pause time, CPU, and thread count. Then I tune heap size with `-Xms` and `-Xmx`, choose a proper GC like `G1` or `ZGC`, enable GC logs, and analyze memory leaks with tools like VisualVM, JConsole, or heap dump. The goal is to reduce latency and avoid OOM.

### Script 10. Can we use customized class as HashMap key?

Yes, we can use a custom class as a `HashMap` key, but we must override `equals()` and `hashCode()` correctly. Also, the key fields should be immutable, because if the key value changes after insertion, `HashMap` may not find it correctly anymore.

## Note

- The recording link above is a signed S3 URL and may expire after its validity window ends.

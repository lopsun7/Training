# HW10

This homework was drafted in Notion and published here as a GitHub-friendly Markdown page.

## 1. What is AOP?

AOP stands for Aspect-Oriented Programming.

It is a programming idea used to separate cross-cutting concerns from the main business logic.

Cross-cutting concerns are things like:

- logging
- security checks
- transaction management
- performance measurement
- exception handling

Without AOP, the same logging or security code may appear in many classes. With AOP, we can write that common behavior once and apply it where needed.

My own summary: AOP helps keep business code cleaner by moving repeated technical logic into separate reusable pieces.

## 2. What is JoinPoint and Aspect in AOP?

### JoinPoint

A JoinPoint is a point during program execution where extra logic can be applied.

In Spring AOP, a JoinPoint is usually a method execution.

For example:

- before a service method runs
- after a repository method returns
- when a controller method throws an exception

### Aspect

An Aspect is the class that contains the AOP logic.

For example, a logging aspect may define what to do before and after a method call.

My own summary:

- JoinPoint = the place where something can happen
- Aspect = the class that defines that extra behavior

## 3. What are Aspect, JoinPoint, Pointcut, Advice, and Target?

### Aspect

The module or class that contains cross-cutting logic.

Example: a logging class marked with `@Aspect`.

### JoinPoint

The point in execution where AOP logic may run.

In Spring AOP, this is mostly method execution.

### Pointcut

A rule that selects which JoinPoints should be matched.

For example, a pointcut may say:

- all methods inside `service` package
- all methods whose names start with `save`

### Advice

The actual action that runs at a matched point.

Types of advice include:

- `@Before`
- `@After`
- `@AfterReturning`
- `@AfterThrowing`
- `@Around`

### Target

The real object whose method is being intercepted by AOP.

For example, if logging is applied to `OrderService`, then `OrderService` is the target object.

## 4. ApplicationContext vs BeanFactory

Both are IoC container interfaces in Spring, but `ApplicationContext` is more powerful.

### BeanFactory

- basic Spring container
- creates and manages beans
- more lightweight
- mainly provides the core bean management ability

### ApplicationContext

- built on top of `BeanFactory`
- supports bean management plus many extra enterprise features
- supports event publishing
- supports internationalization
- supports annotation-based configuration more naturally
- commonly used in Spring Boot applications

### My own summary

`BeanFactory` is the basic container.  
`ApplicationContext` is the richer container used in most real Spring applications.

## 5. How does Spring MVC working flow work?

The typical Spring MVC flow is:

1. The client sends an HTTP request.
2. The request reaches `DispatcherServlet`.
3. `DispatcherServlet` checks the correct controller mapping.
4. The controller method handles the request.
5. The controller may call the service layer.
6. The service layer may call the repository or DAO layer.
7. Data is returned back through the layers.
8. The controller returns either:
   - a view name, or
   - JSON / response data
9. `DispatcherServlet` sends the final response to the client.

### Important components

- `DispatcherServlet`
- Controller
- Service
- Repository
- ViewResolver (for MVC view pages)

### My own summary

`DispatcherServlet` is the front controller. It receives the request, sends it to the correct controller, and helps return the final response.

## 6. How does `@Autowired` work?

`@Autowired` tells Spring to inject a matching bean automatically.

Spring usually resolves the dependency by type.

For example:

```java
@Service
class OrderService {
    @Autowired
    private PaymentService paymentService;
}
```

When Spring creates `OrderService`, it looks for a bean of type `PaymentService` and injects it.

### Important note

In modern Spring projects, constructor injection is usually preferred over field injection.

Example:

```java
@Service
class OrderService {
    private final PaymentService paymentService;

    public OrderService(PaymentService paymentService) {
        this.paymentService = paymentService;
    }
}
```

## 7. `@Autowired` and `@Qualifier`

The homework prompt says `@Qualifer`, but the correct Spring annotation name is `@Qualifier`.

`@Qualifier` is used when there are multiple beans of the same type and Spring needs help deciding which one to inject.

Example:

```java
interface MessageService {
    String send();
}

@Service("emailService")
class EmailService implements MessageService {
    public String send() {
        return "email";
    }
}

@Service("smsService")
class SmsService implements MessageService {
    public String send() {
        return "sms";
    }
}

@Service
class NotificationService {
    private final MessageService messageService;

    public NotificationService(@Qualifier("emailService") MessageService messageService) {
        this.messageService = messageService;
    }
}
```

### Why `@Qualifier` is needed

Without `@Qualifier`, Spring sees two beans of type `MessageService` and may throw an ambiguity error.

## 8. Two use cases for `@PostConstruct` and `@PreDestroy`

### What is `@PostConstruct`?

`@PostConstruct` marks a method that should run once after the bean has been created and dependencies have been injected.

### Use case 1 for `@PostConstruct`

Initialize cached reference data after dependencies are ready.

Example:

- load configuration from database
- build an in-memory lookup map

### Use case 2 for `@PostConstruct`

Perform startup validation.

Example:

- check whether required API keys or config values exist
- verify that necessary dependent beans are properly initialized

### What is `@PreDestroy`?

`@PreDestroy` marks a method that should run before the bean is destroyed.

### Use case 1 for `@PreDestroy`

Release resources before shutdown.

Example:

- close a custom file writer
- close a manually managed network connection

### Use case 2 for `@PreDestroy`

Flush final state before application shutdown.

Example:

- save buffered data
- write final audit logs

## 9. Small summary

- AOP separates repeated technical logic from business logic
- JoinPoint is the execution point, and Aspect is the class containing AOP logic
- Pointcut chooses where Advice should run
- `ApplicationContext` is a more powerful container than `BeanFactory`
- Spring MVC flow is centered around `DispatcherServlet`
- `@Autowired` injects beans automatically
- `@Qualifier` helps when multiple beans have the same type
- `@PostConstruct` is for initialization after injection
- `@PreDestroy` is for cleanup before shutdown

## 10. Mock practice recording and script

### Recording

- Mock Practice 02 video: [Mock Practice 02.mov](https://amzn-s3-shykid7-bucket.s3.us-east-2.amazonaws.com/Mock%20Practice%2002.mov?response-content-disposition=inline&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECUaCXVzLWVhc3QtMiJHMEUCIQDMhV8B9z6r7G8weAAfQlpmF6AluLEWMaBYs7FJOVLcZQIgAI29V4R%2F1ZFV7dFxGnzFXAKEOOnSJLfjVSp04SPwXqMqwgMI7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw0MTc4MzUwMTQxNDUiDPYoE8u5BghfL7AhdyqWA9Fv8KwtXWD4VimoL5qEPrZKF4acmAU69kJrhFseVpVx%2B0SM1SPlRQSoWj5XZah3CSvEGhYj87OL413JJsSi%2Fd30mPXuwK3oExg%2Fn2F5F%2F3cgUbqi8%2FqYk34MNlGilNSAOkk9eSMl93QWMY%2BROz6D%2BxgcDilUrWV2PiheN6uzzOgansLp1JTee7L5XrJczkNGKWfjCthwj9E7jY32RJCd%2Bk1AVh1oFGdJZAM0AYSWNp9WGAeogHSc%2FfJWiutx8s9Ln2Q%2BuLU8wg4EQpii7ZRcInLZGANtJz5kdVong%2FascLPNaYoSpHP0hWBAWWVQG3JoOKKAAe4N%2BZmAxc7M5MTUPyONLcEXDDQ5j4Q1IyacLS%2Boc2h%2BpP5NxnIt0jGiD7H%2FzhcmaB%2FI7j1hfNc7SSt2TVOMLvXGDODHJd%2BsbBIdbhpu%2FGuHgi1IYKIzg3CuiJFq2Lrrk0a9W86mLv%2FgI6isAk1iEgPhS5CS43AbtsHnlU6yxntx5ACTC%2BqAlO7dY%2FMdwO3h61Ge%2FahPUW3rHqYreecA2yio7wwiaun0QY63gKNRs9MPejk7Jn9ximxmD2Af5eu%2FULR%2FocS7ber9dafoMDblyNzSHnf8P0%2BkeABVdcOi8HX50bKvdP%2BdVN9aB293DuWXIIL5bO8CEqkPKF5RkGLbkUbsUnTcRAdPF87a4Wf8mezLZwLwNJaw2Ecd%2B7jiO%2BFFOB3tuiKjkdm%2FcHd0hMCFE2zeuPaA4kt95ymjfXzvzRiMa5xHUUakgdJluVqYLPrPtUmUIH7bE%2FQDfsaJeI20sX8pc6kBN%2B%2Bnc95MZuUDA1z3G3N0h2i2DaJTmzXaUaBij9copBTN7PenfGjHkz7E40X4nSrmEd8eGsSuCEk1BAG7aNJ0SOQ4hBQ0dnCVtYR4V8bmTsY%2BPSlrpMT3DK%2FnPxNUdzqDDfFUhn5GxuiMcxjV99KEq%2BdPamAWXmE4LnxT75OODFE4AIHiHXS4JIBVJXP5j3TffUxa5RFqDQkPws8AVpdkhdWMNX1ZQ%3D%3D&X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=ASIAWCSHILAA7JCNP4S4%2F20260610%2Fus-east-2%2Fs3%2Faws4_request&X-Amz-Date=20260610T212523Z&X-Amz-Expires=43200&X-Amz-SignedHeaders=host&X-Amz-Signature=95ac0d297b566587bc6cd601c2231c9ae1b6735c2dd6a09d4de798ad9551b8a6)

### Script 1: Recursion vs Iteration

Recursion means a method calls itself to solve smaller subproblems. Iteration means using loops like `for` or `while`. Recursion is cleaner for tree, DFS, and divide-and-conquer problems, but it uses stack memory. Iteration is usually more efficient and avoids stack overflow.

### Script 2: LinkedHashMap vs HashMap

`HashMap` stores key-value pairs but does not guarantee order. `LinkedHashMap` also stores key-value pairs, but it keeps insertion order by using a linked list. If I only need fast lookup, I use `HashMap`. If I need predictable order, I use `LinkedHashMap`.

### Script 3: How to group people by age

I can use Stream API with `Collectors.groupingBy`. For example, I stream the people list and group by `Person::getAge`, then I get a `Map<Integer, List<Person>>`. The key is age, and the value is people with that age.

### Script 4: How can you use Optional

`Optional` is used to handle possible null values safely. Instead of returning `null`, I can return `Optional<User>`. Then I can use `orElse`, `orElseThrow`, or `map`. It helps avoid `NullPointerException` and makes the code cleaner.

### Script 5: How to write REST API in Spring Boot

I create a controller with `@RestController`, define endpoints with `@GetMapping`, `@PostMapping`, `@PutMapping`, and `@DeleteMapping`. The controller receives the request, calls the service layer, and the service calls the repository layer. Finally, it returns data with proper HTTP status code.

### Script 6: How did you debug

First, I reproduce the issue. Then I check logs, HTTP status code, exception message, and request payload. After that, I narrow it down from controller, service, database, cache, or external API. If needed, I use IntelliJ breakpoint to inspect variables step by step.

### Script 7: What is FairLock

FairLock means threads get the lock in the order they requested it. In Java, `new ReentrantLock(true)` creates a fair lock. It prevents starvation, but performance may be lower. The default unfair lock usually has better throughput.

### Script 8: New features in Java 11

Java 11 added useful String methods like `isBlank`, `strip`, `lines`, and `repeat`. It also introduced the standard HTTP Client. Java 11 is also an LTS version, so many companies use it in production.

### Script 9: Design a task management app

I would design `Task` with fields like `id`, `title`, `description`, `status`, `priority`, `assignee`, and `due date`. Then I create CRUD APIs: create task, get task, update task, delete task, and query tasks by status or assignee. Backend uses controller, service, repository. Database can be MySQL or PostgreSQL, with indexes on status, assignee, and due date.

### Script 10: Locking schema: method1 waits for method2

I can use `wait()` and `notifyAll()`. Method1 checks a shared flag. If the flag is false, it waits. Method2 changes the flag to true and calls `notifyAll()`. I would use `while`, not `if`, because after waking up the thread should check the condition again.

## 11. Note

- The recording link above is a signed S3 URL and may expire after its validity window ends.

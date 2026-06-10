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

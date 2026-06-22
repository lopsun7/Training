# Mock 04

This entry records the fourth mock interview practice notes, focused on Singleton, Strategy Pattern, microservices, Hibernate, and SQL questions.

## Video Submission

- Recording date: `2026-06-22`
- Storage: AWS S3
- S3 object path: `s3://amzn-s3-shykid7-bucket/Mocking 04.mov`

## Notes

- The recording path above is an S3 object path, not a signed browser URL.

## Mock Scripts

### 1. What is Singleton design pattern

Singleton is a creational design pattern that ensures a class has exactly one instance throughout the JVM lifecycle and provides one global access point to it.

The main use case is when we want one shared instance, for example:

- configuration manager
- cache manager
- logger wrapper
- connection-related manager

### 2. Why Singleton

We use Singleton when:

- only one shared instance is needed
- object creation is expensive or unnecessary to repeat
- we want centralized access to one shared resource

In Spring, most beans are singleton by default, so service beans are often managed that way by the IoC container.

### 3. Singleton code implementation: eager loading

Eager loading creates the instance when the class is loaded.

```java
public class EagerSingleton {

    private static final EagerSingleton INSTANCE = new EagerSingleton();

    private EagerSingleton() {
    }

    public static EagerSingleton getInstance() {
        return INSTANCE;
    }
}
```

Pros:

- simple
- thread-safe because class loading is thread-safe

Cons:

- object is created even if never used

### 4. Singleton code implementation: lazy loading

Lazy loading creates the instance only when it is first needed.

Classic Java implementation is double-checked locking:

```java
public class LazySingleton {

    private static volatile LazySingleton instance;

    private LazySingleton() {
    }

    public static LazySingleton getInstance() {
        if (instance == null) {
            synchronized (LazySingleton.class) {
                if (instance == null) {
                    instance = new LazySingleton();
                }
            }
        }
        return instance;
    }
}
```

Why check twice:

- the first check avoids locking after the instance already exists
- the second check is for correctness inside the synchronized block

Why `volatile` matters:

- without `volatile`, instruction reordering can happen
- another thread may see a non-null reference before the object is fully initialized
- `volatile` helps safe publication

### 5. What is Strategy Pattern

Strategy Pattern is a behavioral design pattern. It defines a family of algorithms, encapsulates each algorithm into a separate class, and makes them interchangeable at runtime.

The main idea is:

- same interface
- different implementations
- service code depends on the interface instead of concrete classes

### 6. Why Strategy Pattern

We use Strategy Pattern to:

- avoid large `if-else` or `switch` blocks
- keep code cleaner
- extend behavior more easily
- follow the Open-Closed Principle

When a new behavior is needed, we usually add a new strategy class instead of changing the main business logic.

### 7. How I implemented Strategy Pattern in a project

In my project, I used Strategy Pattern in a multi-tenant SaaS system to calculate service fees for different tenant tiers.

Different tenants had different pricing rules.

For example:

- standard tenants used a fixed percentage fee
- enterprise tenants used volume-based discounts or customized contract logic

I created a common interface called `FeeCalculationStrategy`, with:

- `calculate()`
- `supportedTier()`

Then I created different implementations such as:

- `StandardFeeStrategy`
- `EnterpriseFeeStrategy`

In Spring, I marked each strategy as `@Component`, so Spring managed them as beans.

In the service class, I injected a `List<FeeCalculationStrategy>`, and Spring automatically provided all implementations of that interface.

Then I converted that list into a map:

- key = `TenantTier`
- value = matching strategy

When calculating the fee, I got the tenant tier from the order, found the matching strategy, and called `strategy.calculate(order)`.

This design avoided a large `if-else` block. If we add a new tenant tier later, we only need to add a new strategy class, without changing the main service logic.

### 8. Where can we set CORS, backend or frontend or both

CORS is mainly enforced by the browser, and it is mainly configured on the backend response.

Common practical answer:

- backend must allow the frontend origin
- frontend may also need correct request configuration, but the real permission comes from backend headers

So the short answer is:

- mostly backend
- sometimes both sides need matching configuration

### 9. Can you write hint in Hibernate

Yes. Hibernate supports query hints.

Examples:

```java
query.setHint("org.hibernate.fetchSize", 50);
query.setHint("org.hibernate.readOnly", true);
query.setHint("org.hibernate.timeout", 5);
```

With JPA style:

```java
typedQuery.setHint("jakarta.persistence.query.timeout", 5000);
```

Hints are used to influence query behavior such as fetch size, timeout, read-only mode, or caching behavior.

### 10. Monolithic vs microservices

Monolithic architecture means the whole application is built and deployed as one unit.

Microservices architecture means the system is split into multiple smaller services, and each service focuses on one business capability.

My summary:

- monolith is simpler to build and deploy at the beginning
- microservices are better for scaling teams and services independently, but add more operational complexity

### 11. Will you choose stored procedures or Java Hibernate logic

My practical answer is:

- for most normal business logic, I prefer Java service logic plus Hibernate
- for very database-specific heavy operations, sometimes stored procedures can make sense

Why I usually prefer Java + Hibernate:

- better readability for Java teams
- easier version control and testing in application code
- easier to keep business logic in one place

When stored procedures may be useful:

- very heavy batch SQL logic
- performance-critical database-side processing
- legacy systems with strong DB-centered architecture

### 12. Person table: give me one record with the oldest person

If table is `person(name, age)`, the SQL can be:

```sql
SELECT name, age
FROM person
WHERE age = (SELECT MAX(age) FROM person);
```

If multiple people share the oldest age, this returns all of them.

### 13. SQL coding: given order table and customer table, find largest price in 10 years and return price plus customer name

Example assumption:

- `orders(order_id, customer_id, price, order_date)`
- `customer(customer_id, customer_name)`

SQL:

```sql
SELECT c.customer_name, o.price
FROM orders o
JOIN customer c
  ON o.customer_id = c.customer_id
WHERE o.order_date >= CURRENT_DATE - INTERVAL '10 years'
  AND o.price = (
    SELECT MAX(price)
    FROM orders
    WHERE order_date >= CURRENT_DATE - INTERVAL '10 years'
  );
```

### 14. What annotations and configurations did you use in Eureka in Spring Boot

Basic Eureka setup usually includes:

- `@EnableEurekaServer` on the Eureka server
- `@EnableDiscoveryClient` on service side in older style examples

Common configuration:

```properties
spring.application.name=user-service
eureka.client.service-url.defaultZone=http://localhost:8761/eureka
eureka.instance.prefer-ip-address=true
```

If the service is only a client, it registers itself with Eureka Server and discovers other services by name.

### 15. What was your responsibility in microservices

A good answer is:

- design and implement one or more business services
- create REST APIs
- integrate database and messaging
- handle service-to-service communication
- add logging, exception handling, validation, and monitoring
- support deployment, debugging, and performance troubleshooting

My summary:
In microservices, responsibility is usually focused on a bounded business domain plus the reliability of that service in production.

### 16. If microservices call each other A -> B -> C and some return 500 errors, what should we do

Common things to do:

1. trace the request path with logs and trace IDs
2. identify which service is the real source of failure
3. check timeout, retry, and fallback strategy
4. return clear error mapping instead of leaking internal details
5. add circuit breaker or degrade gracefully if needed
6. monitor metrics and alerts

My summary:
Do not only fix the surface 500 error in service A. We need tracing, root cause analysis, and resilience patterns.

### 17. How do you secure communication in microservices

Common approaches:

- HTTPS / TLS between services
- mTLS for service-to-service trust
- token-based authentication such as JWT or OAuth2
- API gateway
- service mesh in some environments
- least-privilege access
- secrets management

### 18. When to use message queue between services

Message queue is useful when:

- services should be loosely coupled
- async processing is acceptable
- retry is needed
- traffic bursts need buffering
- one request should trigger multiple downstream actions

Examples:

- order created -> send email later
- payment success -> update analytics and inventory asynchronously

### 19. SQL: join, group by, and count

Example:

```sql
SELECT d.name AS department_name, COUNT(e.emp_id) AS employee_count
FROM dept d
LEFT JOIN emp e
  ON d.dept_id = e.dept_id
GROUP BY d.dept_id, d.name;
```

This means:

- `JOIN` combines department and employee tables
- `GROUP BY` groups rows by department
- `COUNT` counts employees in each department

### 20. SQL coding: emp table and dept table

If `emp(dept_id fk)` and `dept` table are given, a common query is to list employee names with department names:

```sql
SELECT e.emp_name, d.dept_name
FROM emp e
JOIN dept d
  ON e.dept_id = d.dept_id;
```

If the question is about employee count by department:

```sql
SELECT d.dept_name, COUNT(e.emp_id) AS employee_count
FROM dept d
LEFT JOIN emp e
  ON d.dept_id = e.dept_id
GROUP BY d.dept_id, d.dept_name;
```

## Final summary

- Singleton controls one shared instance, and common implementations are eager loading and lazy loading with double-checked locking.
- Strategy Pattern uses one interface with multiple interchangeable implementations and is great for avoiding large condition blocks.
- CORS is mainly configured on the backend, though both sides may need coordinated setup.
- Hibernate hints can tune query behavior such as fetch size, read-only mode, or timeout.
- In distributed systems, we should think about service tracing, retries, fallbacks, secure communication, and when async messaging is better than direct calls.
- For SQL questions, a clean answer usually includes both the query and the reason behind the join, group, aggregate, or filter choice.

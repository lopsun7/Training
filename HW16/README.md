# HW16

This homework summarizes selected Spring Cloud and microservices videos from the requested playlist.

Playlist:
[Spring Cloud Playlist](https://www.youtube.com/playlist?list=PLVz2XdJiJQxz3L2Onpxbel6r72IDdWrJh)

Source videos requested:

1. [Spring Cloud Eureka | Java Techie](https://www.youtube.com/watch?v=irBEdp7XlSQ)
2. [Spring cloud config server using GitHub repository](https://www.youtube.com/watch?v=x1BR0D-buQg)
3. [Client side Load Balancer using Spring Cloud Ribbon](https://www.youtube.com/watch?v=ueyVjOnDHYQ)
6. [Distributed log tracing using Spring Cloud Sleuth & Zipkin | PART-7](https://www.youtube.com/watch?v=M19XC0zJUrA)
9. [MicroService Architecture | Java Techie](https://www.youtube.com/watch?v=hL452TMF20o)
14. [Spring Cloud Hystrix Circuit Breaker with spring boot | Java Techie](https://www.youtube.com/watch?v=2x9G8daUM8A)
25. [Microservice | Resilience4J Circuit Breaker Implementation on Spring Boot | JavaTechie](https://www.youtube.com/watch?v=b6R4dElDtRc)

## 1. Eureka

### Main idea

Eureka is a service discovery tool in the Spring Cloud Netflix ecosystem.

### Key points

- microservices register themselves to Eureka Server
- other services discover them by service name
- avoids hardcoding IP and port
- useful when service instances scale up or down dynamically

### Practical note

In a microservice system, service discovery makes service-to-service communication more flexible. Instead of calling a fixed host, a service can ask Eureka for the available instances of another service.

## 2. Config Server

### Main idea

Spring Cloud Config Server centralizes configuration for multiple microservices.

### Key points

- configuration can be stored in Git
- different services can read config from one central place
- supports environment-specific config such as `dev`, `test`, and `prod`
- avoids duplicating config files across many services

### Practical note

This is useful for properties like:

- database URL
- service ports
- API keys
- logging level

In a real microservice environment, centralized config makes changes easier to manage.

## 3. Ribbon

### Main idea

Ribbon is a client-side load balancer from the older Spring Cloud Netflix stack.

### Key points

- client chooses one instance from multiple service instances
- can work with Eureka-discovered services
- load balancing happens on the client side

### Practical note

Ribbon was widely used in older Spring Cloud systems, but today it is considered legacy and newer projects often use Spring Cloud LoadBalancer instead.

## 4. Sleuth and Zipkin

### Main idea

Sleuth and Zipkin are used for distributed tracing.

### Key points

- Sleuth adds trace IDs and span IDs to requests
- Zipkin collects and visualizes traces
- helps follow one request across multiple microservices

### Why it matters

If one request goes through:

- API Gateway
- Order Service
- Payment Service
- Notification Service

then distributed tracing helps us understand where time was spent and where failures happened.

### Practical note

This is very important in debugging, performance analysis, and production monitoring.

## 5. Microservice Architecture

### Main idea

Microservice architecture splits one large system into multiple smaller services, where each service owns one business capability.

### Key points

- each service can be developed and deployed independently
- each service can have its own database
- services communicate over HTTP, messaging, or other protocols

### Benefits

- better scalability
- better team ownership
- independent deployment

### Challenges

- more operational complexity
- service discovery
- centralized config
- tracing
- fault tolerance
- monitoring

### My summary

Microservices are powerful, but they need more infrastructure than a monolith.

## 6. Hystrix

### Main idea

Hystrix is a circuit breaker library from the Netflix stack.

### Key points

- prevents cascading failures
- opens the circuit when failures cross a threshold
- supports fallback logic
- protects the system from repeated calls to an unhealthy downstream service

### Example

If Service A calls Service B, and Service B keeps failing or timing out, Hystrix can stop repeated calls for a while and return fallback data instead.

### Practical note

Hystrix was very popular in older Spring Cloud Netflix systems, but it is now in maintenance mode and newer systems usually prefer Resilience4J.

## 7. Resilience4J Circuit Breaker

### Main idea

Resilience4J is a modern fault-tolerance library commonly used in Spring Boot microservices.

### Key points

- supports circuit breaker
- retry
- rate limiter
- bulkhead
- timeout handling

### Circuit breaker concept

If a downstream service fails repeatedly:

- circuit becomes open
- calls are blocked temporarily
- fallback or error response is returned

Later:

- circuit can move to half-open
- system tests whether the downstream service recovered

### Practical note

Compared with Hystrix, Resilience4J is the more modern choice in recent Spring ecosystems.

## Overall notes

These videos together show the classic supporting pieces around microservices:

- Eureka for service discovery
- Config Server for centralized configuration
- Ribbon for client-side load balancing in older systems
- Sleuth and Zipkin for distributed tracing
- Hystrix and Resilience4J for fault tolerance
- Microservice architecture as the bigger design model

## What I should remember

1. Eureka helps services find each other dynamically.
2. Config Server keeps configuration centralized.
3. Ribbon is older client-side load balancing technology.
4. Sleuth and Zipkin help trace requests across multiple services.
5. Hystrix and Resilience4J are for circuit breaker and resilience.
6. Microservices give flexibility, but also require more operational support.

## Modern context note

Some tools in the older Netflix stack are now less common in new projects:

- Ribbon is largely replaced by Spring Cloud LoadBalancer
- Hystrix is in maintenance mode

But these videos are still useful because they explain core microservice concepts very clearly.

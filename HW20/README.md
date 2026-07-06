# Homework 20 - Testing, Security, and Code Quality

## Project

For this homework, I used my Spring Boot Student Management System project.

- Application repository: `git@github.com:lopsun7/student-management-system.git`
- Main project folder: `/Users/lopsun/Documents/New project 4`
- Testing command: `./mvnw clean verify`
- Local verification result: 41 tests passed
- JaCoCo instruction coverage: 96.51%

## Implementation Summary

I added JUnit 5 unit tests and integration tests to the Spring Boot project. The tests mirror the same package structure as the source code directory, including controller, service implementation, repository, client, config, exception, and model tests.

I also integrated JaCoCo into Maven so the build generates coverage reports and fails if coverage drops below 90%. I updated the Jenkins pipeline so CI/CD runs tests, publishes the JaCoCo report, sends coverage data to SonarQube, waits for the SonarQube quality gate, and only then deploys to EC2.

## Test Structure

```text
src/test/java/com/studentmanagement
|-- client
|   `-- DownstreamAggregationClientTest.java
|-- config
|   |-- AsyncConfigTest.java
|   `-- DownstreamPropertiesTest.java
|-- controller
|   |-- DownstreamAggregationControllerTest.java
|   `-- StudentControllerTest.java
|-- exception
|   `-- GlobalExceptionHandlerTest.java
|-- model
|   |-- FailedDownstreamRequestTest.java
|   `-- StudentTest.java
|-- repository
|   |-- FailedDownstreamRequestRepositoryIntegrationTest.java
|   `-- StudentRepositoryIntegrationTest.java
|-- service
|   `-- DownstreamAggregationServiceTest.java
|-- serviceimpl
|   |-- DownstreamRecoveryServiceImplTest.java
|   `-- StudentServiceImplTest.java
`-- StudentManagementSystemApplicationTests.java
```

## Question Answers

### 1. Write unit test and integration test for your project

I wrote unit tests for the service layer using JUnit 5 and Mockito. For example, `StudentServiceImplTest` mocks the repository and async service, then verifies create, update, delete, search, and not-found behavior.

I wrote integration tests for the repository and controller layers. The repository integration tests use H2 with the real SQL schema, and the controller tests use MockMvc to call the REST endpoints like a real HTTP client.

### 2. TDD vs BDD vs DDD

TDD means Test-Driven Development. I write a failing test first, then write the minimum code to pass it, then refactor.

BDD means Behavior-Driven Development. It describes behavior in business language, usually with Given-When-Then style scenarios.

DDD means Domain-Driven Design. It focuses on modeling the core business domain with clear concepts like entities, value objects, repositories, services, and bounded contexts.

### 3. What is JUnit?

JUnit is the main Java testing framework I use to write and run automated tests. In this project I use JUnit 5 through Spring Boot Starter Test.

### 4. What is Mockito?

Mockito is a mocking framework. I use it when I want to test one class in isolation without calling the real database, real HTTP client, or real async worker.

### 5. How do you test your application?

I test the controller layer with MockMvc, the service layer with JUnit and Mockito, the repository layer with H2 integration tests, and the downstream client with a local test HTTP server. Then I run `./mvnw clean verify` so Maven runs all tests and checks coverage.

### 6. What tools do you use to do code quality analysis?

I use JUnit 5 and Mockito for correctness, JaCoCo for test coverage, SonarQube for code smells, bugs, vulnerabilities, duplicated code, and quality gate checks, and Jenkins for CI/CD automation.

### 7. Authentication vs Authorization

Authentication answers "Who are you?" For example, a user logs in with a username and password or a token.

Authorization answers "What are you allowed to do?" For example, a normal user can read their own profile, but only an admin can delete users.

### 8. Encryption vs Hashing vs Encoding

Encryption protects data by making it unreadable unless someone has the key to decrypt it. It is reversible with the correct key.

Hashing turns data into a fixed-length value and is one-way. I would use hashing for passwords, usually with salt and a strong algorithm like bcrypt.

Encoding changes data into another format for transport or storage, like Base64. Encoding is not security because it is easily reversible.

### 9. How do you secure your application?

I would secure the application by using HTTPS, validating all input, using Spring Security, storing passwords with salted hashing, using JWT or OAuth2 for token-based access, keeping secrets in environment variables or AWS Secrets Manager, limiting database permissions, adding role-based authorization, and scanning code through SonarQube.

### 10. What is JWT?

JWT means JSON Web Token. It is a signed token that can carry user identity and claims. A backend can verify the signature and use the claims to decide who the user is and what they can access.

### 11. What is OAuth2?

OAuth2 is an authorization framework that lets an application access resources on behalf of a user without directly handling the user's password. A common example is logging in with Google and receiving an access token.

## CI/CD Pipeline Integration with SonarQube

I updated the Jenkins pipeline to include these stages:

1. Checkout source code from GitHub.
2. Run `./mvnw clean verify`.
3. Publish JUnit test results.
4. Publish the JaCoCo HTML coverage report.
5. Run SonarQube analysis with the Maven Sonar scanner.
6. Wait for the SonarQube quality gate.
7. Deploy to EC2 only if the build and quality gate pass.

The pipeline is triggered by a GitHub webhook when code is pushed to the target branch, such as `dev`.

## JaCoCo Integration

I added the `jacoco-maven-plugin` to `pom.xml`.

The plugin does three things:

1. Attaches the JaCoCo agent before tests run.
2. Generates the HTML and XML coverage reports during `verify`.
3. Fails the build if instruction coverage is below 90%.

Report files:

```text
target/site/jacoco/index.html
target/site/jacoco/jacoco.xml
```

## SonarQube Integration

I added SonarQube Maven properties so SonarQube can read the JaCoCo XML report:

```text
sonar.coverage.jacoco.xmlReportPaths=target/site/jacoco/jacoco.xml
```

In Jenkins, the SonarQube stage runs:

```bash
./mvnw org.sonarsource.scanner.maven:sonar-maven-plugin:sonar
```

The quality gate is checked before deployment, so bad code quality can stop the pipeline.

## Coverage Result

The latest local verification passed:

```text
Tests run: 41, Failures: 0, Errors: 0, Skipped: 0
Instruction coverage: 96.51% (1189/1232 covered)
```

This satisfies the requirement that unit test coverage must reach 90% or above.

## Interview-Style Summary

In my project, I added both unit tests and integration tests. Unit tests use JUnit 5 and Mockito to test service logic in isolation. Integration tests use Spring Boot, MockMvc, H2, and a local HTTP test server to verify real application behavior across layers.

For code quality, I integrated JaCoCo and SonarQube into the Maven and Jenkins pipeline. JaCoCo enforces a 90% coverage gate, and SonarQube checks quality issues before deployment. My current result is 41 passing tests with 96.51% instruction coverage.

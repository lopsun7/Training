# HW07

This homework was drafted in Notion and published here as a GitHub-friendly Markdown page.

Source page: [HW 07 on Notion](https://app.notion.com/p/376c5511f06081c99dbdc240f89f24c3)

## Homework 7: Spring Boot Student Management Project

### Assignment

Build a Student Management project with Spring Boot by following the linked employee management tutorial and adapting the domain from employee management to student management.

- Tutorial video: [Employee Management System tutorial](https://www.youtube.com/watch?v=v1IFQWzuSrw)

### Submission Repository

- Project repo: [student-management-system](https://github.com/lopsun7/student-management-system)

### What Was Built

- Spring Boot REST API for student management
- CRUD endpoints for create, read, update, and delete operations
- PostgreSQL configuration for application runtime
- H2-backed automated tests for local verification
- Validation and error handling for invalid requests and missing resources

### Tech Stack

- Java 21
- Spring Boot 3.5
- Spring Web
- Spring Data JPA
- PostgreSQL
- Maven

### API Endpoints

Base URL: `http://localhost:8080/api/v1/students`

- `GET /api/v1/students`
- `POST /api/v1/students`
- `GET /api/v1/students/{id}`
- `PUT /api/v1/students/{id}`
- `DELETE /api/v1/students/{id}`

### Verification

```bash
./mvnw test
```

All tests passed before publishing.

# HW06

This homework was drafted in Notion and published here as a GitHub-friendly Markdown page.

Source page: [HW 06 on Notion](https://app.notion.com/p/375c5511f06081cbb70bf6e9f968e5f5)

## Recording

- Video link: [HW06.mov](https://amzn-s3-shykid7-bucket.s3.us-east-2.amazonaws.com/HW06.mov?response-content-disposition=inline&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLWVhc3QtMiJIMEYCIQCYKnKwOYuv0foXZER3FAZRF07SmVB0ckEEyFLJkAacEQIhANMMvnRHMFejAY4YG8ez%2BspNEISYTfDECYA9ecBxNhOdKrkDCF0QABoMNDE3ODM1MDE0MTQ1Igz13ieL5JjVA9Uf5OoqlgN0epipSeN5Mc7yXaGb9ek0gKrFZn%2FmgRr3cfp%2BO7MWNeKPC1YI4xuxbEZ27CpUsyNzbPDEpNt0Wnx%2FxXLAkU3bLnpVfJ4%2FmMMrBORRTg4ClSfKkVaj%2BSmS%2Fny9bU71%2BIZyGYkOXrm1L4sL2j0EZgaZMXeErd4hSpG%2BkTvWsxemQoz9jgzdR9KRWpYT8L%2B57wzp8z1PXHmVtJK7zc2Gs5kew7iFsrQz4s8uSStsGZH9QCG2pE1nNvBFb2pMe5mK%2FxOdw39%2BakbsVTW3D6vNQZ7LHivWnvirGIynHYMC%2FVoJSLY%2FeNEIz1RfGnFaM5NgnAqrxYnW48pyKC22ZsdlbDuRXZ9sasAlH3x4fam%2BSjXFKb8CAm4T4UreooNs58U38OWJ5XTbqJYd%2B9GBtd2rThBTStPYb1xveBJIEk3ygsvKOEyh29aLFH481b%2FozoJHNbugW7ncaHVuDVFsQ%2F3ENiSvl5MJpbPo7Xv9EAlR7Wo1%2BLfaeDti%2FQ01wOY9g%2FqVaHALrrbIViyq%2BZGQweJTqWN04PV7tVA%2BMJe7h9EGOt0CUlahqtdVN3ogFSrXd1mCpT4YqKd9FW%2FDnftMUHmhpW4NkAiLJAocujMGTSXZh4KIxXSw6qyxnUy5vaodd%2FR9KU9%2Bop706%2FFydzCAjoTjAVef3Rgmu65rs67sO5vleYRtPKxxlw0ouC1cL7%2BDxNVPEhy8KIUy96OJteGM6ve%2FbP0eJvZNRnz0QBL%2BSoU799bJZ16FT7VTCka1hNm%2B2Nmu60C6ZXZYG%2F4KveSpw72FvwEEXNkcK%2FDlWeHAIPQ9%2F14%2BpIOi70fG9DO%2F7aSuX8Vh4VrX2z8nYyJ5735EDkYMlsF4z5ze%2FQBYVOQCNdZtrBzfAoYr0vabzwfiZzFWXYAOfhkhx594PKIKlcDy8gbpIxKVBTRGEsv%2FHaIdW4p5XR4yN37DQoFMLWo3q9IlVHHj%2BI5HrksuHvIQZWfb30e8VKaOtVj53bq76J47yEE3NwYqW%2F3XEQCqiaz5BwBUmA%3D%3D&X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=ASIAWCSHILAA2ZGHXO5Q%2F20260604%2Fus-east-2%2Fs3%2Faws4_request&X-Amz-Date=20260604T202121Z&X-Amz-Expires=43200&X-Amz-SignedHeaders=host&X-Amz-Signature=be54aa485b936d3386401ec5b5f64664ed49deeca54bc100b52b4e12996ad2f5)

## Homework 6: System Design Concepts Oral Script

## Opening

Today I will explain several important software engineering concepts in my own words. These concepts are commonly used in modern web applications, backend systems, cloud deployment, and system design.

## 1. Client-server model

The client-server model is a common architecture where the client sends requests and the server processes those requests and sends back responses.

For example, when I open a website, my browser is the client. It sends a request to the server, and the server returns the web page data.

The main idea is that the client focuses on user interaction, while the server focuses on business logic, data processing, and storage.

## 2. Application service

An application service is a part of the backend that handles the main business logic of an application.

For example, in an online shopping system, an order service may handle creating orders, checking inventory, and calculating prices.

It usually sits between the controller or API layer and the database layer. Its job is to organize the workflow and make sure the business rules are applied correctly.

## 3. HTTP request and response

HTTP request and response is the basic way a client and server communicate on the web.

The client sends an HTTP request to ask for something, such as getting user information or submitting a form. The request usually includes a method like `GET`, `POST`, `PUT`, or `DELETE`, a URL, headers, and sometimes a body.

The server then returns an HTTP response. The response includes a status code, headers, and usually some data, often in JSON format.

For example, status code `200` means success, `404` means not found, and `500` means server error.

## 4. Horizontal scaling vs vertical scaling

Vertical scaling means making one machine stronger. For example, we add more CPU, memory, or disk to one server.

Horizontal scaling means adding more machines. For example, instead of using one powerful server, we use multiple servers to handle traffic together.

Vertical scaling is simpler, but it has a hardware limit. Horizontal scaling is more flexible and is commonly used in modern cloud systems.

## 5. Load balancer

A load balancer distributes incoming traffic to multiple servers.

For example, if many users visit a website at the same time, the load balancer decides which server should handle each request.

The benefit is that it improves performance, avoids overloading one server, and increases availability. If one server goes down, the load balancer can send traffic to other healthy servers.

## 6. Microservice

A microservice is an architecture style where a large application is divided into small, independent services.

Each service focuses on one business function. For example, an e-commerce app may have user service, order service, payment service, and inventory service.

Each microservice can be developed, deployed, and scaled independently. This makes the system more flexible, but it also adds complexity, such as service communication, monitoring, and data consistency.

## 7. Microfrontend

Microfrontend is similar to microservices, but it is used on the frontend side.

It means splitting a large frontend application into smaller independent frontend modules.

For example, in an e-commerce website, the product page, shopping cart, and user profile could be developed by different teams and deployed separately.

The benefit is that frontend teams can work independently, but it also requires good coordination to keep the user experience consistent.

## 8. Database: relational database and nonrelational database

A relational database, or SQL database, stores data in tables with rows and columns. It usually has a fixed schema and supports SQL queries.

Examples include MySQL, PostgreSQL, and Oracle.

It is good for structured data and transactions, such as banking systems, order systems, and user accounts.

A nonrelational database, or NoSQL database, stores data in more flexible formats, such as documents, key-value pairs, graphs, or wide columns.

Examples include MongoDB, Redis, Cassandra, and DynamoDB.

NoSQL databases are good for flexible data models, large-scale data, and high-performance use cases.

## 9. API gateway

An API gateway is an entry point between clients and backend services.

Instead of the client calling many microservices directly, the client sends requests to the API gateway, and the gateway routes the requests to the correct service.

It can also handle common tasks such as authentication, rate limiting, logging, request routing, and response aggregation.

In microservice systems, an API gateway makes the backend easier to manage and protects internal services from direct exposure.

## 10. Message queue

A message queue is used for asynchronous communication between services.

Instead of one service calling another service directly and waiting for the result, it sends a message to a queue. Another service can process the message later.

For example, after a user places an order, the order service can send a message to a queue, and the email service can later send a confirmation email.

Message queues help improve scalability, reliability, and system decoupling.

Common examples are Kafka, RabbitMQ, and AWS SQS.

## 11. Log and monitor

Logs are records of what happens inside an application.

For example, logs can show when a user logs in, when an error happens, or how long a request takes.

Monitoring means continuously checking the health and performance of the system.

For example, we can monitor CPU usage, memory usage, error rate, request latency, and service availability.

Logs help us debug problems, while monitoring helps us detect problems early.

## 12. Deployment with AWS, Azure, or GCP

Deployment means putting an application into a real environment so users can access it.

Cloud platforms like AWS, Azure, and GCP provide servers, databases, storage, networking, and many managed services.

For example, we can deploy a backend service on AWS EC2, Elastic Beanstalk, ECS, or Kubernetes. We can store files in S3 and use RDS for databases.

The benefit of cloud deployment is that it is scalable, reliable, and easier to manage compared with maintaining physical servers ourselves.

## 13. Security: authentication and authorization

Authentication means verifying who the user is.

For example, when a user logs in with an email and password, the system checks the user's identity.

Authorization means checking what the user is allowed to do.

For example, a normal user may view their own profile, but only an admin can delete users or manage system settings.

So authentication is about identity, and authorization is about permission.

## 14. Why testing?

Testing is important because it helps us make sure the software works correctly before users use it.

It can catch bugs early, reduce production issues, and make the code easier to maintain.

For example, unit tests check small pieces of code, integration tests check how different components work together, and end-to-end tests check the full user workflow.

Testing is also useful when we change existing code. If tests still pass, we have more confidence that we did not break old features.

So testing improves quality, reliability, and developer confidence.

## Ending

Overall, these concepts help developers build applications that are scalable, reliable, secure, and maintainable.

## Notes

- The recording link above is an S3 signed URL and may expire after its validity window ends.

# HW17

This homework records the question list for Docker, AWS services, system design, and Spring Boot deployment on AWS.

## Docker

### Core concepts

1. What is Docker?
2. What is a Dockerfile?
3. What is a Docker image?
4. What is a Docker container?

### Comparison

1. Docker vs VM

### Real project usage

1. Talk about how to use Docker in a real project

## AWS Services

Check the following AWS services and explain how to use them in a project:

1. EC2
2. ECS
3. ECR
4. RDS
5. DocumentDB
6. DynamoDB
7. Lambda Function
8. API Gateway
9. AWS Kinesis
10. IAM
11. SNS
12. SQS

## System Design

1. Design an online shopping system
2. Only use AWS services
3. Draw the architecture

## Deployment

1. Deploy the Spring Boot project on AWS
2. Use EC2 or ECS

## ECS Deployment Progress Update

This section records the real deployment progress for my Spring Boot project using AWS ECS.

### Target project

- local project: `New project 4`
- deployment style: `Docker + ECR + ECS`
- AWS region: `us-east-2`

### What I completed

1. Installed and configured AWS CLI with SSO login
2. Confirmed AWS account access worked with `aws sts get-caller-identity`
3. Added Docker support for the Spring Boot project
4. Built the application image locally
5. Created an ECR repository named `student-management-system`
6. Pushed the Docker image to ECR
7. Created an ECS cluster named `student-management-cluster`
8. Prepared ECS task definition and deployment scripts locally

### Current status

The image is already in ECR and the ECS cluster already exists, but the application is not fully running on ECS yet.

I did not create the final running ECS service because I wanted to avoid paid resources.

### Why I stopped here

To run the Spring Boot project fully on ECS in a normal way, I would still need resources such as:

- ECS Fargate task or service
- networking configuration
- database service such as RDS if I want PostgreSQL in AWS
- possibly a load balancer for public access

These resources can create charges, so I paused before turning them on.

### Billing check

On June 26, 2026, I checked the AWS Billing page and the estimated grand total was `USD 0.00`.

I also checked the main compute-related resources in `us-east-2` and found:

- no running ECS services
- no running ECS tasks
- no EC2 instances
- no RDS instances
- no load balancer
- no NAT gateway

### My summary

This means the ECS deployment preparation is mostly done:

- Docker image built
- image pushed to ECR
- ECS cluster created
- billing still `USD 0.00`

The remaining work is to create and run the ECS service when I am ready to allow chargeable AWS resources.

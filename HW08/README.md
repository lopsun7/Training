# HW08

This homework was drafted in Notion and published here as a GitHub-friendly Markdown page.

Source page: [HW 08 on Notion](https://app.notion.com/p/379c5511f06081c8a12ce2b736fa219e)

## 1. TCP 3-way handshaking

TCP uses a three-step handshake to build a reliable connection before real data transfer starts.

### Step 1: SYN

The client sends a `SYN` packet to the server. This means, "I want to start a connection, and here is my initial sequence number."

### Step 2: SYN-ACK

The server replies with `SYN-ACK`. This means, "I received your request, I also want to connect, and here is my sequence number."

### Step 3: ACK

The client sends back `ACK`. This means, "I received your reply, so the connection is now confirmed."

After these three steps, both sides know the other side is reachable and ready to communicate.

## 2. TCP vs UDP

### TCP

TCP is connection-oriented. It builds a connection first, guarantees delivery, preserves packet order, and does error checking and retransmission. It is more reliable, but usually slower than UDP.

Typical uses:

- web pages
- login systems
- file transfer
- database communication

### UDP

UDP is connectionless. It does not build a connection first, does not guarantee delivery, and does not guarantee ordering. It is simpler and faster, so it is useful when speed matters more than perfect reliability.

Typical uses:

- video streaming
- online gaming
- voice calls
- DNS queries

### My own summary

TCP is like registered mail: slower but safer.  
UDP is like shouting a message quickly: faster, but some parts may be lost.

## 3. What is Tomcat?

Tomcat is a Java web server and servlet container. It is commonly used to run Java web applications.

In simple words, Tomcat receives HTTP requests, passes them to Java web components such as servlets, and sends HTTP responses back to the client.

## 4. Basic components of Tomcat

### Server

The top-level container that represents the whole Tomcat instance.

### Service

A group that connects one or more connectors with one engine.

### Connector

The part that listens for client requests, such as HTTP requests on a port like `8080`. It converts network traffic into request objects Tomcat can process.

### Engine

The main request-processing container inside a service. It decides which virtual host should handle the request.

### Host

Represents a virtual host, such as a domain name. One Tomcat server can contain multiple hosts.

### Context

Represents one web application. For example, one deployed application inside Tomcat is usually one context.

### Servlet container

This is the part that manages servlets, their lifecycle, and request dispatching.

## 5. What is a web server?

A web server is software that receives HTTP requests from clients and returns web content or backend responses.

It may serve:

- static content, like HTML, CSS, JavaScript, and images
- dynamic content, often by forwarding to application logic

Examples include Nginx, Apache HTTP Server, and Tomcat.

## 6. What is 3-tier architecture?

3-tier architecture means splitting an application into three layers.

### Presentation layer

This is the user-facing part, such as browser pages, mobile screens, or frontend UI. Its job is to display information and collect user input.

### Business logic layer

This is the backend logic layer. It handles rules, validation, calculations, workflows, and application behavior.

### Data layer

This layer stores and retrieves data. It usually includes the database and data access logic.

### My own summary

3-tier architecture separates responsibilities, so the system is easier to develop, test, maintain, and scale.

## 7. What is the OSI Model?

The OSI Model is a conceptual model that explains how network communication works in seven layers. Each layer has a specific job.

## 8. What does each OSI layer do?

### Layer 7: Application

This is the layer closest to the user. It provides network services to applications. Examples include HTTP, SMTP, and FTP.

### Layer 6: Presentation

This layer handles data formatting, translation, encryption, and compression. It helps make sure both sides understand the data format.

### Layer 5: Session

This layer manages sessions or conversations between systems. It helps open, maintain, and close communication sessions.

### Layer 4: Transport

This layer provides end-to-end communication. It handles segmentation, reliability, flow control, and error recovery. TCP and UDP belong here.

### Layer 3: Network

This layer handles logical addressing and routing. IP works here. Its job is to decide how data travels across different networks.

### Layer 2: Data Link

This layer handles node-to-node delivery inside the same local network. It uses MAC addresses and helps detect local transmission errors.

### Layer 1: Physical

This layer is the actual hardware transmission layer. It sends raw bits through cables, radio signals, switches, and network interfaces.

## 9. Small summary in my own words

TCP and UDP are transport choices for network communication.  
Tomcat is a Java server that helps run web applications.  
3-tier architecture separates UI, business logic, and data storage.  
The OSI model helps us understand networking from user applications all the way down to physical hardware.

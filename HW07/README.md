# HW07

This homework was drafted in Notion and published here as a GitHub-friendly Markdown page.

Source page: [HW 07 on Notion](https://app.notion.com/p/379c5511f06081489976d155c51e4872)

## 1. Optimized Singleton Version

```java
public class Singleton {
    private static volatile Singleton instance;

    private Singleton() {
    }

    public static Singleton getInstance() {
        if (instance == null) {
            synchronized (Singleton.class) {
                if (instance == null) {
                    instance = new Singleton();
                }
            }
        }
        return instance;
    }
}
```

This version is usually called the double-checked locking singleton. It is optimized because it keeps thread safety but avoids locking every time after the object has already been created.

## 2. Explain each line of code

### `public class Singleton {`

This defines a class named `Singleton`. The goal of this class is to make sure only one object of this class exists in the whole application.

### `private static volatile Singleton instance;`

`private` means other classes cannot directly change this variable.  
`static` means this variable belongs to the class itself, not to individual objects.  
`Singleton instance` is the place where we store the single object.  
`volatile` is important in multithreading. It tells the JVM not to let one thread see a half-finished object created by another thread.

### `private Singleton() {`

The constructor is private, so other classes cannot call `new Singleton()` directly. This is the key rule that prevents outside code from creating many objects.

### `}`

This closes the constructor body. In this example the constructor does not do extra work, but it still exists to block outside object creation.

### `public static Singleton getInstance() {`

This is the public method that other classes use to get the only singleton object.  
`public` means other classes can call it.  
`static` means we can call it using `Singleton.getInstance()` without first creating an object.

### `if (instance == null) {`

This is the first check. If the object has not been created yet, the code will enter the block. If the object already exists, we skip synchronization and return it directly, which improves performance.

### `synchronized (Singleton.class) {`

This adds a lock on the `Singleton.class` object. Only one thread can enter this block at a time. It protects the object creation step.

### `if (instance == null) {`

This is the second check. It is needed because two threads may both pass the first `if` before the first one gets the lock. After one thread creates the object, the second thread must check again to avoid creating another one.

### `instance = new Singleton();`

This creates the singleton object and stores it in the static variable.

### closing braces `}`

These close the inner `if`, the synchronized block, the outer `if`, the method, and finally the class.

### `return instance;`

This returns the single shared object to the caller.

## 3. What is reflection?

Reflection is a Java feature that allows a program to inspect and operate on classes, methods, fields, and constructors at runtime.

In simple words, reflection lets the program look at itself while it is running.

For example, with reflection we can:

- get a class name at runtime
- read or modify private fields
- call methods dynamically
- create objects without directly calling the constructor in normal code

Reflection is powerful, but it should be used carefully because it can reduce readability, hurt performance, and even break singleton protection if we use it to call a private constructor.

## 4. What is HTTP?

HTTP stands for HyperText Transfer Protocol. It is the standard communication rule used between a client and a server on the web.

In simple words, the browser, mobile app, or frontend sends a request, and the server sends back a response. That request-response style is the basic idea of HTTP.

## 5. What are HTTP status codes?

HTTP status codes are short numeric results returned by the server to tell the client what happened.

### `200 OK`

The request succeeded and the server returned the expected result.

### `201 Created`

The request succeeded and a new resource was created. For example, after creating a new user or order, `201` is often the best response.

### `202 Accepted`

The server accepted the request, but the work is not finished yet. This is useful for asynchronous jobs, such as background processing.

### `204 No Content`

The request succeeded, but the server does not return a response body. This is common after a successful delete or update when no extra data is needed.

### `301 Moved Permanently`

The resource has a new permanent address. Browsers and search engines should use the new URL in the future.

### `307 Temporary Redirect`

The resource is temporarily somewhere else. The important point is that the HTTP method should stay the same during the redirect.

### `400 Bad Request`

The request is invalid. Usually the client sent wrong data, missing fields, or malformed syntax.

### `401 Unauthorized`

The client is not properly authenticated. Usually this means login or a valid token is missing.

### `403 Forbidden`

The client is known, but it still does not have permission to access the resource. So this is more about authorization than identity.

### `404 Not Found`

The server cannot find the requested resource. For example, asking for a user ID that does not exist may return `404`.

### `500 Internal Server Error`

Something went wrong on the server side. This usually means the client did not do anything wrong, but the backend failed while processing the request.

## 6. What are GET, POST, PUT, DELETE, and PATCH?

### `GET`

Used to read data from the server. It should not change server data.

### `POST`

Used to create a new resource or submit data for processing. It often creates something new.

### `PUT`

Used to replace or fully update a resource. The client usually sends the full new version of the resource.

### `DELETE`

Used to remove a resource.

### `PATCH`

Used to partially update a resource. The client sends only the fields that need to change.

## 7. POST vs PATCH

`POST` is usually used to create a new resource or trigger processing.  
`PATCH` is usually used to partially modify an existing resource.

So the main difference is:

- `POST` often means create or submit
- `PATCH` means change part of something that already exists

## 8. POST vs PUT

`POST` usually creates a new resource and the server often decides the final resource ID.  
`PUT` usually updates or fully replaces a resource at a known URL.

For example:

- `POST /users` may create a new user
- `PUT /users/10` means replace or update user `10`

## 9. What is idempotent?

An idempotent operation means that doing the same request multiple times has the same final effect as doing it once.

For example, if deleting user `10` once removes the user, deleting it again should not create extra changes. The result is still that user `10` is gone.

## 10. Which HTTP methods are idempotent?

Common idempotent methods are:

- `GET`
- `PUT`
- `DELETE`
- `PATCH` can be idempotent in some designs, but not always

`POST` is usually not idempotent because sending the same create request multiple times may create multiple resources.

## 11. Small summary in my own words

HTTP is the language used by clients and servers to talk on the web.  
The method tells the server what kind of action we want.  
The status code tells us what result came back.  
Idempotent methods are important because they make retries safer in distributed systems.

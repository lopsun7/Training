# HW05

This homework was drafted in Notion and published here as a GitHub-friendly Markdown page.

Source page: [HW 05 on Notion](https://www.notion.so/36fc5511f060810aa0f0ea6eadbe5b04)

## Homework 5: Java Multi-threading Interview Version

### 1. How to create a thread? 4 ways

There are four common ways: extend `Thread`, implement `Runnable`, implement `Callable`, and use `ExecutorService` or a thread pool. `Thread` and `Runnable` are simple but do not return results, while `Callable` can return a value through `Future`. In production, thread pools are preferred because they reuse threads and control resource usage.

```java
new MyThread().start();
new Thread(() -> System.out.println("Runnable")).start();
new Thread(new FutureTask<>(() -> "Callable result")).start();
Executors.newFixedThreadPool(2).submit(() -> System.out.println("Pool task"));
```

### 2. Thread lifecycle

A Java thread commonly moves through `NEW`, `RUNNABLE`, `BLOCKED`, `WAITING`, `TIMED_WAITING`, and `TERMINATED`. After creating a `Thread` object, it is in `NEW`; after calling `start()`, it becomes `RUNNABLE`. It may become `BLOCKED` when waiting for a lock, `WAITING` after `wait()` or `join()`, `TIMED_WAITING` after `sleep()` or timed waiting, and finally `TERMINATED` when the task finishes.

### 3. How does thread pool work?

A thread pool reuses worker threads instead of creating a new thread for every task. When a task comes in, it first uses an idle core thread; if core threads are busy, the task enters the queue; if the queue is full, the pool creates extra threads up to `maximumPoolSize`. If both the queue and max threads are full, the rejection policy handles the task.

### 4. Problems of newCachedThreadPool and newFixedThreadPool

`newCachedThreadPool` can create up to `Integer.MAX_VALUE` threads, so under high traffic it may create too many threads and cause OOM. `newFixedThreadPool` has a fixed number of threads, but its task queue is unbounded, so too many pending tasks may also cause OOM. The key problem is hidden unbounded resource usage, so custom `ThreadPoolExecutor` is safer in production.

### 5. What is Future?

`Future` represents the result of an asynchronous task. It provides methods like `get()`, `isDone()`, and `cancel()`. Its main limitation is that `get()` blocks the current thread, so it is not good for complex non-blocking task chains.

### 6. What is CompletableFuture?

`CompletableFuture` is an enhanced async tool introduced in Java 8. It supports async execution, result transformation, task chaining, combining multiple tasks, and exception handling. It is commonly used in web applications because it can reduce blocking and make async workflows cleaner.

### 7. Future vs CompletableFuture

`Future` is mainly used to get the result of one async task, but calling `get()` blocks the current thread. `CompletableFuture` supports non-blocking callbacks and chaining, such as `thenApply`, `thenCompose`, and `thenCombine`. So `Future` is basic result retrieval, while `CompletableFuture` is better for async workflow orchestration.

### 8. Lock vs synchronized

Both are used to protect critical sections and ensure thread safety. `synchronized` is simpler because the JVM automatically releases the lock when the block exits. `Lock` is more flexible because it supports `tryLock()`, interruptible lock acquisition, fairness, and multiple `Condition` objects, but it must be released manually in `finally`.

### 9. wait(), notify(), notifyAll(), join()

`wait()`, `notify()`, and `notifyAll()` are `Object` methods used for thread communication and must be called inside synchronized code. `wait()` releases the monitor and waits, `notify()` wakes one waiting thread, and `notifyAll()` wakes all waiting threads. `join()` is a `Thread` method that makes the current thread wait until another thread finishes.

### 10. runAsync() / supplyAsync()

`runAsync()` runs an async task without returning a result, so it returns `CompletableFuture<Void>`. `supplyAsync()` runs an async task with a return value, so it returns `CompletableFuture<T>`. Use `runAsync()` for actions like sending logs, and use `supplyAsync()` for tasks like querying data.

### 11. thenApply() / thenApplyAsync()

`thenApply()` transforms the result of the previous stage, like mapping a user object to a user name. `thenApplyAsync()` does the same transformation but runs asynchronously, usually in another thread. Use the async version when the next step may be time-consuming or should not run in the same thread.

### 12. handle() / exceptionally()

Both are used for exception handling in `CompletableFuture`. `handle()` receives both the normal result and the exception, so it can handle success and failure together. `exceptionally()` only runs when an exception happens and is mainly used to provide a fallback value.

### 13. thenCompose()

`thenCompose()` is used when the second async task depends on the result of the first async task. For example, first query `userId`, then use that `userId` to query orders. It avoids nested futures and keeps the chain as `CompletableFuture<T>` instead of `CompletableFuture<CompletableFuture<T>>`.

### 14. thenCombine()

`thenCombine()` is used when two independent async tasks can run in parallel and their results need to be merged. For example, query user information and score information at the same time, then combine them into one response. It improves performance because the two tasks do not need to wait for each other.

### 15. allOf()

`allOf()` waits until all given `CompletableFuture` tasks complete. It returns `CompletableFuture<Void>`, so we usually collect each task's result after all tasks finish. It is useful for batch processing or parallel remote calls where all results are required.

### 16. anyOf()

`anyOf()` completes when any one of the given tasks finishes first. It returns the first completed result as `Object`. It is useful when we only need the fastest response, such as racing multiple service providers.

### 17. Other commonly used CompletableFuture APIs

`thenAccept()` consumes the previous result but does not return a new value. `thenRun()` runs the next action without using the previous result. `whenComplete()` observes success or failure without changing the result, and `join()` gets the final result but throws unchecked `CompletionException` instead of checked exceptions.

### 18. Minimal CompletableFuture demo

```java
CompletableFuture.supplyAsync(() -> "userId")
        .thenCompose(id -> CompletableFuture.supplyAsync(() -> "orders of " + id))
        .thenApply(result -> result.toUpperCase())
        .exceptionally(ex -> "fallback")
        .thenAccept(System.out::println);
```

This demo shows a typical async chain: start a task, compose a dependent task, transform the result, handle exceptions, and consume the final result. In interviews, I would explain the idea first and only write code when the interviewer asks for implementation.

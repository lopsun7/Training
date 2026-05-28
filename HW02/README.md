# HW02

This homework was drafted in Notion and published here as a GitHub-friendly Markdown page.

Source page: [HW 02 on Notion](https://www.notion.so/36ec5511f06081e48305d6dba5e34c39)

## 1. String vs StringBuilder vs StringBuffer

`String` is immutable, so every modification creates a new `String` object. `StringBuilder` is mutable and faster, but not thread-safe. `StringBuffer` is mutable and thread-safe because its methods are synchronized, but it is slower.

## 2. Comparator vs Comparable

`Comparable` defines the natural ordering inside the class by overriding `compareTo()`. `Comparator` defines external custom ordering by overriding `compare()`. Use `Comparable` for one default sort rule, and use `Comparator` when we need multiple sorting rules or cannot modify the original class.

```java
class User implements Comparable<User> {
    int age;
    public int compareTo(User other) { return this.age - other.age; }
}

Comparator<User> byAgeDesc = (a, b) -> b.age - a.age;
```

## 3. Overriding vs Overloading

Overloading means the same method name but different parameters in the same class, and it is compile-time polymorphism. Overriding means a child class provides a new implementation for a parent method with the same signature, and it is runtime polymorphism.

## 4. JRE vs JDK vs JVM

JVM executes Java bytecode and manages memory. JRE includes JVM and runtime libraries to run Java applications. JDK includes JRE plus development tools like `javac`, so developers use JDK to write and compile Java code.

## 5. Java 8 Basic Data Types

Java has 8 primitive data types: `byte`, `short`, `int`, `long`, `float`, `double`, `char`, and `boolean`. They store simple values directly and are not objects.

## 6. Primitive Type vs Reference Type

Primitive variables store actual values, such as `int a = 10`. Reference variables store references to objects in heap, such as `Person p = new Person()`. When two references point to the same object, modifying the object through one reference affects the other.

## 7. How JVM Works

Java source code is compiled into `.class` bytecode by `javac`. JVM loads bytecode through `ClassLoader`, verifies it, executes it through the execution engine, and manages memory and GC. This is why Java can be platform-independent.

```text
.java -> javac -> .class bytecode -> ClassLoader -> JVM -> run
```

## 8. JVM Memory Data Model

JVM memory mainly includes Method Area / Metaspace, Heap, Stack, PC Register, and Native Method Stack. Heap stores objects, Stack stores method calls and local variables, and Method Area / Metaspace stores class-level information such as metadata and static members.

## 9. How GC Works

GC starts from GC Roots and finds all reachable objects. Objects that cannot be reached are considered garbage and can be collected. GC mainly works on heap memory, but it has performance cost.

## 10. Young / Old / Perm Generation

Young Generation stores new objects and is collected frequently by Minor GC. Old Generation stores long-lived objects and is collected less often but more expensively. `PermGen` stored class metadata before Java 8, and Java 8 replaced it with Metaspace.

## 11. Different Types of GC

Serial GC is simple and single-threaded. Parallel GC focuses on throughput, while G1 balances throughput and pause time. ZGC and Shenandoah focus on low latency, and Epsilon is a no-op GC mainly used for testing.


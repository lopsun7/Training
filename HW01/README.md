# HW01

This homework was drafted in Notion and then published here as a GitHub-friendly Markdown page.

Source page: [HW 01 on Notion](https://www.notion.so/36dc5511f060806697eefc36e06d8b22)

## Part 1: Collections And Data Structures

### 1. List vs Set

`List` allows duplicates and maintains insertion order. `Set` does not allow duplicates and generally does not guarantee order. Use `LinkedHashSet` for insertion order or `TreeSet` for sorted order.

```java
List<Integer> list = new ArrayList<>(Arrays.asList(1, 2, 2, 3)); // [1, 2, 2, 3]
Set<Integer> set = new HashSet<>(Arrays.asList(1, 2, 2, 3));     // [1, 2, 3]
```

### 2. LinkedList vs ArrayList

`ArrayList` uses a dynamic array internally, making it fast for random access by index (`O(1)`). `LinkedList` uses a doubly-linked chain of nodes, making insertion and deletion faster (`O(1)`) but random access slower (`O(n)`). Use `ArrayList` by default; switch to `LinkedList` only when you have frequent insertions/deletions.

```java
ArrayList<String> al = new ArrayList<>();    // fast get(i)
LinkedList<String> ll = new LinkedList<>();  // fast add/remove at head or tail
```

### 3. What is the Map Interface

`Map` stores key-value pairs where keys are unique. It is not part of the Collections framework because it does not extend `Iterable`. Common implementations are `HashMap`, `LinkedHashMap`, and `TreeMap`.

```java
Map<String, Integer> map = new HashMap<>();
map.put("Alice", 90);
map.get("Alice"); // 90
```

### 4. How does HashMap work

When you call `put(key, value)`, `HashMap` calls `hashCode()` on the key to find a bucket index. It then uses `equals()` to check if the key already exists in that bucket. If no collision, the entry is stored directly; if collision, it is chained as a linked list and may later be converted to a Red-Black tree.

```java
Map<String, Integer> map = new HashMap<>();
map.put("Alice", 1); // hashCode("Alice") -> bucket index -> store
map.put("Bob", 2);
```

### 5. What is a Hash Collision

A hash collision happens when two different keys produce the same hash value and land in the same bucket. Java resolves this by chaining entries in a linked list at that bucket. In Java 8+, if the chain exceeds 8 entries, it converts to a Red-Black tree for `O(log n)` performance instead of `O(n)`.

### 6. What is Collections used for

`Collections` is a utility class with static helper methods for working with collection objects. It provides methods like `sort()`, `shuffle()`, `reverse()`, `min()`, `max()`, and `unmodifiableList()`.

```java
List<Integer> nums = new ArrayList<>(Arrays.asList(3, 1, 2));
Collections.sort(nums);    // [1, 2, 3]
Collections.reverse(nums); // [3, 2, 1]
int max = Collections.max(nums); // 3
```

### 7. What is an Immutable Class

An immutable class is one whose state cannot be changed after it is created. `String` is the most common example. To make a custom class immutable: mark it `final`, make all fields `private final`, and provide no setters.

```java
final class Point {
    private final int x, y;
    public Point(int x, int y) { this.x = x; this.y = y; }
    public int getX() { return x; }
}
```

### 8. HashTable vs HashMap vs ConcurrentHashMap

| Feature | HashTable | HashMap | ConcurrentHashMap |
| --- | --- | --- | --- |
| Thread-safe | Yes (legacy) | No | Yes (recommended) |
| Null keys/values | No | Yes | No |
| Performance | Poor (global lock) | Best single-thread | Best multi-thread |

`HashTable` locks the entire map on every operation. `HashMap` is not thread-safe but is fastest for single-threaded use. `ConcurrentHashMap` locks more efficiently and is the go-to for multi-threaded environments.

### 9. String vs StringBuilder vs StringBuffer

| Type | Immutable | Thread-safe | Best use |
| --- | --- | --- | --- |
| `String` | Yes | Yes | Few or no modifications |
| `StringBuilder` | No | No | Single-threaded, frequent changes |
| `StringBuffer` | No | Yes | Multi-threaded, frequent changes |

```java
String s = "hello";
s += " world"; // creates a NEW object

StringBuilder sb = new StringBuilder("hello");
sb.append(" world"); // modifies same object
```

### 10. Why override hashCode and equals together

`HashMap` and `HashSet` use `hashCode()` first to find the bucket, then `equals()` to confirm the key match. If you override `equals()` but not `hashCode()`, two "equal" objects may land in different buckets and never be found.

```java
Set<Point> set = new HashSet<>();
set.add(new Point(1, 2));
set.contains(new Point(1, 2)); // false if hashCode is inconsistent
```

### 11. Common Data Structure API Practice

```java
// LIST
List<String> list = new ArrayList<>();
list.add("a"); list.add("b"); list.add("c");
list.get(0);        // "a"
list.remove("b");
list.size();        // 2

// SET
Set<String> set = new HashSet<>();
set.add("x"); set.add("x"); // only one "x" stored
set.contains("x");  // true

// MAP
Map<String, Integer> map = new HashMap<>();
map.put("Alice", 95);
map.getOrDefault("Bob", 0); // 0
map.containsKey("Alice");   // true
for (Map.Entry<String, Integer> e : map.entrySet()) {
    System.out.println(e.getKey() + ": " + e.getValue());
}

// QUEUE (FIFO)
Queue<String> queue = new LinkedList<>();
queue.offer("first"); queue.offer("second");
queue.poll();   // "first"
queue.peek();   // "second"

// STACK (LIFO)
Deque<String> stack = new ArrayDeque<>();
stack.push("a"); stack.push("b");
stack.pop();    // "b"
stack.peek();   // "a"
```

## Part 2: Java Fundamentals

### 1. String vs StringBuilder vs StringBuffer

`String` is immutable, so every modification creates a new object. `StringBuilder` is mutable and fast but not thread-safe. `StringBuffer` is mutable and thread-safe, but slower due to synchronization.

```java
String s = "hello";
s += " world";          // new object created

StringBuilder sb = new StringBuilder("hello");
sb.append(" world");    // same object modified
sb.insert(5, ",");      // "hello, world"
sb.reverse();           // "dlrow ,olleh"

StringBuffer sbf = new StringBuffer("hello");
sbf.append(" world");   // thread-safe version
```

### 2. Comparable vs Comparator

`Comparable` is implemented inside the class by overriding `compareTo()` and defines the natural order. `Comparator` is external and lets you define custom sorting strategies without modifying the class.

```java
class Employee implements Comparable<Employee> {
    int salary;
    public int compareTo(Employee other) {
        return this.salary - other.salary;
    }
}

Comparator<Employee> byAge = (a, b) -> a.age - b.age;
Comparator<Employee> byName = Comparator.comparing(e -> e.name);

List<Employee> employees = new ArrayList<>();
Collections.sort(employees);
employees.sort(byAge);
employees.sort(byName);
```

### 3. Overriding vs Overloading

Overloading means methods in the same class share the same name but have different parameter lists. Overriding means a child class redefines a parent method with the same signature but different behavior.

```java
class Calculator {
    int add(int a, int b)          { return a + b; }
    double add(double a, double b) { return a + b; }
    int add(int a, int b, int c)   { return a + b + c; }
}

class Animal {
    void speak() { System.out.println("..."); }
}

class Dog extends Animal {
    @Override
    void speak() { System.out.println("Woof"); }
}
```

### 4. Java 8 Primitive Data Types

| Type | Size | Example |
| --- | --- | --- |
| `byte` | 8-bit | `-128` to `127` |
| `short` | 16-bit | `-32,768` to `32,767` |
| `int` | 32-bit | most common integer |
| `long` | 64-bit | large numbers, add `L` suffix |
| `float` | 32-bit | decimals, add `F` suffix |
| `double` | 64-bit | precise decimals |
| `char` | 16-bit | single character |
| `boolean` | JVM-defined | `true` or `false` |

```java
byte b = 100;
short s = 30000;
int i = 100000;
long l = 10000000000L;
float f = 3.14F;
double d = 3.14159265;
char c = 'A';
boolean flag = true;
```

### 5. Primitive Type vs Reference Type

Primitive types store the actual value directly. Reference types store a memory address pointing to an object. This is why assigning primitives copies values, while assigning reference types can point two variables to the same object.

```java
int a = 5;
int b = a;
b = 10;     // a is still 5

int[] arr1 = {1, 2, 3};
int[] arr2 = arr1;
arr2[0] = 99; // arr1[0] is now 99 too

String s1 = new String("hello");
String s2 = new String("hello");
s1 == s2;       // false
s1.equals(s2);  // true
```

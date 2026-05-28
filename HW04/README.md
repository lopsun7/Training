# HW04

This homework was drafted in Notion and published here as a GitHub-friendly Markdown page.

Source page: [HW 04 on Notion](https://www.notion.so/36ec5511f0608121ae8bfdf62f2fcb5e)

## 1. What Is A Functional Interface

A functional interface is an interface with exactly one abstract method. It can be used with lambda expressions and method references. `Runnable`, `Predicate`, `Consumer`, `Supplier`, and `Function` are common examples.

```java
@FunctionalInterface
interface MyTask {
    void run();
}
```

## 2. What Is A Default Method

A default method is a method with implementation inside an interface. It allows interfaces to add new behavior without breaking existing implementations. It is declared with the `default` keyword.

```java
interface A {
    default void hello() { System.out.println("hello"); }
}
```

## 3. Predicate vs Supplier vs Consumer vs Function

`Predicate<T>` takes `T` and returns `boolean`. `Supplier<T>` takes nothing and returns `T`. `Consumer<T>` takes `T` and returns nothing, while `Function<T, R>` takes `T` and returns `R`.

## 4. Code For Predicate, Supplier, Consumer, Function

```java
Predicate<Integer> isAdult = age -> age >= 18;
Supplier<String> supplier = () -> "hello";
Consumer<String> printer = s -> System.out.println(s);
Function<String, Integer> length = s -> s.length();
```

These interfaces are useful because they allow behavior to be passed as data. They are heavily used in Stream API and callback-style code.

## 5. What Is Method Reference

Method reference is a shorter way to write a lambda when the lambda only calls an existing method. Common forms include `ClassName::staticMethod`, `object::instanceMethod`, and `ClassName::instanceMethod`.

```java
list.forEach(System.out::println);
```

## 6. What Is CompletableFuture

`CompletableFuture` is used for asynchronous programming in Java. It allows tasks to run in the background and chain follow-up actions. It is useful for non-blocking operations and combining multiple async tasks.

```java
CompletableFuture.supplyAsync(() -> "hello")
        .thenApply(String::toUpperCase)
        .thenAccept(System.out::println);
```

## 7. default Keyword vs Java Default Scope

`default` keyword in an interface defines a method with implementation. Default scope means package-private access modifier, which is used when no access modifier is written. They are unrelated concepts with the same word.

## 8. Stream Coding Setup

```java
class Student {
    String name;
    int age;
    int score;
    String gender;

    Student(String name, int age, int score, String gender) {
        this.name = name;
        this.age = age;
        this.score = score;
        this.gender = gender;
    }
}

List<Student> list = Arrays.asList(
    new Student("Alice", 18, 90, "girl"),
    new Student("Bob", 19, 55, "boy"),
    new Student("Amy", 18, 80, "girl")
);
```

## 9. Find All Students Whose Name Starts With A

Use `filter()` to keep students whose name starts with `A`.

```java
List<Student> result = list.stream()
        .filter(s -> s.name.startsWith("A"))
        .toList();
```

## 10. Get Sum Of All Students' Score

Use `mapToInt()` to convert `Student` to score, then use `sum()`.

```java
int sum = list.stream().mapToInt(s -> s.score).sum();
```

## 11. Find All Students Whose Score >= 60

Use `filter()` to keep students who passed.

```java
List<Student> passed = list.stream()
        .filter(s -> s.score >= 60)
        .toList();
```

## 12. Retrieve All Students' Names

Use `map()` to transform `Student` objects into names.

```java
List<String> names = list.stream()
        .map(s -> s.name)
        .toList();
```

## 13. Count Frequency Of Each Age

Use `Collectors.groupingBy()` with `Collectors.counting()`.

```java
Map<Integer, Long> ageCount = list.stream()
        .collect(Collectors.groupingBy(s -> s.age, Collectors.counting()));
```

## 14. Count Number Of Boys And Girls

Group students by gender and count each group.

```java
Map<String, Long> genderCount = list.stream()
        .collect(Collectors.groupingBy(s -> s.gender, Collectors.counting()));
```

## 15. Intermediate Operation vs Terminal Operation

Intermediate operations return another stream and are lazy, such as `filter()`, `map()`, and `sorted()`. Terminal operations produce a final result and trigger execution, such as `collect()`, `forEach()`, `count()`, and `sum()`.

## 16. Count Frequency Of Each Char Using Stream API

Convert chars to stream elements, then group by character and count.

```java
char[] arr = {'a', 'b', 'b', 'c'};
Map<Character, Long> freq = new String(arr).chars()
        .mapToObj(c -> (char) c)
        .collect(Collectors.groupingBy(c -> c, Collectors.counting()));
```

## 17. Stream API: map() vs flatMap()

`map()` transforms each element into one new element. `flatMap()` transforms each element into a stream and then flattens all streams into one stream. Use `flatMap()` when dealing with nested collections.

```java
List<List<Integer>> nums = Arrays.asList(Arrays.asList(1, 2), Arrays.asList(3));
List<Integer> flat = nums.stream()
        .flatMap(List::stream)
        .toList();
```

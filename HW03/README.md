# HW03

This homework was drafted in Notion and published here as a GitHub-friendly Markdown page.

Source page: [HW 03 on Notion](https://www.notion.so/36ec5511f06081e18959e1728cc60a7b)

## 1. Access Modifiers

`public` can be accessed from anywhere. `private` can only be accessed inside the same class. `protected` can be accessed in the same package and by subclasses, while default scope can only be accessed within the same package.

## 2. Static Scope

`static` means the member belongs to the class, not a specific object. Static variables and methods can be accessed by class name and are shared by all instances.

```java
class Counter {
    static int count = 0;
}
```

## 3. How ClassLoader Works

`ClassLoader` loads `.class` files into JVM memory when the class is needed. The loading process usually includes loading, linking, and initialization. Java uses parent delegation model to let parent classloaders try loading the class first.

## 4. Checked vs Unchecked Exceptions

Checked exceptions are checked at compile time, so the method must catch them or declare `throws`. Unchecked exceptions happen at runtime and usually extend `RuntimeException`. Checked exceptions are for recoverable cases, while unchecked exceptions often mean programming errors or invalid input.

## 5. finally vs final vs finalize

`final` is a keyword used for class, method, or variable. `finally` is a block that usually executes after try-catch and is often used for cleanup. `finalize()` was a method called before GC, but it is unreliable and should not be used in modern Java.

## 6. Try-with-resource

Try-with-resource automatically closes resources that implement `AutoCloseable`. It is cleaner than ordinary try-finally because we do not need to manually close resources in the finally block.

```java
try (FileReader reader = new FileReader("a.txt")) {
    reader.read();
}
```

## 7. Runtime Exception

`RuntimeException` is an unchecked exception that occurs during program execution. The compiler does not force us to catch or declare it. Example: `NullPointerException`, `IllegalArgumentException`, or `ArithmeticException`.

```java
int x = 10 / 0; // ArithmeticException
```

## 8. NoClassDefFoundError vs ClassNotFoundException

`ClassNotFoundException` is a checked exception thrown when code tries to load a class by name but cannot find it. `NoClassDefFoundError` is an `Error` that happens when the class existed at compile time but is missing at runtime.

## 9. Why Clean Up I/O Resources

I/O resources use system resources such as file handles, sockets, and database connections. If they are not closed, the application may have resource leaks. `finally` or try-with-resource ensures cleanup happens even if an exception occurs.

## 10. OutOfMemoryError

`OutOfMemoryError` happens when JVM cannot allocate more memory. Common reasons include memory leak, too many objects, huge arrays, or insufficient heap size. It is an `Error`, not a normal exception, so the better solution is usually fixing memory usage or JVM settings.

## 11. Generics And Advantages

Generics allow classes, interfaces, and methods to work with types as parameters. They improve type safety and reduce manual casting. For example, `List<String>` only accepts `String` values.

```java
List<String> names = new ArrayList<>();
names.add("Tom");
```

## 12. How Generics Work And Type Erasure

Java generics are mainly checked at compile time. At runtime, generic type information is erased, which is called type erasure. For example, `List<String>` and `List<Integer>` both become raw `List` at runtime.

## 13. List<? extends T> vs List<? super T>

`List<? extends T>` means the list can contain `T` or its subclasses, so it is good for reading. `List<? super T>` means the list can contain `T` or its parent classes, so it is good for writing. A simple rule is PECS: Producer Extends, Consumer Super.

## 14. Optional Class

`Optional` is a container that may or may not contain a value. It helps reduce direct null checks and avoid `NullPointerException`. Common methods include `ofNullable()`, `orElse()`, and `orElseThrow()`.

```java
String name = null;
String result = Optional.ofNullable(name).orElse("default");
String mustHave = Optional.ofNullable(name).orElseThrow();
```

## 15. What is OOP

OOP means Object-Oriented Programming. It organizes code around objects that contain data and behavior. Four key principles are encapsulation, inheritance, polymorphism, and abstraction.

## Coding Exercise 1: Third Highest Frequency Character

Use a frequency map to count characters, sort frequencies in descending order, then return characters with the third highest frequency.

```java
public static List<Character> thirdHighest(char[] arr) {
    Map<Character, Integer> map = new HashMap<>();
    for (char c : arr) map.put(c, map.getOrDefault(c, 0) + 1);

    List<Integer> freqs = map.values().stream()
            .distinct()
            .sorted(Comparator.reverseOrder())
            .toList();
    if (freqs.size() < 3) return Collections.emptyList();

    int target = freqs.get(2);
    return map.entrySet().stream()
            .filter(e -> e.getValue() == target)
            .map(Map.Entry::getKey)
            .toList();
}
```

## Coding Exercise 2: Reverse A String

Use two pointers or `StringBuilder.reverse()` to reverse a string.

```java
public static String reverse(String s) {
    return new StringBuilder(s).reverse().toString();
}
```

## Coding Exercise 3: Pair Sum To Target

Use a map to store numbers and find whether `target - current` exists. After using a pair, decrease or remove counts so each element is used once.

```java
public static List<List<Integer>> pairSum(int[] nums, int target) {
    Map<Integer, Integer> count = new HashMap<>();
    for (int n : nums) count.put(n, count.getOrDefault(n, 0) + 1);

    List<List<Integer>> res = new ArrayList<>();
    for (int n : nums) {
        if (count.getOrDefault(n, 0) == 0) continue;
        count.put(n, count.get(n) - 1);
        int need = target - n;
        if (count.getOrDefault(need, 0) > 0) {
            res.add(Arrays.asList(n, need));
            count.put(need, count.get(need) - 1);
        }
    }
    return res;
}
```


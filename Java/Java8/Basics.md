
# Java 8 Features 

## 1. Lambda Expressions

- Introduced to support **functional programming**.
- Syntax: `(parameters) -> expression`
- Example:
  ```java
  List<String> names = Arrays.asList("John", "Jane", "Doe");
  names.forEach(name -> System.out.println(name));
  ```

### Key Points:
- Enables treating functions as method arguments.
- Helps write cleaner and concise code.
- Can be used to replace **anonymous classes**, especially with functional interfaces.

---

## 2. Functional Interfaces

- Interface with **exactly one abstract method**.
- Annotated with `@FunctionalInterface`.
- Examples: `Runnable`, `Callable`, `Comparator`, `Function`, `Predicate`, `Supplier`, `Consumer`.

```java
@FunctionalInterface
interface MyFunctional {
    void execute();
}
```

---

## 3. Default and Static Methods in Interfaces

- Java 8 allows **default method** implementations in interfaces.
- **Static methods** can also be added.

```java
interface Vehicle {
    default void start() {
        System.out.println("Vehicle started");
    }

    static void stop() {
        System.out.println("Vehicle stopped");
    }
}
```

---

## 4. Method References

- Shorthand for lambda expressions.
- Types:
  - Static Method: `ClassName::staticMethod`
  - Instance Method: `object::instanceMethod`
  - Constructor: `ClassName::new`

```java
List<String> names = Arrays.asList("a", "b");
names.forEach(System.out::println);
```

---

## 5. Streams API

- **Functional-style operations** on collections.
- Lazy, efficient processing.
- Intermediate Ops: `map()`, `filter()`, `sorted()`, `distinct()`
- Terminal Ops: `collect()`, `forEach()`, `reduce()`, `count()`

```java
List<String> names = Arrays.asList("a", "bb", "ccc");
List<String> result = names.stream()
  .filter(s -> s.length() > 1)
  .map(String::toUpperCase)
  .collect(Collectors.toList());
```

---

## 6. Optional Class

- Avoids **NullPointerException**.
- Container for nullable values.
- Methods: `isPresent()`, `ifPresent()`, `orElse()`, `orElseGet()`, `map()`

```java
Optional<String> opt = Optional.ofNullable("test");
opt.ifPresent(System.out::println);
```

---

## 7. New Date and Time API (java.time) *(Highly Asked)*

- Replaces old `Date`, `Calendar`.
- Immutable, thread-safe.
- Key classes:
  - `LocalDate`, `LocalTime`, `LocalDateTime`
  - `ZonedDateTime`, `Instant`
  - `Duration`, `Period`
  - `DateTimeFormatter`

```java
LocalDate date = LocalDate.now();
LocalDate tomorrow = date.plusDays(1);
```

---

## 8. Collectors Utility Class

- Used with Streams to **collect** data.
- Examples:
  - `toList()`, `toSet()`
  - `joining()`, `groupingBy()`, `partitioningBy()`
  - `summarizingInt()`, `counting()`

```java
Map<Boolean, List<String>> partitioned =
    names.stream().collect(Collectors.partitioningBy(s -> s.length() > 2));
```

---

## 9. Nashorn JavaScript Engine

- Allows execution of JavaScript code inside Java.
- Usage:
  ```java
  ScriptEngineManager manager = new ScriptEngineManager();
  ScriptEngine engine = manager.getEngineByName("nashorn");
  engine.eval("print('Hello from JS')");
  ```

---

## 10. CompletableFuture (java.util.concurrent)

- Asynchronous programming support.
- Works with `ExecutorService`.
- Methods: `supplyAsync()`, `thenApply()`, `thenAccept()`, `thenCombine()`

```java
CompletableFuture.supplyAsync(() -> "Hello")
    .thenApply(s -> s + " World")
    .thenAccept(System.out::println);
```

---

## 11. New Annotations

- `@FunctionalInterface`: Checks interface has a single abstract method.
- `@Repeatable`: Allows repeating annotations on same declaration.

```java
@FunctionalInterface
interface Printer {
    void print();
}
```

---

## 12. Type Annotations (JSR 308)

- More places to use annotations (e.g., on generic types, method receivers).

```java
@NonNull List<String> list = new ArrayList<>();
```

---

## 13. Parallel Streams

- Leverage multi-core architectures.
- Just use `.parallelStream()` instead of `.stream()`.

```java
list.parallelStream().forEach(System.out::println);
```

---

## 14. Base64 Encoding and Decoding

- Utility class: `java.util.Base64`

```java
String encoded = Base64.getEncoder().encodeToString("hello".getBytes());
byte[] decoded = Base64.getDecoder().decode(encoded);
```

---

## 15. New Java IO Enhancements

- `Files.list(Path)`
- `Files.lines(Path)`
- `Files.find()`

---

## 16. Concurrency Enhancements

- `StampedLock` – alternative to `ReadWriteLock`
- `LongAdder` and `DoubleAdder` – better performance under high contention.

---
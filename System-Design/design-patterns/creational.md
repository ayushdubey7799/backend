
# Creational Design Patterns

## ✅ Singleton Pattern

### 🔹 Description
Ensures that a class has only one instance and provides a global access point to it. Ideal for shared resources like configuration or logging systems.

### 🔹 Example Code

```java
public class Singleton {
    private static final Singleton instance = new Singleton();

    private Singleton() {}

    public static Singleton getInstance() {
        return instance;
    }
}
```

### 🔹 Industry Use Cases

1. **Configuration Manager**
   - *Problem*: Need one central configuration instance throughout the application.
   - *Solution*: Singleton ensures only one instance is used application-wide.

2. **Application Logger**
   - *Problem*: Prevent multiple logger objects from different services.
   - *Solution*: Singleton provides a shared logging instance.

3. **Thread Pool Manager**
   - *Problem*: Multiple threads need access to a shared thread pool.
   - *Solution*: Singleton provides synchronized access to the pool.

---

## ✅ Factory Method Pattern

### 🔹 Description
Provides an interface for creating objects in a superclass but allows subclasses to alter the type of objects that will be created.

### 🔹 Example Code

```java
interface Product {
    void use();
}

class ConcreteProductA implements Product {
    public void use() {
        System.out.println("Using Product A");
    }
}

class ProductFactory {
    public Product createProduct(String type) {
        if ("A".equals(type)) return new ConcreteProductA();
        return null;
    }
}
```

### 🔹 Industry Use Cases

1. **Notification Service**
   - *Problem*: Need to support multiple types of notifications (SMS, Email).
   - *Solution*: Factory returns specific notifier objects based on input.

2. **Document Parser**
   - *Problem*: Different formats like JSON, XML, CSV need different parsers.
   - *Solution*: Factory provides the right parser object dynamically.

3. **Shape Drawing Application**
   - *Problem*: Create various shape objects like Circle, Rectangle at runtime.
   - *Solution*: Factory instantiates correct shape based on input string.

---

## ✅ Abstract Factory Pattern

### 🔹 Description
Creates families of related objects without specifying their concrete classes. It is useful when the system needs to be independent of how its objects are created.

### 🔹 Example Code

```java
interface GUIFactory {
    Button createButton();
    Checkbox createCheckbox();
}

class WinFactory implements GUIFactory {
    public Button createButton() { return new WinButton(); }
    public Checkbox createCheckbox() { return new WinCheckbox(); }
}
```

### 🔹 Industry Use Cases

1. **Cross-platform UI Toolkit**
   - *Problem*: UI components should work for both Windows and macOS.
   - *Solution*: Abstract Factory returns platform-specific components.

2. **Theme Manager**
   - *Problem*: Switch between light and dark themes dynamically.
   - *Solution*: Abstract Factory returns components matching the theme.

3. **Cloud Service Provider Integration**
   - *Problem*: Need to interact with AWS, Azure, GCP in the same codebase.
   - *Solution*: Abstract Factory creates service objects for each provider.

---

## ✅ Builder Pattern

### 🔹 Description
Separates the construction of a complex object from its representation so that the same construction process can create different representations.

### 🔹 Example Code

```java
class User {
    private String firstName;
    private String lastName;
    private int age;

    public static class Builder {
        private String firstName;
        private String lastName;
        private int age;

        public Builder setFirstName(String firstName) { this.firstName = firstName; return this; }
        public Builder setLastName(String lastName) { this.lastName = lastName; return this; }
        public Builder setAge(int age) { this.age = age; return this; }

        public User build() {
            return new User(this);
        }
    }

    private User(Builder builder) {
        this.firstName = builder.firstName;
        this.lastName = builder.lastName;
        this.age = builder.age;
    }
}
```

### 🔹 Industry Use Cases

1. **Object Construction with Many Optional Fields**
   - *Problem*: Telescoping constructor makes object creation unreadable.
   - *Solution*: Builder simplifies and makes object construction flexible.

2. **HTML/XML Builders**
   - *Problem*: Build structured documents programmatically.
   - *Solution*: Builder pattern generates tag hierarchy cleanly.

3. **Query Builder**
   - *Problem*: Construct SQL queries with dynamic clauses.
   - *Solution*: Builder assembles query string in fluent style.

---

## ✅ Prototype Pattern

### 🔹 Description
Creates new objects by copying an existing object, known as the prototype. It avoids the cost of creating a new object from scratch.

### 🔹 Example Code

```java
abstract class Shape implements Cloneable {
    public Object clone() throws CloneNotSupportedException {
        return super.clone();
    }
}
```

### 🔹 Industry Use Cases

1. **UI Element Templates**
   - *Problem*: Need many similar UI elements (e.g., buttons).
   - *Solution*: Clone a prototype rather than recreate from scratch.

2. **Object Caching**
   - *Problem*: Creating expensive objects like DB connections repeatedly.
   - *Solution*: Use a cached prototype and clone it when needed.

3. **Game Character Creation**
   - *Problem*: Create many NPCs with similar traits.
   - *Solution*: Clone a base character and customize it.

---
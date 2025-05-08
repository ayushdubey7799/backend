# Structural Design Patterns

## ✅ Adapter Pattern

### 🔹 Description
Allows incompatible interfaces to work together by acting as a bridge between them.

### 🔹 Example Code

```java
interface Target {
    void request();
}

class Adaptee {
    void specificRequest() {
        System.out.println("Specific request");
    }
}

class Adapter implements Target {
    private Adaptee adaptee = new Adaptee();

    public void request() {
        adaptee.specificRequest();
    }
}
```

### 🔹 Industry Use Cases

1. **Legacy System Integration**  
   - *Problem*: Old payment gateway doesn't match new interface.  
   - *Solution*: Adapter bridges between old and new APIs.

2. **Third-Party API Integration**  
   - *Problem*: Library has different method signatures.  
   - *Solution*: Adapter converts app-specific calls to library-specific ones.

3. **Database Driver Wrapping**  
   - *Problem*: Standardize calls to various database drivers.  
   - *Solution*: Use Adapter to unify interface for SQL execution.

---

## ✅ Decorator Pattern

### 🔹 Description
Adds new behaviors to objects dynamically by wrapping them in decorator classes.

```java
interface Notifier {
    void send(String message);
}

class EmailNotifier implements Notifier {
    public void send(String message) {
        System.out.println("Email: " + message);
    }
}

class SMSDecorator implements Notifier {
    private Notifier notifier;

    public SMSDecorator(Notifier notifier) {
        this.notifier = notifier;
    }

    public void send(String message) {
        notifier.send(message);
        System.out.println("SMS: " + message);
    }
}
```

### 🔹 Industry Use Cases

1. **Multi-Channel Alerts**  
   - *Problem*: Send alerts through multiple media.  
   - *Solution*: Use decorators to add extra channels.

2. **Logging Enhancements**  
   - *Problem*: Add metadata or format logs.  
   - *Solution*: Decorator enhances base logger functionality.

3. **Stream Filters**  
   - *Problem*: Add encryption or buffering.  
   - *Solution*: Decorator layers filters on stream objects.

---

## ✅ Proxy Pattern

### 🔹 Description
Provides a surrogate or placeholder for another object to control access to it.

```java
interface Service {
    void perform();
}

class RealService implements Service {
    public void perform() {
        System.out.println("Performing real service");
    }
}

class ServiceProxy implements Service {
    private RealService realService;

    public void perform() {
        if (realService == null) {
            realService = new RealService();
        }
        System.out.println("Access control proxy");
        realService.perform();
    }
}
```

### 🔹 Industry Use Cases

1. **Lazy Loading**  
   - *Problem*: Delay expensive instantiation.  
   - *Solution*: Proxy controls creation and caching.

2. **Access Control**  
   - *Problem*: Restrict unauthorized access.  
   - *Solution*: Proxy checks credentials.

3. **Remote Proxy**  
   - *Problem*: Treat remote services like local ones.  
   - *Solution*: Proxy handles remote communication.

---

## ✅ Facade Pattern

### 🔹 Description
Provides a simplified interface to a complex subsystem.

```java
class ComplexSystem {
    void init() { System.out.println("Init"); }
    void process() { System.out.println("Processing"); }
    void shutdown() { System.out.println("Shutdown"); }
}

class Facade {
    private ComplexSystem system = new ComplexSystem();

    public void run() {
        system.init();
        system.process();
        system.shutdown();
    }
}
```


### 🔹 Industry Use Cases

1. **Simplifying Subsystems**  
   - *Problem*: Reduce client-side complexity.  
   - *Solution*: Facade exposes high-level API.

2. **Media Conversion Tool**  
   - *Problem*: Complex codec interactions.  
   - *Solution*: Facade provides simple interface.

3. **Database Utility Layer**  
   - *Problem*: JDBC boilerplate code.  
   - *Solution*: Facade abstracts common DB operations.

---

## ✅ Composite Pattern

### 🔹 Description
Treat individual and group objects uniformly.

```java
interface Component {
    void showDetails();
}

class Leaf implements Component {
    public void showDetails() {
        System.out.println("Leaf");
    }
}

class Composite implements Component {
    private List<Component> children = new ArrayList<>();

    public void add(Component component) {
        children.add(component);
    }

    public void showDetails() {
        for (Component c : children) {
            c.showDetails();
        }
    }
}
```


### 🔹 Industry Use Cases

1. **File System**  
   - *Problem*: Uniform operations on files and folders.  
   - *Solution*: Composite pattern enables recursive structure.

2. **UI Widget Trees**  
   - *Problem*: Nest widgets of varying complexity.  
   - *Solution*: Composite allows tree-based rendering.

3. **Organization Structure**  
   - *Problem*: Employees and managers share actions.  
   - *Solution*: Composite supports nested hierarchies.

---

## ✅ Bridge Pattern

### 🔹 Description
Decouple abstraction from implementation so both can vary independently.

```java
interface DrawAPI {
    void drawCircle();
}

class RedCircle implements DrawAPI {
    public void drawCircle() {
        System.out.println("Drawing Red Circle");
    }
}

abstract class Shape {
    protected DrawAPI drawAPI;

    protected Shape(DrawAPI drawAPI) {
        this.drawAPI = drawAPI;
    }

    abstract void draw();
}

class Circle extends Shape {
    public Circle(DrawAPI drawAPI) {
        super(drawAPI);
    }

    public void draw() {
        drawAPI.drawCircle();
    }
}
```

### 🔹 Industry Use Cases

1. **Graphics APIs**  
   - *Problem*: Vary shapes and drawing mechanisms.  
   - *Solution*: Bridge separates shape logic from rendering.

2. **Payment Gateways**  
   - *Problem*: Multiple payment processors.  
   - *Solution*: Bridge enables switching gateways independently.

3. **Device Controllers**  
   - *Problem*: Multiple devices need control.  
   - *Solution*: Bridge decouples device abstraction from controller logic.

---

## ✅ Flyweight Pattern

### 🔹 Description
Reduces memory usage by sharing common parts of object state between multiple objects.

```java
class Flyweight {
    private final String sharedState;

    public Flyweight(String sharedState) {
        this.sharedState = sharedState;
    }

    public void operation(String uniqueState) {
        System.out.println("Shared: " + sharedState + ", Unique: " + uniqueState);
    }
}
```

### 🔹 Industry Use Cases

1. **Text Editor Fonts**  
   - *Problem*: Characters with same font cause memory bloat.  
   - *Solution*: Flyweight shares font style across characters.

2. **Game Map Tiles**  
   - *Problem*: Terrain tiles with repeated types.  
   - *Solution*: Flyweight reuses tile objects with shared data.

3. **Web Session Optimization**  
   - *Problem*: Store redundant session attributes.  
   - *Solution*: Flyweight stores shared session metadata separately.

---


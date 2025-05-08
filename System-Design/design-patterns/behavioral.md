# Behavioral Design Patterns

## ✅ Chain of Responsibility Pattern

### 🔹 Description
Allows passing a request along a chain of handlers. Each handler can either process the request or pass it on to the next handler in the chain.

```java
interface Handler {
    void handleRequest(int request);
}

class ConcreteHandler1 implements Handler {
    private Handler nextHandler;

    public void setNextHandler(Handler nextHandler) {
        this.nextHandler = nextHandler;
    }

    public void handleRequest(int request) {
        if (request < 10) {
            System.out.println("Handler1 processing request: " + request);
        } else if (nextHandler != null) {
            nextHandler.handleRequest(request);
        }
    }
}

class ConcreteHandler2 implements Handler {
    public void handleRequest(int request) {
        if (request >= 10) {
            System.out.println("Handler2 processing request: " + request);
        }
    }
}
```

### 🔹 Industry Use Cases

1. **Logging Systems**  
   - *Problem*: Need for flexible logging levels.  
   - *Solution*: Chain handlers to handle different logging levels (e.g., debug, error).

2. **Event Handling in GUI Applications**  
   - *Problem*: Multiple components may handle an event.  
   - *Solution*: Chain of responsibility allows multiple components to process the event.

3. **Request Handling in Web Servers**  
   - *Problem*: A request may require different levels of validation.  
   - *Solution*: Chain of responsibility delegates validation tasks to multiple handlers.

---

## ✅ Command Pattern

### 🔹 Description
Encapsulates a request as an object, thereby allowing for parameterization of clients with queues, requests, and operations.

```java

interface Command {
    void execute();
}

class LightOnCommand implements Command {
    private Light light;

    public LightOnCommand(Light light) {
        this.light = light;
    }

    public void execute() {
        light.turnOn();
    }
}

class Light {
    void turnOn() {
        System.out.println("The light is ON");
    }
}

class RemoteControl {
    private Command command;

    public void setCommand(Command command) {
        this.command = command;
    }

    public void pressButton() {
        command.execute();
    }
}
```

### 🔹 Industry Use Cases

1. **Undo/Redo Functionality**  
   - *Problem*: Store actions for undo and redo.  
   - *Solution*: Command pattern stores operations as objects for easy rollback.

2. **Macro Commands**  
   - *Problem*: Combine multiple actions into a single command.  
   - *Solution*: Command allows creation of complex macros.

3. **Request Handling in GUI**  
   - *Problem*: Button press triggers multiple operations.  
   - *Solution*: Command pattern encapsulates operations into command objects.

---

## ✅ Interpreter Pattern

### 🔹 Description
Defines a grammar for the language and uses an interpreter to evaluate sentences in that language.

```java
interface Expression {
    boolean interpret(String context);
}

class TerminalExpression implements Expression {
    private String data;

    public TerminalExpression(String data) {
        this.data = data;
    }

    public boolean interpret(String context) {
        return context.contains(data);
    }
}

class OrExpression implements Expression {
    private Expression expr1, expr2;

    public OrExpression(Expression expr1, Expression expr2) {
        this.expr1 = expr1;
        this.expr2 = expr2;
    }

    public boolean interpret(String context) {
        return expr1.interpret(context) || expr2.interpret(context);
    }
}
```

### 🔹 Industry Use Cases

1. **SQL Query Parsing**  
   - *Problem*: SQL queries need to be parsed and interpreted.  
   - *Solution*: Interpreter pattern can be used to parse and evaluate SQL queries.

2. **Mathematical Expression Evaluation**  
   - *Problem*: Evaluating complex mathematical expressions.  
   - *Solution*: Use Interpreter pattern to parse and evaluate expressions dynamically.

3. **Pattern Matching**  
   - *Problem*: Search for patterns in text.  
   - *Solution*: Interpreter interprets and matches regular expressions in text.

---

## ✅ Iterator Pattern

### 🔹 Description
Provides a way to access the elements of an aggregate object sequentially without exposing its underlying representation.

```java
interface Iterator {
    boolean hasNext();
    Object next();
}

interface Aggregate {
    Iterator createIterator();
}

class ConcreteIterator implements Iterator {
    private List<Object> items;
    private int index = 0;

    public ConcreteIterator(List<Object> items) {
        this.items = items;
    }

    public boolean hasNext() {
        return index < items.size();
    }

    public Object next() {
        return hasNext() ? items.get(index++) : null;
    }
}

class ConcreteAggregate implements Aggregate {
    private List<Object> items = new ArrayList<>();

    public void addItem(Object item) {
        items.add(item);
    }

    public Iterator createIterator() {
        return new ConcreteIterator(items);
    }
}
```
### 🔹 Industry Use Cases

1. **Collections and Data Structures**  
   - *Problem*: Need to iterate through complex data structures.  
   - *Solution*: Iterator allows easy iteration over collections.

2. **Navigation in Complex UI Components**  
   - *Problem*: Need to navigate through a tree of UI components.  
   - *Solution*: Iterator provides sequential access to the UI elements.

3. **Database Records Retrieval**  
   - *Problem*: Need to access records from a database one by one.  
   - *Solution*: Iterator pattern simplifies traversal of database records.

---

## ✅ Mediator Pattern

### 🔹 Description
Defines an object that controls the interactions between a group of objects, preventing direct references between them.

```java
class Mediator {
    private User user1;
    private User user2;

    public Mediator(User user1, User user2) {
        this.user1 = user1;
        this.user2 = user2;
    }

    public void send(String message, User sender) {
        if (sender == user1) {
            user2.receive(message);
        } else {
            user1.receive(message);
        }
    }
}

class User {
    private String name;
    private Mediator mediator;

    public User(String name, Mediator mediator) {
        this.name = name;
        this.mediator = mediator;
    }

    public void send(String message) {
        System.out.println(this.name + " sends: " + message);
        mediator.send(message, this);
    }

    public void receive(String message) {
        System.out.println(this.name + " received: " + message);
    }
}
```

### 🔹 Industry Use Cases

1. **Chat Application**  
   - *Problem*: Users need to communicate without directly linking to each other.  
   - *Solution*: Mediator handles message passing between users.

2. **Workflow Management Systems**  
   - *Problem*: Complex interactions between different components of a workflow.  
   - *Solution*: Mediator coordinates the interactions between various system components.

3. **GUI Event Handling**  
   - *Problem*: Multiple UI components interacting directly.  
   - *Solution*: Mediator pattern handles communication between GUI elements.

---

## ✅ Memento Pattern

### 🔹 Description
Allows capturing and restoring an object's internal state without exposing its implementation.

```java
class Memento {
    private String state;

    public Memento(String state) {
        this.state = state;
    }

    public String getState() {
        return state;
    }
}

class Originator {
    private String state;

    public void setState(String state) {
        this.state = state;
    }

    public String getState() {
        return state;
    }

    public Memento saveState() {
        return new Memento(state);
    }

    public void restoreState(Memento memento) {
        this.state = memento.getState();
    }
}
```

### 🔹 Industry Use Cases

1. **Undo/Redo Functionality**  
   - *Problem*: Track and revert to previous states.  
   - *Solution*: Memento stores the state for undo/redo functionality.

2. **Game Save/Load Systems**  
   - *Problem*: Save and restore game state.  
   - *Solution*: Memento pattern captures the state of a game character.

3. **Version Control**  
   - *Problem*: Track and manage multiple versions of a document.  
   - *Solution*: Memento stores different states of the document for versioning.

---

## ✅ State Pattern

### 🔹 Description
Allows an object to alter its behavior when its internal state changes.

```java
interface State {
    void doAction();
}

class StartState implements State {
    public void doAction() {
        System.out.println("Player is in start state");
    }
}

class StopState implements State {
    public void doAction() {
        System.out.println("Player is in stop state");
    }
}

class Player {
    private State state;

    public void setState(State state) {
        this.state = state;
    }

    public void doAction() {
        state.doAction();
    }
}
```

### 🔹 Industry Use Cases

1. **Game Characters' States**  
   - *Problem*: Characters' behavior changes based on their state (e.g., idle, moving).  
   - *Solution*: State pattern manages transitions between different states.

2. **Workflow Management**  
   - *Problem*: Process workflows with states like pending, approved, or rejected.  
   - *Solution*: State pattern handles state transitions in the workflow.

3. **Order Processing**  
   - *Problem*: Orders go through various states (e.g., created, shipped, delivered).  
   - *Solution*: State pattern models transitions in the order lifecycle.

---

## ✅ Strategy Pattern

### 🔹 Description
Defines a family of algorithms and allows them to be interchangeable.

```java
interface Strategy {
    int execute(int a, int b);
}

class AddStrategy implements Strategy {
    public int execute(int a, int b) {
        return a + b;
    }
}

class SubtractStrategy implements Strategy {
    public int execute(int a, int b) {
        return a - b;
    }
}

class Context {
    private Strategy strategy;

    public Context(Strategy strategy) {
        this.strategy = strategy;
    }

    public int executeStrategy(int a, int b) {
        return strategy.execute(a, b);
    }
}
```

### 🔹 Industry Use Cases

1. **Payment Methods**  
   - *Problem*: Users can pay via different methods (credit card, PayPal, etc.).  
   - *Solution*: Strategy pattern allows dynamic selection of payment methods.

2. **Sorting Algorithms**  
   - *Problem*: Different sorting algorithms depending on the data type.  
   - *Solution*: Strategy selects the appropriate sorting algorithm at runtime.

3. **Tax Calculation**  
   - *Problem*: Different tax calculations based on user type (e.g., regular, corporate).  
   - *Solution*: Strategy allows choosing tax calculation strategy based on user type.

---

## ✅ Template Method Pattern

### 🔹 Description
Defines the skeleton of an algorithm in a method, deferring some steps to subclasses.

```java
abstract class Game {
    abstract void initialize();
    abstract void startPlay();
    abstract void endPlay();

    public final void play() {
        initialize();
        startPlay();
        endPlay();
    }
}

class Cricket extends Game {
    void initialize() {
        System.out.println("Cricket Game Initialized.");
    }
    void startPlay() {
        System.out.println("Cricket Game Started.");
    }
    void endPlay() {
        System.out.println("Cricket Game Ended.");
    }
}
```

### 🔹 Industry Use Cases

1. **Game Framework**  
   - *Problem*: Games have common flow but differ in rules.  
   - *Solution*: Template method defines the game flow, with customizable steps for each game.

2. **Document Generation**  
   - *Problem*: Generating documents with common structure but different content.  
   - *Solution*: Template method defines document layout and allows customization of sections.

3. **Data Processing Pipelines**  
   - *Problem*: Standard processing pipeline with variable steps.  
   - *Solution*: Template method defines processing steps and allows customization.

---

## ✅ Visitor Pattern

### 🔹 Description
Allows adding new operations to existing class hierarchies without modifying their classes.

```java
interface Visitor {
    void visit(Element element);
}

class ConcreteVisitor implements Visitor {
    public void visit(Element element) {
        System.out.println("Visiting " + element.getClass().getSimpleName());
    }
}

abstract class Element {
    abstract void accept(Visitor visitor);
}

class ConcreteElement extends Element {
    void accept(Visitor visitor) {
        visitor.visit(this);
    }
}
```

### 🔹 Industry Use Cases

1. **Document Structure**  
   - *Problem*: Applying operations (like formatting) across a document hierarchy.  
   - *Solution*: Visitor pattern adds operations on document nodes.

2. **Complex Data Models**  
   - *Problem*: Data model evolves, requiring new operations without changing classes.  
   - *Solution*: Visitor allows new operations on data models.

3. **Graph Traversal**  
   - *Problem*: Perform different actions on nodes in a graph.  
   - *Solution*: Visitor pattern separates traversal from actions on nodes.

---

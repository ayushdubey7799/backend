# Spring Core Module

## IoC & Dependency Injection

### Inversion of Control (IoC)
- A design principle where control of object creation and binding is transferred from the application code to a container (like Spring).
- Goal is to decouple classes from their dependencies, improving modularity and testability.

### Dependency Injection (DI)
- A technique to implement IoC by providing dependent objects (beans) from outside the class.
- Spring manages object lifecycles and injects dependencies at runtime.
- Types of Dependency Injection in Spring
    - Constructor Injection (Preferred)
    - Setter Injection: uses setter methods - optional dependencies; more flexible, but allows mutability.
    - Field Injection: Least recommended - Injects directly into fields; Not ideal for testing and immutability; harder to track and refactor.

### Annotations to know
- @Autowired: Tells Spring to inject the dependency automatically.
- @Qualifier("beanName"): Used with @Autowired when multiple beans of the same type exist.
- @Primary: Marks one bean as the default to be injected when multiple candidates exist.


## Bean Lifecycle

### Spring Bean Lifecycle – Key Stages
- Instantiation: Spring creates the bean instance (via constructor or factory method).
- Property Population: Spring injects dependencies (via constructor/setter/field).
- Initialization: Custom logic can be added before the bean is put into service.
- Ready to Use: Bean is now available in the container for use.
- Destruction: When the container is shutting down, beans are cleaned up.

### Key Interfaces (implement and use methods)
- `InitializingBean`
    - Method: afterPropertiesSet()
    - Called after property population.
    - Use for custom initialization logic (though @PostConstruct is preferred).
- `DisposableBean`
    - Method: destroy()
    - Called on container shutdown for cleanup logic.

### Lifecycle Annotations (Preferred over interfaces unless building framework level components)
- `@PostConstruct`: Marks a method to be called after dependency injection but before use.
- `@PreDestroy`: Marks a method to be called before bean is destroyed.
 
### Extras
- `BeanPostProcessor` - postProcessBeforeInitialization() and postProcessAfterInitialization()
    - Hooks into bean lifecycle before and after initialization.
    - Often used for modifying beans or adding proxy behavior (e.g., Spring AOP).
- `BeanFactoryPostProcessor` - postProcessBeanFactory(ConfigurableListableBeanFactory factory) 
    - Invoked before any beans are instantiated, allows modifying bean definitions (not actual beans)
    - Used for configuration-level changes, e.g., in Spring Boot auto-configuration.




4. BeanFactory vs ApplicationContext
BeanFactory: Lightweight, lazy-init, core container

ApplicationContext: Feature-rich, supports events, messages, resource loading

Common types: ClassPathXmlApplicationContext, AnnotationConfigApplicationContext

5. Java-based Configuration
@Configuration + @Bean vs Component scanning

Bean definition control and customization

Conditional beans: @Conditional, @Profile

6. Component Scanning & Stereotype Annotations
@Component, @Service, @Repository, @Controller

@ComponentScan, basePackages, excludeFilters, includeFilters

7. Spring Expression Language (SpEL)
Syntax: #{expression}

Common use: @Value("#{systemProperties['user.home']}")

Used in conditional logic, bean definitions

8. Environment and Property Management
External config: application.properties, application.yml

@Value, @PropertySource, Environment

Profile activation: @Profile("dev")

9. Aware Interfaces
Gain access to Spring internals:

ApplicationContextAware

BeanNameAware

EnvironmentAware

10. Spring Utilities (Core)
Packages: org.springframework.core, org.springframework.util

Key utilities: StringUtils, CollectionUtils, Assert, ObjectUtils

1. `How does Spring implement Inversion of Control (IoC) internally?`
Spring's IoC container manages object creation and wiring. The BeanFactory interface serves as the core container, responsible for instantiating, configuring, and assembling beans. The ApplicationContext extends BeanFactory, adding more enterprise-specific functionality like event propagation and internationalization.

2. `What happens internally when a bean is autowired?`
When a bean is annotated with @Autowired, Spring's dependency injection mechanism resolves the required dependency by type. During the bean creation process, the container identifies the appropriate bean to inject, either by type or by qualifier if specified, and sets it into the dependent bean.

3. `How does Spring choose a constructor when multiple exist?`
Spring uses the following rules to determine which constructor to use:
- If a constructor is annotated with @Autowired, it is given preference.
- If multiple constructors are annotated, Spring will attempt to resolve dependencies for each and choose the most suitable one.
- If no constructor is annotated, Spring will use the default no-argument constructor.

4. `Difference between field injection, setter injection, and constructor injection — and which is preferred?`
Field Injection: Injects dependencies directly into fields using @Autowired. It's concise but makes unit testing harder due to private fields.
Setter Injection: Uses setter methods to inject dependencies. It allows for optional dependencies and is more testable.
Constructor Injection: Injects dependencies through the constructor. It's preferred for mandatory dependencies and promotes immutability.

5. `What is circular dependency and how does Spring handle it?`
A circular dependency occurs when two or more beans depend on each other. Spring can resolve circular dependencies for singleton beans by creating early references. However, for prototype beans, circular dependencies are not resolved automatically and will result in a BeanCurrentlyInCreationException.

6. `Can you explain the full lifecycle of a Spring bean?`
- The lifecycle of a Spring bean involves several steps:
    - Instantiation: The container creates the bean instance.
    - Populate Properties: Dependencies are injected.
    - BeanNameAware and other Aware interfaces:The bean is provided with its name and other context info.
    - BeanPostProcessor pre-initialization methods: Custom modifications before initialization
    - Initialization: Custom init methods or afterPropertiesSet() if InitializingBean is implemented.
    - BeanPostProcessor post-initialization methods: Further custom modifications.
    - Destruction: When the container is closed, destroy() or custom destroy methods are called.


7. `What’s the difference between @PostConstruct, InitializingBean.afterPropertiesSet(), and BeanPostProcessor?`
- @PostConstruct: A method annotated with this is invoked after dependency injection is done.
- InitializingBean.afterPropertiesSet(): A method from the InitializingBean interface, called after properties are set.
- BeanPostProcessor: Allows for custom modification of new bean instances before and after initialization.

8. `How would you run custom logic before and after a bean is fully initialized?`
Implement the BeanPostProcessor interface and override its postProcessBeforeInitialization() and postProcessAfterInitialization() methods to add custom logic before and after bean initialization.

9. `How does BeanPostProcessor differ from BeanFactoryPostProcessor?`
BeanPostProcessor: Operates on bean instances, allowing modification after instantiation.
BeanFactoryPostProcessor: Operates on the bean definitions before any beans are instantiated.

10. `What are the differences between ApplicationContext and BeanFactory?`
- BeanFactory: Basic container providing fundamental functionalities.
- ApplicationContext: Extends BeanFactory with additional features like event propagation, internationalization, and application-layer specific contexts.


11. `When would you prefer one over the other? BeanFactory vs ApplicationContext`
Use BeanFactory for lightweight applications where only basic dependency injection is needed. Prefer ApplicationContext for most applications due to its advanced features.

12. `What is the purpose of AnnotationConfigApplicationContext?`
AnnotationConfigApplicationContext is a standalone application context that accepts annotated classes as input, particularly @Configuration annotated classes, and registers beans defined in them.

13. `How does Spring inject a prototype bean into a singleton?`
Injecting a prototype-scoped bean into a singleton-scoped bean can lead to issues since the prototype bean is created only once during the singleton's initialization. To ensure a new instance each time, you can use method injection or lookup methods.

14. `What strategies are used to manage non-singleton scopes inside singleton beans?`
Use ObjectFactory or Provider to retrieve a new instance of the prototype bean each time it's needed within a singleton bean.

15. `Can you create a custom bean scope? How?`
Yes, by implementing the Scope interface and registering it with the ConfigurableBeanFactory. This allows for defining custom scoping behavior.

16. `What’s the difference between @Component and @Bean?`
@Component: Indicates that a class is a Spring-managed component. It's detected through classpath scanning.
@Bean: Used to declare a bean within a @Configuration class. It allows for more control over bean instantiation.

17. `Can you explain how @ComponentScan works under the hood?`
@ComponentScan instructs Spring to scan specified packages for classes annotated with stereotypes like @Component, @Service, @Repository, and @Controller, and registers them as beans in the application context.

18. `What are the limitations of component scanning?`
Component scanning may inadvertently include unwanted classes or miss classes if not properly configured. It relies on annotations, so classes without the appropriate annotations won't be registered.

19. `What happens when multiple beans of the same type are detected?`
Spring will throw a NoUniqueBeanDefinitionException unless one is marked as @Primary or qualified with @Qualifier to resolve the ambiguity.

20. `What is a BeanDefinition and where is it used?`
A BeanDefinition describes a bean instance, including its properties, constructor arguments, and other configuration metadata. Spring uses it internally to manage bean creation and configuration.


# 🌱 Spring Core Module – Key Classes & Interfaces

## 🧩 Bean Lifecycle & Management

- 🔥 `BeanFactory` – Core interface for accessing a Spring container (basic DI container).
- 🔥 `ApplicationContext` – Advanced container interface with support for events, i18n, etc.
- `ConfigurableApplicationContext` – Extends `ApplicationContext` with lifecycle methods.
- `BeanDefinition` – Metadata abstraction for a bean including scope, class, and dependencies.
- `BeanDefinitionRegistry` – Interface to register `BeanDefinition` programmatically.

---

## 🧪 Bean Initialization & Customization

- 🔥 `BeanPostProcessor` – Modify new bean instances before and after initialization.
- 🔥 `BeanFactoryPostProcessor` – Modify bean definitions before beans are instantiated.
- `InitializingBean` – Defines logic via `afterPropertiesSet()` after dependency injection.
- `DisposableBean` – Allows destruction logic through `destroy()` method.
- `SmartInitializingSingleton` – Callback after all singletons are fully initialized.

---

## ⚙️ Dependency Injection & Autowiring

- 🔥 `@Autowired` – Automatically inject dependencies by type.
- 🔥 `@Qualifier` – Resolve ambiguity when multiple candidates are present.
- 🔥 `@Value` – Inject values from properties or SpEL expressions.
- `ObjectFactory<T>` – Provides lazy access to dependencies (e.g., prototype beans).

---

## 📦 Configuration & Metadata

- 🔥 `@Configuration` – Marks a class as a source of bean definitions.
- 🔥 `@Bean` – Declares a bean inside a `@Configuration` class.
- 🔥 `@ComponentScan` – Enables component scanning for `@Component`, etc.
- 🔥 `@Component` – Declares a class as a Spring-managed component.
- `@Import` – Imports other configuration classes or selectors.
- `@PropertySource` – Loads `.properties` files into the Spring `Environment`.

---

## 🧠 Awareness Interfaces

- `ApplicationContextAware` – Injects `ApplicationContext` into a bean.
- `BeanNameAware` – Provides access to the bean's name.
- `EnvironmentAware` – Injects the active `Environment` instance.
- `ResourceLoaderAware` – Gives access to a `ResourceLoader` for file access.

---

## 📘 Utility & Support Classes

- 🔥 `Environment` – Represents current environment properties and profiles.
- `StandardEnvironment` – Default implementation of `Environment`.
- `DefaultListableBeanFactory` – Most versatile and configurable `BeanFactory`.
- 🔥 `AnnotationConfigApplicationContext` – Context for Java-based configuration.
- `GenericApplicationContext` – Flexible, programmatic Spring context.

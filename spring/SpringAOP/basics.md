# 📌 Spring AOP - In-depth Notes

## 📖 Table of Contents

1. [Introduction to AOP](#introduction-to-aop)
2. [Spring AOP vs AspectJ](#spring-aop-vs-aspectj)
3. [Core Concepts of AOP](#core-concepts-of-aop)
4. [Spring AOP Terminology](#spring-aop-terminology)
5. [Enabling Spring AOP](#enabling-spring-aop)
6. [Defining Aspects Using @Aspect](#defining-aspects-using-aspect)
7. [Types of Advice](#types-of-advice)
8. [Pointcut Expressions](#pointcut-expressions)
9. [Combining Pointcuts](#combining-pointcuts)
10. [Order of Execution](#order-of-execution)
11. [Proxy Mechanism in Spring AOP](#proxy-mechanism-in-spring-aop)
12. [Limitations of Spring AOP](#limitations-of-spring-aop)
13. [Use Cases](#use-cases)
14. [Best Practices](#best-practices)

---

## 📌 Introduction to AOP

- **AOP** stands for *Aspect-Oriented Programming*.
- It allows separation of cross-cutting concerns like logging, security, and transactions.
- Helps in clean modularization of concerns that cut across multiple classes.

---

## 🆚 Spring AOP vs AspectJ

| Feature | Spring AOP | AspectJ |
|--------|------------|---------|
| Proxy-based | Yes | No |
| Compile-time weaving | No | Yes |
| Runtime weaving | Yes (via proxies) | Yes |
| Supports only method execution join points | Yes | No (can intercept field, constructor, etc.) |
| Easier configuration | Yes | No |

---

## 💡 Core Concepts of AOP

- **Aspect**: A module of cross-cutting functionality.
- **Join Point**: A point during execution of the program (method execution, object instantiation, etc.).
- **Advice**: Action taken by an aspect at a join point.
- **Pointcut**: Predicate that matches join points.
- **Weaving**: Linking aspects with other application types or objects to create an advised object.

---

## 🧠 Spring AOP Terminology

- **@Aspect**: Declares a class as an aspect.
- **@Before, @After, @Around**: Advices that run at join points.
- **@Pointcut**: Declares reusable pointcut expressions.

---

## 🔧 Enabling Spring AOP

### Annotation-based

```java
@Configuration
@EnableAspectJAutoProxy
public class AppConfig {
}
```

### XML-based

```xml
<aop:aspectj-autoproxy />
```

---

## 🧩 Defining Aspects Using `@Aspect`

```java
@Aspect
@Component
public class LoggingAspect {

    @Before("execution(* com.example.service.*.*(..))")
    public void logBefore(JoinPoint joinPoint) {
        System.out.println("Before method: " + joinPoint.getSignature().getName());
    }
}
```

---

## 🕹️ Types of Advice

### 1. `@Before`

Runs before method execution.

```java
@Before("execution(* com.example.service.*.*(..))")
public void beforeAdvice() {
    System.out.println("Executing Before Advice");
}
```

### 2. `@After`

Runs after method execution (regardless of outcome).

```java
@After("execution(* com.example.service.*.*(..))")
public void afterAdvice() {
    System.out.println("Executing After Advice");
}
```

### 3. `@AfterReturning`

Runs after method returns successfully.

```java
@AfterReturning(pointcut = "execution(* com.example.service.*.*(..))", returning = "result")
public void afterReturningAdvice(Object result) {
    System.out.println("Returned value: " + result);
}
```

### 4. `@AfterThrowing`

Runs if method throws exception.

```java
@AfterThrowing(pointcut = "execution(* com.example.service.*.*(..))", throwing = "ex")
public void afterThrowingAdvice(Exception ex) {
    System.out.println("Exception: " + ex.getMessage());
}
```

### 5. `@Around`

Wraps method execution, can modify input/output.

```java
@Around("execution(* com.example.service.*.*(..))")
public Object aroundAdvice(ProceedingJoinPoint joinPoint) throws Throwable {
    System.out.println("Before around");
    Object result = joinPoint.proceed();
    System.out.println("After around");
    return result;
}
```

---

## 🎯 Pointcut Expressions

```java
execution([modifiers] return-type fully-qualified-method-name(arguments))
```

### Examples

```java
execution(* com.example.service.UserService.*(..))       // Any method in UserService
execution(* com.example..*Service.find*(..))             // All find* methods in any *Service class
execution(public String com.example.MyClass.get*(..))    // Specific method signature
```

---

## ➕ Combining Pointcuts

```java
@Pointcut("execution(* com.example.service.*.*(..))")
private void serviceLayer() {}

@Pointcut("execution(* com.example.repository.*.*(..))")
private void repositoryLayer() {}

@Before("serviceLayer() || repositoryLayer()")
public void logAll() {
    System.out.println("Called service or repository");
}
```
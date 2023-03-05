# AOP - Aspect-Oriented Programming

## What is AOP?

Before address what is AOP, we need to know what is Cross-cutting concerns first:

-   **Cross-cutting concerns** are concerns or aspects of a software system that affect multiple parts of the system and cannot be cleanly separated from the core business logic of the application. Examples of cross-cutting concerns include logging, security, transaction management, caching, error handling, and performance monitoring.

-   These concerns are "cross-cutting" because they affect multiple modules or layers of an application and cannot be easily encapsulated within a single module or layer. This can lead to code duplication, increased complexity, and maintenance problems.

To solve those problems, we need AOP:

-   **Aspect-oriented programming** (AOP) is a technique used to address cross-cutting concerns by separating them from the core business logic and encapsulating them into separate modules called aspects. By using AOP, developers can isolate cross-cutting concerns and apply them consistently throughout the application without modifying the core business logic.

## How Spring implement AOP?

Internally, AOP in Spring works by intercepting method invocations at runtime and applying additional behavior defined in advice modules. This is done through the use of **dynamic proxies** or bytecode manipulation.

### The flow step implement

When a Spring bean is created, Spring creates a proxy object that wraps the bean. The proxy intercepts method invocations and delegates them to an interceptor chain. The interceptor chain consists of the advice modules that have been applied to the target bean, which are executed in a specific order according to their advice types and pointcut expressions.

The following steps describe how AOP works internally in Spring:

1. Spring creates a proxy object that wraps the target bean.
2. When a method is called on the proxy, the proxy intercepts the method invocation.
3. The proxy then delegates the method invocation to an interceptor chain, which consists of the advice modules that have been applied to the target bean.
4. The advice modules are executed in a specific order according to their advice types and pointcut expressions.
5. The advice modules can modify the method invocation, add additional behavior before or after the method invocation, or prevent the method invocation from being executed.
6. The modified or intercepted method invocation is then returned to the proxy, which returns it to the calling code.

**NOTE:**

-   Advice is something like what should do.
-   Advice and Pointcut Expression will talk later.

### Proxy Object

A proxy object is an object that acts as a stand-in or substitute for another object. In software engineering, a proxy is a design pattern that provides a surrogate or placeholder for another object in order to control access to it or to add additional functionality.

In the context of AOP, a proxy object is typically used to intercept method invocations on a target object and apply advice or interceptors before or after the target method is executed. The proxy object typically implements the same interface as the target object, so that the calling code can use the proxy object as if it were the target object.

### The simple example code in java implement AOP

First, let's define a simple target class:

```java
public class UserService {
    public void save(User user) {
        System.out.println("Saving user: " + user);
    }
}
```

Next, let's define an aspect that adds logging before and after the `save` method:

```java
public class LoggingAspect {
    public void logBeforeSave(User user) {
        System.out.println("Before saving user: " + user);
    }

    public void logAfterSave(User user) {
        System.out.println("After saving user: " + user);
    }
}
```

To apply the logging aspect to the `UserService` class, we need to create a proxy object that wraps the target object and intercepts method invocations. We can do this by defining an interface that declares the methods we want to intercept:

```java
public interface UserServiceProxy {
    void save(User user);
}
```

We can then create a dynamic proxy object that implements the `UserServiceProxy` interface and delegates method invocations to the target object. We can also define an invocation handler that intercepts method invocations and applies the logging aspect:

```java
public class UserServiceProxyHandler implements InvocationHandler {
    private final UserService target;
    private final LoggingAspect aspect;

    public UserServiceProxyHandler(UserService target, LoggingAspect aspect) {
        this.target = target;
        this.aspect = aspect;
    }

    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        // Check if the method is the "save" method
        if (method.getName().equals("save")) {
            // Apply the "logBeforeSave" advice
            aspect.logBeforeSave((User) args[0]);
        }

        // Invoke the target method
        Object result = method.invoke(target, args);

        // Check if the method is the "save" method
        if (method.getName().equals("save")) {
            // Apply the "logAfterSave" advice
            aspect.logAfterSave((User) args[0]);
        }

        // Return the result of the target method invocation
        return result;
    }
}
```

We can then use this code to create a `UserService` object and a `UserServiceProxy` object that applies the logging aspect:

```java
public class Main {
    public static void main(String[] args) {
        UserService target = new UserService();
        LoggingAspect aspect = new LoggingAspect();

        UserServiceProxy proxy = (UserServiceProxy) Proxy.newProxyInstance(
                UserService.class.getClassLoader(),
                new Class[]{UserServiceProxy.class},
                new UserServiceProxyHandler(target, aspect)
        );

        // Call the "save" method on the proxy object
        proxy.save(new User("John"));
    }
}
```

When we run this code, we should see the following output:

```sql
Before saving user: User{name='John'}
Saving user: User{name='John'}
After saving user: User{name='John'}
```

---

# AOP Terminology

I will give both example and define:

```java
public class UserService {
    public void save(User user) {
        System.out.println("Saving user: " + user);
    }
}

public class LoggingAspect {
    @before("execution(* *(..))")
    public void logBeforeSave(User user) {
        System.out.println("Before saving user: " + user);
    }
}
```

1. **Advice**: is the implementation of Cross-cutting Concern
   Advice is method `logBeforeSave`

2. **JoinPoint**: is where the advice will be applied.
   JoinPoint is method `save`

3. **Target**: at Runtime, where is the JoinPoint located
   Target is class `UserService`

4. **PointCut**: a collection of Points describe when the Advice will be executed
   PointCut is `@before`, `@after`

5. **Aspect**: Combination of Advice and PointCut
   Aspect is `LoggingAspect`

---

# What are differences Spring AOP and AspectJ?

The main difference between Spring AOP and AspectJ is in their implementation approach:

-   Spring AOP is a lightweight AOP framework that uses **dynamic proxies** to weave aspects into Spring-managed beans at **runtime**. Spring AOP only supports method-level join points and offers a limited set of advice types, such as before, after, and around advice.

-   AspectJ is a full-featured AOP framework that uses bytecode manipulation to weave aspects into any Java class at **Compile-time**, **Post-compile** or **Load-time**, not just Spring-managed beans. AspectJ supports a rich set of join points, including field access, constructor execution, and exception handling, and offers a wide range of advice types, such as before, after, around, and intro advice.

## A little deep dive understanding about process

AspectJ makes use of three different types of weaving:

1. **Compile-time weaving**: The AspectJ compiler takes as input both the source code of our aspect and our application and produces a woven class files as output
2. **Post-compile weaving**: This is also known as binary weaving. It is used to weave existing class files and JAR files with our aspects
3. **Load-time weaving**: This is exactly like the former binary weaving, with a difference that weaving is postponed until a class loader loads the class files to the JVM

Spring AOP makes use of runtime weaving. Spring AOP is a proxy-based AOP framework. This means that to implement aspects to the target objects, it'll create proxies of that object. This is achieved using either of two ways:

1. **JDK dynamic proxy** – the preferred way for Spring AOP. Whenever the targeted object implements even one interface, then JDK dynamic proxy will be used
2. **CGLIB proxy** – if the target object doesn't implement an interface, then CGLIB proxy can be used

![spring aop process](image/springaop-process.png)

=> AspectJ doesn't do anything at runtime as the classes are compiled directly with aspects. Unlike Spring AOP, it doesn't require any design patterns

## Difference about Joinpoints

**We cannot apply cross-cutting concerns (or aspects) across classes that are “final” because they cannot be overridden and thus it would result in a runtime exception.**

The same applies for static and final methods. Spring aspects cannot be applied to them because they cannot be overridden. Hence Spring AOP because of these limitations, only supports method execution join points.

However, **AspectJ weaves the cross-cutting concerns directly into the actual code before runtime**. Unlike Spring AOP, it doesn't require to subclass the targetted object and thus supports many others joinpoints as well. Following is the summary of supported joinpoints:

| **Joinpoint**                | **Spring AOP Supported** | **AspectJ Supported** |
| ---------------------------- | ------------------------ | --------------------- |
| Method Call                  | No                       | Yes                   |
| **Method Execution**         | **Yes**                  | **Yes**               |
| Constructor Call             | No                       | Yes                   |
| Constructor Execution        | No                       | Yes                   |
| Static initializer execution | No                       | Yes                   |
| Object initialization        | No                       | Yes                   |
| Field reference              | No                       | Yes                   |
| Field assignment             | No                       | Yes                   |
| Handler execution            | No                       | Yes                   |
| Advice execution             | No                       | Yes                   |

## A thing you might be confused

When searching about Spring AOP and AspectJ, you might see a result called "**Spring AOP AspectJ**". It mean "**Spring AOP with AspectJ pointcuts**"

Spring provides simple and powerful ways of writing custom aspects by using either a **schema-based approach** (XML) or the **@AspectJ annotation style** (`@AspectJ` support). Both of these styles offer fully typed advice and use of the AspectJ pointcut language while still using pure Spring AOP for weaving.

The `@AspectJ` support can be enabled with XML- or Java-style configuration. In either case, you also need to ensure that have AspectJ’s `aspectjrt.jar` or `aspectjweaver.jar` library in classpath

**NOTE:** According to description in maven respository:

-   `aspectjrt`: The AspectJ runtime is a small library necessary to run Java programs enhanced by AspectJ aspects during a previous compile-time or post-compile-time (binary weaving) build step.
-   `aspectjweaver`: The AspectJ weaver applies aspects to Java classes. It can be used as a Java agent **in order to apply load-time weaving (LTW)** during class-loading and also **contains the AspectJ runtime classes**.

=> `aspectjweaver` is need for LTW and also include `aspectjrt` inside it, so no need to include `aspectjrt` when have `aspectjweaver`

---

# How to setup AOP project

## With `@AspectJ`support (prefer way)

POM.xml

```xml
<!-- https://mvnrepository.com/artifact/org.aspectj/aspectjweaver -->
<dependency>
    <groupId>org.aspectj</groupId>
    <artifactId>aspectjweaver</artifactId>
    <version>1.9.19</version>
</dependency>
```

SpringConfig.java

```java
@Configuration
@ComponentScan("aop.term")
// Default is JDK proxy (interface-base)
@EnableAspectJAutoProxy
// Force use CGLIB (class-base)
@EnableAspectJAutoProxy(proxyTargetClass = true)
public class SpringConfig {
}
```

You can `EnableAspectJAutoProxy` by using XML

```xml
<aop:aspectj-autoproxy/>
<aop:aspectj-autoproxy proxy-target-class="true"/>
```

LogAspect.java

```java
@Aspect
// Autodetecting aspects through component scanning
// Otherwise, you need declare bean through @Bean or XML
@Component
public class LogAspect {

    @Before("execution(* *(..))")
    public void logBefore(){
        System.out.println("logging before method being executed....");
    }
}
```

## With Schema-base AOP support

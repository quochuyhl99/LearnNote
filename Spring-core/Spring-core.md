# IOC (Inversion of Control) & DI (Dependency Injection)

Spring Core is based on the Inversion of Control (IoC) and Dependency Injection (DI) principles.

## 1. Inversion of Control (IoC)

IOC is a design principle that aims to invert the control flow of a system. Instead of the objects in the system calling the methods of other objects, the control flow is reversed, and objects are passed to the methods of other objects. This means that the objects in the system are not in control of their own behavior, but are instead controlled by an external entity.

## 2. Dependency Injection (DI)

DI is a specific implementation of the IoC principle. It is a technique for achieving IoC by passing objects to the methods of other objects. The objects being passed are called dependencies, and the process of passing them is called injection.

There are 3 main ways to perform Dependency Injection (DI) in Spring:

-   **Constructor-based DI:** This involves injecting dependencies into a bean through its constructor. This is the preferred way of injecting dependencies in Spring and is the most straightforward approach.

-   **Setter-based DI:** This involves injecting dependencies into a bean through its setter methods. This is a less intrusive way of injecting dependencies than constructor-based DI, as it does not require a constructor.

-   **Field/Property-based DI:** This involves injecting dependencies directly into a bean's fields using Reflection API. This is a convenient way of injecting dependencies, but it can lead to a more tightly coupled architecture, as the fields can be accessed directly from other beans.

## 3. Conclusion

Spring Core uses these principles to provide a container for managing the lifecycle and configuration of objects in an application. The container is responsible for instantiating, configuring, and assembling the objects that make up an application, such as beans. It also provides a way for objects to be configured and wired together using DI, allowing for a more flexible and testable architecture.

In summary, Spring Core is built on the principles of Inversion of Control (IoC) and Dependency Injection (DI) which allows for a more flexible and testable architecture and a container for managing the lifecycle and configuration of objects in an application.

---

# Container

In Spring, a container is an essential part of the framework that is responsible for managing the lifecycle of beans and their dependencies. A container is a place where objects (beans) are created, configured, managed, and stored. The container provides the necessary services to manage the beans, such as instantiating, configuring, and wiring the beans together, and resolving dependencies between them.

There are two types of containers in Spring:

## 1. BeanFactory:

-   This is the simplest container in Spring and is responsible for managing the basic lifecycle of beans. It provides basic services such as bean instantiation, configuration, and dependency resolution.

## 2. ApplicationContext:

-   This is an advanced container that extends the BeanFactory and provides additional features, such as event handling, internationalization, and AOP. The ApplicationContext is the preferred container in most Spring-based applications, as it provides a comprehensive set of services for managing beans.

The container gets its instructions on what objects to instantiate, configure, and assemble by reading configuration metadata.

![Digram How Spring Work](https://docs.spring.io/spring-framework/docs/current/reference/html/images/container-magic.png)

## 3. Configuration Metadata

In Spring, configuration metadata refers to the information that is used to configure the beans and their dependencies within the Spring container. This information can be defined in various ways, including XML configuration files, Java-based configuration, and annotations.

### 3.1. XML configuration files:

This is the traditional way of defining configuration metadata in Spring and involves writing XML files that describe the beans and their dependencies. These files can be loaded into the Spring container using the `ClassPathXmlApplicationContext` or `FileSystemXmlApplicationContext` classes.

Worthy to note that we can not only use one XML file but also use multiple XML files to config Container.

Here is a demo of configuration metadata in Spring using XML configuration files:

1. Create a new XML configuration file, such as "applicationContext.xml", and define the beans and their dependencies in the file. For example:

```xml
<beans xmlns="http://www.springframework.org/schema/beans"
   xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
   xsi:schemaLocation="http://www.springframework.org/schema/beans
   http://www.springframework.org/schema/beans/spring-beans.xsd">

   <bean id="messageService" class="com.example.MessageServiceImpl">
      <property name="message" value="Hello World!" />
   </bean>

   <bean id="messagePrinter" class="com.example.MessagePrinter">
      <property name="messageService" ref="messageService" />
   </bean>

</beans>
```

2. Load the XML configuration file into the Spring container using the `ClassPathXmlApplicationContext` or `FileSystemXmlApplicationContext` classes:

```java
ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
```

3. Retrieve the bean from the container and use it:

```java
MessagePrinter printer = context.getBean("messagePrinter", MessagePrinter.class);
printer.printMessage();
```

### 3.2. Java-based configuration:

This involves using Java classes to define the configuration metadata, rather than XML files. The configuration metadata is defined using annotations, such as `@Configuration`, `@Bean`, and `@Autowired`, and can be loaded into the Spring container using the `AnnotationConfigApplicationContext` class.

Here is a demo of configuration metadata in Spring using Java-based configuration:

1. Create a new Java configuration class, such as "AppConfig.java", and define the beans and their dependencies in the class using annotations. For example:

```java
@Configuration
public class AppConfig {

   @Bean
   public MessageService messageService() {
      return new MessageServiceImpl("Hello World!");
   }

   @Bean
   public MessagePrinter messagePrinter() {
      MessagePrinter printer = new MessagePrinter();
      printer.setMessageService(messageService());
      return printer;
   }
}
```

2. Load the Java configuration class into the Spring container using the AnnotationConfigApplicationContext class:

```java
ApplicationContext context = new AnnotationConfigApplicationContext(AppConfig.class);
```

3. Retrieve the bean from the container and use it:

```java
MessagePrinter printer = context.getBean(MessagePrinter.class);
printer.printMessage();
```

### 3.3. Annotations:

This involves using annotations directly on the beans to define the configuration metadata. Common annotations include `@Component`, `@Service`, `@Repository`, and `@Controller`, which are used to indicate that a class is a candidate for component scanning and can be managed by the Spring container.

Here is a demo of configuration metadata in Spring using annotations:

1. Annotate classes with relevant annotations, such as @Component, @Service, @Repository, or @Controller, to indicate that they are candidates for component scanning and can be managed by the Spring container. For example:

```java
@Service
public class MessageServiceImpl implements MessageService {

   private String message;

   public MessageServiceImpl(String message) {
      this.message = message;
   }

   public String getMessage() {
      return message;
   }
}
```

```java
package com.example;

// Default Bean name is "messagePrinter" (Lower case first letter)
@Component
// You can explicit give a name to bean
@Component("randomName")
public class MessagePrinter {

   private MessageService messageService;

   public void setMessageService(MessageService messageService) {
      this.messageService = messageService;
   }

   public void printMessage() {
      System.out.println(messageService.getMessage());
   }
}
```

2. Enable component scanning in the configuration class, such as "AppConfig.java", using the `@ComponentScan` annotation:

```java
@Configuration
//value in ComponentScan is base-package where Component placed
@ComponentScan("com.example")
// for multi place
//@ComponentScan({"com.example1", "com.example2"})
public class AppConfig {

}
```

or using XML:

```XML
<beans xmlns="http://www.springframework.org/schema/beans"
   xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
   xmlns:context="http://www.springframework.org/schema/context"
   xsi:schemaLocation="http://www.springframework.org/schema/beans
   http://www.springframework.org/schema/beans/spring-beans.xsd
   http://www.springframework.org/schema/context
   http://www.springframework.org/schema/context/spring-context.xsd">

   <context:component-scan base-package="com.example"/>

   <!-- Multi package -->
   <context:component-scan base-package="com.example1;com.example2"/>

</beans>
```

The XML namespace for the context schema is also declared using the `xmlns:context` attribute.

3. Load the Java configuration class or XML into the Spring container

4. Retrieve the bean from the container and use it

---

# Bean Scope

The "bean scope" in Spring Core refers to the lifecycle and number of instances of a bean that the Spring container creates and manages. The Spring Framework supports six scopes, four of which are available only if you use a web-aware `ApplicationContext`

1. **Singleton**: Only one instance of the bean is created and shared throughout each Spring IoC container (BeanFactory/ApplicationContext).

2. **Prototype**: A new instance of the bean is created each time it is requested.

3. **Request**: One instance of the bean is created for each HTTP request. Only valid in the context of a web-aware Spring `ApplicationContext`.

4. **Session**: One instance of the bean is created for each user session. Only valid in the context of a web-aware Spring `ApplicationContext`.

5. **Application**:

    - One instance of the bean is created for a `ServletContext`. Only valid in the context of a web-aware Spring `ApplicationContext`.
    - In one web appliction, we can have multiple `ApplicationContext` but only one `ServletContext`.
    - The **Application** scope is similar to the **Singleton** scope, with the exception that it creates a single instance of the bean that is shared by all servlets and JSPs in `ServletContext`

6. **Websocket**: One instance of the bean is created for a `Websocket`. Only valid in the context of a web-aware Spring `ApplicationContext`.

By default, beans in Spring are singleton scoped. The scope of a bean can be specified using the `scope` attribute in the bean definition.

We also can create a **Custom** scope.

## 1. Singleton and Prototype

We can use XML or java-based to change scope

**XML-based:**

```xml
<!-- Default scope is "Singleton" -->
<bean id="accountService" class="com.something.DefaultAccountService"/>

<!-- the following is equivalent, though redundant (singleton scope is the default) -->
<bean id="accountService" class="com.something.DefaultAccountService" scope="singleton"/>
```

**Java-based:**

```java
@Configuration
public class AppConfig {

   @Bean
   @Scope("prototype")
   public MessageService messageService() {
      return new MessageServiceImpl("Hello World!");
   }

   // default is "Singleton"
   @Bean
   public MessagePrinter messagePrinter() {
      MessagePrinter printer = new MessagePrinter();
      printer.setMessageService(messageService());
      return printer;
   }
}
```

**Annotation-based:**

```java
package com.example;

@Component
// Default is "singleton"
@Scope("prototype")
public class MessagePrinter {

   private MessageService messageService;

   public void setMessageService(MessageService messageService) {
      this.messageService = messageService;
   }

   public void printMessage() {
      System.out.println(messageService.getMessage());
   }
}
```

## 2. Request, Session, Application, and WebSocket Scopes (web-scoped beans)

Will learn later in Spring MVC

---

# @Component and Further Stereotype Annotations

![@Component Diagram](image/%40Component.PNG)

`@Component` is a stereotype annotation in Spring that is used to indicate that a class is a component that can be managed by the Spring container. The class is automatically registered as a bean with the container and can be used for dependency injection.

There are several other stereotype annotations in Spring that are based on the `@Component` annotation:

## 1. `@Service`:

Indicates that the annotated class is a service component that provides business logic.

Service classes are typically used to implement business logic in a Spring-based application. They provide a way to encapsulate and isolate the business logic from other components, such as controllers, which handle incoming HTTP requests and present data to the user.

## 2. `@Repository`:

Indicates that the annotated class is a repository component that provides data access, responsible for managing data persistence.

Repository classes are typically used to implement data access logic in a Spring-based application. They provide a way to encapsulate and isolate the data access logic from other components

## 3. `@Controller`:

Indicates that the annotated class is a controller component that provides the web tier with a way to interact with the business logic.

`@Controller` is a specialized stereotype annotation that is used to indicate that a class serves as a controller in a web application. The annotated class handles incoming HTTP requests, processes user input, and returns appropriate responses.

The `@Controller` annotation can be used in combination with other annotations, such as "@RequestMapping", to define the specific HTTP requests that a controller handles, and to map those requests to specific methods within the controller class.

**CONCLUSION**: These annotations are used as a shorthand way of indicating the role of a class in an application and help to organize and categorize the components in a Spring application. The specific annotation used is not important, as they are all equivalent to the `@Component` annotation and have the same effect.

---

# Spring Bean Lifecycle

The Spring Bean Lifecycle refers to the various stages that a bean goes through from its creation to its destruction within a Spring application context. The lifecycle of a bean is managed by the Spring container and typically includes the following phases:

1. Bean instantiation
2. Dependency injection
3. Bean post-processing
4. Bean ready for use
5. Bean destruction.

Each phase in the bean lifecycle is controlled by specific methods or hooks in the Spring framework, allowing developers to customize the behavior of their beans at various points in their lifecycle. This can include tasks such as configuring the bean's properties and dependencies, initializing resources, and cleaning up resources when the bean is destroyed.

I divide Spring Bean Lifecycle into 2 big state.

## 1. Creation

**XML config**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/context
       http://www.springframework.org/schema/context/spring-context.xsd">

    <context:component-scan base-package="org.example" />
<!--    <bean id="customer" class="org.example.Customer" init-method="customInit">-->
<!--        <constructor-arg name="name" value="Teo" />-->
<!--        <property name="name" value="Huy" />-->
<!--    </bean>-->

<!--    <bean id="customBeanPostProcessor" class="org.example.CustomBeanPostProcessor" />-->
</beans>
```

**CustomBeanPostProcessor**

```java
@Component
public class CustomBeanPostProcessor implements BeanPostProcessor {
    @Override
    public Object postProcessBeforeInitialization(Object bean, String beanName) throws BeansException {
        System.out.println("postProcessBeforeInitialization");
        return BeanPostProcessor.super.postProcessBeforeInitialization(bean, beanName);
    }

    @Override
    public Object postProcessAfterInitialization(Object bean, String beanName) throws BeansException {
        System.out.println("postProcessAfterInitialization");
        return BeanPostProcessor.super.postProcessAfterInitialization(bean, beanName);
    }
}
```

**Customer**

```java
public class Customer implements BeanNameAware, BeanFactoryAware, ApplicationContextAware, InitializingBean {

    private String name;

    public Customer() {
        System.out.println("Customer default constructor");
    }

    public Customer(String name) {
        System.out.println("Customer name constructor " + name);
        this.name = name;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        System.out.println("Customer set name " + name);
        this.name = name;
    }

    @Override
    public void setBeanName(String name) {
        System.out.println("name of bean is: " + name);
    }

    @Override
    public void setBeanFactory(BeanFactory beanFactory) throws BeansException {
        System.out.println("Is singleton: " + beanFactory.isSingleton("customer"));
    }

    @Override
    public void setApplicationContext(ApplicationContext applicationContext) throws BeansException {
        System.out.println("Is prototype: " + applicationContext.isPrototype("customer"));
    }

    @Override
    public void afterPropertiesSet() throws Exception {
        System.out.println("in afterPropertiesSet()");
    }
}
```

Creation Lifecycle go in order:

**1. Instantiate:** call default constructor. If have custom constructor, then call it instead of default (XML).
**2. Populate property:** set property by call setter (XML)
**3. Aware-Interface:** have 3 common interface:

-   BeanNameAware: call method `void setBeanName(String name)`
-   BeanFactoryAware: call method `void setBeanFactory(BeanFactory beanFactory)`
-   ApplicationContextAware: call method `setApplicationContext(ApplicationContext applicationContext)`

**4. BeanPostProcessor:** pre-initalization
**5. InitializingBean interface:** afterPropertySet()
**6. Custom init method (best solution):** 2 way:

-   If use XML: InitializingBean -> Custom init method
-   If use `@PostConstruct`: Custom init -> InitializingBean method

**7. BeanPostProcessor:** post-initalization 8. Bean is ready to use

**NOTE:**

-   In java 9 and later, `@PostConstruct` in other package, need dependency **javax.annotation-api**
-   `BeanPostProcessor` is like global, it effect all beans in the same IOC container

## 2. Destruction

**XML config**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/context
       http://www.springframework.org/schema/context/spring-context.xsd">

    <context:component-scan base-package="destruction" />
<!--        <bean id="customer" class="destruction.Customer" destroy-method="customDestroy"></bean>-->
</beans>
```

**Customer**

```java
@Component
//@Scope("prototype") Destruction not work on Prototype scope
public class Customer implements DisposableBean {
    @Override
    public void destroy() throws Exception {
        System.out.println("DisposableBean destroy");
    }

    @PreDestroy
    public void customDestroy() {
        System.out.println("customDestroy");
    }
}
```

Although the initialization lifecycle callback methods are called on all objects regardless of their scope, the destruction lifecycle callback methods are only called for singleton-scoped beans in Spring, not called for prototype-scopeed beans.

This is because the container does not keep track of individual instances of prototype-scoped beans and does not manage their lifecycle in the same way it does for singleton-scoped beans.

Destruction Lifecycle is:

1. IOC container shutdown
2. DisposableBean interface: method destroy()
3. Custom destroy method (best solution): 2 way:
    - If use XML: DisposableBean -> Custom destroy method
    - If use `@PreDestroy`: Custom destroy method -> DisposableBean

**NOTE:**

-   In java 9 and later, `@PreDestroy` in other package, need dependency **javax.annotation-api**

---

# Autowiring

Spring Bean Autowiring is the process of automatically wiring beans together by looking at the properties and constructors of a bean and matching it with other beans that have been defined in the application context. There are several types of autowiring in Spring:

1. **no**: No autowiring. Bean properties must be set manually.This is default in XML
2. **byName**: Autowiring by property name. Spring looks for a bean with the same name as a property. Using **Setter**
3. **byType**: Autowiring by property type. Spring looks for a bean of the same type as a property.This is default in Java Configuration. Using **Setter**
4. **constructor**: Autowiring by constructor argument type. Spring looks for beans of the same type as the constructor arguments.
    - First: autowire constructor argument **byType**
    - If autowire **byType** fail, then autowire constructor argument **byName**
    - If autowire **byName** fail, then autowire constructor argument **no (default constructor)**
5. **autodetect (deprecated)**: First tries to autowire by constructor, then by type if constructor autowiring fails.

You can specify the autowiring mode for a bean using the autowire attribute of the `<bean>` definition in XML configuration, or by using the `@Autowired` annotation in Java configuration. The default is **no** autowiring.

---

# @Autowired Annotation

The `@Autowired` annotation in Spring is used to automatically wire a bean to its dependencies. It can be used on property, setter methods, and constructors. When a bean is annotated with `@Autowired`, Spring will attempt to find a bean of the same type in the application context and wire it to the annotated property or constructor argument. If multiple beans of the same type are found, Spring will use one of them, depending on the autowiring mode specified (e.g. byType, byName, etc.). Default is byType.

1. **property**: spring use Reflection API to inject dependency. This way is not recommend.
2. **setter**
3. **constructors (best)**

---

# @Qualifier vs @Primary

`@Qualifier` and `@Primary` are both annotations in Spring used to specify which bean should be used when there are multiple beans of the same type in the application context.

`@Qualifier` is used to specify the exact bean that should be used by providing its unique identifier. For example:

```java
@Autowired
@Qualifier("exampleBean1")
private ExampleBean exampleBean;
```

Here, the `@Qualifier("exampleBean1")` annotation tells Spring to use the `ExampleBean` bean with the identifier "exampleBean1" when wiring the `exampleBean` property.

`@Primary` is used to specify a default bean to use when multiple beans of the same type are found in the application context. The `@Primary` annotation can be placed on a single bean definition, and that bean will be used as the primary candidate for autowiring. For example:

```java
@Configuration
public class ExampleConfig {

  @Bean
  @Primary
  public ExampleBean exampleBean1() {
    return new ExampleBean("1");
  }

  @Bean
  public ExampleBean exampleBean2() {
    return new ExampleBean("2");
  }
}
```

In this example, if a bean property of type `ExampleBean` is annotated with `@Autowired`, Spring will autowire the `exampleBean1` bean because it has the `@Primary` annotation and is the default candidate for autowiring. If a property is annotated with `@Qualifier("exampleBean2")`, the `exampleBean2` bean will be used instead.

---

# @Component vs @Bean

1. **@Component:**

    - Apply on class, bean name is class name with lowercase first letter.
    - Indicate that a class is a candidate for component scanning.
    - Mapping one-to-one between beanName and bean. It means only one beanName match to one bean in container

2. **@Bean:**
    - Apply on method, bean name is method name.
    - Used in conjunction with the @Configuration annotation to create beans in a Java-based configuration.

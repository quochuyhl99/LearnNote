# Front Controller

The Front Controller design pattern is a software architectural pattern that provides a centralized point of control for handling requests from multiple clients. It acts as a single entry point for all requests, routing them to the appropriate request handler and coordinating the response.

The purpose of the Front Controller is to provide a common interface for managing request handling and processing, allowing for a unified and consistent user experience. It can be used to implement security and other cross-cutting concerns, such as logging and request validation, in a modular and maintainable manner.

In a Java-based web application, the Front Controller pattern is often implemented using a servlet, only one servlet **(DispatcherServlet)**

How **DispatcherServlet** work (simple)?

-   It's based on 2 Map
    -   Map<URL, Controller>: Find what POJO mapping with URL
    -   Map<URL, Method>: Find what method in POJO mapping with URL

![Front Controller](/image//FrontController.PNG)

---

# Major Components

1. **DispatcherServlet**: The DispatcherServlet is the central component in Spring MVC that dispatches incoming requests to appropriate handlers, typically controllers. It acts as the front controller and is responsible for delegating requests to appropriate handlers, preparing models and views, and triggering view rendering.
2. **Model**: The Model is a central component in Spring MVC that holds the data to be displayed on the view. It is an abstract representation of the data, typically containing data obtained from a database or other data source. The Model is passed from the controller to the view, and can be accessed by the view to display the data.
3. **View**: The View is responsible for rendering the data contained in the model in a user-friendly format. It can be implemented as a JSP, a template engine, or a custom component, and is responsible for rendering the HTML that is returned to the user's browser.
4. **Controller**: The Controller is the component that handles user requests and updates the model. It receives incoming requests, processes the data, and returns a view name to the DispatcherServlet. Controllers are typically annotated with @Controller, and can handle requests for specific URL patterns.
5. **HandlerMapping**: The HandlerMapping is a component that maps requests to controllers. It is responsible for determining which controller should handle a given request based on the URL and other information in the request. There are several implementations of HandlerMapping available in Spring, including BeanNameUrlHandlerMapping, SimpleUrlHandlerMapping, and more.
6. **ViewResolver**: The ViewResolver is responsible for resolving the view name returned by the controller into an actual view. It takes the view name and maps it to a specific view, such as a JSP or a template engine. ViewResolvers are configured in the DispatcherServlet and can be used to return views in different formats, such as HTML or JSON.
7. **Form Validation**: Spring MVC provides support for form validation out-of-the-box. This includes support for server-side validation using the JSR-303 Bean Validation API, as well as client-side validation using JavaScript. Form validation is typically performed in the controller, and the errors are added to the model and displayed on the view.
8. **Exception Handling**: Exception handling is an important aspect of any web application. Spring MVC provides support for exception handling through the @ExceptionHandler annotation. This allows developers to define custom handlers for specific exceptions, and to return specific error pages or messages to the user in the event of an error.

**Basic WorkFlow:**

1. The user makes a request to the web server.
2. The DispatcherServlet, which acts as the front controller, receives the request.
3. The DispatcherServlet uses the HandlerMapping to determine the appropriate controller for the request.
4. The selected controller processes the request and updates the model with the necessary data.
5. The controller returns a view name to the DispatcherServlet.
6. The DispatcherServlet uses the ViewResolver to determine the appropriate view for the request.
7. The selected view, typically implemented as a JSP or a template engine, is rendered using the data in the model.
8. The HTML response is sent back to the user's browser.
9. If there is a need for form validation, the controller performs validation on the user's input and adds any errors to the model.
10. If any exceptions occur during the processing of the request, they are handled by the exception handler and an appropriate error message or error page is displayed to the user.

---

# Setup by XML

**web.xml**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee
                      http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">

  <!-- Define Front Controller: DispatcherServlet  -->
  <!-- This is the only Servlet in entire application -->
  <servlet>
    <servlet-name>DispatcherServlet</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <init-param>
      <!-- This is Spring config-->
      <!-- But in SpringMVC api, it's actually called Servlet Application Context-->
      <param-name>contextConfigLocation</param-name>
      <param-value>WEB-INF/config/springmvc-config.xml</param-value>
    </init-param>
  </servlet>

  <servlet-mapping>
    <servlet-name>DispatcherServlet</servlet-name>
    <url-pattern>/</url-pattern>
  </servlet-mapping>
</web-app>
```

**spring-config.xml**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/context
       http://www.springframework.org/schema/context/spring-context.xsd
       http://www.springframework.org/schema/mvc
       http://www.springframework.org/schema/mvc/spring-mvc.xsd">

    <!-- enable scan component -->
    <context:component-scan base-package="springmvc.example" />
    <!-- enable mvc annotation to working -->
    <mvc:annotation-driven/>

    <!-- ViewResolver, resolve viewName to find actual view -->
    <bean id="viewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="prefix" value="/WEB-INF/views/" />
        <property name="suffix" value=".jsp" />
    </bean>

</beans>
```

---

# Root Application Context vs Servlet Application Context

## 1. Root Application Context

The root application context is a shared context created by the **ContextLoaderListener** and it contains the beans that are not specific to any servlet. These beans can be used by multiple servlets and are often used for services and DAOs.

**Example**:
Consider a scenario where you want to configure a data source to be used by multiple servlets. You would create a bean for the data source in the root application context as follows:

```xml
<bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
   <property name="driverClassName" value="com.mysql.jdbc.Driver"/>
   <property name="url" value="jdbc:mysql://localhost:3306/testdb"/>
   <property name="username" value="root"/>
   <property name="password" value="password"/>
</bean>
```

Load it by using **ContextLoaderListener**

```xml
<web-app>
   <listener>
      <listener-class>
         org.springframework.web.context.ContextLoaderListener
      </listener-class>
   </listener>

   <context-param>
      <param-name>contextConfigLocation</param-name>
      <param-value>/WEB-INF/applicationContext.xml</param-value>
   </context-param>

   <!-- other web configuration goes here -->

</web-app>
```

## 2. Servlet Application Context

The servlet application context is created by the DispatcherServlet and it contains the beans that are specific to a servlet. These beans are often used for controllers, view resolvers, and exception resolvers.

---

# Setup by Java

Create **DispatcherServlet** (web.xml)

```java
public class DispatcherServletInit extends AbstractAnnotationConfigDispatcherServletInitializer {
    @Override
    protected Class<?>[] getRootConfigClasses() {
        return null;
        // if have
        // return new Class[]{RootConfig.class};
    }

    @Override
    protected Class<?>[] getServletConfigClasses() {
        return new Class[]{WebApplicationConfig.class};
    }

    @Override
    protected String[] getServletMappings() {
        return new String[]{"/"};
    }
}
```

Create **SpringConfig/Servlet Application Context** (spring-config.xml)

```java
@Configuration
@EnableWebMvc
@ComponentScan("springmvc.example")
public class WebApplicationConfig implements WebMvcConfigurer {

    @Bean
    public InternalResourceViewResolver internalResourceViewResolver() {
        InternalResourceViewResolver viewResolver = new InternalResourceViewResolver();
        viewResolver.setPrefix("/WEB-INF/views/");
        viewResolver.setSuffix(".jsp");
        return viewResolver;
    }
}
```

If have **Root Application Context**

```java
@Configuration
public class RootConfig {
   // Add configuration for root application context
}
```

---

# Declarative Routing Annotations

**@RequestMapping, @GetMapping, @PostMapping**

@RequestMapping(value="/", RequestMethod.GET) = @GetMapping
@RequestMapping(value="/", RequestMethod.POST) = @PostMapping

The difference is:

-   @RequestMapping can be used on both **Class and Method**
-   @GetMapping, @PostMapping can be only used on **Method**

---

# Controller Method Arguments & Return Type

This section summarizes all things need to know about Controller method, which is include what can use in Method Arguments and what Type can those methods return.

## Method Arguments

1. Model / ModelMap
   Note: `ModelMap = Model.asMap()`
2. @RequestParam
3. @PathVariable
4. @ModelAttribute
5. HttpSesstion / HttpServletRequest
6. @SessionAttributes <- Class Level, SesstionStatus
   Note: `@SessionAttributes` use specific for a controller, which mean we can't not directly get SessionAttributes if using model, we can get through @SessionAttribute or HttpServletRequest / httpSession
7. RedirectAttributes
8. @CookieValue
9. BidingResult
10. DomainModel
11. @RequestBody

## Return Type

1. String: return jsp name, can have "/forward:" or "/redirect:"
2. Model & ModelMap (don't understand but this is not important)
3. ModelAndView: = ModelMap + view name (String)
4. @ResponseBody
5. Void
6. ResponseEntity<>

---

# @RequestParam

```java
//  /product/listProducts?categoryId=1&categoryName=computer
//  /product/listProducts?categoryId=1 still ok if have required = false
@GetMapping("/listProducts")
public String getProductByCategory(
        @RequestParam String categoryId,
        @RequestParam(value = "categoryName", required = false, defaultValue = "laptop") String name)
```

Compare to HttpServletRequest:

-   **HttpServletRequest**: All param recieved is **String** type
-   **@RequestParam**: can cast to other type

```java
public String getProductByCategory(
            // categoryId is Integer, so type is int
            // Spring auto cast from string to int
            // if can't cast => exception
            @RequestParam int categoryId)
```

Code example using **HttpServletRequest**:

```java
@GetMapping("/listProducts")
public String getProductByCategory(HttpServletRequest req) {
    String categoryId = req.getParameter("categoryId");
    String name = req.getParameter("categoryName");
    System.out.println(categoryId + " " + name);
    return "welcome";
}
```

---

# @PathVariable

```java
    // /product/list/categoryId/55/categoryName/ipad
    @GetMapping("/list/categoryId/{cateid}/categoryName/{catename}")
    public String getProduct(
            @PathVariable int cateid,
            @PathVariable("catename") String name
    ) {
        System.out.println(cateid + " " + name);
        return "welcome";
    }
```

Note: unlike **@RequestParam**, **@PathVariable** is required

---

# Data binding

**Problem**:
We can also use @RequestParam in GET/POST request to retrieve info from input form. But what happend if the form have too many input?

**Solution**:

-   Use Model. A model is just a regular class
-   Data biding: When info which user input from the Form send to Fields/Properties of Model through **Setter** of Properties
-   This is also called Automatically Data Binding when param name / input name is identical with fields / properties name of Model

**Example**:

**Model User**:

```java
package model;

public class User {
    private String firstName;
    private String lastName;

    public User() {
        System.out.println("Default Constructor");
    }

    public User(String firstName, String lastName) {
        System.out.println("Custom Constructor");
        this.firstName = firstName;
        this.lastName = lastName;
    }

    public String getFirstName() {
        return firstName;
    }

    public void setFirstName(String firstName) {
        System.out.println("Setter firstName");
        this.firstName = firstName;
    }

    public String getLastName() {
        return lastName;
    }

    public void setLastName(String lastName) {
        System.out.println("Setter lastName");
        this.lastName = lastName;
    }
}
```

**JSP**:

```html
<body>
    <form action="signup" method="post">
        <p>First name: <input name="firstName" /></p>
        <p>Last name: <input name="lastName" /></p>
        <input type="submit" value="Sign up" />
    </form>
</body>
```

**Controller**:

```java
    @PostMapping("/signup")
    public String saveUser(User user) {
        System.out.println(user.getFirstName() + " " + user.getLastName());
        return "result";
    }
```

In the example above, note that:

-   The name in input tag is identical with properties name
-   Data binding also can automatically convert String to int, double etc if input is those type

**Advance data binding**:
Consider scenario User has object address which contains street and zipcode

```java
public class User {
    private String firstName;
    private String lastName;

    private Address address;

    // getter and setter
```

```java
public class Address {
    private String street;
    private int zipcode;

    // getter and setter
```

If we want data binding point to `address.street` or `address.zipcode` then JSP form must like this. Look at the name of input

```html
<body>
    <form action="signup" method="post">
        <p>First name: <input name="firstName" /></p>
        <p>Last name: <input name="lastName" /></p>
        <input type="submit" value="Sign up" />

        <p>Address:</p>
        <p>Street: <input name="address.street" /></p>
        <p>Zipcode: <input name="address.zipcode" /></p>
    </form>
</body>
```

---

# Model Interface

The **`Model`** interface in Spring MVC is part of the Spring Web MVC framework and is used to pass data from a controller to a view. It is an interface that extends the **`Map`** interface and provides a convenient way to add attributes to the model, which can then be used in the view to render the response.

In a Spring MVC application, a controller method can add attributes to the model using the **`addAttribute`** method:

```java
@Controller
public class ExampleController {
   @RequestMapping(value = "/example", method = RequestMethod.GET)
   public String getExampleData(Model model) {
      model.addAttribute("message", "Hello World");
      return "exampleView";
   }
}
```

In the above example, the **`message`** attribute is added to the model with the value "Hello World". This attribute can then be accessed in the view (exampleView.jsp) using the JSP expression language (EL):

```html
<h1>${message}</h1>
```

**NOTE**:

-   `model.addAttribute("message", "Hello World");`
    equals to
    `request.setAttribute("message", "Hello World")`

-   As mention above, **`Model`** is a **`Map`**

---

# @ModalAttribute

**`@ModalAttribute`** is used in 2 places:

## 1. Method argument:

If use in Method argument, that **ModelAttribute** is only exist in the request call that method

1. First, it will try to retrieve the infomation from the request and put it in Object argument (**Data binding** that we learn above).
2. If the **Data binding** process is not happen (wrong key, not a form...), then it will create a new instance of that object and put into the **`Model`** at the end of method, before return.

```java
public class productController() {
    @GetMapping
    public String getProductForm(
        @ModelAttribute Product product,
        // or @ModelAttribute("keyhere") Product product,
        Model model
    ) {
        // If not data binding, that code above equals to this
        // model.addAttribute("product", new Product());
        return "productForm";
    }
}
```

## 2. Method:

If use in Method, that **ModelAttribute** is exist in every request call to that controller

```java
public class productController() {

    @ModelAttribute("listProduct")
    public List<Product> getListProduct() {
        return productService.getListProduct();
    }

    @GetMapping
    public String getProductForm() {
        // That code above will exist in every method like this
        // model.addAttribute("listProduct", productService.getListProduct());
        return "productForm";
    }
}
```

---

# Custom Data Binding Formatter

Custom data binding formatters in Spring MVC are used to format and parse objects into strings, and vice-versa, in a way that is customized for a specific type of object.

A custom formatter can be implemented by creating a class that implements the **`Formatter`** interface, which defines two methods: **`parse`** and **`print`**:

-   The **`parse`** method is used to convert a string representation of an object into an actual object
-   the **`print`** method is used to convert an object into a string representation.

**Example**:

**Model**:

```java
public class Customer {
    private String firstName;
    private String lastName;
    private Phone phone;

    // getter and setter
}

public class Phone {
    private String areaCode;
    private String prefix;
    private String number;

    // getter and setter
}
```

**Controller**:

```java
@Controller
public class CustomerController {
    @GetMapping("/getCustomerForm")
    public String getCustomerForm() {
        return "customerForm";
    }

    @PostMapping("/saveCustomer")
    public String saveCustomer(Customer customer) {
        return "customerDetail";
    }
}
```

**Form**:

```html
<form action="saveCustomer" method="post">
    <p>First name: <input name="firstName" /></p>
    <p>Last name: <input name="lastName" /></p>
    <p>Phone: <input name="phone" /></p>
    <input type="submit" value="save" />
</form>
```

In above example, clearly that **Data binding** can't bind the input `name="phone"` type of `String` to property `phone` type of `Phone` (can't convert from String to Phone and vice versa, can't convert from `phone` to `String` to print output to jsp)
So using Formatter.

**Formatter**

```java
public class PhoneFormatter implements Formatter<Phone> {
    @Override
    public Phone parse(String text, Locale locale) throws ParseException {
        // text="123-456-1111"
        String[] s = text.split("-");
        Phone phone = new Phone();
        phone.setPrefix(s[0]);
        phone.setNumber(s[1]);
        phone.setAreaCode(s[2]);
        return phone;

    }

    @Override
    public String print(Phone object, Locale locale) {
        return object.getPrefix + "-" + object.getNumber + "-" + object.getAreaCode;
    }
}
```

We need to do one more thing to make Spring recognize this **Formatter**. There are 2 ways

1. Java Config:

```java
@Configuration
@EnableWebMvc
@ComponentScan("springmvc.example")
public class WebApplicationConfig implements WebMvcConfigurer {

    @Bean
    public InternalResourceViewResolver internalResourceViewResolver() {
        InternalResourceViewResolver viewResolver = new InternalResourceViewResolver();
        viewResolver.setPrefix("/WEB-INF/views/");
        viewResolver.setSuffix(".jsp");
        return viewResolver;
    }

    // Add formatter
    @Override
    public void addFormatters(FormatterRegistry registry) {
        registry.addFormatter(new PhoneFormatter());
    }
}
```

2. XMl config:

```xml
<mvc:annotation-driven conversion-service="conversionService"/>

<bean id="conversionService"
      class="org.springframework.format.support.FormattingConversionServiceFactoryBean">
    <property name="formatters">
        <set>
            <bean class="springmvc.example.formatter.PhoneFormatter"></bean>
            <!-- other formmater bean continue here -->
        </set>
    </property>
</bean>
```

---

# Spring Form Tag Library

-   **Command Object**: also know as DTO or a **@ModelAttribute** argument

-   All others tags are used in side (inner) the `form` tag
-   The `form` tag provide the **Command Object** so inner tags can bind value to property of Command Object
-   The `input` Tag: type="text" by default, can use other type
-   Only `checkbox` is special because it can hold multiple values. So it can have 3 types: `List/Array`, `object` and `boolean`
    -   Only `boolean` property can omit `value` attribute

**Example**
The example below demonstrate all ways to use all tag lib.

```html
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%@ taglib prefix="form" uri="http://www.springframework.org/tags/form" %>
<html>
<head>
    <title>Customer form</title>
</head>
<body>
<form:form action="saveCustomer" method="post" modelAttribute="customer">
    <form:label path="firstName">First Name:</form:label>
    <form:input path="firstName"/>
    <form:label path="lastName">Last Name:</form:label>
    <form:input path="lastName"/>
    <p>Phone: <form:input path="phone"/></p>

    <p>Select country:
        <%-- 1. List: value is same as label --%>
        <form:select path="country.id" items="${countryList}" />
        <br>
        <%-- 2. Map: value is key, label is value --%>
        <form:select path="country.id" items="${countryMap}" />
        <br>
        <%-- 3. List<Object>: value is key, label is value --%>
        <form:select path="country.id" items="${countryObjectList}" itemLabel="name" itemValue="id"/>
        <br>
        <form:select path="country.id">
            <form:option value="--Select one--"/>
        <%-- Has 3 way like above --%>
            <form:options items="${countryMap}" />
        </form:select>
    </p>

    <p>Check box:
        <%-- Approach 1: Property is of type java.lang.Boolean --%>
        <form:checkbox path="booleanVariable"/>

        <%-- Approach 2: Property is of an array or of type java.util.Collection --%>
        <form:checkbox path="stringArrayVariable" value="China"/>
        <form:checkbox path="stringArrayVariable" value="United States"/>
        <form:checkbox path="stringArrayVariable" value="Vietnam"/>

        <%-- 1. List: value is same as label --%>
        <form:checkboxes path="stringArrayVariable" items="${countryList}"/>
        <br>
        <%-- 2. Map: value is key, label is value --%>
        <form:checkboxes path="stringArrayVariable" items="${countryMap}"/>
        <br>
        <%-- 3. List<Object>: value is key, label is value --%>
        <form:checkboxes path="stringArrayVariable" items="${countryObjectList}" itemLabel="name" itemValue="id"/>
    </p>

    <p>Radio:
        <form:radiobutton path="booleanVariable" value="true"/>

            <%-- 1. List: value is same as label --%>
        <form:radiobuttons path="country.id" items="${countryList}"/>
        <br>
            <%-- 2. Map: value is key, label is value --%>
        <form:radiobuttons path="country.id" items="${countryMap}"/>
        <br>
            <%-- 3. List<Object>: value is key, label is value --%>
        <form:radiobuttons path="country.id" items="${countryObjectList}" itemLabel="name" itemValue="id"/>
    </p>

  <input type="submit" value="save"/>
</form:form>
</body>
</html>
```

---

# Forward and Redirect

See in note Servlet&JSP about this Forward and Redirect.

How to use in SpringMVC? Instead return viewName, we return like this:

-   Forward: `return "forward:/link";`
-   Redirect: `return "redirect:/link";`

**NOTE:**
In Spring, by default, if you use `model.addAttribute` to set attribute which are primitive, not object, and then `Redirect`, attributes are automatically appended to URL params

```java
model.addAttribute("name", "huy"); // name=huy
model.addAttribute("phone", 123456); // phone=123456
model.addAttribute("phone", address); // ignore because object
model.addAttribute("huy"); // string=huy
model.addAttribute(12.0); // double=12.0
```

To turn off this behavior, config this:
XML:

```xml
<mvc:annotation-driven ignore-default-model-on-redirect="true">
```

JAVA:

```java
// updating
```

---

# Model Scoped Attribute

## 1. JSP page

```html
// both HTTP Servlet and Spring MVC declare Page scope attribute like this
<c:set var="name" value="huy" />
```

## 2. Request

```java
// HTTP Servlet
request.setAttribute("key", "value");

// SpringMVC
model.addAttribute("key", "value")
```

## 3. Session

How to set:

```java
// HTTP Servlet
session.setAttribute("key", "value");

// SpringMVC
// Step 1: applied on Controller class annotation
@SessionAttributes({"key"})
// Step 2: key must be same
model.addAttribute("key", "value")
```

How to remove:

```java
// HTTP Servlet
session.removeAttribute("key");

// SpringMVC
@GetMapping("/")
String setSessionAttribute(SessionStatus sessionStatus) {
    // Only remove all Attributes set by @SessionAttributes
    // Only remove after the page was rendered
    sessionStatus.setComplete();
    return "session-scope-attribute";
}
```

**NOTE:**
`@SessionAttributes` is designed only for current Controller, it is not recommend for using it to share SessionAttributes among Controllers.

## 4. Application

```java
// HTTP Servlet
request.getServletContext.setAttribute("key", "value");

// SpringMVC
// Method 1: use @Autowired on Servlet context and set like HTTP Servlet
@Autowired
ServletContext servletContext;

servletContext.setAttribute("key", "value");

// Method 2: use Controller Advice
// need to update
```

---

# Handle Static Resource

Static Resource: js, css, image, pdf, html...
For those static resources, we don't need a Controller to hanlde it like JSP. Spring MVC provide another way to handle that.

## Solution 1:

Enable `default-servlet-handler`

By default, Spring MVC tries to handle every incoming request, and this can cause issues when trying to serve static resources efficiently. By enabling the default-servlet-handler configuration element, Spring MVC delegates the handling of static resource requests to the servlet container's default servlet, which is typically optimized for serving static content.

XML:

```xml
<mvc:default-servlet-handler />
```

Java:

```java
@Override
public void configureDefaultServletHandling(DefaultServletHandlerConfigurer configurer) {
    configurer.enable();
}
```

## Solution 2:

Use `resource mapping`

XML:

```xml
<mvc:resources mapping="/assets/*" location="/resources/" />
```

JAVA:

```java
//<mvc:resources mapping="/abc/**" location="/resources/" />
@Override
public void addResourceHandlers(final ResourceHandlerRegistry registry) {
    registry.addResourceHandler("/abc/**").addResourceLocations("/resources/");
}
```

**NOTE:**

Universally speaking asterisks (in wildcard role) mean

-   `/welcome*` : anything in THIS folder or URL section, that starts with "/welcome" and ends before next "/" like /welcomePage.

-   `/welcome**` : any URL, that starts with "/welcome" including sub-folders and sub-sections of URL pattern like /welcome/section2/section3/ or /welcomePage/index.

-   `/welcome/*` : any file, folder or section inside welcome (before next "/") like /welcome/index.

-   `/welcome/**` : any files, folders, sections, sub-folders or sub-sections inside welcome.

In other words one asterisk \* ends before next "/", two asterisks \*\* have no limits.

---

# Flash Attributes

The `RedirectAttributes` is a class used to pass data between 2 requests when using the `redirect:` prefix in the controller method. There have 2 ways to do this:

1. `addAttribute()`: stores the attribute in the URL query string. This is the default action in SpringMVC. To disable it, config this in the xml:

```xml
<mvc:annotation-driven ignore-default-model-on-redirect="true"/>
```

2. `addFlashAttribute()`: stores the attribute temporarily in the session, and then automatically removes it after the next request.

**EXAMPLE:**

```java
redirectAttributes.addFlashAttribute("name", "teo123");

Product p = new Product();
redirectAttributes.addFlashAttribute(p); // key: "product"
```

**NOTE:**
If you want to get the Flash Attribute after redirect, use `Model.asMap()`

```java
@GetMapping("/")
public String index(RedirectAttributes redirectAttributes) {
    redirectAttributes.addFlashAttribute("name", "teo123");
    return "redirect:/url1";
}

@GetMapping("/url1")
public String url1(Model model) {
    String name = (String) model.asMap().get("name");
    System.out.println(name);
    return "index";
}
```

---

# Validation In Spring

1. Spring Validator Interface
2. Bean Validation

## Spring Validator Interface

-   Spring Validator is a validation framework provided by the Spring framework. It allows you to define custom validation logic using Java code or XML configuration

## Bean Validation

-   Bean Validation is a validation framework provided by the Java EE specification. It is a standardized API for declarative validation of Java beans.

-   Have multiple implementations. The popular one is Hibernate. If you are using java 8, use 6.0 series of the Hibernate

## Step by step to implement Bean Validation

**1. Get the dependency.**

-   If you're using spring 6 (jakarta), use

```xml
<!-- https://mvnrepository.com/artifact/org.hibernate.validator/hibernate-validator -->
<dependency>
    <groupId>org.hibernate.validator</groupId>
    <artifactId>hibernate-validator</artifactId>
    <version>8.0.0.Final</version>
</dependency>
```

-   If you are using spring 5 (javax), find the compatible with it or add Javax.validation API dependency

```xml
<!-- https://mvnrepository.com/artifact/javax.validation/validation-api -->
<dependency>
    <groupId>javax.validation</groupId>
    <artifactId>validation-api</artifactId>
    <version>2.0.1.Final</version>
</dependency>
```

**2. Add constrains to Model**
**3. Add method arguments to controller**

```java
    @PostMapping("/add")
    public String postUserForm(@Valid @ModelAttribute("user") User user, BindingResult result, RedirectAttributes redirectAttributes) {
        if (result.hasErrors()) {
            return "forward:add";
        }
        redirectAttributes.addFlashAttribute(user);
        return "redirect:userDetails";
    }
```

**NOTE:** The rule of order: **ModelAtribute** is place before **BindingResults** and nothing else between them

**4. add errors tag to jsp**

```html
<!-- use cssClass to style customize errors message -->
<!-- the Asterisk(*) is show all errors of all paths -->
<form:errors path="*" cssClass="errors" />

<!-- errors for specify path -->
<p>User name: <form:input path="name" /></p>
<form:errors path="name" cssClass="errors" />
```

## Externalized Error Message

1. Give key to attribute message of validate annotation

```java
@Size(min = 2, max = 10, message="{abc}")
private String name;

@NotNull(message="{notnull.age}")
private int age;

@NotBlank(message="{address.street}")
private String street;
```

2. Make a properties file to hold key and message

```properties
abc=Size of name must between 2 and 30
notnull.age=Age must be greater than 18
addr.street=Street cannot be blank
```

-   You can make message more dynamic with **PlaceHolder**

```properties
abc=Size of {0} must between {2} and {1}
notnull.age={0} must be greater than 18
addr.street={0} cannot be blank
```

{0} : field name
{1, 2...n} : attribute in annotation, the order number is sort by name. Example: min = 2, max = 10, max < min in order so {2} is min and {1} is max

-   You can customize field name {0} by use the key same as field name

```properties
abc=Size of {0} must between {2} and {1}
notnull.age={0} must be greater than 18
address.street={0} cannot be blank

name=User Name
age=Age
address.street= Address Street
```

-   For typeMismatch (often convert String to Number error), there has 2 ways to customize it

```properties
// give the general exception
typeMismatch.int={0} must be a number

// specify field name, higher priority compare to general
typeMismatch.zipcode=Zipcode must be a number
```

3. Config Spring recognize that properties file

**XML WAY:**

```xml
<mvc:annotation-driven validator="validator"/>

<bean id="messageSource" class="org.springframework.context.support.ReloadableResourceBundleMessageSource">
    <property name="basename" value="classpath:errorsMessage" />
</bean>

<!-- or use ResourceBundleMessageSource-->
<bean id="messageSource" class="org.springframework.context.support.ResourceBundleMessageSource">
    <!-- value don't have classpath -->
    <property name="basename" value="errorsMessage" />
    <!-- setting multi properties files -->
    <property name="basenames" value="errorsMessage, message" />
</bean>

<bean id="validator" class="org.springframework.validation.beanvalidation.LocalValidatorFactoryBean">
    <property name="validationMessageSource" ref="messageSource" />
</bean>
```

**JAVA WAY:**

```java
@Bean
public MessageSource messageSource() {
    ResourceBundleMessageSource resource = new ResourceBundleMessageSource();
    resource.setBasenames("errorsMessage", "message");
    return resource;
}

@Bean
public LocalValidatorFactoryBean validator() {
    LocalValidatorFactoryBean factory = new LocalValidatorFactoryBean();
    factory.setValidationMessageSource(messageSource());
    return factory;
}

@Override
public Validator getValidator() {
    return validator();
}

```

4. For Internationalization, you can use Spring tag to use message in JSP

```properties
form.name=User Name
form.age=Age
```

```html
<h1><spring:message code="form.name" /></h1>
<h1><spring:message code="form.age" /></h1>
<!-- Text attribute is a fallback/default text if can't found key in properties -->
<h1><spring:message code="form.email" text="my email" /></h1>
```

---

# Internationalization & Localization

Internationalization and Localization in Spring MVC refers to the ability of the framework to display content in different languages and formats based on the user's locale settings. Spring MVC provides a number of features to support this functionality.

1. **MessageSource**: Spring's MessageSource is an interface that provides a way to retrieve localized messages in the application. It can be implemented in various ways, such as using properties files, database, or XML files. The messages can then be retrieved in the code using the getMessage() method of the MessageSource.

2. **LocaleResolver**: The LocaleResolver is responsible for resolving the user's locale. Spring provides several built-in implementations, such as `AcceptHeaderLocaleResolver`, `SessionLocaleResolver`, and `CookieLocaleResolver`. The `AcceptHeaderLocaleResolver` uses the Accept-Language header of the HTTP request to determine the user's locale. The `SessionLocaleResolver` stores the user's locale in the session, while the `CookieLocaleResolver` stores it in a cookie.

3. **Interceptors**: Interceptors can be used to change the user's locale based on certain criteria, such as a user preference or the URL. Spring provides a `LocaleChangeInterceptor` that can be used to change the locale based on a request parameter or a path variable.

Summary:

-   Use Interceptor to config
-   Append `?lang=XXX` after the URL when switch language
-   Have 3 base:
    -   1. Brwoser: the requets header have "accept-language" attribute. Use it to determine language
    -   2. Session: language is keeped in a session
    -   3. Cookie: language is keeped until cookie expire

**EXAMPLE**

1. Not hard coding, externalize message by using `spring:message` taglib:

```html
<%@ page contentType="text/html;charset=UTF-8" language="java" %> <%@ taglib
prefix="spring" uri="http://www.springframework.org/tags" %>
<html>
    <head>
        <title>
            <spring:message code="userform.title" text="Default Title"/>
        </title>
    </head>
    <body>
        <p>
            <a href="?language=en_US">English</a>|<a href="?language=zh_CN"
                >Chinese</a
            >
        </p>
        <h1><spring:message code="userform.title" text="Default Title" /></h1>
        <form>
            <p>
                <label
                    ><spring:message
                        code="userform.username"
                        text="Default Username: " />
                    <input name="username"
                /></label>
            </p>
            <p>
                <label
                    ><spring:message
                        code="userform.password"
                        text="Default Password: " />
                    <input name="password"
                /></label>
            </p>
            <input type="submit" value="<spring:message
                code="userform.signup"
                text="Default Password: "
            />" />
        </form>
    </body>
</html>
```

2. Make properties file to hold message.

```properties
#messages.properties
userform.title=Add New User
userform.username=User Name:
userform.password=Password:

#messages_zh_CN.properties
userform.title=新成员
userform.username=\u7528\u6237\u540D\uFF1A
userform.password=\u5BC6\u7801\uFF1A
```

3. Config to work

**XML**

```xml
// Config make Spring recogize properties file
// id is fixed, if changed then it won't work
// value of basename is name of properties file
<bean id="messageSource" class="org.springframework.context.support.ResourceBundleMessageSource">
    <property name="basename" value="messages"/>
</bean>

// Use interceptor to do internationalize and localize
// value of paramName is param append in URL
<mvc:interceptors>
    <bean class="org.springframework.web.servlet.i18n.LocaleChangeInterceptor">
        <property name="paramName" value="language"></property>
    </bean>
</mvc:interceptors>

// choose one base to resolve locale
<bean id="localeResolver" class="org.springframework.web.servlet.i18n.SessionLocaleResolver">
    <property name="defaultLocale" value="en_US"/>
</bean>

<!--    <bean class="org.springframework.web.servlet.i18n.CookieLocaleResolver">-->
<!--        -->
<!--    </bean>-->
```

**JAVA WAY**

```java
 @Bean
public MessageSource messageSource() {
    ResourceBundleMessageSource resource = new ResourceBundleMessageSource();
    resource.setBasename("messages");
    return resource;
}

@Override
public void addInterceptors(InterceptorRegistry registry) {
    LocaleChangeInterceptor localeChangeInteceptor = new LocaleChangeInterceptor();
    localeChangeInteceptor.setParamName("language");
    registry.addInterceptor(localeChangeInteceptor);
}


@Bean
public CookieLocaleResolver localeResolver(){
    CookieLocaleResolver localeResolver = new CookieLocaleResolver();
    localeResolver.setDefaultLocale(Locale.ENGLISH);
    localeResolver.setCookieName("my-locale-cookie");
    localeResolver.setCookieMaxAge(3600);
    return localeResolver;
}
```

---

# Interceptor in Spring MVC

Interceptors in Spring MVC are used to perform operations before or after a request is processed. They are used to perform common tasks such as logging, authorization, and setting up the environment for the request.

Interceptors are implemented using the **`HandlerInterceptor`** interface, which has three methods:

**1. `preHandle()`** - This method is called before the request is handled by the controller method. It returns a boolean value to indicate whether the request should be further processed or not.

**2. `postHandle()`** - This method is called after the request is handled by the controller method, but before the view is rendered. It allows for modification of the ModelAndView object before it is passed to the view for rendering.

**3. `afterCompletion()`** - This method is called after the view has been rendered. It is typically used for cleanup tasks.

Interceptors can be configured using XML or Java-based configuration. They can be registered globally for all requests, or for specific paths or URL patterns.

#### Interceptor vs filter

Interceptors and filters are both used to intercept and manipulate incoming HTTP requests and outgoing responses.

-   Filters operate at the **servlet** level and intercept requests before they are processed by the **servlet** (**DispatcherServlet** in SpringMVC).

-   Interceptors operate at the **Spring MVC** level and intercept requests **after** they have been processed by the DispatcherServlet but **before** they are handled by the controller

**EXAMPLE:**

-   DemoInterceptor.java

```java
public class DemoInterceptor implements HandlerInterceptor {
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        long startTime = System.currentTimeMillis();
        request.setAttribute("startTime", startTime);
        return true;
    }

    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
        long endTime = System.currentTimeMillis();
        long startTime = (long) request.getAttribute("startTime");
        long duration = endTime - startTime;
        System.out.println("request " + request.getRequestURI() + " duration: " + duration);
        assert modelAndView != null : "modelAndView is null";
        modelAndView.addObject("duration", duration);
    }
}
```

-   spring-config.xml

```xml
<mvc:interceptors>
    <mvc:interceptor>
        <mvc:mapping path="/**"/>
        <bean class="com.example.interceptor.DemoInterceptor"/>
    </mvc:interceptor>
</mvc:interceptors>
```

-   java config

```java
@Override
public void addInterceptors(InterceptorRegistry registry) {
    registry.addInterceptor(new  ProcessingTimeLogInterceptor());
}
```

---

# Upload files

This section just instruct how to upload a file:

-   POM.xml

```xml
<!-- https://mvnrepository.com/artifact/commons-fileupload/commons-fileupload -->
<dependency>
    <groupId>commons-fileupload</groupId>
    <artifactId>commons-fileupload</artifactId>
    <version>1.5</version>
</dependency>
```

-   spring-config.xml

```xml
<bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver"/>
```

-   Model

```java
//User.java
private MultipartFile avatar;

public MultipartFile getAvatar() {
    return avatar;
}

public void setAvatar(MultipartFile avatar) {
    this.avatar = avatar;
}
```

-   Form.jsp

```html
<!-- use enctype="multipart/form-data"-->
<form:form
    action="${url}"
    method="post"
    modelAttribute="user"
    enctype="multipart/form-data"
>
    <p>Avatar: <form:input path="avatar" type="file" /></p
></form:form>
```

-   Controller

```java
// In the PostMapping
// Save the image to the resource folder in server side
MultipartFile multipartFile = user.getAvatar();
if (multipartFile != null || !multipartFile.isEmpty()) {
    String fileName = servletContext.getRealPath("/") + "resources\\images\\" + multipartFile.getOriginalFilename();
    // System.out.println("fileName" + fileName);
    try {
        multipartFile.transferTo(new File(fileName));
    } catch (IOException e) {
        throw new RuntimeException(e);
    }
}
```

-   Success.jsp

```html
<!-- make sure you enable default servlet or config Resource handle in spring-config.xml -->
<img src="<spring:url
    value="/assets/images/${user.avatar.originalFilename}"
/>">
```

---

# Exception Handling

## I. @ResponseStatus

**`@ResponseStatus`** is a Spring annotation that can be used to indicate the HTTP status code to be returned to the client when a particular exception is thrown by a controller method.

### 1. Apply on method

```java
@DeleteMapping("/{id}")
@ResponseStatus(HttpStatus.NO_CONTENT)
public void deleteResource(@PathVariable Long id) {
    // delete the resource with the given ID
}
```

The **`@ResponseStatus`** annotation specifies that if the method completes successfully (i.e., the resource is successfully deleted), a 204 No Content status code should be returned to the client (more detailed than default 200)

### 2. Combine with others annotations

Can combine with these:

-   @ExceptionHandler
-   @ControllerAdvice
-   Apply on custom Exception class

```java
// try with and without this annotation, access method throw exception to see how it work
@ResponseStatus(value=HttpStatus.NOT_FOUND, reason="user not found")
public class UserNotFoundException extends RuntimeException {
    // ...
}
```

## II. @ExceptionHandler

Only apply on method

```java
@Controller
public class UserController {

    @GetMapping("/getUser")
    public String getUserById(@RequestParam int id) {
        throw new UserNotFoundException("cannot found user");
    }

    @ExceptionHandler(UserNotFoundException.class)
    public String handleException(UserNotFoundException e, Model model) {
        model.addAttribute("errorMessage", e.getMessage())
        return "error";
    }
}
```

This exception handler only work on the controller where it palced. Same Exception in others controller can't invoke that method.

## III. @ControllerAdvice

-   Can apply only on Class
-   You can think it is like **Interceptor** but in annotation
-   This annotation behaves Globally, it mean it appears on all Controllers. All annotations we learned above is specified to a Controller its defined, but if combine with **`@ControllerAdvice`**, it work on all controller
-   In class have **`@ControllerAdvice`**, you can have these annotation
    -   @InitBinder
    -   @ModelAttribute
    -   @ExceptionHandler + @ResponseStatus

```java
// applied to all controller
@ControllerAdvice
// You can narrow down the package to applied
// @ControllerAdvice(basePackages={"com.example.controller"})
public class GlobalExceptionHandler {

    // This attribute will avaiable in all Request
    @ModelAttribute("channelName")
    public String channelName() {
        return "miss xing";
    }

    // This Exception will avaiable in all Controller
    @ExceptionHandler(UserNotFoundException.class)
    public String handleUserNotFoundException(UserNotFoundException e, Model model) {
        model.addAttribute("msg", e.getMessage());
        return "error";
    }

    @ExceptionHandler(Exception.class)
    public String exceptionHandler(Exception e, Model model) {
        model.addAttribute("msg", e.getMessage());
        return "error";
    }
}
```

---

# View Resolver

There are several view resolvers available in Spring MVC. Here are some of the most commonly used ones:

1. **InternalResourceViewResolver**: It resolves views to JSP pages in the web application.

2. **UrlBasedViewResolver**: It resolves views based on the URL pattern. It can be used for resolving views to JSP pages, Velocity templates, or other view technologies.

3. **ResourceBundleViewResolver**: It resolves views based on a ResourceBundle, which contains view names mapped to view classes.

4. **XmlViewResolver**: It resolves views based on an XML file that maps view names to view classes.

5. **FreeMarkerViewResolver**: It resolves views to FreeMarker templates.

6. **TilesViewResolver**: It resolves views to Apache Tiles definition files, which can define the layout and structure of a view.

7. **ThymeleafViewResolver**: use for Thymeleaf
8. **ContenegotiatingViewResolver**: PDF/XML/JSON/Excel

In one MVC project, you can have multiple ViewResolver and set order property to decide which one will go in action first

**NOTE**: give **InternalResourceViewResolver** lowest priority, because only InternalResourceViewResolver will return if it can't resolve, other ViewResolver will find next one if exist

**EXAMPLE config for thymeleaf and jsp**

```xml
<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
    <property name="prefix" value="/WEB-INF/jsp/"/>
<!--        <property name="suffix" value=".jsp"/>-->
    <property name="viewNames" value="*.jsp"/>
    <property name="order" value="1"/>
</bean>

<bean id="templateResolver" class="org.thymeleaf.spring5.templateresolver.SpringResourceTemplateResolver">
    <property name="prefix" value="/WEB-INF/thymeleaf/"/>
<!--        <property name="suffix" value=".html"/>-->
</bean>

<bean id="templateEngine" class="org.thymeleaf.spring5.SpringTemplateEngine">
    <property name="templateResolver" ref="templateResolver"/>
</bean>

<bean class="org.thymeleaf.spring5.view.ThymeleafViewResolver">
    <property name="templateEngine" ref="templateEngine"/>
    <property name="viewNames" value="*.html"/>
    <property name="order" value="0"/>
</bean>
```

## ContenegotiatingViewResolver

**`ContentNegotiatingViewResolver`** is a view resolver that is used to resolve views based on the type of content that is being requested by the client, and returns the appropriate view that can render the response in the format required by the client.

For example, if the client requests content in XML format, the **`ContentNegotiatingViewResolver`** will resolve a view that can render the response in XML format. Similarly, if the client requests content in JSON format, the **`ContentNegotiatingViewResolver`** will resolve a view that can render the response in JSON format.

### Example for json

POM.xml

```xml
<!-- https://mvnrepository.com/artifact/com.fasterxml.jackson.core/jackson-databind -->
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-databind</artifactId>
    <version>2.10.0</version>
</dependency>
```

SpringConfig.java

```java
@Configuration
@EnableWebMvc
@ComponentScan(basePackages = {"xing.rujuan"})
public class WebApplicationConfig implements WebMvcConfigurer {
    @Bean
    public ViewResolver getInternalResourceViewResolver() {
        InternalResourceViewResolver internalResourceViewResolver = new InternalResourceViewResolver();
        internalResourceViewResolver.setPrefix("/WEB-INF/jsp/");
        internalResourceViewResolver.setSuffix(".jsp");
        return internalResourceViewResolver;
    }

    // just for pretty print
    @Bean
    public MappingJackson2JsonView jsonView(){
        MappingJackson2JsonView jsonView = new MappingJackson2JsonView();
        jsonView.setPrettyPrint(true);
        return jsonView;
    }

    @Bean
    public ViewResolver contentNegotiatingViewResolver(ContentNegotiationManager manager){
        ContentNegotiatingViewResolver viewResolver = new ContentNegotiatingViewResolver();
        viewResolver.setContentNegotiationManager(manager);
        List<View> views = new ArrayList<>();
        views.add(jsonView());
        // new if don't declare bean above
        // views.add(new MappingJackson2JsonView());
        viewResolver.setDefaultViews(views);
        return viewResolver;
    }
}
```

![Json View](/image/JsonView.PNG)

It will turn **`all model attributes`** to json if we add `.json` after the URL

### Example for XML

POM.xml

```xml
<!-- https://mvnrepository.com/artifact/com.fasterxml.jackson.core/jackson-databind -->
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-databind</artifactId>
    <version>2.10.0</version>
</dependency>

<!-- https://mvnrepository.com/artifact/com.fasterxml.jackson.dataformat/jackson-dataformat-xml -->
<dependency>
    <groupId>com.fasterxml.jackson.dataformat</groupId>
    <artifactId>jackson-dataformat-xml</artifactId>
    <version>2.10.0</version>
</dependency>
```

SpringConfig.java

```java
@Bean
public ViewResolver contentNegotiatingViewResolver(ContentNegotiationManager manager){
    ContentNegotiatingViewResolver viewResolver = new ContentNegotiatingViewResolver();
    viewResolver.setContentNegotiationManager(manager);
    List<View> views = new ArrayList<>();
    // change from json to xml
    views.add(new MappingJackson2XmlView());
    viewResolver.setDefaultViews(views);
    return viewResolver;
    }
```

-   XML only support 1 attribute in model
-   It will turn only 1 attribute to xml if we add `.xml` after the URL

### XML config version

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:p="http://www.springframework.org/schema/p"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="
        http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/mvc
        http://www.springframework.org/schema/mvc/spring-mvc.xsd
        http://www.springframework.org/schema/context
        http://www.springframework.org/schema/context/spring-context.xsd">

    <context:component-scan base-package="xing.rujuan"/>
    <mvc:annotation-driven/>

    <bean id="viewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="prefix" value="/WEB-INF/jsp/"/>
        <property name="suffix" value=".jsp"/>
    </bean>

    <bean class="org.springframework.web.servlet.view.ContentNegotiatingViewResolver">
        <property name="defaultViews">
            <list>
                <ref bean="jsonView"/>
                <ref bean="xmlView"/>
            </list>
        </property>
    </bean>

    <bean id="jsonView" class="org.springframework.web.servlet.view.json.MappingJackson2JsonView">
        <property name="prettyPrint" value="true"/>
    </bean>

    <bean id="xmlView" class="org.springframework.web.servlet.view.xml.MappingJackson2XmlView">
    </bean>

</beans>
```

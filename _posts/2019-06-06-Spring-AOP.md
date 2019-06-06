---
title:  "Spring AOP"
tags: Spring, AOP
---

### Day 2: Spring boot Web Evolved - AOP

<br /><br />

## Recording every HTTP request

### Step 1: Add AOP dependencies

Spring-boot-starter-aop

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-aop</artifactId>
</dependency>
```

### Step 2: Create a class and @Aspect @Component

@Before methods will be executed before the corresponding methods.

```java
@Before("execution(public * com.imooc.controller.GirlController.girlList(..))")
public  void log(){
    System.out.println(1111111);
}
```

If we want to execute before any method, simply change the method name to a "*".

```java
@Before("execution(public * com.imooc.controller.GirlController.*(..))")
public  void log(){
    System.out.println(1111111);
}
```

Similarly, @After methods will be executed after that specified method.

We can also declare a pointcut method.

```java
@Aspect
@Component
public class HttpAspect {

    private final static Logger logger = LoggerFactory.getLogger(HttpAspect.class);

@Pointcut("execution(public * com.imooc.controller.GirlController.*(..))")
    public void log() {
    }

    @Before("log()")
    public void doBefore(JoinPoint joinPoint) {
        ServletRequestAttributes attributes = (ServletRequestAttributes) RequestContextHolder.getRequestAttributes();
        HttpServletRequest request = attributes.getRequest();

        //url
        logger.info("url={}", request.getRequestURL());

        //method
        logger.info("method={}", request.getMethod());

        //ip
        logger.info("ip={}", request.getRemoteAddr());

        //类方法
        logger.info("class_method={}", joinPoint.getSignature().getDeclaringTypeName() + "." + joinPoint.getSignature().getName());

        //参数
        logger.info("args={}", joinPoint.getArgs());
    }

    @After("log()")
    public void doAfter() {
        logger.info("222222222222");
    }

    @AfterReturning(returning = "object", pointcut = "log()")
    public void doAfterReturning(Object object) {
        logger.info("response={}", object.toString());
    }
}
```

Notice that we imported a class org.slf4j.Logger. This is a logger framework of Spring.
Use logger.info() to print information log, use logger.error() to print error message.

* The parameter should be the class type of the current class, HttpAspect.class. 
`private final static Logger logger = LoggerFactory.getLogger(HttpAspect.class);`

### Step 3: Record HTTP request information

From the above code, we can see that a ServletRequestAttributes object is injected. This object is used for 
getting the Http request.

* Use curly braces to pass in a parameter. `logger.info("ip={}", request.getRemoteAddr());`

### Returning object

In the log, we can also record the returning object by @AfterReturning

```java
@AfterReturning(returning = "object", pointcut = "log()")
    public void doAfterReturning(Object object) {
        logger.info("response={}", object.toString());
    }
```

This will print the json object that is returned.
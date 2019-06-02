---
title:  "Spring boot configuration"
tags: Spring boot, configuration, maven
---

## Run a Spring boot application

Usually a Spring boot application can be compile and run with Intellij IDEA or any other IDE. However, there are other
ways to starting a Spring application with command line. This is especially useful if the application is on the remote 
server.

1. mvn spring-boot:run

2. mvn clean package, then go to the `target` folder, run `java -jar ***-SNAPSHOT.jar`

## Configuration

A database configuration example:

```properties
spring.datasource.url: jdbc:mysql://127.0.0.1:3306/
spring.datasource.username: root
spring.datasource.password: root
spring.datasource.driver-class-name: com.mysql.jdbc
```

* Change the default port: server.port=8081
* server.servlet.context-path=/luckymoney

Then we need to go to `localhost:8081/luckymoney/hello`.

We can also create a new configuration file, for example, application.yml

```yaml
server:
  port: 8081
  servlet:
    context-path: /luckymoney

minMoney: 1
description: At least ${minMoney} dollar
```

Then, in our application, we can use it in the program.

```java
@RestController
public  class HelloController {
    @Value("${minMoney}")
    private BigDecimal minMoney;
    
    @Value("${description}")
    private String description;
    
    @GetMapping("/hello")
    public String say(){
        return "minMoney: " + minMoney + ", means " + description;
    }
}
```

## Using configuration file, config with object

Step 1: put the values in the application.yml file inside a whole parent value.

```yaml
limit:
  minMoney: 2
  maxMoney: 9999
  description: At least ${limit.minMoney} dollar
``` 

Step 2: Create a java file and @ConfigurationProperties(prefix = "***")

```java
@ConfigurationProperties(prefix = "limit")
@Component
public class LimitConfig {

    private BigDecimal minMoney;

    private BigDecimal maxMoney;

    private String description;

    public BigDecimal getMinMoney() {
        return minMoney;
    }

    public void setMinMoney(BigDecimal minMoney) {
        this.minMoney = minMoney;
    }

    public BigDecimal getMaxMoney() {
        return maxMoney;
    }

    public void setMaxMoney(BigDecimal maxMoney) {
        this.maxMoney = maxMoney;
    }

    public String getDescription() {
        return description;
    }

    public void setDescription(String description) {
        this.description = description;
    }
}
```

Step 3: @Autowired and use it

```java
@RestController
public class HelloController {

    @Autowired
    private LimitConfig limitConfig;


    @GetMapping("/hello")
    public String say(){
        return "Description: " + limitConfig.getDescription();
    }
}

```

## Separate development environment and production environment

1. Create/copy two more configuration files, application-dev.yml and application-prod.yml. Then give any configuration 
in each file as desired.

2. Change the original application.yml file.

```yaml
spring:
  profiles:
    active: dev
```

3. When get to the production stage, add a parameter when running the jar file.

    * mvn clean package
    * java -jar [-Dspring.profiles.active=prod] luckymoney-0.01-SNAPSHOT.jar

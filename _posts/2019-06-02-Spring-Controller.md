---
title:  "Spring boot controller"
tags: Spring boot, controller
---

## Template

If you only use @Controller instead of @RestController, then you have to use a template like 
Thymeleaf.

1. In pom,xml, import thymeleaf

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-thymeleaf</artifactId>
</dependency>
```

2. Create a html file in template folder, for example, say "Hello World" in a <h1> tag.

3. In controller java class, @Controller and return the name of that html file. 

```java
@Controller
public class HelloController {

    @Autowired
    private LimitConfig limitConfig;


    @GetMapping("/hello")
    public String say(){
//        return "Description: " + limitConfig.getDescription();
        return "index";
    }
}
```

## @Controller + @ResponseBody = @RestController

If you want to return both html and String/Json data, then you can @Controller on the class level, 
and @ResponseBody on the method level. However, this approach is not recommended. Nowadays, we 
usually separate back-end and front-end.

## url

### Multiply url for a same method

```java
@GetMapping({"/hello", "/hi"})
public String say(){
    return "Description: " + limitConfig.getDescription();
}
```

### url chain
```java
@RestController
@RequestMapping("/hello")
public class HelloController {

    @Autowired
    private LimitConfig limitConfig;

    @GetMapping("/say")
    public String say(){
        return "Description: " + limitConfig.getDescription();
    }
}
```

## POST and GET

It is better to Specify the request method, either GetMapping or PostMapping. 
It is not recommended to use RequestMapping both a method, which means both will work.

## Request Parameters

* @PathVariable

```java
@GetMapping("/say/{id}")
public String say(@PathVariable("id") Integer id){
    return "id: " + id;
}
```

* @RequestParam

```java
@GetMapping("/say")
public String say(@RequestParam("id") Integer id){
    return "id: " + id;
}
```

* Optional parameters

```java
@GetMapping("/say")
public String say(@RequestParam(value = "id", required = false, defaultValue = "0") Integer id){
    return "id: " + id;
}
```

* postman

To send a Post request with parameters, use postman, and choose `Body`, then choose `x-www-form-urlencoded`

















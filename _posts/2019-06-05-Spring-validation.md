---
title:  "Spring validation"
tags: Spring, validation
---

### Day 1: Spring boot Web Evolved (Plan to make a flutter tutorial website)

<br /><br />

## Passing parameters with an Object

When we have more and more parameters in the controller methods, we might want to pass them in an easier way. 
We can pass in an object instead of passing the parameters separately.

```java
@PostMapping(value = "/girls")
public Result<Girl> girlAdd(@Valid Girl girl, BindingResult bindingResult) {
    ...
}
```

## Validation

If we need to validate a parameter value, we can do the following steps.
 
* First, in the object class, we @NotBlank(message = "this cannot be blank") or @Min(value = 18, message = "At least 18")

```java
@Entity
public class Girl {
    @NotBlank(message = "cupSize cannot be blank")
    private String cupSize;

    @Min(value = 18, message = "At least 18")
//    @NotNull
//    @Max()
//    @Length()
    private Integer age;
}
``` 

* Secondly, we @Valid in the parameter of the controller. The validation result will be returned to an object typed
BindingResult

```java
@PostMapping(value = "/girls")
public Result<Girl> girlAdd(@Valid Girl girl, BindingResult bindingResult) {

}
```







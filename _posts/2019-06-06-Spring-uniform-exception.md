---
title:  "Spring Exception Handling"
tags: Spring, Exception Handling
---

### Day 2: Spring boot Web Evolved - Uniform Exception

<br /><br />

## Uniformed returned type

In different controller methods, we may return different types, sometimes an error or 
exception will be thrown. This is difficult for the front-end developer to handle 
every case. In an enterprise application, it is necessary to make a uniform returned type.

In the following example, the returned type is always a JSON like this:

```json
{
  "code": 0,
  "msg": "success",
  "data": {
    "id": 20,
    "age": 25
  }
}
```

```java

public class Result<T> {

    private Integer code;

    private String msg;

    private T data;

}
```

### Then we change the returned type of our methods to this formal return type.

```java
@PostMapping(value = "/girls")
public Result<Girl> girlAdd(@Valid Girl girl, BindingResult bindingResult) {
    if (bindingResult.hasErrors()) {
        return ResultUtil.error(1, bindingResult.getFieldError().getDefaultMessage());
    }

    return ResultUtil.success(girlRepository.save(girl));
}
```

The ResultUtil is a helper class to create a new returned object depending on whether the method success.

## Throw an exception instead of returning the message directly.

In the service method, sometimes we need to validate the input parameter first, then execute the real logic.
Or in some cases, an error should stop the method and signal the frontend. In these cases, it it convenient
to throw an exception instead of returning the error message directly.

However, the problem is that if we throw an exception to the front-end, the return type will be a default 
exception structure, which is different from our uniform return type. Therefore, we create our own exception
class.

```java
@ControllerAdvice
public class ExceptionHandle {

    private final static Logger logger = LoggerFactory.getLogger(ExceptionHandle.class);

    @ExceptionHandler(value = Exception.class)
    @ResponseBody
    public Result handle(Exception e) {
        if (e instanceof GirlException) {
            GirlException girlException = (GirlException) e;
            return ResultUtil.error(girlException.getCode(), girlException.getMessage());
        }else {
            logger.error("System error{}", e);
            return ResultUtil.error(-1, "Unknown error");
        }
    }
}
```

@ControllerAdvice can be applied on all the controller. And also, we want to identify our own exception with our error 
code. So we create a new Exception class.

```java
public class GirlException extends RuntimeException{

    private Integer code;

    public GirlException(ResultEnum resultEnum) {
        super(resultEnum.getMsg());
        this.code = resultEnum.getCode();
    }

    public Integer getCode() {
        return code;
    }

    public void setCode(Integer code) {
        this.code = code;
    }
}
```

*Notice that Spring only support transaction roll back for RuntimeException.*

## Enum error

Because the error code and message are always binding together, it is better to maintain these code messages in our 
enum class.

```java
public enum ResultEnum {
    UNKONW_ERROR(-1, "Unknown error"),
    SUCCESS(0, "success"),
    PRIMARY_SCHOOL(100, "You are in primary school"),
    MIDDLE_SCHOOL(101, "You are in middle school"),

    ;

    private Integer code;

    private String msg;

    ResultEnum(Integer code, String msg) {
        this.code = code;
        this.msg = msg;
    }

    public Integer getCode() {
        return code;
    }

    public String getMsg() {
        return msg;
    }
}
```



















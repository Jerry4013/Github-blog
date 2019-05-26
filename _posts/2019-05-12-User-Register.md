---
layout: post
title:  "User Register"
tags: Java, Spring
---
### \#Tip of the Day: 

<br /><br />

## Metronic templates

Frontend templates website, but it is not free.

## Project Architecture

* View Object: Display to users

* Controller: Receive Http request from frontend, and return formatted result to frontend.

* DOMapper: manipulate databases.

* DO: data objects. Java objects mapping with databases. The attributes are exactly the same as databases.

* BusinessException: Remember, business logic exceptions are http "200", they are not network problems.

* CommonReturnType: We should define a format for the returned response to communicate with the frontend.

* Service: This is the place you really "do something". Usually, it depends on DOMappers because many works need 
databases.

* Model: A complete data package returned by services. 

## StringUtils

Many libraries have a better implementation for a String operation. For example, they may check null before 
comparing two Strings.

## Always check null to make your code robust

## String check

For an web application, null and empty String are the same, which means the user did not input any information.

## Register process should be in a transaction "@Transactional"

## Validation must be done on both frontend and backend

## Cross Origin

Must specify the scope of Cross Origin on both backend and frontend.

```java
@CrossOrigin(allowCredentials = "true",allowedHeaders = "*")
```

```javascript
xhrFields:{withCredentials:true}
```

## MD5 encoding

Java's embedded MD5 implementation only support 16-bit. 




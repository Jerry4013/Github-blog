---
layout: post
title:  "Java Reflection"
tags: Java Reflection
---
### \#Tip of the Day: 不积跬步，无以至千里；不积小流，无以成江海。

<br/><br/>

## Everything in the universe is an Object. 

## Therefore, any class that we created is also an Object!

## Any class that we created is an instance of "Class"

## How to get these class types? Three ways:

```Java
Class c1 = Foo.class;
Class c2 = foo1.getClass();
Class c3 = Class.ForName("com.imooc.reflect.Foo");
```
c1 == c2 == c3

## Create an object by its class type.
```Java
Foo foo = (Foo)c1.newInstance();
```

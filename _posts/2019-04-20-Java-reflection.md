---
layout: post
title:  "Java Reflection"
tags: Java Reflection
---
### \#Tip of the Day: 不积跬步，无以至千里；不积小流，无以成江海。

<br/><br/>

[Video Link](https://www.imooc.com/video/3734/0)

## Class is an object 

Everything in the universe is an Object. 

Therefore, any class that we created is also an Object!

Any class that we created is an instance of "Class"

## How to get these class types? Three ways:

```java
Class c1 = Foo.class; // method 1
Class c2 = foo1.getClass(); // method 2
Class c3 = Class.ForName("com.imooc.reflect.Foo"); //method 3, dynamically load class
```
c1 == c2 == c3, **because they are the same object!**

## Create an object by its class type.

```java
Foo foo = (Foo)c1.newInstance();
```

## Loading a class dynamically

`Class.ForName("com.imooc.reflect.Foo"); `Here we are dynamically loading a class at run time.

Example: Use class type create an instance dynamically
```java
try {
    if (args.length > 0) {
        Class c = Class.forName(args[0]);
        OfficeAble oa = (OfficeAble) c.newInstance();
        oa.start();
    }
} catch (Exception e) {
    e.printStackTrace();
}
```
## Get class information

Most of java primitive types or keywords have their own class type, including int, Double, void.
```java
Class c1 = int.class;//int class type
Class c2 = String.class;//String class type
Class c3 = double.class;
Class c4 = Double.class;
Class c5 = void.class;

System.out.println(c1.getName());
System.out.println(c2.getName());
System.out.println(c2.getSimpleName());//without package name
System.out.println(c5.getName());
```

To get all the methods of a class:
```java   
/*
 * Method is also a class
 * one method is a Method object
 * getMethods(), get all public methods, including those inherited from super class. 
 * getDeclaredMethods(), get all the methods declared by itself, no matter public or private
 */
Method[] ms = c.getMethods();//c.getDeclaredMethods()
for(int i = 0; i < ms.length;i++){
    Class returnType = ms[i].getReturnType();
    System.out.print(returnType.getName()+" ");
    System.out.print(ms[i].getName()+"(");
    Class[] paramTypes = ms[i].getParameterTypes();
    for (Class param : paramTypes) {
        System.out.print(param.getName()+",");
    }
    System.out.println(")");
}
```

To get all the fields of a class:
```java
Field[] fs = c.getDeclaredFields();
for (Field field : fs) {
    Class fieldType = field.getType();
    String typeName = fieldType.getName();
    String fieldName = field.getName();
    System.out.println(typeName+" "+fieldName);
}
```

## Methods reflection
1. Get a method from class type
2. Method reflection operation

```java
A a1 = new A();
Class c = a1.getClass();
/*
 * getMethod get public method
 * getDelcaredMethod its own method
 */
try {
    //Method m =  c.getMethod("print", new Class[]{int.class,int.class});
    Method m = c.getMethod("print", int.class,int.class);
    //a1.print(10, 20);
    //if no return value , return null
    //Object o = m.invoke(a1,new Object[]{10,20});
    Object o = m.invoke(a1, 10, 20);
    System.out.println("==================");
    
    //get method print(String,String)
    Method m1 = c.getMethod("print", String.class, String.class);
    //reflection operation
    //a1.print("hello", "WORLD");
    o = m1.invoke(a1, "hello", "WORLD");
    System.out.println("===================");
    
    // Method m2 = c.getMethod("print", new Class[]{});
    Method m2 = c.getMethod("print");
    // m2.invoke(a1, new Object[]{});
    m2.invoke(a1);
} catch (Exception e) {
    // TODO Auto-generated catch block
    e.printStackTrace();
} 
```

## Understand Generic by reflection

Generic type only works at compile time. Its purpose is avoiding input typo

Proof:
```java
ArrayList list1 = new ArrayList();
ArrayList<String> list2 = new ArrayList<String>();
list2.add("hello");
//list2.add(20); error!
Class c1 = list1.getClass();
Class c2 = list2.getClass();
System.out.println(c1 == c2); //true
```

Use method reflection to bypass generic
```java
try {
    Method m = c2.getMethod("add", Object.class);
    m.invoke(list2, 20);
    System.out.println(list2.size()); //size is 2
    System.out.println(list2);//[hello, 20]
} catch (Exception e) {
  e.printStackTrace();
}
```


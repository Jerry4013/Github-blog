---
title:  "Spring Unit test"
tags: Spring, Unit test
---

### Day 3: Spring boot Web Evolved - Unit test

<br /><br />

## Testing a service method

Using Intellij IDEA, in the service class, right click the method name, Go To, Test. In this way, we can create a new
 test class and check the methods you want to test.
 
```java
@RunWith(SpringRunner.class)
@SpringBootTest
public class GirlServiceTest {

    @Autowired
    private GirlService girlService;

    @Test
    public void findOneTest() {
        Girl girl = girlService.findOne(73);
        Assert.assertEquals(new Integer(13), girl.getAge());
    }
}
```

## Testing a controller method

For controller methods, we want to test a HTTP request. Here we use @AutoConfigureMockMvc and MockMvc class.

```java
@RunWith(SpringRunner.class)
@SpringBootTest
@AutoConfigureMockMvc
public class GirlControllerTest {

    @Autowired
    private MockMvc mvc;

    @Test
    public void girlList() throws Exception {
        mvc.perform(MockMvcRequestBuilders.get("/girls"))
                .andExpect(MockMvcResultMatchers.status().isOk())
        .andExpect(MockMvcResultMatchers.content().string("abc"));
    }

}
```

## Maven package tests

When we package with maven, maven will run all the unit tests for us before packaging.

If we want to skip unit test when `mvn clean package`, we can execute the command below:

```
mvn clean package -Dmaven.test.skip=true
```


 
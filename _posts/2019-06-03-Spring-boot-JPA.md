---
title:  "Spring boot JPA"
tags: Spring boot, JPA
---

### 从前，车马很慢，书信很远，一生只够爱一人

<br /><br />

## Import database dependencies

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-jpa</artifactId>
</dependency>

<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
</dependency>
```

## Database configuration

MySql database driver: choose `com.mysql.cj.jdbc.Driver`, this is the new version

```yaml
spring:
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://127.0.0.1:3306/luckymoney
    username: root
    password: root
  jpa:
    hibernate:
      ddl-auto: update
    show-sql: true
```

### Create a new database in mysql

Create a database called luckymoney using command line or navicat.

## Manipulate database without any SQL

JPA is so powerful that we do not need to write any SQL.

1. Create a java class called Luckymoney, add @Entity on the class.

```java
@Entity
public class Luckymoney {

    @Id
    @GeneratedValue
    private Integer id;

    private BigDecimal money;

    private String producer;

    private String consumer;

    public Integer getId() {
        return id;
    }

    public void setId(Integer id) {
        this.id = id;
    }

    public BigDecimal getMoney() {
        return money;
    }

    public void setMoney(BigDecimal money) {
        this.money = money;
    }

    public String getProducer() {
        return producer;
    }

    public void setProducer(String producer) {
        this.producer = producer;
    }

    public String getConsumer() {
        return consumer;
    }

    public void setConsumer(String consumer) {
        this.consumer = consumer;
    }
}
```

2. Run the application, then all the tables will be generated automatically.

Because we config that show-sql: true, so we can see the information below in the console:

```
Hibernate: drop table if exists hibernate_sequence
Hibernate: drop table if exists luckymoney
Hibernate: create table hibernate_sequence (next_val bigint) engine=MyISAM
Hibernate: insert into hibernate_sequence values ( 1 )
Hibernate: create table luckymoney (id integer not null, consumer varchar(255), money decimal(19,2), producer varchar(255), primary key (id)) engine=MyISAM
```

## Implement the methods

1. Create a DAO

```java
public interface LuckymoneyRepository extends JpaRepository<Luckymoney, Integer> {
}
```

Follow the naming convention in JPA, which means this interface name should be ***Repository.
Extending JpaRepository = CrudRepository + PagingAndSortingRepository
The first generic type is the Object type of the entity in the database, which is alse our DAO class;
The second generic type is integer, because our Id type is integer.

2. Inject DAO in our controller

```java
@RestController
public class LuckymoneyController {

    @Autowired
    private LuckymoneyRepository repository;

    @GetMapping("/luckymoneys")
    public List<Luckymoney> list(){
        return repository.findAll();
    }

    @PostMapping("/luckymoney")
    public Luckymoney create(@RequestParam("producer") String producer,
                             @RequestParam("money")BigDecimal money){
        Luckymoney luckymoney = new Luckymoney();
        luckymoney.setProducer(producer);
        luckymoney.setMoney(money);
        return repository.save(luckymoney);
    }

    @GetMapping("/luckymoney/{id}")
    public Luckymoney findById(@PathVariable("id") Integer id){
        return repository.findById(id).orElse(null);
    }

    @PutMapping("/luckymoney/{id}")
    public Luckymoney update(@PathVariable("id") Integer id,
                             @RequestParam("consumer") String consumer){
        Optional<Luckymoney> optional = repository.findById(id);
        if (optional.isPresent()) {
            Luckymoney luckymoney = optional.get();
            luckymoney.setConsumer(consumer);
            return repository.save(luckymoney);
        }
        return null;
    }
}

``` 

## Transaction

Sometimes, we want an operation either success or fail as a whole. For example, when we transfer money 
in a bank, we must completely finish the whole procedure at one time, and must not be interrupted in 
the middle. In this scenario, we can use @Transaction on a method.
 
1. Create a service class

```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;

import java.math.BigDecimal;

/**
 * @author: Jingchao Zhang
 * @createDate: 2019/06/03
 **/
@Service
public class LuckymoneyService {

    @Autowired
    private LuckymoneyRepository repository;

    @Transactional
    public void createTwo(){
        Luckymoney luckymoney1 = new Luckymoney();
        luckymoney1.setProducer("Jerry");
        luckymoney1.setMoney(new BigDecimal("520"));
        repository.save(luckymoney1);

        Luckymoney luckymoney2 = new Luckymoney();
        luckymoney2.setProducer("Jerry");
        luckymoney2.setMoney(new BigDecimal("1314"));
        repository.save(luckymoney2);
    }
}
```

2. Change the database engine to InnoDB

Use Navicat, right click the table, design table.
Select the optional tab, Change the engine to InnoDB.









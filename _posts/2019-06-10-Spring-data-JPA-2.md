---
title:  "Spring data JPA(2)"
tags: Spring, Spring data, JPA
---

### Day 6: 


## Repository interface

Repository is the core interface in Spring Data. There is no method in this interface.

The first generic parameter is the Entity Class that we want to operate. The second parameter is the Id of the first
parameter.

We can also use @RepositoryDefinition instead of extending this interface. For example:

```java
@RepositoryDefinition(domainClass = Employee.class, idClass = Integer.class)
public interface EmployeeRepository {
    public Employee findByName(String name);
}
```

## The interfaces that extend Repository

* Tips: press Ctrl + t can go to the subclasses.

Inheritance tree:

```
Repository
    CrudRepository (save, findOne, exists, findAll, count, delete, deleteAll)
        PagingAndSortingRepository (findAll-sort, findAll-pageable)
            JpaRepository (findAll, findAll-sort, findAll-ids, save-entities, flush, getOne)
```

## Naming rules

* findByLastnameAndFirstname
* findByLastnameOrFirstname
* findByStartDateBetween
* findByAgeLessThan
* findByAgeGreaterThan
* findByStartDateAfter
* findByStartDateBefore
* findByAgeIsNull
* findByAgeIsNotNull
* findByFirstnameLike
* findByFirstnameNotLike
* findByFirstnameStartingWith
* findByFirstnameEndingWith
* findByFirstnameContaining
* findByAgeOrderByLastnameDesc
* findByLastnameNot
* findByAgeIn
* findByAgeNotIn
* findByActiveTrue
* findByActiveFalse

```java
public interface EmployeeRepository extends Repository<Employee, Integer> {

    public Employee findByName(String name);

    public List<Employee> findByNameStartingWithAndAgeLessThan(String name, Integer age);

    public List<Employee> findByNameEndingWithAndAgeLessThan(String name, Integer age);

    public List<Employee> findByNameInOrAgeLessThan(List<String> names, Integer age);

    public List<Employee> findByNameInAndAgeLessThan(List<String> names, Integer age);
}
```

## @Query

We can also write our own customized SQL statement instead of following the naming rules above.

```java
@Query("select o from Employee o where id=(select max(id) from Employee t1)")
public Employee getEmployeeByMaxId();

@Query("select o from Employee o where o.name=?1 and o.age=?2")
public List<Employee> queryParams1(String name, Integer age);

@Query("select o from Employee o where o.name=:name and o.age=:age")
public List<Employee> queryParams2(@Param("name")String name, @Param("age")Integer age);

@Query("select o from Employee o where o.name like %?1%")
public List<Employee> queryLike1(String name);

@Query("select o from Employee o where o.name like %:name%")
public List<Employee> queryLike2(@Param("name")String name);

@Query(nativeQuery = true, value = "select count(1) from employee")
public long getCount();
``` 

## Transaction

If we want to insert, update or delete an item in the table, we must put the methods in a transaction.
A transaction is usually done in a service method.

```java
@Service
public class EmployeeService {

    @Autowired
    private EmployeeRepository employeeRepository;

    @Transactional
    public void update(Integer id, Integer age){
        employeeRepository.update(id, age);
    }
}

```

Notice that we added the @Transactional on the update method.

Additionally, we need to @Modifying on our query method in the repository interface.

```java
@Modifying
@Query("update Employee o set o.age = :age where o.id =:id")
public void update(@Param("id")Integer id, @Param("age")Integer age);
```

## CrudRepository

CrudRepository extends Repository interface and has more methods. To use it, we simply create our repository 
interface and extends CrudRepository.

```java
public interface EmployeeCrudRepository extends CrudRepository<Employee, Integer> {

}
```

Then, inject this repository into the service class. All the methods defined in this interface can be used directly.

```java
@Autowired
private EmployeeCrudRepository employeeCrudRepository;

@Transactional
public void saveAll(List<Employee> employees){
    employeeCrudRepository.saveAll(employees);
}
```

## PagingAndSortingRepository

PagingAndSortingRepository extends CrudRepository and supports paging and sorting. It has two useful methods,
findAll(Sort sort), findAll(Pageable pageable).

```java
public interface EmployeePagingAndSoringRepository extends PagingAndSortingRepository<Employee, Integer> {
}
```

First create a pageRequest. Use it to find a Page object.
```java
@Test
public void testPage(){
    PageRequest pageRequest = PageRequest.of(0, 5);
    Page<Employee> employeePage = employeePagingAndSoringRepository.findAll(pageRequest);

    System.out.println("Total pages = " + employeePage.getTotalPages());
    System.out.println("Total elements = " + employeePage.getTotalElements());
    System.out.println("Current page number = " + employeePage.getNumber() + 1);
    System.out.println("Content of current page = " + employeePage.getContent());
    System.out.println("Number of elements of current page = " + employeePage.getNumberOfElements());
}

@Test
public void testPageAndSort(){
    Sort sort = new Sort(Sort.Direction.DESC, "id");
    PageRequest pageRequest = PageRequest.of(0, 5, sort);
    Page<Employee> employeePage = employeePagingAndSoringRepository.findAll(pageRequest);

    System.out.println("Total pages = " + employeePage.getTotalPages());
    System.out.println("Total elements = " + employeePage.getTotalElements());
    System.out.println("Current page number = " + employeePage.getNumber() + 1);
    System.out.println("Content of current page = " + employeePage.getContent());
    System.out.println("Number of elements of current page = " + employeePage.getNumberOfElements());
}
```

## JpaRepository

JpaRepository extends PagingAndSortingRepository.

```java
public interface EmployeeJpaRepository extends JpaRepository<Employee, Integer> {
    
}
```

```java
@Test
public void findAllById(){
    List<Integer> ids = new ArrayList<>();
    ids.add(99);
    List<Employee> employees = employeeJpaRepository.findAllById(ids);
    for (Employee employee : employees) {
        System.out.println(employee);
    }
}

@Test
public void exists(){
    Employee employee = new Employee("test97", 3);
    System.out.println(employeeJpaRepository.exists(Example.of(employee)));
}
```

## JpaSpecificationExecutor

This interface has a filter beyond the interface above.

We can implement this class besides JpaRepository.
```java
public interface EmployeeJpaSpecificationExecutorRepository
        extends JpaRepository<Employee, Integer>,
        JpaSpecificationExecutor<Employee> {

}
```

For example, if we want to query all the employees whose age is greater than 50 and also sorting descending with paging.
```java
@Test
public void query(){
    Sort sort = new Sort(Sort.Direction.DESC, "id");
    Pageable pageable = PageRequest.of(0, 5, sort);

    Specification<Employee> specification = new Specification<Employee>() {
        @Override
        public Predicate toPredicate(Root root,
                                     CriteriaQuery criteriaQuery,
                                     CriteriaBuilder criteriaBuilder) {
            Path path = root.get("age");
            return criteriaBuilder.gt(path, 50);
        }
    };

    Page<Employee> employeePage = employeeJpaSpecificationExecutorRepository.findAll(specification, pageable);

    System.out.println("Total pages = " + employeePage.getTotalPages());
    System.out.println("Total elements = " + employeePage.getTotalElements());
    System.out.println("Current page number = " + employeePage.getNumber() + 1);
    System.out.println("Content of current page = " + employeePage.getContent());
    System.out.println("Number of elements of current page = " + employeePage.getNumberOfElements());
}
```









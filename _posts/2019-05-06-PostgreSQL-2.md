---
layout: post
title:  "PostgreSQL----Connection and JDBC"
tags: Database, PostgreSQL, JDBC
---
### \#Tip of the Day: Always record your passwords somewhere!

<br/><br/>

## Three mistakes I made:

1. `user` is a default table in postgreSQL, so, never use it as your table name, use `userinfo` instead.

2. If there is any variable inside the sql String, like `String sql = "insert into students (id,Name,Sex,Age) values(?,?,?,?)";`
we must make sure that assigning every variable for it.

3. For the auto incremented primary key, never assign any value for it. Specify all the other 
fields and use the same number of question marks. 

## JDBC Connection:

Steps:

1. Register a JDBC driver. Load this class: `Class.forName(driver);`
2. Create a connection with this driver.
3. Prepare SQL String.
4. Execute SQL statement.
5. Handle the result set if needed.
6. Close connection.

I used a helper method to create the connection first:

```java
private Connection getConnection() throws SQLException, ClassNotFoundException {
    String driver = "org.postgresql.Driver";
    String url = "jdbc:postgresql://localhost:5432/hmail";
    String username = "postgres";
    String password = "postgres";
    Connection connection;
    Class.forName(driver);
    connection = DriverManager.getConnection(url, username, password);

    return connection;
}
```

The driver name is always "org.postgresql.Driver". **Remember, we need to import the library first.**

In the url, I specified the database name as hmail. I think if I did not, then I had to specify
the database in a sql statement.

In my register method, I assigned all the values from a user object.

```java
public void register(User user) throws SQLException, ClassNotFoundException {

    Connection connection = getConnection();
    PreparedStatement statement;

    System.out.println("Opened database successfully");

    try {
        String sql="insert into hmail.userinfo(username, password,nickname," +
                "email,state,code) values(?,?,?,?,?,?)";
        statement = connection.prepareStatement(sql);
        statement.setString(1, user.getUsername());
        statement.setString(2, user.getPassword());
        statement.setString(3, user.getNickname());
        statement.setString(4, user.getEmail());
        statement.setInt(5, user.getState());
        statement.setString(6, user.getCode());
        statement.executeUpdate();
        statement.close();
        connection.close();
    } catch (SQLException e) {
        e.printStackTrace();
    }
}
```


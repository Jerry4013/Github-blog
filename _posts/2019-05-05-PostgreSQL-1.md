---
layout: post
title:  "PostgreSQL----installation on Windows"
tags: Database, PostgreSQL
---
### \#Tip of the Day: Always record your passwords somewhere!

<br/><br/>

## Record the Password

If the installation application prompt to input username and password, record it in an excel file,
or txt file, or on a notebook. I forgot my password and I had a big trouble.

NEVER TRY TO MEMORIZE YOUR PASSWORDS IN YOUR HEAD!

Many experts told us do not write down your passwords because others can see it.

In reality, we create hundreds of user names and passwords everywhere. How can a human being 
remember those passwords? That is impossible. In most cases, security is not a concern for us,
but once we forget the password, we have a big problem.

So, Always record your passwords! Always record your passwords! Always record your passwords!

When install this database, we also need to install a database driver, a database server.
I selected pgJDBC V42.2.5-1 and PostgreSQL(64 bit) v11.2-2

## Default Configurations

The default server: localhost

The default super user: postgres

The default port: 5432

The default database: postgres

## Change Password

In Windows, the easiest way to change a password is using pgAdmin GUI. Under `Login/Group Roles`,
right click the user `postgres`, then click `properties`. In the Definition tab, we can easily 
type in the password and save it. 

### Restart the service

After I changed the password, there was a red cross on one of my databases. I right click the 
computer icon on my desktop, and click manage, under "services and Applications", click Service.
I restarted the PostgreSQL service, then the red cross disappeared. 

## Login PostgreSQL

To use it, we can use command line. Before that, it is convenient if we set a environment 
variable path for it. Put your own paths in it.

```
C:\Program Files\PostgreSQL\11\bin
C:\Program Files\PostgreSQL\11\lib
```
Command parameters:

-h the host to connect to
-U the user to connect with
-p the port to connect to (default is 5432)

For example:

```
psql -h localhost -U postgres -p 5432
```

### User name must be provided, otherwise, it will use the name of your Windows account.

A easier way is using the SQL Shell installed together with PostgreSQL.

For the default value that you don't want to change, just press Enter.

## Create a Schema

Unlike MySQL, PostgreSQL needs a schema to put all the tables into it.  

When we need to use a table, it's better to specify the schema name. For example: `hmail.userinfo`.


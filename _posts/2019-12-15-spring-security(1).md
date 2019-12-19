---
title:  "Spring Security(1)"
tags: Spring Security
---

## Create Maven Projects

### packaging config

Config Maven packaging type with the tag `<packaging>...</packaging>`.

jar, war, pom

For example:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.imooc.security</groupId>
    <artifactId>imooc-security</artifactId>
    <version>1.0-SNAPSHOT</version>
    <packaging>pom</packaging>
    
</project>
```



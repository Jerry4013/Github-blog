---
title:  "Spring boot email(2)"
tags: Spring boot, email
---

### Day 8: 

## Sending an email with images

```java
public void sendInlineResourceMail(String to, String subject, String content,
                                       String rscPath, String rscId) throws MessagingException {
        MimeMessage message = mailSender.createMimeMessage();
        MimeMessageHelper helper = new MimeMessageHelper(message, true);
        helper.setFrom(from);
        helper.setTo(to);
        helper.setSubject(subject);
        helper.setText(content, true);

        FileSystemResource res = new FileSystemResource(new File(rscPath));
        helper.addInline(rscId, res);
        mailSender.send(message);
    }
```

```java
@Test
public void senInlineResourceMail() throws MessagingException {
    String imgPath = "";
    String rscId = "***";
    String content = "<html><body><img src=\'cid:" + rscId
            + "\'></img></body></html>";
    mailService.sendInlineResourceMail("***", "test", content, imgPath, rscId);
}
```

## Sending a template email with thymeleaf

Step 1: Import dependency. 

* Notice: Do not import thymeleaf until you really start using it.

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-thymeleaf</artifactId>
</dependency>
```

Step 2: Create an html template

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8"/>
    <title>Email</title>
</head>
<body>
    Hello! Activate account!
    <a href="#" th:href="@{https://www.google.ca}">Activate account</a>
</body>
</html>
```

Step 3: Send this email

```java
@Test
public void templateMail() throws MessagingException {
    Context context = new Context();
//        context.setVariable("id", "006");
    String emailTemplate = templateEngine.process("emailTemplate", context);
    mailService.sendHTMLMail("send to", "test", emailTemplate);
}
```

## Exception handler

Sending an email is usually an asynchronous request. We need to handle the exceptions in the try catch.
So we add a logger in the service.

Step 1: Add Logger as an instance variable.

```java
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

private final Logger logger = LoggerFactory.getLogger(this.getClass());
```

Step 2: try catch and print log

```java
public void sendInlineResourceMail(String to, String subject, String content,
                                   String rscPath, String rscId) {
    logger.info("Sending email start: {},{},{},{},{}", to, subject, content, rscPath, rscId);
    MimeMessage message = mailSender.createMimeMessage();
    MimeMessageHelper helper = null;
    try {
        helper = new MimeMessageHelper(message, true);
        helper.setFrom(from);
        helper.setTo(to);
        helper.setSubject(subject);
        helper.setText(content, true);

        FileSystemResource res = new FileSystemResource(new File(rscPath));
        helper.addInline(rscId, res);
        mailSender.send(message);
        logger.info("Sending email success!");
    } catch (MessagingException e) {
        logger.error("Sending email failed:",e);
    }
}
```

Step 3: test

Run the same test method, and we can see the log in the terminal.
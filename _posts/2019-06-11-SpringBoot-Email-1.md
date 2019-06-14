---
title:  "Spring boot email(1)"
tags: Spring boot, email
---

### Day 7: 

## Email protocols

* SMTP: upload and transfer between mail servers.

* POP3: pull emails to local.

* IMAP: After receiving the email, the email is still reserved on the server. Mapping client and server.

* Mime: SMTP was using ASCII. Mime extends to binary transfer.

## Unit test

We need to add two annotations in the unit test class

```java
@RunWith(SpringRunner.class)
@SpringBootTest
```

## Spring-boot-starter-mail

* Maven dependency

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-mail</artifactId>
</dependency>
```

* property file

```properties
spring.mail.host=smtp.gmail.com
spring.mail.username=<your email username>
spring.mail.password=<your email password>
spring.mail.default-encoding=utf-8

#If you use Gmail, the follow two lines are necessary
spring.mail.properties.mail.smtp.socketFactory.port = 465
spring.mail.properties.mail.smtp.socketFactory.class = javax.net.ssl.SSLSocketFactory
```

## Sending a simple text email

```java
@Service
public class MailService {

    @Value("${spring.mail.username}")
    private String from;

    @Autowired
    private JavaMailSender mailSender;

    public void sendSimpleMail(String to, String subject, String content) {
        SimpleMailMessage message = new SimpleMailMessage();
        message.setTo(to);
        message.setSubject(subject);
        message.setText(content);
        message.setFrom(from);

        mailSender.send(message);
    }
}
```

Test this service

```java
@RunWith(SpringRunner.class)
@SpringBootTest
public class MailServiceTest {

    @Resource
    MailService mailService;

    @Test
    public void sendSimpleMail() {
        mailService.sendSimpleMail("<Send to email address>",
                                "Test email",
                                "Hello World");
    }
}
```

## Sending a HTML email

```java
public void sendHTMLMail(String to, String subject, String content) throws MessagingException {
    MimeMessage message = mailSender.createMimeMessage();
    MimeMessageHelper helper = new MimeMessageHelper(message, true);
    helper.setFrom(from);
    helper.setTo(to);
    helper.setSubject(subject);
    helper.setText(content, true);
    mailSender.send(message);
}
```

Test this service

```java
@Test
public void sendHTMLMail() throws MessagingException {
    String content = "<html>\n<body>\n<h3> Hello world! This is a HTML email.</h3>\n</body>\n</html>";
    mailService.sendHTMLMail("<Send to email address>", "HTML Email", content);
}
```

## Sending an email with attachments

```java
public void sendAttachmentMail(String to, String subject, String content, String filePath) throws MessagingException {
    MimeMessage message = mailSender.createMimeMessage();
    MimeMessageHelper helper = new MimeMessageHelper(message, true);
    helper.setFrom(from);
    helper.setTo(to);
    helper.setSubject(subject);
    helper.setText(content, true);

    FileSystemResource file = new FileSystemResource(new File(filePath));
    String filename = file.getFilename();
    helper.addAttachment(filename, file);
    mailSender.send(message);
}
```

```java
@Test
public void sendAttachmentMail() throws MessagingException {
    String filePath = "C:\\Users\\***";
    mailService.sendAttachmentMail("<Send to email address>",
            "Test email",
            "Hello World", filePath);
}
```





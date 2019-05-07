---
layout: post
title:  "Mail Server----How is an email transported?"
tags: Mail Server, SMTP, POP
---
### \#Tip of the Day: Always record your passwords somewhere!

<br/><br/>

## Mail server

Nowadays, we use emails almost everyday. How is an email transported on the Internet?

Before using an email service, we need to register a digital account on a website, like gmail.com,
outlook.com, or yahoo.com, etc. But what are they?

Actually, they are mail servers. When a user apply for an account, they create a record in their 
database and assign some space for this user, for example 10G.

To transfer an email, a transfer protocol is necessary. When sending an email, we use the 
Simple Mail Transfer Protocol (SMTP), while receiving an email, we use Post Office Protocol 
version 3 (POP3). 

![mailserver]({{site.baseurl}}/assets/images/201905/mailserver.png)

But we know that a server is simply a computer. Therefore, we could use our own PC as 
a mail server. 

I tried a few mail-server applications, eventually I chose hmailserver.

I set up a database for hmailserver, because it needs some space to save all the emails.
It could not recognize MySQL database, I don't why, so I use PostgreSQL instead.

I added a new domain called jmail.com. Then every account created in this service, localhost,
has a postfix @jmail.com.

Then I created two email accounts called user01@jmail.com and user02@jmail.com.

![accounts]({{site.baseurl}}/assets/images/201905/accounts.png)

## Add accounts in outlook

Outlook is a client app for sending and receiving emails. We can add some accounts for test.

![addaccount]({{site.baseurl}}/assets/images/201905/addaccount.png)

* Choose Manually setup or additional server types, click next.

* Choose POP or IMAP, click next.

* Then we can specify our name, email address, incoming mail server, outgoing mail server, etc.

![addaccount2]({{site.baseurl}}/assets/images/201905/addaccount2.png)

The new account will displayed on the leftside of your outlook window. Then we can send emails
with these accounts as usually what we do.



---
layout: post
title:  "Products Module"
tags: Java, Spring boot
---
### \#Tip of the Day: Design Models before design databases

<br /><br />

## Tips

* Design field model first, then design databases. 先设计领域模型，再设计数据库表！

* Use not null: To avoid null pointer exception, use not null in database as much as possible.

* In service methods, check parameters first(后端的service方法，养成习惯，先校验入参)
入库前的校验入参是必须的

* Return the object that you created (养成习惯，要返回创建后的object给前端.要让别人知道创建后的对象是什么样的)

* Price should be BigDecimal. (Double在用JSON传输时会丢失精度，一定要用BigDecimal)

BigDecimal BigDecimal(double d); //不允许使用
BigDecimal BigDecimal(String s); //常用,推荐使用
static BigDecimal valueOf(double d); //常用,推荐使用

* Make controller simple. Put the logic in service classes.

* VO should be different from model.
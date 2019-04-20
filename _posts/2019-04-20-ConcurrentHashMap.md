---
layout: post
title:  "Java ConcurrentHashMap"
tags: Java ConcurrentHashMap data structure
---
### \#Tip of the Day: 不积跬步，无以至千里；不积小流，无以成江海。

<br/><br/>

[Blog Link](https://www.cnblogs.com/ITtangtang/p/3948786.html)

## Why use ConcurrentHashMap?

HashMap: In multi-threads environment, HashMap is not secure. No protection, 100% forbidden!

HashTable: use synchronized to secure concurrency, but efficiency is poor. This is because it locks the 
whole table with only one key.

### ConcurrentHashMap: divide the table into smaller segments, for each segment, use one lock to protect it.

![ConcurrentHashMap]({{site.baseurl}}/assets/images/ConcurrentHashMap.png)

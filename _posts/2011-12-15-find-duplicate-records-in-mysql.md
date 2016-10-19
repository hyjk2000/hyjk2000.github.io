---
title: MySQL 查找某些列有重复的数据项
author: James Shih
layout: post
permalink: /2011/12/15/find-duplicate-records-in-mysql/
categories:
  - SQL
  - Web 开发
tags:
  - find duplicate
  - MySQL
  - 查找重复项
---
很久以前在 [StackOverflow][1] 上看到有一个人提问，怎么查找 MySQL 数据库中的重复项。我觉得有个人回答得非常好，所以发上来分享。

<!--more-->

提问者先是用以下 SQL 语句查找：

```sql
SELECT address, count(id) as cnt FROM list
GROUP BY address HAVING cnt > 1
```

但是，他只得到了有一个地址重复了 2 次，却看不到是哪些人的地址重复了：

	100 MAIN ST    2

其实，只要先用提问者的那个 SQL 语句查出有重复的地址，然后用原表去连接它，就能得到想要的结果了：

```sql
SELECT firstname, lastname, list.address FROM list
INNER JOIN (SELECT address FROM list
GROUP BY address HAVING count(id) > 1) dup ON list.address = dup.address
```

	JIM    JONES    100 MAIN ST
	JOHN   SMITH    100 MAIN ST

 [1]: http://stackoverflow.com/questions/854128/find-duplicate-records-in-mysql
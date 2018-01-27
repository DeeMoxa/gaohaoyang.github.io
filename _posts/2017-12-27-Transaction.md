---
layout: post
title: Transaction
permalink: /posts/transaction/
excerpt: "事务简介..."
tags:
  - transaction
---

[事务详解博客转载](https://my.oschina.net/huangyong/blog/160012)<br>
事务有个基本的单元，什么是一个事务的最小单位，这个单位当中，是一个整体，这样归纳为事务的 **原子性**。<br>
在操作数据库时，往往会因为并发的问题，导致数据不统一。e.g. 银行转账时，A，B账户交互，金额却不统一，这是完全不被允许的。这样就体现出，**一致性的重要**。<br>
然而，在并发的情况下依然存在问题，不同的用户对相同的银行账户进行操作，那么最终的数额，究竟以什么为标准。为了保证，各自互不影响，规则顺应而出，**隔离性**。

1. READ_UNCOMMITTED
2. READ_COMMITTED
3. REPEATABLE_READ
4. SERIALIZABLE 事务隔离级别从上到下，并发性能越来越差，安全性随之越来越高。

事务特性 | des
---- | -----------
原子性  | Atomicity
一致性  | Consistency
隔离性  | Isolation
持久性  | Durability

此上4条为事务ACID特性。<br>
其中，隔离级别为处理手段，即我们通常需要处理的阶段。

1.脏读：事务 A 读取了事务 B 未提交的数据，并在这个基础上又做了其他操作。  
2.不可重复读：事务 A 读取了事务 B 已提交的更改数据。  
3.幻读：事务 A 读取了事务 B 已提交的新增数据。

事务隔离级别           | 脏读 | 不可重复读 | 幻读
---------------- | -- | ----- | --
READ_UNCOMMITTED | 允许 | 允许    | 允许
READ_COMMITTED   | 禁止 | 允许    | 允许
REPEATABLE_READ  | 禁止 | 禁止    | 允许
SERIALIZABLE     | 禁止 | 禁止    | 禁止

在spring中提出的传播特性：  
PROPAGATION_REQUIRED  
RROPAGATION_REQUIRES_NEW  
PROPAGATION_NESTED  
PROPAGATION_SUPPORTS  
PROPAGATION_NOT_SUPPORTED  
PROPAGATION_NEVER  
PROPAGATION_MANDATORY  

---
layout: post
title: 查看java heap、stack信息
permalink: /posts/tostackinfo/
tags:
  - stack
  - heap
---

| 格式            | 简述     |
| :------------- | :------------- |
| pidof program      | 找出program程序的进程PID，如果有多个就会全部列出，program不能是shell脚本名称。      |
|  pidof -s program | 找出program程序的进程PID，只列出一个。（Single shot - this instructs the program to only return one pid.）  |   
| pidof -x script  | 找出shell脚本script的进程PID。  |   
|   |   |   
| jstack       | 查找这个线程的信息       |
|jstack -l pid   |   用于查看线程是否存在死锁|
|free   | 查看内存使用   |

```
jstack -l 7938|more
```

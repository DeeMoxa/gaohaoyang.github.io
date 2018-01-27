---
layout: post
title: Linux防火墙设置
tags: [firewall]
---

CentOS 7.0默认使用的是firewall作为防火墙

firewall操作如下：

- 命令的方式添加端口
```
firwall-cmd --permanent --add-port=9527/tcp
```

参数介绍：

```
1、firwall-cmd：是Linux提供的操作firewall的一个工具；  
2、--permanent：表示设置为持久；  
3、--add-port：标识添加的端口；
```

例如：添加8010端口

```
firewall-cmd --zone=public --permanent --add-port=8010/tcp  
// --zone=public：指定的zone为public；
```
- 常用命令

```
1、重启、关闭、开启firewalld.service服务  
service firewalld restart 重启 service firewalld start 开启 service   firewalld stop 关闭  
注意：以上对firewalld 的操作只有重启之后才有效  
2、查看firewall服务状态  
systemctl status firewall  
3、查看firewall的状态  
firewall-cmd --state  
4、查看防火墙规则  
firewall-cmd --list-all  
```
CentOS切换为iptables防火墙  
切换到iptables首先应该关掉默认的firewalld，然后安装iptables服务。


1、关闭firewall：
```
service firewalld stop  
systemctl disable firewalld.service #禁止firewall开机启动
```
2、安装iptables防火墙
```
yum install iptables-services #安装
```
3、编辑iptables防火墙配置
```
vi /etc/sysconfig/iptables #编辑防火墙配置文件  
systemctl restart iptables.service　#重启　  
systemctl enable iptables.service #设置防火墙开机启动
```
配置文件详情：
```
\*filter  
:INPUT ACCEPT [0:0]  
:FORWARD ACCEPT [0:0]  
:OUTPUT ACCEPT [0:0]  
-A INPUT -m state --state RELATED,ESTABLISHED -j ACCEPT  
-A INPUT -p icmp -j ACCEPT  
-A INPUT -i lo -j ACCEPT  
-A INPUT -p tcp -m state --state NEW -m tcp --dport 22 -j ACCEPT  
-A INPUT -m state --state NEW -m tcp -p tcp --dport 80  -j ACCEPT  
-A INPUT -j REJECT --reject-with icmp-host-prohibited  
-A FORWARD -j REJECT --reject-with icmp-host-prohibited  
COMMIT
```

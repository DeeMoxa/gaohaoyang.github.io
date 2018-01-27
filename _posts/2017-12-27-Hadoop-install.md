---
layout: post
title: Hadoop
tags:
  - hadoop
---
<img src="{{ site.url }}{{ site.baseurl }}/images/open_sites_logo/hadoop-logo.jpg" alt="hadoop-logo">
- 下载
````
brew install Hadoop
````
- 安装、配置
````
安装完成后，在目录/usr/local/Cellar下查看
````
因为安装hadoop需要远程登入的功能,所以需要安装ssh工具。  
Mac OS X只需在“系统偏好设置”的“共享”的“远程登录”勾选就可以使用ssh了。  
确认能否不输入口令就用ssh登录localhost:  
$ ssh localhost  
注意：如果没有执行远程登陆勾选操作，在运行ssh localhost的时候会出现：  
**mac ssh: connect to host localhost port 22: Connection refused。**
在终端中依次输入如下代码配置SSH免密码登陆：
```
$ ssh-keygen -t dsa -P '' -f ~/.ssh/id_dsa  
$ cat ~/.ssh/id_dsa.pub >> ~/.ssh/authorized_keys  
解释：
第一行：ssh -keygen 代表生成密钥，-t代表指定生成的密钥类型，dsa代表dsa密钥认证的意思（密钥类型）；-P用于提供密语，-f 指定生成的密钥文件  
第二行：将公钥加入到用于认证的公钥文件中
```

---
layout: post
title: Mac开发端口转发
tags: [mac]
---
#### MAC端口转发问题
mac自带的apache会占用80端口。  
解决方案：把apache的默认端口改掉。找到apache配置文件：/etc/apache2/httpd.conf
```
//把apache的默认端口修改为81

Listen 81

重启apache:  sudo apachectl restart
```

#### tomcat在Mac下非root权限80端口是启动不了的，所以我们可以利用pfctl端口转发来将本地80端口上的请求转发到比如8080端口，从而实现通过80端口的访问。

_注意：Mac OS 会使用80端口做网络文件共享， 需要先关闭掉。_

一、修改
```
/etc/pf.conf
```

先对pf.conf进行备份：
```
cp /etc/pf.conf  /etc/pf.conf.normal.bak
```

之后在该文件中以下行：

```
rdr-anchor "com.apple/\*"
```

后面添加一行配置，如下：
```
rdr on lo0 inet proto tcp from any to 127.0.0.1 port 80 -> 127.0.0.1 port 8080
```

_ps：lo0 通过ifconfig 看自己那个设备绑定的是127.0.0.1, lo0是这个网络设备的名字_

二、依次执行以下命令：


```
sudo pfctl -d  //关闭pf
sudo pfctl -f /etc/pf.conf  //最后导入并允许运行
sudo pfctl -e //使用 -e 命令启用 pf 服务。
```

从 Mavericks 起 pf 服务不再默认开机自启。如需开机启动 pf 服务，请往下看。

新版 Mac OS 10.11 EI Captian 加入了系统完整性保护机制，需重启到安全模式执行下述命令关闭文件系统保护。

```
$ csrutil enable --without fs
然后才能修改 /System/Library/LaunchDaemons/com.apple.pfctl.plist 文件实现开机自启用配置。
```
向 plist 文件中添加 -e 行，如下所示：
```
<string>pfctl</string>
<string>-e</string>
<string>-f</string>
<string>/etc/pf.conf</string>
```

_尤其注意：如果有apache等服务器占用了80端口，则需要将其停掉方能成功!_

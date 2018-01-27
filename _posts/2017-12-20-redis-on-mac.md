---
layout: post
title: Mac下安装redis
tags: [redis,mac]
---

mac 上安装 redis 首先必须保证mac 已经安装 xcode.

因为make时要用到 Xcode 的command Tools .

(1)  [下载 redis ](https://redis.io/download)
```
$ wget http://download.redis.io/releases/redis-4.0.2.tar.gz
$ tar xzf redis-4.0.2.tar.gz
$ cd redis-4.0.2
$ make
```
(2)终端下载:
```
curl -O http://redis.googlecode.com/files/redis-2.8.7.tar.gz
sudo tar -zxf redis-2.8.7.tar.gz
```
(3)修改文件夹名,编译

```
mv redis-2.8.7 redis
cd redis/
sudo make
sudo make test
sudo make isntall
```
(4)打开conf文件
```
vim /usr/local/redis-4.0.2/redis.conf
```
找到
```
dir  ./
```
这一行配置
此配置是将内存中的数据写入一个文件,这个数据库文件要保存到什么地方 就是这个配置项起到的作用.
我在mac根目录下创建了 ```/Users/cc/develop/config/redis== ```的文件夹(*注意此文件夹必须有可读写权限*)

所以这一行的配置是 dir /Users/cc/develop/config/redis

修改后保存配置文件,同时将配置文件移动到 /etc 目录下.


```
sudo mv redis.conf /etc
```


(5) 上面第三步 ```make install== ```成功后,你就应该在这个目录下看到redis


```
ZK-=Cnlive2
/usr/local/bin/redis-server
```

(6) 尝试启动一下 redis


```
./usr/local/bin/redis-server /etc/redis.conf
```
(7)conf文件相关配置

功能 | 操作实现
---|---
设置线程守护模式 ==后台启动==| daemonize=yes
设置进程锁文件 | pidfile /usr/local/redis/redis.pid
端口|port 6379
客户端超时时间|timeout 300
日志级别|loglevel debug
日志位置|logfile /usr/local/redis/log-redis.log
指定本地数据库路径|dir /usr/local/redis/db/  
设置数据库的数量，默认数据库为0，可以使用SELECT <dbid>命令在连接上指定数据库id|databases 8
(8)redis-cli命令总结

功能 | 命令
---|---
远程链接 | redis-cli -h {host} -p {port} {command}
关闭 | redis-cli shutdown

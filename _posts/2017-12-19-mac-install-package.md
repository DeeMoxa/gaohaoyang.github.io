---
layout: post
title: Mac环境开发汇总
tags:
  - mac
  - homebrew
  - gem
  - ruby
  - jekyll
  - redis
  - idea
---

##  Homebrew
Red hat有yum，Ubuntu有apt-get而mac第三方支持：Homebrew简称brew， 是Mac OSX上的软件包管理工具，能在Mac中方便的安装软件或者卸载软件
默认安装路径为：/usr/loacl/Cellar  

brew         | 相关命令
------------ | ---------------------------------------
安装任意包        | `brew install [packageName]`
安装wget       | `brew install wget`
卸载任意包        | `brew uninstall [packageName]`
卸载git        | `brew uninstall git`
查询可用包        | `brew search [packageName]`
查看已安装包列表     | `brew info [packageName]`
删除该gem包      | gem uninstall [gemname]
删除某指定版本gem   | gem uninstall [gemname] --version=[ver]
更新Homebrew   | `brew update`
查看Homebrew版本 | `brew -v [packageName]`
Homebrew帮助信息 | `brew -h [packageName]`

##  ruby(脚本语言)

```
 ruby -e "$(curl -fsSL
    <https://raw.githubusercontent.com/Homebrew/install/master/install>)"
     ==>This script will install: /usr/local/bin/brew
      /usr/local/Library/... /usr/local/share/man/man1/brew.1
```
```
 Press RETURN to continue or any other key to abort ==>Downloading and
  installing Homebrew... remote: Counting objects: 3693, done.
  remote: Compressing objects: 100% (3525/3525), done.
  remote: Total 3693 (delta 38), reused 527 (delta 27), pack-reused 0 Receiving objects: 100% (3693/3693), 3.04 MiB | 79.00 KiB/s, done. Resolving deltas: 100% (38/38), done.
  From <https://github.com/Homebrew/homebrew>

  [new branch] master -> origin/master HEAD is now at 9c41fb8
  update man page ==>Installation successful! ==>
  Next steps Run `brew help` to get started
```
##  [gem](https://rubygems.org/pages/download)(ruby程序的包管理器类似Homebrew) [国内源站](https://gems.ruby-china.org)

gem                                                   | 相关命令
----------------------------------------------------- | ------------------------------------------------
查看源列表                                                 | gem sources -l
将源移除                                                  | gem sources --remove <https://rubygems.org/>
添加国内源                                                 | gem sources --add <https://gems.ruby-china.org/>
缓存                                                    | gem sources -u
查看gem安装环境（gem命令安装的软件在 GEM PATHS 中的lib path目录gems文件夹下） | gem environment

##  [jekyll](https://www.jekyll.com.cn) (git page)

```
 gem install jekyll
```


##  MAC下如何显示隐藏文件
```
//在终端上输入以下命令 > defaults write com.apple.finder AppleShowAllFiles -bool true
//重新启动Finder > 使用快捷键 Command + Option + esc
或者执行 killall Finder命令 这样就可以显示隐藏文件了。
//不显示隐藏文件的命令为 > defaults write com.apple.finder AppleShowAllFiles -bool false
同样也需要东西启动Finder才生效。
```

##  secureCRT
```
1、下载破解文件 securecrt_mac_crack.pl
2、在终端执行命令。（请注意对应文件的目录） sudo perl ~/Downloads/securecrt_mac_crack.pl /Applications/SecureCRT.app/Contents/MacOS/SecureCRT
等到出现了 crack successful就可以了，终端不要关闭
3、打开SecureCRT，点击Enter License Data.. 输入刚才终端的数据就完成了破解，
破解信息在刚才终端窗口，你的可能和我的不一样，以终端显示的为准。
（请注意逐一拷贝，不要一次拷贝）
Name: bleedfly Company: bleedfly.com
Serial Number: 03-29-002542
License Key: ADGB7V 9SHE34 Y2BST3 K78ZKF ADUPW4 K819ZW 4HVJCE P1NYRC
Issue Date: 09-17-2013
```
## redis

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

##  mac下 idea debugger [加载慢的问题](https://stackoverflow.com/questions/20658400/intellij-idea-hangs-while-finished-saving-caches)

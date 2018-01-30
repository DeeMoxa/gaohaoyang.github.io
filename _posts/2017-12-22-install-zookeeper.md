---
layout: post
title: 安装zookeeper
tags: [zookeeper,mac]
---


  安装及部署  
1、 下载zookeeper二进制安装包  
  ● 下载（以3.3.6为例）  
curl -L -O http://mirror.bit.edu.cn/apache/zookeeper/zookeeper-3.3.6/zookeeper-3.3.6  
  ● 解压  
tar zxvf zookeeper-3.4.6.tar.gz    
2、 设置JAVA_HOME环境变量（zookeeper本身由Java编写，运行在java环境，故必须先装好java环境，才可以继续zookeeper的安装）  

{% highlight java %}
@echo off
REM Licensed to the Apache Software Foundation (ASF) under one or more
REM contributor license agreements.  See the NOTICE file distributed with
REM this work for additional information regarding copyright ownership.
REM The ASF licenses this file to You under the Apache License, Version 2.0
REM (the "License"); you may not use this file except in compliance with
REM the License.  You may obtain a copy of the License at  
REM  
REM     http://www.apache.org/licenses/LICENSE-2.0  
REM  
REM Unless required by applicable law or agreed to in writing, software
REM distributed under the License is distributed on an "AS IS" BASIS,
REM WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
REM See the License for the specific language governing permissions and
REM limitations under the License.
set JAVA_HOME="C:\Program Files\Java\jdk1.7.0_75"

if not defined JAVA_HOME (  
  echo Error: JAVA_HOME is not set.  
  goto :eof  
)  

if not exist %JAVA_HOME%\bin\java.exe (  
  echo Error: JAVA_HOME is incorrectly set.  
  goto :eof  
)  

set JAVA=%JAVA_HOME%\bin\java

setlocal
call "%~dp0zkEnv.cmd"

set ZOOMAIN=org.apache.zookeeper.server.quorum.QuorumPeerMain
echo on
call %JAVA% "-Dzookeeper.log.dir=%ZOO_LOG_DIR%" "-Dzookeeper.root.logger=%ZOO_LOG4J_PROP%" -cp "%CLASSPATH%" %ZOOMAIN% "%ZOOCFG%" %*

endlocal

{% endhighlight %}

3、 配置  
配置文件存放在$ZOOKEEPER_HOME/conf/目录下，将zoo_sample.cfd文件名称改为zoo.cfg, 缺省的配置内容如下：

{% highlight java %}
# The number of milliseconds of each tick

# The number of ticks that the initial
# synchronization phase can take

# The number of ticks that can pass between
# sending a request and getting an acknowledgement

# the directory where the snapshot is stored.
# do not use /tmp for storage, /tmp here is just
# example sakes.

# the port at which the clients will connect

#
# Be sure to read the maintenance section of the
# administrator guide before turning on autopurge.
#
# http://zookeeper.apache.org/doc/current/zookeeperAdmin.html#sc_maintenance
#
# The number of snapshots to retain in dataDir
#autopurge.snapRetainCount=3
# Purge task interval in hours
# Set to "0" to disable auto purge feature
#autopurge.purgeInterval=1

tickTime=2000
initLimit=10
syncLimit=5
dataDir=/usr/local/zookeeperLog
clientPort=2183
server.1=127.0.0.1:2881:3881
server.2=127.0.0.1:2882:3882
server.3=127.0.0.1:2883:3883

{% endhighlight %}

● 配置说明：（上文为集群配置）  
tickTime：这个时间是作为 Zookeeper   服务器之间或客户端与服务器之间维持心跳的时间间隔，也就是每个 tickTime 时间就会发送一个心跳。  
dataDir：顾名思义就是 Zookeeper   保存数据的目录，默认情况下，Zookeeper   将写数据的日志文件也保存在这个目录里。  
dataLogDir: log目录, 同样可以是任意目录.   如果没有设置该参数, 将使用和dataDir相同的设置。该目录下存在myid文件需要指向服务名称（以上述配置为例）

{% highlight java %}
server.1=127.0.0.1:2881:3881 //服务器1的myid文件内容为1
server.2=127.0.0.1:2882:3882//以此类推
server.3=127.0.0.1:2883:3883
{% endhighlight %}

（若是本地模拟集群，需要建立三个不同的myid文件）  
clientPort：这个端口就是客户端连接 Zookeeper   服务器的端口，Zookeeper   会监听这个端口，接受客户端的访问请求。  
5、 启动Zookeeper  
当这些配置项配置好后，你现在就可以启动zookeeper了：
linux启动
```
./zkServer.sh start
```
Mac启动
```
zkServer  status
zkServer  start
```

查看zookeeper端口
```
netstat -tunlp|grep 2181
```
（快速查看zookeeper启动可使用jps命令，如下图QuorumPeerMain即为zookeeper成功启动进程）

```
$ jps
26480 QuorumPeerMain
26694 Jps
19256 Bootstrap
26425 QuorumPeerMain
26365 QuorumPeerMain
```

停止  
```
./zkServer.sh stop
```

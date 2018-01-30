---
layout: post
title: 分布式
permalink: /posts/distributed/
excerpt: "分布式概念简述..."
tags:
  - distributed
---
**wiki:** Distributed computing is a field of computer science that studies distributed systems. A distributed system is a model in which components located on networked computers communicate and coordinate their actions by passing messages.[1] The components interact with each other in order to achieve a common goal. Three significant characteristics of distributed systems are: concurrency of components, lack of a global clock, and independent failure of components.[1] Examples of distributed systems vary from SOA-based systems to massively multiplayer online games to peer-to-peer applications.
{: .notice--info}  

那么维基的描述到底说了什么，_分布式系统是由一组通过网络进行通信，协调合作工作的计算机节点组成的系统。_  

关键字：网络通信、协作。
{: .notice--success}   

分布式系统的理念是把业务模块分而治之，多台计算机分别完成一部分业务，他们之间通过网络交互，从而完成业务需求。那么每一小段业务，你可以通过一台或者多台计算机完成，这样当访问量增多时，分布式的优点逐渐体现出来：  
1. 不同的业务分支并不会相互影响，并且他们独立完成自己负责的业务部分，极大的提高了性能。
2. 每块业务可以部署多台计算机，提升的系统的可用性。

- 看上去很美好  
在了解了分布式之后，看样子可以提升系统运行速度了、可以解决高并发问题了，然而随之而来的是分布式各模块之间通信，各节点出现突发情况依然需要保证系统运行的[难题](https://en.wikipedia.org/wiki/Fallacies_of_distributed_computing)。
- 看起来是单机
分布式说起来只是针对系统设计者，用户并不关心你用什么实现的，对于他们来说，看起来就是个整体。
A distributed system is a collection of independent computers that appears to its users as a single coherent system.　
{: .notice--info}
之前说到，单一业务可以部署多台计算机，那么当请求来临时，到底要使用具体哪个节点，这时衍生出一个新的概念--[负载均衡]({{ site.url }}{{ site.baseurl }}/posts/loadbalancer/)

分布式特性  
- 可扩展性：分布式系统的根本目标就是为了处理单个计算机无法处理的任务，当任务增加的时候，分布式系统的处理能力需要随之增加。简单来说，要比较方便的通过增加机器来应对数据量的增长，同时，当任务规模缩减的时候，可以撤掉一些多余的机器，达到动态伸缩的效果

- 可用性与可靠性：一般来说，分布式系统是需要长时间甚至7*24小时提供服务的。可用性是指系统在各种情况对外提供服务的能力，简单来说，可以通过不可用时间与正常服务时间的必知来衡量；而可靠性而是指计算结果正确、存储的数据不丢失。

- 高性能：不管是单机还是分布式系统，大家都非常关注性能。不同的系统对性能的衡量指标是不同的，最常见的：高并发，单位时间内处理的任务越多越好；低延迟：每个任务的平均时间越少越好。这个其实跟操作系统CPU的调度策略很像

- 一致性：分布式系统为了提高可用性可靠性，一般会引入冗余（复制集）。那么如何保证这些节点上的状态一致，这就是分布式系统不得不面对的一致性问题。一致性有很多等级，一致性越强，对用户越友好，但会制约系统的可用性；一致性等级越低，用户就需要兼容数据不一致的情况，但系统的可用性、并发性很高很多。

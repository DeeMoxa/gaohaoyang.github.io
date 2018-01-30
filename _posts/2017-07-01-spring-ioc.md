---
layout: post
title: "spring原理-IOC"
excerpt: ""
tag: spring
---
- IOC容器实现：  
核心思想：依赖对象的获得被反转了。  
[BeanFactory核心接口](http://docs.spring.io/spring/docs/current/javadoc-api/)：
由接口一系列方法可以了解container的基本模型，对bean的不同检索方法即为container最基本入口
![]({{ site.url }}{{ site.baseurl }}/images/component/spring-ioc01.png)

由上图深入理解spring介绍的两种IOC设计主线

1. BeanFactory-->HierarchicalBeanFactory-->ConfigurableBeanFactory
2. BeanFactory-->ListableBeanFactory-->ApplicationContext-->拓展WebApplicationContext、ConfigurableApplicationContext等

BeanFactory设计原理   
BeanFactory接口提供了IOC容器的规范，并且提供了符合IOC的一系列容器。

- IOC的基本操作（参考XmlBeanFactory、WebApplicationContext实现过程）
1. 创建IOC配置文件的资源，即获取资源（BeanDefinition）的方式
2. 创建一个BeanFactory或其子类
3. 创建一个资源读取器(e.g. XmlBeanDefinitionReader)
4. 定义好资源位置并且读入配置信息，通过（XmlBeanDefinitionReader解析），这时IOC Container就建立起来了。
- [ApplicationContext api](http://docs.spring.io/spring/docs/current/javadoc-api/)

前文类举的XmlBeanFactory是一类简单拓展的BeanFactory的ioc容器。
ApplictionContext为高级形态的IOC  
1. 扩展MessageSource接口，表示可以支持国际化实现
2. Resource提供了资源访问能力
3. 继承ApplicationEventPublisher为上下文提供了事件机制，给bean在生命周期中提供了更多的便利
4. 在具备这些功能后，一般更建议ApplicationContext作为IOC的基本形式    

- IOC初始化过程
通过refresh()方法（实例Container启动时，需要创建的方法参考），它标志着IOC正式启动，具体包含BeanDefinition的定位、载入、注册（Spring目前将这三个过程用不同的模块实现，为用户提供更加灵活的调用和拓展）
1. 定位 查找数据的过程
2. 载入 把实体Bean转换成IOC内部的数据结构即BeanDefinition
载入时，IOC若已经启动，则它做一个类似重启的操作，将从各中resouces[]加载的Resource并解析为Document对象，同时记录载入Bean的数量。在完成document对象解析之后，接着按照Spring的Bean规则由（documentReader）进行解析e.g. <bean></bean>等常见元素信息。
3. 注册 BeanDefinition会被注入到一个HashMap（理论容量：[1073741824个元素](https://stackoverflow.com/questions/7554106/hashmap-absolute-capacity)）中，IOC会通过这个HashMap持有BeanDefinition

注册的调用过程，如下图：

- IOC容器的依赖注入  
上面所说的IOC初始化过程，不包含Bean注入的实现。IOC实现中这是两个独立过程，依赖注入的过程是用户第一次向IOC容器所要Bean时触发的（用户可以通过lazyinit微控bean被加载的时机，实现预实例化）详情可查询各容器的 getBean()方法，以触发依赖注入

依赖注入的过程，如下图：
![]({{ site.url }}{{ site.baseurl }}/images/component/spring-ioc02.png)

在Bean的创建和对象依赖注入的过程中，需要依据BeanDefiniton中的信息来递归的完成依赖注入。这些递归都是以getBean为入口，以下不区分顺序：  
1. 在上下文体系中查找需要的bean和创建Bean的递归调用。
2. 依赖注入时，通过递归调用容器的getBean，得到当前Bean的依赖Bean同时出发对依赖Bean的创建和注入。
3. 对Bean的属性进行注入解析时，也是一个递归过程。
- 对于容器本身的初始化及销毁
e.g. BeanFactory/ApplicationContext  


![]({{ site.url }}{{ site.baseurl }}/images/component/spring-ioc03.png)
由上图可以看到，ApplicationContext启动过程是在AbstractApplicationContext中实现的。在使用上下文时，要在preparedFactory()中做一些准备工作。
同样，容器关闭时，也需要一系列工作，由doClose()完成。在这个方法中，发出关闭容器的信号，然后将bean逐个关闭，最后关闭容器自身。

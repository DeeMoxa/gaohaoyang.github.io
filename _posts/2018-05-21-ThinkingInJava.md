---
layout: post
title: ThinkingInJava
excerpt: "读书笔记"
tags: se
categories: java
---
[JLS官方](https://docs.oracle.com/javase/specs/jls/se8/html/index.html)  
- 第一章
1. 对象本身就是容纳了很多动作的集合
2. 三个概念，组合has-a，纯粹代替is-a，拓展基类like-a
3. 多态的体现之一，动态绑定（面向对象后期的绑定）
4. [容器]({{ site.url }}{{ site.baseurl }}/posts/se)
5. GC的关键作用
6. [并发]({{ site.url }}{{ site.baseurl }}/posts/thread)
7. [CGI](https://en.wikipedia.org/wiki/CGI) Java与网络的桥梁
8. applet?  
[第一章体现的设计思想]({{ site.url }}{{ site.baseurl }}/posts/principle)
- 第二章
1. [JVM内存模型]({{ site.url }}{{ site.baseurl }}/posts/jvm/)
2. _作用域概念_ 每个对象都拥有自己的scope，但是引用在作用域的终点就会消失（即我们在超出作用域范围后，无法访问该对象），然而new()创建的对象却会一直保留。为了避免内存溢出则体现了GC的重要性。（回看第一条）
3. _static关键字是脱离了对象的,且是所有对象共享_，正式因为这样的特点，main方法才能成为程序的入口。
Java程序初始化的三原则：  
1）静态对象优先于非静态  
a.静态对象只初始化一次   
b.静态对象在被调用时、所属类被初始化的时候或者才会在初始化   
c.非静态对象可能初始化多次  
2）父类优于子类  
3）按成员变量顺序  
4）成员优于构造器被调用  
- 第三章
1. <span id="thinkinjava-3">老生常谈的值传递</span>
要说明这个问题，先要明确两点：  
1） 不要试图与C进行类比，Java中没有指针的概念  
2） 程序运行永远都是在栈中进行的，因而参数传递时，只存在传递基本类型和对象引用的问题。不会直接传对象本身。    
看懂以上两点，可以明白Java在方法调用传递参数时，由于没有指针，<span style=" background:yellow">所以它都是进行传值调用</span>（这点可以参考C的传值调用）。所以，很多书里面都说Java是进行传值调用，这点没有问题。  
<span style=" background:yellow">
_但是传引用的错觉是如何造成的呢？  
在运行栈中，基本类型和引用的处理是一样的，都是传值，所以，如果是传引用的方法调用，也同时可以理解为“传引用值”的传值调用，即引用的处理跟基本类型是完全一样的。但是当进入被调用方法时，被传递的这个引用的值，被程序解释（或者查找）到堆中的对象，这个时候才对应到真正的对象。如果此时进行修改，修改的是引用对应的对象，而不是引用本身，即：修改的是堆中的数据。所以这个修改是可以保持的了_
</span>
2. ++a、-\-a先运算自增、减运算，再表达式运算  
a++、a-\-先表达式运算再自增、减（尤其在赋值时注意）  
3.     
category | operator                            | related
:------- | :---------------------------------- | :------
一元       | + + - ！〜                            | 左到右
乘性       | * /％                                | 左到右
加性       | + -                                 | 左到右
移位       | >> >>> <<                           | 左到右
关系       | >> = << =                           | 左到右
相等       | == !=                               | 左到右
按位与      | ＆                                   | 左到右
按位异或     | ^                                   | 左到右
按位或      | \|                                  | 左到右
逻辑与      | &&                                  | 左到右
逻辑或      |             \|   \|                 | 左到右
条件       | ？：                                  | 从右到左
赋值       | =+  =- =* =/ =％ =>> =<<  =＆ =^ =\| =       | 从右到左
逗号       | ，                                   | 左到右  

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;上表优先级由上到下递减
- 第四章  
主要内容是迭代，在多层迭代嵌套中会遇到 _label:_(冒号是关键)
```
public static void main(String[] args) {
       int[] m = {1, 2, 3, 4};
       int[] n = {4, 5, 6};
       SignLabel:
       for (int i : m) {
           for (int j : n) {
               if (j == i) {
                   continue SignLabel;
               }
               System.out.println(j);
           }
       }
}
```
- 第五章
1. [GC回收对象]({{ site.url }}{{ site.baseurl }}/posts/jvm/#垃圾回收算法)
2. 第二章初始化顺序内容回顾
3. 自定义枚举

```
// 当我们创建了Coin实例PENNY等，那么必须有参数列表对应的构造函数
enum Coin {
    PENNY(1), NICKEL(5), DIME(10), QUARTER(25);
    Coin(int value) { this.value = value; }

    private final int value;
    public int value() { return value; }
}
```
- 第六章
- 第七章  
1. protected关键字，在设计基类时，它是主要设计供子类使用的方法。
2. final关键字  
1）之前并不被熟知的概念，当final修饰基本数据类型引用时，使数值不变  
2）当final修饰对象引用时，尽管也使引用恒定不变，然而引用所指向的对象确实可以被修改的。（此概念可以类比值传递）  
3）blank final 我们知道final关键字修饰的declare必须在使用前被初始化，但是它允许declare时未指定值。那么这样为我们提供了一种新的功能，即在使用时在动态给final域赋值。  
4）final修饰方法参数，那么意味着无法修改参数引用指向的对象  
5）private方法隐式指定为final的，由于无法获取private方法，自然也无法覆盖。  
6）final修饰类无法被继承（慎用）

```
public class Test {
    final int i = 1;
    final int j;
    public Test(int j) {
        this.j = j;
    }
    @Override
    public String toString() {
        return "Test{" + "i=" + i + ", j=" + j + '}';
    }
    public static void main(String[] args) {
        System.out.println(new Test(2));
    }
}
```
- 第八章   
1. 从多态的概念，引申出了基类与子类对于private的method覆盖的问题，以下阐述问题。[基类申明了private修饰的方法，我们知道private成员无法被本类外的其他类访问，但是子类依然可以访问，并且试图重写该方法的话，那么无法覆盖该方法。](https://stackoverflow.com/questions/4716040/do-subclasses-inherit-private-fields)这里标注重点是因为多态会有个缺陷，由于访问域时（除方法外），这个访问在编译期就会解析。
2. 初始化的实际过程有一个重要条件，_在任何事物发生之前，将分配给对象的存储空间初始化为零_（因此，编写构造器的时候，有一条准则，用尽可能简单的方法使对象进入正常状态，如果可以的话，避免调用其他的方法。在构造器中唯一可以安全调用的那些方式使基类中的final方法）
3. java语言中的包装类属于多态的范畴吗？
- 第九章  

| abstract     | interface     |
| :------------- | :------------- |
|extend   |implement   |
| 有abstract方法，一定是抽象类；反之，抽象类可以没有抽象方法       | 域都是final、static修饰的，所以域必须被初始化|
|   | 访问修饰符都是public的   |
|  抽象方法要被实现，所以不能是静态的，也不能是私有的 |   |
|单根继承   |多实现   |

1. 在设计层面，抽象类的概念要远大于接口<a href ="{{ site.url }}{{ site.baseurl }}/images/thinkinginjava/interface-abstract.png">接口与抽象</a>
2. 适配器模式  
3. 工厂模式    

- 第十章 内部类  
创建内部对象：  
1）非静态，必须用外部类对象+点+new关键字  
2）静态，可以直接new  
3）如果内部类为private修饰的，其他类将无法访问（阻止依赖类型的编码）  
4）对比匿名内部类的好处是可以实例化    

- 第十三章 String  
1. 在<a href="#thinkinjava-3">第三章</a>的时候我们讨论过Java 中值传递的问题，这一章进一步加强这个概念，<span style="color:red">当字符串作为方法参数时，它只会在方法运行时存在（称为局部引用），一旦方法运行结束，则局部引用随即消失。</span>
2. 

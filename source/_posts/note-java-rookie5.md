title: Java新手的通病(5)——对虚拟机(JVM)了解不足
date: 2015-04-28 14:17:11
categories: 笔记
tags: [java]
---

##*1.关于基本类型和引用类型*

###　　*1.1 这两种类型在内存存储上有什么区别？*

基本类型有int,float,double,char,byte,boolean,short,long,存储在栈内存。引用类型其引用地址存栈内存，实际对象存堆内存。

###　　*1.2 这两种类型在性能上有什么区别？*

堆相对进程来说是全局的，能被所有线程访问，而栈是线程局部的，只能本线程访问。申请堆内存的复杂度和时间开销比栈要大很多。从栈里面申请内存，虽然又简单又快，但是栈的大小有限，分配不了太多内存。

###　　*1.3 这两种类型对于GC有什么区别？*

栈当中分配的内存一般离开方法区就会销毁（全局变量除外），gc会遍历对象的引用，找到垃圾对象（没有引用可到达），一般来讲堆内存比较紧张时，jvm会调用gc，但是gc执行的时间点是无法准确预知的。所以引用型对象不光创建时开销大，销毁时也是有开销的。

##*2.关于垃圾回收（Garbage Collection）*

###　　*2.1 GC是如何判断哪些对象已经失效？*

遍历所以引用，那些可达的对象为有效对象，反之则为无效对象。

###　　*2.2 GC对性能会有哪些影响？*

1.造成当前线程的停顿。2.遍历所以对象引用的开销。3.清理和回收垃圾的开销。

###　　*2.3 如何通过JVM的参数调优GC的性能？*

jvm运行时图:
![jvm](http://www.myexception.cn/img/2012/09/20/0015554597.jpg)

 按照官方的说法：“Java 虚拟机具有一个堆，堆是运行时数据区域，所有类实例和数组的内存均从此处分配。堆是在 Java 虚拟机启动时创建的。”“在JVM中堆之外的内存称为非堆内存(Non-heap memory)”。可以看出JVM主要管理两种类型的内存：堆和非堆。简单来说堆就是Java代码可及的内存，是留给开发人员使用的；非堆就是JVM留给自己用的，所以方法区、JVM内部处理或优化所需的内存(如JIT编译后的代码缓存)、每个类结构(如运行时常数池、字段和方法数据)以及方法和构造方法的代码都在非堆内存中。 

评价gc性能的两个指标；吞吐量和停顿时间。吞吐量指**jvm不用于gc的时间占总时间的比**。吞吐量越大越好，停顿时间越小越好。

```xml
-Xms　　设置初始堆内存
-Xmx　　设置最大堆内存
-Xmn　　设置年轻代的大小
-XX:NewRatio=n　　设置年轻代与年老代的比例为“n”
-XX:NewSize=n　　设置年轻代大小为“n”
```

设置合理的堆内存。（gc分为①主要收集同时清理年老和年轻代，②次要收集只针对年轻代）

**一般来说，jvm默认的参数已经够用，如要调整，要确保真的比默认性能好。**


##*3.关于字符串*

> 对于Java提供的String和StringBuilder，想必很多人都知道：String用于常量字符串，StringBuilder用于可变字符串。那Java当初为什么要这样设计捏？为啥不用一个类来统一搞定捏？



##*4.关于范型（Generic Programming）*

###　　*4.1 GP是在编译时实现的还是在运行时实现的？为什么要这么实现？*

###　　*4.2 GP的类型擦除机制是咋回事？有啥优点/缺点？*

###　　*4.3 使用范型容器（相对于传统容器）在性能上有啥影响？为什么？*


##*5.关于多线程*

###　　*5.1 synchronized关键字是怎么起作用滴？*

###　　*5.2 synchronized的作用域如何？是针对某个类还是针对某个类对象实例？*

###　　*5.3 synchronized对性能有没有影响？为什么？*

###　　*5.4 volatile关键字又是派啥用滴？啥时候需要用这个关键字捏？*

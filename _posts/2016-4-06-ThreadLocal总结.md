---
layout: post
title: ThreadLocal总结
description: ThreadLocal总结
categories: Java
keywords: java, ThreadLocal, github, 博客, m5jun
---


# 核心思想

空间换时间

# 实现原理
ThreadLocal并不是代表当前线程，ThreadLocal其实是采用哈希表的方式来为每个线程都提供一个变量的副本。
从而保证各个线程间数据安全。
每个线程的数据不会被另外线程访问和破坏，我们可以简单理解为它就是一个隐式的包含在你启动的线程中的变量，在这个线程执行的生命周期中它如影随形。

* ThreadLocal如何实现维护线程的变量的呢？

    Java每个Thread中都存在一个Map， Map的类型是ThreadLocal.ThreadLocalMap。
    Map中的key为一个threadlocal实例，value为用户需要保存的值，注意这个ThreadLocalMap是在ThreadLocal中定义的。

# 注意
* 子线程不能获取到父线程的变量；
* ThreadLocal的子类 InheritableThreadLocal可以获取到父线程的变量；
* 在使用线程池的过程中，由于线程是不断的回收和利用，故ThreadLocal在服务器中也是被反复利用的，在使用中如果不进行清查操作，很容易导致变量污染；

# 思考
* 为什么ThreadLocal.set(T value)的值由Thread对象来缓存，为什么不像自定义实现那样放在ThreadLocal中？

    一种解释是出于性能考虑。因为如果放在ThreadLocal中，由于多线程操作同一个Map对象，将不得不加锁保护。而将value直接放在Thread对象中，不同的线程有各自的Thread对象，因此也就无需加锁。
    
* 既然value存放在Thread中，为什么不直接由Thread提供getter/setter接口，而需要额外有一个ThreadLocal类？

    value虽然和Thread之间存在映射关系，但是不属于Thread的属性，放在ThreadLocal更多是性能上的考虑，因此由Thread提供getter/setter并不适合。

# 内存泄露问题

如下图所示，其中实线表是强引用，虚线表示弱引用。
ThreadLocal使用本身实例作为key将用户的值保存在Thread的ThreadLocalMap中，但ThreadLocalMap使用WeakReference来保持对ThreadLocal的实例的gc的支持。
因此，当ThreadLocal实例为空时,又没有任何强引用指向ThreadLocal实例，ThreadLocal将被JVM垃圾收集器回收，而其中value值又没有被回收，它将成为一个不能被访问且又不被回收的区域。
只有Thread结束后，Map，value和CurrentThread等将会被完全回收。但如果这些长期存活在线程中，则会出现我们常说的“内存泄露”现象。

![threadLocal](/images/posts/java/threadlocal.png)
     
# ThreadLocal与 synchronized的联系与区别

* ThreadLocal通过为每个线程维护自己的变量，使得每一个线程可以独立的改变自己的变量，而不会影响其他线程，从而隔离了多个线程共享数据，保证了线程的安全。
这种方式内存开销大，访问速度快，使用简单；
* Synchronized通过利用锁的机制，使变量或代码块在某一时该只能被一个线程访问，从而实现多线程对资源的并发访问控制。
这种方式速度相对较慢，实现复杂；
* ThreadLocal采用了以空间换时间的方式，Synchronized采用了以时间换空间的方式处理多线程的并发问题。
* ThreadLocal主要应用于线程间数据隔离，解决多线程中因并发引起的数据不一致问题。
* Synchronized则用于线程间的数据共享，解决多线程共享数据同步的问题。





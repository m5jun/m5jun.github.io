---
layout: post
title: Java集合HashMap总结
description: Java集合HashMap总结
categories: Java
keywords: 集合, HashMap, Java, github, 博客, m5jun
---

Java里HashMap是用得最多的数据结构之一，现总结如下：

# 特点
* HashMap允许使用null值和null键；
* 采用链接法散列；

# 数据结构
如下图所示，HashMap底层就是一个数组结构，数组中的每一项又是一个链表。
当新建一个HashMap的时候，就会初始化一个数组。

![HashMap结构](/images/posts/mysql/hashmap.png)

# 思考
* HashMap为什么数组的长度都是2的倍数？

为了快速定位数组的索引， 用这种解法执行效率更高 h & (length - 1)。

* HashMap为什么扩容比较耗性能？

当HashMap中的元素个数超过数组大小*loadFactor时，就会进行数组扩容，默认情况下，数组大小为16，那么当HashMap中元素个数超过16 * 0.75=12的时候，
就把数组的大小扩展为 2 * 16=32，即扩大一倍，需要重新创建一个新size的HashMap存储空间，然后重新计算每个元素在数组中的位置。
这是一个非常消耗性能的操作，所以如果我们已经预知HashMap中元素的个数，那么预设元素的个数能够有效的提高HashMap的性能。

# Fail-Fast机制
* HashMap不是线程安全的，因此如果在使用迭代器的过程中有其他线程修改了map，那么将抛出ConcurrentModificationException，这就是所谓fail-fast策略。
* 由所有HashMap类的“collection 视图方法”所返回的迭代器都是快速失败的：在迭代器创建之后，如果从结构上对映射进行修改，除非通过迭代器本身的 remove 方法，
其他任何时间任何方式的修改，迭代器都将抛出 ConcurrentModificationException。
因此，面对并发的修改，迭代器很快就会完全失败，而不会冒着在将来不确定的时间发生任意不确定行为的风险。     


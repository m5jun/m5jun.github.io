---
layout: post
title: 解决在mac系统中idea中不能查看jdk6的javadoc问题
description: 解决在mac系统中idea中不能查看jdk6的javadoc问题
categories: JDK
keywords: jdk6, javadoc, mac
---

## 问题描述
我发现在mac系统中，在intellij idea中无法查看java6的javadoc，jdk版本是1.6.0_65，例如我想查看HashMap的put方法，点开put方法的源代码如下：

```java
  public V put(K var1, V var2) {
        if(var1 == null) {
            return this.putForNullKey(var2);
        } else {
            int var3 = hash(var1.hashCode());
            int var4 = indexFor(var3, this.table.length);

            for(HashMap.Entry var5 = this.table[var4]; var5 != null; var5 = var5.next) {
                if(var5.hash == var3) {
                    Object var6 = var5.key;
                    if(var5.key == var1 || var1.equals(var6)) {
                        Object var7 = var5.value;
                        var5.value = var2;
                        var5.recordAccess(this);
                        return var7;
                    }
                }
            }

            ++this.modCount;
            this.addEntry(var3, var1, var2, var4);
            return null;
        }
    }
```

## 原因分析
在网上也查了些资料，发现在/Library/Java/JavaVirtualMachines/1.6.0.jdk/Contents/Home下没有src.jar源代码的包导致的，低版本jdk6是没有的。

## 解决方案
升级版本，需要去apple developer下载更高的版本，下载地址： https://developer.apple.com/downloads/, 我下载的版本是：Java for OS X 2013-005 Developer Package, 下载完安装后，在idea的project structure中添加你刚才安装的jdk版本，再打开put方法就能清晰地看见源代码了。

```java
    /**
      * Associates the specified value with the specified key in this map.
      * If the map previously contained a mapping for the key, the old
      * value is replaced.
      *
      * @param key key with which the specified value is to be associated
      * @param value value to be associated with the specified key
      * @return the previous value associated with <tt>key</tt>, or
      *         <tt>null</tt> if there was no mapping for <tt>key</tt>.
      *         (A <tt>null</tt> return can also indicate that the map
      *         previously associated <tt>null</tt> with <tt>key</tt>.)
      */
    public V put(K key, V value) {
         if (key == null)
             return putForNullKey(value);
         int hash = hash(key.hashCode());
         int i = indexFor(hash, table.length);
         for (Entry<K,V> e = table[i]; e != null; e = e.next) {
             Object k;
             if (e.hash == hash && ((k = e.key) == key || key.equals(k))) {
                 V oldValue = e.value;
                 e.value = value;
                 e.recordAccess(this);
                 return oldValue;
             }
         }
 
         modCount++;
         addEntry(hash, key, value, i);
         return null;
    }
```

 



















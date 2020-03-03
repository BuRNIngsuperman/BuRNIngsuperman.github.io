---
title: 深入理解HashMap
date: 2020-03-03 19:42:37
tags:
    - Java
    - 集合
categories:
    - Java基础
---

最近为了准备面试看了一些面经，发现集合的问题问的比较多，尤其是HashMap的底层实现原理。之前学习集合的时候只知道怎么用HashMap，因此写了这篇博客记录一下关于集合类HashMap的底层原理及相关应用。<!--more-->

## HashMap底层原理

HashMap的底层数据结构在JDK1.8中是由数组、链表和红黑树实现的。它是基于Hash表的概念存储数据的。它是实现了集合的Map接口。

初始情况下，一般是由数组作为hash表中的桶(bin)，所有HashCode相同的键值会存放在同一个桶中，其中桶内的结构是采用链表结构。但是当桶内的结点个数大于预定的阈值8时，桶内的数据结构就会变为红黑树，这样会加快桶内数据的查询时间。

至于为什么阈值会设置为8就会有很多考虑，通常我们理解就是当结点阈值为8时，红黑树的查找时间复杂度为O(lg8) = o(3)，而链表的平均查找时间复杂度为O(8/2)=O(4)，这时使用红黑树会加快查询速度。

Collections的synchronizedMap方法使HashMap具有同步的能力，或者使用ConcurrentHashMap。

### HashMap的put()的方法

查看HashMap的源码，对put()方法进行分析

```java
   public V put(K key, V value) {
        return putVal(hash(key), key, value, false, true);
    }
    /**
     * Implements Map.put and related methods.
     *
     * @param hash hash for key
     * @param key the key
     * @param value the value to put
     * @param onlyIfAbsent if true, don't change existing value
     * @param evict if false, the table is in creation mode.
     * @return previous value, or null if none
     */
    final V putVal(int hash, K key, V value, boolean onlyIfAbsent,
                   boolean evict) {
        Node<K,V>[] tab; Node<K,V> p; int n, i;
        //判断Hash中数组为空,就重新初始化数组
        if ((tab = table) == null || (n = tab.length) == 0)
            n = (tab = resize()).length;
        //根据键的hash值利用散列表分配数组，如果数组为空则直接放入包含键值对的结点
        if ((p = tab[i = (n - 1) & hash]) == null)
            tab[i] = newNode(hash, key, value, null);
        else {
            Node<K,V> e; K k;
            //根据key相等确定链表的头结点相等，就作替换
            if (p.hash == hash &&
                ((k = p.key) == key || (key != null && key.equals(k))))
                e = p;
            // 结构为红黑树就插入树中
            else if (p instanceof TreeNode)
                e = ((TreeNode<K,V>)p).putTreeVal(this, tab, hash, key, value);
            else {
                // 将Node插入合适的位置
                for (int binCount = 0; ; ++binCount) {
                    if ((e = p.next) == null) {
                        p.next = newNode(hash, key, value, null);
                        if (binCount >= TREEIFY_THRESHOLD - 1) // -1 for 1st
                            treeifyBin(tab, hash);
                        break;
                    }
                    if (e.hash == hash &&
                        ((k = e.key) == key || (key != null && key.equals(k))))
                        break;
                    p = e;
                }
            }
            if (e != null) { // existing mapping for key
                V oldValue = e.value;
                if (!onlyIfAbsent || oldValue == null)
                    e.value = value;
                afterNodeAccess(e);
                return oldValue;
            }
        }
        ++modCount;
        if (++size > threshold)
            resize();
        afterNodeInsertion(evict);
        return null;
    }

```

**步骤①**：若哈希table为null，或长度为0，则做一次扩容操作； 
**步骤②**：根据index找到目标bucket后，若当前bucket上没有结点，那么直接新增一个结点，赋值给该bucket；
**步骤③**：若当前bucket上有链表，且头结点就匹配，那么直接做替换即可；
**步骤④**：若当前bucket上的是树结构，则转为红黑树的插入操作；
**步骤⑤**：若步骤①、②、③、④都不成立，则对链表做遍历操作。
  a) 若链表中有结点匹配，则做value替换；
  b）若没有结点匹配，则在链表末尾追加。同时，执行以下操作：
    i) 若链表长度大于`TREEIFY_THRESHOLD`，则执行红黑树转换操作；
    ii) 若**条件i)** 不成立，则执行扩容resize()操作。
以上5步都执行完后，再看当前Map中存储的k-v对的数量是否超出了**`threshold`**，若超出，还需再次扩容。

### 根据key值找到散列表中的索引

#### 计算key的hash值

```java
static final int hash(Object key) {
    int h;
    return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
}
```

我们根据key的hashCode方法得出HashCode值作为输入得到hash值，这采取了一种高位运算的算法。

从以上知识点，我们可以得到一个**推论**：
**对于两个对象，若其hashCode相同，那么两个对象的hash值就一定相同。**

一般情况下，HashCode的值是根据对象的内存地址得出来的。

这里还牵涉到另外一个知识点。对于HashMap中key的类型，必须满足以下的条件：
若两个对象逻辑相等，那么他们的hashCode一定相等，反之却不一定成立。

例如对于String对象，它的HashCode实现就是根据内容来实现的。所以equals相等内容的对象HashCode值就相等。

#### 取模操作

我们根据key的hash值获得相应的数组索引操作是根据取模得出来的。取模操作是应用在put方法中

```java
(n - 1) & hash //hash表长度与hash值做与运算
```

将HashMap的数组长度-1与hash值做**与**运算，这样做不仅效果和取模操作差不多，而且还大大提高了性能。

这里的操作也和为什么设置HashMap的长度为2的幂次方有关。假如HashCode的倒数第二第三位从0变成了1，但是运算的结果都是1001。也就是说，当HashMap长度为10的时候，有些index结果的出现几率会更大，而有些index结果永远不会出现（比如0111）！

这样，显然不符合**Hash算法均匀分布**的原则。

反观长度16或者其他2的幂，Length-1的值是所有二进制位全为1，这种情况下，index的结果等同于HashCode后几位的值。只要输入的HashCode本身分布均匀，Hash算法的结果就是均匀的

### 处理散列表中的碰撞冲突

我们在每个bucket数组中都以链表或者红黑树的数据结构存放。在同一个bucket中存放的数据量较少的时候，我们会采用链表的结构存放，在数据量较大时，会采用红黑树的数据结构存放。

## HashMap中hash算法中几个问题

```java
// 重新计算hash值    
static final int hash(Object key) {
        int h;
        return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
    }
```

```java
//计算Hash数组中bucket位置
(n - 1) & hash
```

> 这个哈希算法中：
>
> ^ 表示按异或运算，只要位不同就为1，不然为0
>
> `>>>`无符号右移，右边补0

为什么要用无符号右移16位后做异或运算的解答和为什么HashMap长度要是2的幂次方的长度这里有个非常好的解答

[Hash算法的一些问题](https://www.cnblogs.com/zxporz/p/11204233.html)

## HashMap和HashTable的区别

HashTable和HashMap同样是基于哈希表实现的，同样每个元素都是一个key-value对，但是在解决链表冲突问题上，HashMap在JDK8.0上是使用单链表或者红黑树来解决的，而HashTable只使用单链表。

### 继承的父类不同

HahshMap继承自AbstractMap类，而HashTable继承自Dictionary类，但是两者都实现了Map接口

### 线程安全性不同

HashMap不是线程同步的

如果集合应用中不需要线程安全，那么推荐使用HashMap；如果需要高度的线程安全，那么推荐使用`java.util.concurrentHashMap`来代替HashTable。

### key和value是否可以为空

### Hash值不同

在HashTable的底层代码中，Hash值是直接使用对象的HashCode方法得出的，而HashMap则利用对象的HashCode方法重新计算了Hash值

## Java中的Serialization机制和transient关键字

简单来说，Java中的Serialization机制就是指类或者基本的数据类型持久化(persistence)到数据流中(Stream)中，包括文件、字节流、网络数据流。

Java中实现Serialiazation主要依靠两个类:ObjectOutputStream和ObjectInputStream。他们是JavaIO系统里OutputStream和InputStream的子类。既然他们是JavaIO的子类，那么就可以像操作流一样操作他们。

[Java的Serialization机制](https://www.iteye.com/blog/zzy1943-634418)

[Java的关键字transient](https://blog.csdn.net/flynetcn/article/details/2142020)

## 静态内部类的加载时机

1. 外部类初次加载，会初始化静态变量、静态代码块、静态方法，但不会加载内部类和静态内部类。
2. 实例化外部类，调用外部类的静态方法、静态变量，则外部类必须先进行加载，但只加载一次。
3. 直接调用静态内部类时，外部类不会加载
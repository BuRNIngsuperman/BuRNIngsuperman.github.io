---
title: 深入理解Object
date: 2020-03-03 19:46:04
tags:
    - Java
categories:
    - Java基础
---

object类是Java所有类的始祖，也就是它们的超类，在Java中所有的类都是由它拓展而来的。子类都会继承所有Object类的public方法。<!--more-->

## 前言

首先我们先来看看Object类的结构

![Object类结构](https://burningblog.oss-cn-shanghai.aliyuncs.com/img/20200115113808.png)

可以使用Object类型的变量来引用任何类型的对象

```java
Object obj = new Employee("Harry",50000)
```

当然，Object类型的变量只能作为对象各种值的通用持有者，想要对其中的内容进行具体的操作，需要对其进行强制的类型转换。

```java
Employee e = (Employee) obj
```

在Java中，只有基本类型不是变量，所有的数组类型，不管是对象数组还是基本类型的数组都扩展了Object类。

在Objct类中一般有以下三种方法需要我们理解，并且一般需要重写。

## equals方法

```java
public boolean equals(Object obj) {
     return (this == obj);
 }
```

Object类的equals方法用于检测一个对象是否等于另一个对象。在Object类中，这个方法将判断两个类变量是否有相同的引用，如果两个对象有着相同的引用，则这两个变量一定相等。但对于大多数类来说这判断并没有什么意义，因此子类需要重写equals方法来获得更准确的相等信息。

Java语言要求equals方法具有以下特性：

1. **自反性**: 对于任何非空引用x,x.equals(x)都是true。
2. **对称性**：对于任何引用x和y，当且仅当y.equals(x)返回true，那么x.equals(y)也应该返回true。
3. 传递性：对于任何引用x、y和z，如果x.equals(y)返回true，y.equals(z)返回true，那么x.equals(z)也应该返回true。
4. 一致性：如果x和y引用的对象没有发生变化，反复调用x.equals(y)应该返回相同的结果
5. 对于任何非空引用x,x.equals(null)都应该返回false。

所以等于相等性测试中继承问题，我么那就可从这两方面考虑：

- 如果子类能够拥有自己的相等概念，那么对称性需求将强制使用getClass方法进行检测
- 如果由超类决定相等的概念，那么就可以使用instanceof方法进行检测，这样可以在不同子类的对象之间进行相等的比较。

## hashCode方法

```java
public native int hashCode();
```

散列码（hash code）是由对象导出的一个整形值。equals方法与hashCode方法必须一致：如果x.equals(y)返回true，那么x.hashCode()就必须与y.hashCode()具有相同的值。

但是Object类中的HashCode()方法返回的hashcode值是与对象所在的内存地址绑定的，所以对于一些对象我们需要重写HashCode方法，让一些规则上相同但是内存地址不同的对象可以拥有相同的hashCode()方法返回的值。

> 如果重新定义eauals方法，那么就必须重新定义hashCode方法，以便用户可以将对象插入到散列表中。

## toString方法

```java
public String toString() {
    return getClass().getName() + "@" + Integer.toHexString(hashCode());
  }
```

返回一个String对象，一般子类会重写该方法以获得更准确的字符串表达。默认的返回格式为：对象的class名称+@+hashcode的十六进制字符。

## clone方法

```java
protected native Object clone() throws CloneNotSupportedException;
```

该方法实现的是对象的浅复制，只有实现了Cloneable接口的对象才能调用该方法，否则会抛出CloneNotSupportedException异常。

默认的clone方法只是浅拷贝，只能拷贝对象的引用地址，而不会重新分配内存再将要拷贝的对象复制过来。深拷贝则会把引用的对象本身也一起重新创建。

## finalize方法

```java
protected void finalize() throws Throwable { }
```

该方法为保护方法，主要用于在GC的时候被再次调用，如果我们实现了该方法，对象可能在这个方法中再次复活，从而避免了被GC回收。
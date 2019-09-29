---
title: Java学习笔记一
date: 2019-09-11 22:46:37
tags:

categories: 
    - Java
---

## equals和==的区别
- equals方法是检测两个字符串是否相等
- ==只能够确定两个字符串是否在同一位置，如果放置在同一位置就必然相等。

## String、StringBuffer和StringBuilder有什么区别

- String是Java中重要而且基础的类，String类的对象是不可变的字符串，无法唔该字符串中的字符，只能通过拼接修改

- StringBuffer就是为了解决大量拼接字符而产生许多中间问题而提供的一个类。它的本质是一个线程安全的可修改的字符序列。但保证了线程安全就会需要鑫能的代价。

- StringBuffer就是可以让字符串任意拼接而不需要线程安全





---
title: JSP就是这么简单（入门篇）
date: 2019-12-01 17:03:50
tags:
categories:
---

## 什么是JSP

JSP的全称是Java Server Pages,即Java服务器页面。JSP是基于程序的页面其特点是HTML和Java语言并存，JSP页面可以看作在html页面中嵌入Java代码

<!--more-->
> HTML的全称是Hyper Text Markup Language,是一种常见的前端静态页面语言。

静态与动态：

- 不用 和是否有“动感”混为一谈
- 是否随着时间、地点、用户操作的改变而改变

动态网页需要使用到服务端脚本语言（JSP）

## 为什么需要JSP

JSP是为了简化Servelet的工作而出现的替代品，Servelet输出前端HTML页面,JSP就是为了替代Servelet输出HTML页面的。
要了解JSP的原理，我们需要先简单地了解Servelet和网络的工作原理，以在Tomcat开发为例。

## 常见的服务器架构架构

BS： Browser Server

CS：Client Server

## 页面状态码

200： 一切正常

404： 资源不存在

300/301： 页面重定向

403： 权限不足（如果访问a目录，但是a目录设置不可见）

500：服务器内部错误（代码有误）

其他代码：需要积累

## tomcat指定首页

在项目/WEB-INF/web.xml中写入
```xml
<welcome-file-list>

​	<welcome-file>index.jsp</welcome-file>

</welcome-file-list>
```
就会指定index.jsp为首页


## 虚拟路径

将web项目配置到webapps以外的目录：

1. 方式一

conf/server.xml

host标签中:
```xml

<Context docBase=" " path=" " />
```
> docBase:实际路径
>
> path: 虚拟路径（绝对路径、相对路径【相对于webapps】)

缺点：需要重启

2. 方式二

conf/Catalina/localhost/新建项目名.xml

然后再中间写入方式一中语句。

> 如果写入Root.xml就可以不用访问项目名
不需要重启

## 虚拟主机

每一个主机在访问互联网中的域名时，会访问互联网中的域名解析器（DNS）根据域名-IP映射关系找到域名对应的IP地址进行访问。但是在访问互联网中的DNS之前，会首先访问本机的域名解析器，因此可以通过虚拟主机访问某个域名就访问到本机。
通过修改Tomat里面conf/sever.xml文件中Host标签：
```
<Host appBase="项目路径" name="域名名称">
<Context docBase="项目路径" path="url路径虚拟路径">
</Host>

```
然后在Engine中默认defaultHost改为相应的`www.test.com`,最后在driver中host文件加入相应的域名-IP映射。然后就可以访问该域名进而访问到对应的主机。
>将端口号改为80后就可以认为时默认端口，访问域名时就不用加端口号

> 流程：
> www.test.com -> host找映射 -> server.xml找Engine的defaultHost -> 通过“/”映射到对应的项目地址
> 
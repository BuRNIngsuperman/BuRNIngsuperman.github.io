---
title: Jsp与Tomcat入门
date: 2019-12-01 17:03:50
tags:
categories:
---
## JSP：动态网页

jsp页面可以看作在html页面中嵌入Java代码，嵌入符号：<%和%>

静态与动态：

- 不用 和是否有“动感”混为一谈
- 是否随着时间、地点、用户操作的改变而改变

动态网页需要使用到服务端脚本语言（JSP）

## 架构

BS： Browser Server

CS：Client Server

## 创建状态码

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




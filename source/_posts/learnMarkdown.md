---
title: Markdown语法总结
toc: true
date: 2019-04-28 09:28:49
categories:
    - Markdown
tags: 
    - Markdown
---
#  什么是Markdown
Markdown是一种可以使用普通文本编辑器编写的标记语言，通过类似HTML的标记语法，它可以使普通的文本内容具有一定的格式。
<!--more-->
Markdown的目标是实现【易读易写】。写这样一篇语法汇总，旨在可以直观呈现效果和对应的语法格式。你可以把它作为自己在使用 Markdown 初期时手头的参考资料，希望对你开始或者熟练使用 Markdown 能有所帮助。
#  Markdown使用语法总结
##  标题
代码：
```markdown
# 一级标题
## 二级标题
### 三级标题
#### 四级标题

# 一级标题 #
## 二级标题 ##
### 三级标题 ###
#### 四级标题 ####
```

##  改变字体
粗体：Markdown中，用一对**表示强调，被强调的文字以粗体显示。
斜体：Markdown中，用一对*表示斜体
代码：
```md
*要改变字体为斜体的内容*
**要改变字体为粗体的内容**
***要改变字体为斜体+粗体字的内容***
~~删除文字~~
```
显示效果：
>*要改变字体为斜体的内容*
>**要改变字体为粗体的内容**
>***要改变字体为斜体+粗体字的内容***
>~~删除文字~~


## 分割线
可以在一行中用三个以上的星号、减号、底线来建立一个分隔线，同时需要在分隔线的上面空一行。
代码：
```md
--------
********
```

效果：
**********



##  代码工具
###  行内代码
代码：
```md
`这是行间代码`
`This is inline code`
在 Java 输出 Hello, world ：`System.out.print("Hello, World!");`
在 Java 输出 Hello, world ：```System.out.print("Hello, World!");```
```

效果：

`这是行间代码`
`This is inline code`
在 Java 输出 Hello, world ：`System.out.print("Hello, World!");`
在 Java 输出 Hello, world ：```System.out.print("Hello, World!");```

### 多行代码
>语法：
>方法一：只需要每行都缩进 4 个空格即可
>方法二：使用```框起来 

示例一（方法一）：

```md
    // JQuery 的 Hello, world
    $(function(){
    alert("Hello, world!")
    });
```

效果：

    // JQuery 的 Hello, world
    $(function(){
    alert("Hello, world!")
    });

示例二（方法二）【/为转义字符忽略不计】：

```
// JQuery 的 Hello, world
$(function(){
alert("Hello, world!")
});
```

### 代码高亮
在需要高亮的代码块的前一行及后一行使用三个反引号```，同时第一行反引号后面表面代码块所使用的语言。如下面一段代码：
>#include"iostream.h"
>using namespace std;
>
>in main(){
>    cout << "Hello World"; // 输出 Hello World
>    return 0;
>}

显示效果：
```cpp
#include<iostream>
using namespace std;

in main(){
    cout << "Hello World"; // 输出 Hello World
    return 0;
}
```


## 列表

1.在Markdown的语法中，我们可以使用1.、2.表示有序列表，
即数字+点，不过注意序号和后面文字必须得有空格。  
2.无序列表用“-”或者“*”表示，同样的，-和*和后年的文字间必须有空格。且通过按tab键可以实现列表的嵌套

### 无序列表

代码：
```md
- 这是无序列表
- 这是无序列表
    - 这是无序列表
    - 这是无序列表
- 这是无序列表
- 这是无序列表
* 这是无序列表
* 这是无序列表
```
效果：
- 这是无序列表
- 这是无序列表
    - 这是无序列表
    - 这是无序列表
- 这是无序列表
- 这是无序列表
* 这是无序列表
* 这是无序列表

### 有序列表
代码:
```md
1. 这是有序列表
2. 这是有序列表
3. 这是有序列表
4. 这是有序列表
```
效果：
1. 这是有序列表
2. 这是有序列表
3. 这是有序列表
4. 这是有序列表

备注：
> 两个列表之间不能相邻，否则会解释为嵌套的列表
无序列表的项目符号可使用 *,+,- 效果是相同的。
列表与后续内容之间需要一个空行隔开，即：列表是一个段落
列表允许多层次嵌套
可以在项目中包含段落，只需将段落前添加一个 tab 或 4 个空格
区块标记：是指内容独占一块，需前后换行，不和其他标记共处一行的标记。
段落：即是一段连续的文字，可包含*、空格、换行、tab等字符。两个段落之间使用空行分隔。注意：换行不是分段的标识，空行才是.


## 引用
Markdown中，用符号>开启一行引用，如果文字有多行则用多个>，同样地>和文字间必须有空格。
这里补充一点：在Markdown中，空格+空格+Enter为换行，
而Enter为另起一段落，这在书写引用时是非常重要的

代码：
```md
> 这是引用  
> 换行请按`空格+空格+Enter`  
> 这是引用
```
效果：
> 这是引用  
> 换行请按`空格+空格+Enter`  
> 这是引用

大于号 和 文字必须有一个空格
可以在每行之前加 > ，也可以在段落之前加 1 个 >
引用内部可以使用其他 Markdown 标记


## 链接（链接网址/图片）

### 链接网址
代码：
```md
[ 百度网址 ](www.baidu.com/)
```
效果：
[ 百度网址 ](www.baidu.com/)

### 链接图片
代码：
```md
![天气预报]（http://cdn.heweather.com/cond_icon/309.png）    
备注：括号一定要是英文字符哦！
```

效果：
![天气预报](http://cdn.heweather.com/cond_icon/309.png)


### 限制图片大小并居中
```md
许多 MarkDown 编辑器中直接按原图大小显示图片，造成版面凌乱。  
如何让图片像简书中那样大小一致居中显示呢？使用该命令  
<img src="图片地址" width="图片显示宽度" height="显示高度"  
alt="图片名称"/>设置图片大小，再用<div align=center></div>命令包裹达到居中效果。
```
代码：
```md
<div align=center>
<img src="http://ww2.sinaimg.cn/bmiddle/88070423gw1ep30aw8an7g204d04gkgd.gif" width="400" height="400" alt="亦菲表演机器猫"/>
</div>
```
显示效果：
<div align=center>
<img src="http://ww2.sinaimg.cn/bmiddle/88070423gw1ep30aw8an7g204d04gkgd.gif" width="400" height="400" alt="亦菲表演机器猫"/>
</div>


## Markdown和HTML的关系
HTML 是一种发布的格式，Markdown 是一种书写的格式。
Markdown 的格式语法只涵盖纯文本可以涵盖的范围。
在 Markdown 中可直接使用 HTML 标签，但需要注意：
> - 对于 HTML 区块元素――如 div、table、pre、p 等标签，必须在前后加上空行与其它内容区隔开，还要求它们的开始标签与结尾标签不能用制表符(tab)或空格来缩进
> - HTML 的行内标签——如 span、cite、del 可以在 Markdown 的段落、列表或是标题里随意使用.
> - 在 HTML 的区块标签中的 Markdown 标签是没有效果的
---
title: '[译]stylus文档翻译 - 3.变量'
tags:
  - stylus
categories:
  - 前端
  - 翻译
date: 2017-11-01 11:46:51
---

变量
===

我们可能会定义一个变量，然后在样式中去使用它。

```
font-size = 14px

body
    font font-siz Arial,sans-serif
```

编译为:

```
body{
    font：14px Arial,sans-serif;
}
```

变量甚至可以组成表达式列表

```
font-size = 14px
font=font-size "Lucida Grande",Arial

body
    font font,sans-serif
```

编译为:

```
body{
    font:14px "Lucida Grande",Arial,sans-serif;
}
```

<!--more-->

定义(变量名，函数等)可以包含`$`字符，如下面这个例子:

```
$font-size = 14px
body{
    font:$font-size sans-serif;
}
```

## 查找属性

另一个Sytlus独有的很有意思的功能是能够不需要指定变量去直接引用属性。一个很好的例子是垂直和水平居中元素所需要的逻辑(通常用百分比和负边距，如下所示):

```
#logo
    position:absolute
    top:50%
    left:50%
    width:2=150px
    height:h=80px
    margin-left:-(w/2)
    nargin-top:-(h/2)
```

我们使用`@`符号以及属性名就可以代替之前的`w`和`h`:

```
#logo
    position:absolute
    top:50%
    left:50%
    width:150px
    height:80px
    margin-left:-(@width/2)
    margin-top:-(@height/2)
```

另一中用是在已有的属性中有条件的定义属性值，下面这个例子我们应用一个当`z-index`未指定时`z-index`默认为1:

```
position()
    position:arguments
    z-index:1 unless @z-index

#logo
    z-index:20
    position:absolute

#logo2
    position:absolute
```

属性查找才采取冒泡的方式向上查找直到找到或者到栈顶仍未找到返回`null`，在下面例子中`color`将被解析为`blue`:

```
body
    color:red
    ul
        li
            color:blue
            a
                background-color:@color
```

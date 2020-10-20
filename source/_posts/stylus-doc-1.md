---
title: '[译]stylus文档翻译 - 1.介绍'
tags:
  - stylus
categories:
  - 翻译
  - 前端
date: 2017-11-01 11:43:10
---

健壮的、动态的、富有表现力的CSS
===

## CSS需要一个英雄
```
body {
  font: 12px Helvetica, Arial, sans-serif;
}
a.button {
  -webkit-border-radius: 5px;
  -moz-border-radius: 5px;
  border-radius: 5px;
}
```

## 如果赶走括号
```
body
  font: 12px Helvetica, Arial, sans-serif;

a.button
  -webkit-border-radius: 5px;
  -moz-border-radius: 5px;
  border-radius: 5px;
  ```

<!--more-->

  ## 或者冒号分号也消失掉
  ```
  body
  font: 12px Helvetica, Arial, sans-serif

a.button
  -webkit-border-radius: 5px
  -moz-border-radius: 5px
  border-radius: 5px
  ```

## 来点干货
```
border-radius()
  -webkit-border-radius arguments
  -moz-border-radius arguments
  border-radius arguments
  
body
  font 12px Helvetica, Arial, sans-serif
  
a.button
  border-radius(5px)
  ```

## 清晰明了的混合书写
```
border-radius()
  -webkit-border-radius: arguments
  -moz-border-radius: arguments
  border-radius: arguments

body
  font: 12px Helvetica, Arial, sans-serif

a.button
  border-radius: 5px
  ```

## 创造和分享
```
@import 'vendor'

body
  font: 12px Helvetica, Arial, sans-serif

a.button
  border-radius: 5px
```

## 或者是语言函数
```
sum(nums...)
	sum = 0
	sum += n for n in nums
sum(1 2 3 4)
 //=>10
```

## 如果这些都是可选的呢
```
  fonts = helvetica, arial, sans-serif

body {
  padding: 50px;
  font: 14px/1.4 fonts;
}
```

## 获取Stylus

安装Stylus在Nodejs下是非常简单的，请确保你安装了Node的包管理工具npm，通过他们获取二进制文件就可以完成安装。

现在在你的终端中输入:

```
$ npm install stylus -g 
```

下面将是这个富有表现力的css语言的功能列表，如果你想更深入的了解它请前往[Github](https://github.com/stylus/stylus)地址。

## 功能列表

* 可选的冒号
* 可选的分号
* 可选的逗号
* 可选的大括号
* 变量
* 插值
* 混合书写
* 计算
* 强制类型
* 动态导入
* 条件判断
* 迭代
* 嵌套选择器
* 父集引用
* 变量函数调用
* 词汇作用域
* 内置函数(超过60个)
* 语言功能
* 可选的压缩
* 可选的图像内联
* 可执行的Stylus
* 健壮的报错
* 单行或多行注释
* CSS迭代在一些困难的时候
* 字符转义
* TextMate包
* 以及更多!


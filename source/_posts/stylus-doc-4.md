---
title: '[译]stylus文档翻译 - 4.运算符'
tags:
  - stylus
categories:
  - 前端
  - 翻译
date: 2017-11-01 11:53:38
---

运算符
===

## 运算符优先级

下面是运算符优先级表，从高到低：

```
.
[]
!~+-
is defined
** * / %
+ -
... ..
<= >= < >
in
== is != is not isnt
is a 
&& and || or
?:
= := ?= += -= 
not
if unless
```

## 一元表达式

下面是一些可用的一元运算符，`!`,`not`,`-`,`+`,and`～`。

```
!0
// => true

!!0
// => false

!1
// => false

!!5px
// => true

-5px
// => -5px

--5px
// => 5px

not true
// => false

not not true
// => true
```

<!--more-->

逻辑符`not`的优先级较低，因此

```
a = 0
b = 1

!a and !b
// => false
//parsed as: (!a) and (!b)
```

看下面

```
not a or b
// => false
// parse as :not (a or b)
```

##　二元运算符

下标运算符允许我们去通过索引获取表达式内部值(从零开始)，负值可以从表达式中最后的元素开始。

```
list = 1 2 3 
list[0]
// => 1

list[-1]
// => 3
```

括号表达式可以充当元组(例如`(15px,15px),(1,2,3)`):

下面这个例子是一个使用元组的错误处理(并展示结构的多功能)

```
add(a,b)
    if a is a 'unit' and b is a 'unit'
        a + b
    else
        (error 'a and b must be units!)

body
    padding add(1,'5')
    // => padding:error "a and b must units"

    padding add(1,'5')[0]
    // => padding:error;

    padding add(1,'5')[0] == error
    // => padding:true;

    padding add(1,'5')[1]
    // => padding:"a and b must units"
```

这是一个更复杂的例子.现在我们执行内置函数`error()`，当标识符(传入的第一个值)等于`error`时返回错误信息。

```
if(val = add(1,'5'))[5] == error
    error(val[1])
```

## 范围运算符 .. ...

同事提供这包括(开放)范围运算符(`..`)和排除(闭合)范围运算符(`...`)去扩展表达式:

```
1..5
// => 1 2 3 4 5

1...5
// => 1 2 3 4

5..1
// => 5 4 3 2 1
```

## 加减运算符: + -

二元加减运算符会在运算中将单位进行转换，或使用默认字面值。例如`5s-2px`结果是`3s`。

```
15px -5px
// => 10px

5-2
// => 3 

5in - 50mm
// => 3.3031in

5s - 1000ms
// => 4s

20mm + 5in
// => 121.6in

"foo " + "bar"
// => "foo bar"

"num " + 15
// => "num 15"
```

## 乘法运算符: / * %

```
2000ms + (1s * 2)
// => 4000ms

5s/2
// => 2.5s

4%2
// => 0
```

当使用`/`在一个属性值中，你需要用括号包住它，否则`/`会按字面含义进行处理(支持CSS`line-height`):

```
font: 14px/1.5
```

但是下面等同于`14px`÷ `1.5`:

```
font: (14px/1.5);
```

这仅在`/`运算符时需要.

## 指数: **

下面是指数运算符:

```
2 ** 8
// => 256
```

## 相等和关系运算符 : == != >= <= ><

相等运算符可以用在等同的单位，颜色，字符串甚至标识符上。这是一个强大的概念，甚至任意的标识符(像`wahoo`)可以被当做原子使用。方法可能返回`yes`或`no`来代替`true`和`false`(但这是不建议的).

```
5 == 5
// => true

10 > 5
// => true

#fff == #fff
// => true

true == false
// => false

wahoo == yay
// => false

wahoo == wahoo
// => true

"test" == "test"
// => true

true is true
// => true

'hey' is not 'bye'
// => true

'hey' isnt 'bye'
// => true

(foo bar) == (foo bar)
// => true

(1 2 3) == (1 2 3)
// => true

(1 2 3) == (1 1 3)
// => false
```

只有精确地值才可以匹配.例如,`0 == false` 和 `null = false` 都是 `false`.

别名:

```
== is
!= is not
!= isnt
```

## 真实性

在Stylus中几乎大多数都会被解析成`true`,包括单位后缀，甚至`0%`,`0px`,等都将会被解析为`true`(因为这在Stylus的混合书写和函数中认为单位是有效的是很常见的)

然而,`0`本身在算数中就是`false`。

表达式(或者列表)长度大于1时，被认为是真。

`true`的例子:

```
0%
0px
1px
-1
-1px
hey
'hey'
(0 0 0)
(" ")
```

`false`的例子:
```
0
nullfalser
''
```

## 逻辑运算符: && || and or

逻辑运算符`&&`和`||`的别名是`and`和`or`,它们优先级相同.

```
5 && 3
// => 3

0 || 5
// => 5

0 && 5
// => 0

#fff is a 'rgba' and 15 is 'unit'
// => true
```

## 存在运算符: in

检查运算符左边的内容是否存在于运算符右边的表达式中.

简单的例子:

```
nums = 1 2 3
1 in nums
// => true

5 in nums
// => false
```

一些未定义的标识符:

```
words = foo bar baz
bar in words
// => bar

HEY in words
// => false
```

同样可以用在元组中:

```
vals = (error 'one')(error 'two')
error in vals
// => false

(error 'one') in vals
// => true

(error 'two') in vals
// => true

(error 'something') in vals
// => false
```

在混合书写中的例子:

```
pad(types = padding,n=5px)
    if padding in types
        padding n
    if margin in types 
        margin n

body 
    pad()

body
    pad(margin)

body
    pad(padding margin,10px)
```

解析为:

```
body{
    padding:5px;
}

body{
    margin:5px;
}
body{
    padding:10px;
    margin:10px;
}
```

## 条件赋值 ?= :=

条件赋值运算符`?=`（别名是`:=`)，让我们定义变量不需要去破坏原来的值(如果存在的话)。这个操作运算符可以扩展为三元中的一个`is defined`的二元操作。

例如,下面是相等的：

```
color:=white
color?=white
color = color is denfind ? color : white
```

当使用`=`时，可以简单的赋值:

```
color = white
color = black

color
// => black
```

但是当使用`?=`时，我们第二行就没有效果了(当变量已经被定义):
```
color = white
color ?= black

color
// => black
```

## 实例检查: is a
Stylus提供一个二元操作符是`is a`用于类型检测。

```
15 is a 'unit'
// => true

#fff is a 'rgba'
// => true

15 is a `rgba'
// => false
```

或者我们可以使用内置函数`type()`

```
type(#fff) == `rgba`
// => true
```

注意:`color`是唯一的特殊情况，运算符左侧是`RGBA`或`HSLA`时，他都为`true`.

## 变量定义: is defined

这个伪二元运算符不接收右边的运算符，也不通过左边进行求值，它允许我们检测变量是否被赋值。

```
foo is defined
// => false

foo = 15px
foo is defined
// => true

#fff is deined
// => 'invalid "is defined" check non-variable #fff'
```

当然我们也可以使用`lookup(name)`内置函数去实现这个功能-进行动态查找

```
name = 'blue'
lookup('light-' + name)
// => null

light-blue = #80e2e9
lookup('light-' + name)
// => #80e2e9
```

操作符是必要的，因为一个未定义的变量仍然会返回一个真值，例如：

```
body
    if ohnoes
        padding 5px
```

当未定义时，将会将会产生下面的CSS:

```
body{
    padding:5px;
}
```

当然下面的是安全的:

```
body
    if ohnoes is defined
        padding 5px
```

## 三元运算符

三元运算符会运作的和其它大多数语言中期望的一样。他是唯一一个操作三个元素的运算符(条件表达式，真值表达式，假值表达式)

```
num = 15
num ? unit(num,'px'):20px
// => 15px
```

## 强制转换

作为一个简洁的方式，`(expr)`语法可以用来替代`unit`内置函数进行单位强制转换

```
body
    n = 5
    foo: (n)em
    foo: (n)%
    foo: (n+5)%
    foo: (n*5)px
    foo: unit(n+5,'%')
    foo: unit(5+180/2,deg)
```

## 颜色运算符

可以有一个简洁的方式来操作颜色的组成，例如：我们可以对每个RGB值进行运算：

```
#0e0 + #0e0
// => #0f0
```

另一个例子是通过添加或减去一个百分比值，对亮度值进行调整，使之变亮或变暗：

```
#888 + 50%
// => #c3c3c3

#888 - 50%
// => #444
```

也可以通过添加或者减去一个度数单位(deg)来调整颜色的色调，例如，添加`50deg`来让这红色的值变成黄色:

```
#f00 + 50deg
// => #ffd500
```

色值被适当的固定了。例如，我们`旋转`这色调180度，如果当前值是`320deg`，那么会变成`140deg`。

我们也可以一次调整多个值，使用`rgb()`,`rgba()`,`hsl()`,`hsla()`:

```
#f00 - rgba(100,0,0,0.5)
// => rgba(155,0,0,0.5)
```

## 格式化字符串

字符串格式化运算符`%`可以用来生成字面量值，通过传递参数给内置函数`s()`:

```
`X::Microsoft::Crap(%s)` % #fc0
// => X::Microsoft::Crap(#fc0)
```

多个值需要括号括起来:

```
'-webkit-gradident(%s,%s,%s)' % (linear (0,0)(0 100%))
// => -webkit-gradient(liner,0 0,0 100%)
```
















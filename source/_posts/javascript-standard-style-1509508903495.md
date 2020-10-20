---
title: JavaScript风格规范 - 标准风格
tags:
  - javascript
categories:
  - 前端
  - 随笔
date: 2017-11-01 12:01:43
---

最近看到了eslint，他是为了团队代码规范的检查工具，其中一些风格规范也是代码书写时候可以学习和注意的地方，本文针对[JavaScript Standard Style](http://standardjs.com/rules.html#javascript-standard-style)规范进行总结和整理，你也可以通过链接去阅读原文去进行学习。

---

* #### 使用两个空格进行缩进


```
function hello (name) {
  console.log('hi', name)
}
```

* #### 使用单引号引用字符串,嵌套时使用双引号

```
console.log('hello there')
$("<div class='box'>")
```

* #### 不要有未使用的变量

```
//bad
function myFunction () {
  var result = something()   // ✗ avoid 
}

// Write-only variables are not considered as used.
var y = 10;
y = 5;

// By default, unused arguments cause warnings.
(function(foo) {
    return 5;
})();

//good
var x = 10;
alert(x)
```

<!--more-->

* #### 在关键字后添加空格

```
if (condition) { ... }   // ✓ ok 
if(condition) { ... }    // ✗ avoid 
```

* #### 在方法定义的括号前添加空格

```
function name (arg) { ... }   // ✓ ok 
function name(arg) { ... }    // ✗ avoid 
 
run(function () { ... })      // ✓ ok 
run(function() { ... })       // ✗ avoid 
```

* #### 要记着用`===`去代替`==`

*当检查值为 `null` 或 `undefined`时，可以使用`==`*

```
if (name === 'John')   // ✓ ok 
if (name == 'John')    // ✗ avoid 

if (name !== 'John')   // ✓ ok 
if (name != 'John')    // ✗ avoid 
```

* #### 使用空格去隔开运算符与参数/表达式

```
// ✓ ok 
var x = 2
var message = 'hello, ' + name + '!'

// ✗ avoid 
var x=2
var message = 'hello, '+name+'!'
```

* #### 记得在逗号后面加上空格

```
// ✓ ok 
var list = [1, 2, 3, 4]
function greet (name, options) { ... }

// ✗ avoid 
var list = [1,2,3,4]
function greet (name,options) { ... }
```

* #### 把`else`和大括号放在同一行

```
// ✓ ok 
if (condition) {
  // ... 
} else {
  // ... 
}
// ✗ avoid 
if (condition) {
  // ... 
}
else {
  // ... 
}
```

* #### 如果是多行的`if`判断，记得带上大括号

```
// ✓ ok 
if (options.quiet !== true) console.log('done')
// ✓ ok 
if (options.quiet !== true) {
  console.log('done')
}
// ✗ avoid 
if (options.quiet !== true)
  console.log('done')
```

* #### 记得要处理函数的错误状态参数

```
// ✓ ok 
run(function (err) {
  if (err) throw err
  window.alert('done')
})

// ✗ avoid 
run(function (err) {
  window.alert('done')
})
```

* #### 记得在全局对象前使用`window`  

(*这个我也不太理解,应该是不要使用`alert`应该使用`window.alert`吧*)

*使用document, console 和 navigator时除外*

```
window.alert('hi')   // ✓ ok 
```

* #### 多个连续的空行是禁止的

```
// ✓ ok 
var value = 'hello world'
console.log(value)
// ✗ avoid 
var value = 'hello world'
 
 
console.log(value)
```

* #### 当使用多行的三元运算符时,行首应该是`?`或`:`

```
// ✓ ok 
var location = env.development ? 'localhost' : 'www.api.com'
 
// ✓ ok 
var location = env.development
  ? 'localhost'
  : 'www.api.com'
 
// ✗ avoid 
var location = env.development ?
  'localhost' :
  'www.api.com'
```

* #### 使用var定义时，应该一次定义一个变量而不是多个

```
// ✓ ok 
var silent = true
var verbose = true
 
// ✗ avoid 
var silent = true, verbose = true
 
// ✗ avoid 
var silent = true,
    verbose = true
```

* #### 为条件表达式中的赋值操作添加单独的括号

这样的清晰的表达，他是一个赋值操作而不是将`===`写错成`=`
```
// ✓ ok 
while ((m = text.match(expr))) {
  // ... 
}
 
// ✗ avoid 
while (m = text.match(expr)) {
  // ... 
}
```

* #### 不用在行末添加分号

```
window.alert('hi')   // ✓ ok 
window.alert('hi');  // ✗ avoid 
```

* 不要将`(`,`[`,`出现在行首，他们有可能引起潜在的问题(上一行是一个表达式时,并且结尾没有分号),可以将分号放在行首来解决这个问题

```
// ✓ ok 
;(function () {
  window.alert('ok')
}())
 
// ✗ avoid 
(function () {
  window.alert('ok')
}())
// ✓ ok 
;[1, 2, 3].forEach(bar)
 
// ✗ avoid 
[1, 2, 3].forEach(bar)
// ✓ ok 
;`hello`.indexOf('o')
 
// ✗ avoid 
`hello`.indexOf('o')
```

当然尽可能提高可读的语句是更好的

```
;[1, 2, 3].forEach(bar)

//To

var nums = [1, 2, 3]
nums.forEach(bar)
```

---

合理的代码规范有利于团队写成可读性更高，更加标准规范的代码，但是有时很多的约束和限制中很多是一些华而不实的东西，你不会喜欢因为少了一个空格或多了一个空格时代码报错的感觉。
我觉得很多东西权衡的是一个度的问题，团队应该总结和整理出适用于自己或不同阶段的代码规范，相信这会对团队的成长产生帮助。








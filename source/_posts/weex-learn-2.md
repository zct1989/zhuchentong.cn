---
title: weex学习笔记 - 2.weex的HelloWorld-起手式
tags:
  - weex
categories:
  - 前端
date: 2017-11-01 11:41:50
---

回顾
===
之前说上一篇说了weex基本的环境搭建，那么现在可以开始写一个hello world来帮助开始项目的开发。

[1.weex环境搭建](http://zct1989.oschina.io/2016/07/04/weex1/)

---
1.hello world
---
weex主要项目文件是`.we`文件，他一般包括三个部分：

* template	[必选]
* style	[可选]
* script [可选]

如上面所标记的每个we文件必须包含一个template部分。顾名思义，template即是html代码部分，style就是css部分，script是js业务逻辑对应的部分，当然我过使用过却是发现这和`.vue`文件非常相似，而相似的地方不仅仅是这些，weex中前段部分大量借鉴了vue中的方式，学习过vue的童鞋书写起来也应该十分轻松即可上手，当然如果没学过在做weex先学习一下vue也是让你可以轻松上手。额外说一下vue的文档确实很好，十分清晰明了，weex也应该多多借鉴。

<!--more-->

闲话不谈，来看看代码：

main.we
```
<template>

<container>
    <div class="text-container" style="height:{{deviceHeight}};width:{{deviceWidth}}">
        <text class="text">{{launch_text}}</text>
    </div>
    <wxc-button value="click" onclick="clicked" size="small" style="position:fixed;top:30;right:60"></wxc-button>
</container>

</template>

<style>

.text-container {
    flex-direction: column;
    justify-content: center;
}

.text {
    transform: rotate(90deg);
    writing-mode: vertical-lr;
}

</style>

<script>

require("weex-components");
module.exports = {
    data: {
        launch_text: "H E L L O      W O R L D"
    },
    computed: {
        deviceHeight: {
            get: function() {
                return this.$getConfig().env.deviceHeight;
            }
        },
        deviceWidth: {
            get: function() {
                return this.$getConfig().env.deviceWidth;
            }
        }
    },
    ready: function() {
        console.log("恭喜大侠－helloworld-ready");
    },
    methods: {
        clicked: function() {
            var modal = require('@weex-module/modal');
            modal.toast({'message': 'I am toast!', 'duration': 100});
        }
    }
}

</script>
```

在上一遍的环境里面直接可以运行这个文件，跑起来大概是这个效果：

![picture1](http://7xn3fd.com1.z0.glb.clouddn.com/52665153-1858-425E-A5B0-5E9601180EEA.png)

---
2.基本语法
---
基本语法不想多说什么，学weex的大部分人我认为对vue还是多少都了解的，大家可以看看weex官方的文档

[weex语法文档](http://alibaba.github.io/weex/doc/syntax/main.html)

关于weex特别的一些地方我们来看看。

在template中使用到了 `wxc-button` 这个控件很多人启动的时候发现会发报错这是因为其实 `wxc-button` 是 `weex-components` 模块中的一个组件，使用前需要引用 `weex-components`模块

```
require("weex-components");
```

当然如果你没有这个模块请先安装它。

```
npm install weex-components --save
```

代码中使用的 `weex-components` 包含多个组件,`wxc-button`是其中的一个,这些控件的属性和设置和一些html的button不太一样,比如`wxc-button`的大小设置是通过`size="small/large"`的设置,请看`weex-components`模块的文档：

[weex-components文档](https://www.npmjs.com/package/weex-components)

在style中大部分css语法都是兼容的，但是它并不是完全的css，比如如果设置标签样式是无效的：

```
text{
	background:red;
}
```

你会发现这么做是没有效果的。

css的选择器似乎也不支持：
```
.container .text{
	background:red;
}
```

好吧 css的支持情况多少让人有些失望，我们接着向下看。

$getConfig是内置的API函数，它可以获得一些基本的设备和应用信息。

weex很多功能都进行了模块化的封装,内置模块引用需要添加`@weex-module`前缀,使用`require('@weex-module/name')`可以进行引用。

现有模块包括:

* dom
* steam
* modal
* animation
* webview
* navigator

业务逻辑部分基本和vue一样遍不再多说，想详细了解的可以看看vue的相关文档。

最后再附一张weex生命周期的图，希望便于大家了解weex的生命周期。

![weex生命周期](http://7xn3fd.com1.z0.glb.clouddn.com/201607042130233598.png)
















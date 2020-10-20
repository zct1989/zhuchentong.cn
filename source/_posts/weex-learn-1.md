---
title: weex学习笔记 - 1.weex环境搭建
tags:
  - weex
categories:
  - 前端
date: 2017-11-01 11:37:47
---

开始
===

weex在6月30日晚上已经正式开源(说是六月底果真是月底...)，开始学习了不长时间，一遍学习也一边来总结一些相关的知识。

[weex官网](http://alibaba.github.io/weex/)
[weex文档](http://alibaba.github.io/weex/doc/)

---
1.安装 weex-toolkit
---
首先需要做的就是安装weex-toolkit,这是weex的集成环境。

``` bash
  sudo npm install -g weex-toolkit  
```

有了weex-toolkit就可以使用weex命令了

我使用的版本是0.3.3，这个可能变化的很快

<!--more-->

```bash
# weex --version
info 0.3.3
```

先看一下weex命令

```bash
# weex

info
Usage: weex foo/bar/we_file_or_dir_path  [options]
Usage: weex init

选项：
  --qr          display QR code for native runtime, default action     [boolean]
  -o, --output  transform weex we file to JS Bundle, output path must specified
                (single JS bundle file or dir)
                [for create sub cmd]it specified we file output path
                                                  [默认值: "no JSBundle output"]
  --watch       using with -o , watch input path , auto run transform if change
                happen
  -s, --server  start a http file server, weex .we file will be transforme to JS
                bundle on the server , specify local root path using the option
                                                                        [string]
  --port        http listening port number ,default is 8081         [默认值: -1]
  --wsport      websocket listening port number ,default is 8082    [默认值: -1]
  --np          do not open preview browser automatic                  [boolean]
  -f, --force   [for create sub cmd]force to replace exsisting file(s) [boolean]
  --help        显示帮助信息                                           [boolean]
  -h, --host                                               [默认值: "127.0.0.1"]

for example & more information visit https://www.npmjs.com/package/weex-toolkit
```

如果你只是想调试某个we文件，那么执行weex xxx.we即可以运行在本地浏览器中，当然如果执行

``` bash
weex test.we --qr -h
```

`--qr` 即是显示本地地址文件的二维码，安装playground后既可以扫描二维码看到we文件的页面。
`-h` 是热更新,当然只对浏览器有效(不知道是不是本地的问题还是bug，我开启后每次修改文件都会刷新好多次)

---
2.初始化项目
---

ok,如果你只写一个we文件那么这些准备工作已经足够了，但是如果你是准备新建一个项目，那么这刚刚开始。

weex命令的info有一个命令没有提到，那就是 `weex init`即是初始化一个项目，来执行看一看。

``` bash
# mkdir weex-test & cd weex-test
# weex init
prompt: Project Name:  (weex-test)
file: .gitignore created.
file: README.md created.
file: index.html created.
file: package.json created.
file: src/main.we created.
file: webpack.config.js created.
```

*当然喜欢的话，你也可以使用 `weex create`命令创建项目*

接着安装相关依赖
``` bash
# npm install
```


ok,我们创建了一个目录，然后并且在这里初始化了一个weex项目。

让我们看看他的目录结构

![weex目录](http://7xn3fd.com1.z0.glb.clouddn.com/weex1.png)

src-代码目录
index-浏览器启动页面
webpack.config.js-webpack的配置文件

观察package.json里面有这样四个命令
```
"build": "webpack",
"dev": "webpack --watch",
"serve": "serve -p 8080",
"test": "echo \"Error: no test specified\" && exit 1"
```
我习惯改成
```
"dev": "webpack --watch & serve -p 12580",
```
现在试试启动这个项目

``` bash
# npm run dev

> weex-test@1.0.0 dev /Users/myline/Desktop/project/weex-demo/weex-test
> webpack --watch && serve -p 12580

serving /Users/myline/Desktop/project/weex-demo/weex-test on port 12580
Hash: a620c110038cc680ff1e
Version: webpack 1.13.1
Time: 115ms
  Asset     Size  Chunks             Chunk Names
main.js  2.85 kB       0  [emitted]  main
    + 1 hidden modules
```
现在试着访问一下localhost:12580看看

![weex-page](http://7xn3fd.com1.z0.glb.clouddn.com/weex2.png)

good luck! 看来我们已经启动起来了这个项目。

---
3.一些修改
---
可能事实不如想象的美好，看一下webpack.config.js文件:

``` javascript
require('webpack')
require('weex-loader')

var path = require('path')

module.exports = {
  entry: {
    main: path.join(__dirname, 'src', 'main.we?entry=true')
  },
  output: {
    path: 'dist',
    filename: '[name].js'
  },
  module: {
    loaders: [
      {
        test: /\.we(\?[^?]+)?$/,
        loaders: ['weex-loader']
      }
    ]
  }
}
```
webpack的entry仅仅只有一个main.js文件，这当然不可以，修改一下这个文件:
``` javascript
require('webpack')
require('weex-loader')

var path = require('path');
var fs = require('fs');

var entry = {};

(function walk(dir) {
  dir = dir || '.'
  var directory = path.join(__dirname, 'src', dir);
  fs.readdirSync(directory)
    .forEach(function(file) {
      var fullpath = path.join(directory, file);
      var stat = fs.statSync(fullpath);
      var extname = path.extname(fullpath);
      if (stat.isFile() && extname === '.we') {
        var name = path.join('dist', dir, path.basename(file, extname));
        entry[name] = fullpath + '?entry=true';
      } else if (stat.isDirectory() && file !== 'dist' && file !== 'include') {
        var subdir = path.join(dir, file);
        walk(subdir);
      }
    });
})();

module.exports = {
  entry: entry,
  output: {
    path: '.',
    filename: '[name].js'
  },
  module: {
    loaders: [
      {
        test: /\.we(\?[^?]+)?$/,
        loader: 'weex'
      },
      {
        test: /\.js(\?[^?]+)?$/,
        loader: 'weex?type=script'
      },
      {
        test: /\.css(\?[^?]+)?$/,
        loader: 'weex?type=style'
      },
      {
        test: /\.html(\?[^?]+)?$/,
        loader: 'weex?type=tpl'
      }
    ]
  }
}

//获取当前ip地址
function getIPAddress(){
  var os = require('os');
  var ips = os.networkInterfaces();
  var address ;
  for(var item in ips){
    for(var data in ips[item]){
      var ip = ips[item][data];
      if(ip.address.indexOf('192')==0){
          address = ip.address;
          return address;
      }
    }
  }
}

//生成原生调试二维码
var qrcode = require('qrcode-terminal');
qrcode.generate("http://"+getIPAddress()+":12580/dist/main.js");

console.log("\r\n按住ctrl点击右侧地址打开应用--->http://localhost:12580\r\n");

```
里面进行了对src文件夹的遍历，同时根据当前ip生成了一下main.js文件的二维码(方便playground直接调试)

现在你执行 `npm run dev`的时候,就可以编译src目录下的所有文件了。

---
都是自己学习过程中的一些总结，有问题欢迎指正。

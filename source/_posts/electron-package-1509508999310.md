---
title: electron-packager打包笔记
tags:
  - electron
categories:
  - 随笔
date: 2017-11-01 12:03:19
---

使用`electron-packager`可以方便的打包出对应平台的应用程序，具体指令如下

```
electron-packager 
<sourcedir>
 appname
--platform= <platform> win32,darwin 
--arch=all --version=0.33.7 
--icon=<icon>
--out=./
--overwrite 
--ignore=<ignore>
```
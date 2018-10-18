---
title: GO Web 旅行扎记
comments: true
date: 2018-07-17 17:41:54
updated: 2018-07-17 17:41:54
tags:
    - Go
    - Web
categories: 码文
---
## 第一站 · 出发前先收拾好行囊 &emsp;<font color=gray>——环境搭建</font>

&emsp;&emsp;网上有无数的搭建教程，安装操作流程根据OS操作自然不同，这些参考其他同学的就够了。这里想说的一点就是，几乎所有的都提到了 
**GOROOT**和**GOPATH**，但是原谅我，在座的各位……不不不，下面只是按我的方式理解了下这个东西，你能理解我最好，说明咱们脑回路一个死样子。

![huaji](./huaji.jpeg)  

[安装](https://golang.org/doc/install)官方说的很清楚了，这里几个点特意提出下。

- Linux, Mac OS X, and FreeBSD tarballs下，自定义安装路径后，需要设置GOROOT并且加入系统path中
```bash
export GOROOT=$HOME/go1.X
export PATH=$PATH:$GOROOT/bin
```
- Workspaces工作空间  
&emsp;&emsp;用过Eclipse的盆友们更容易理解这个概念，其实就是GO给你了个空间去做你的事情，而不放在GOROOT里，避免污染GO自己的代码。**所以，别把GOROOT和GOPATH放在一个地方，GOROOT不能包含GOPATH，否则会报错。**

>
A workspace is a directory hierarchy with three directories at its root :
- src contains Go source files,
- pkg contains package objects, and 
- bin contains executable commands.  
The go tool builds source packages and installs the resulting binaries to the pkg and bin directories.
  
  &emsp;&emsp;原文上面自取，工作空间是一个和GOROOT同级的文件夹，主要包含这三级文件夹：
  - src里面是Go的源码文件
  - pkg里面是包对象
  - bin里面是可以执行的Go相关命令  
  go工具构建源码并将生成的二进制文件安装到pkg和bin目录。
  
  
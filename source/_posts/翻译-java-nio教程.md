---
title: '[翻译]java nio教程'
comments: true
date: 2020-03-12 11:31:15
updated: 2020-03-12 11:31:15
tags:
    - java
    - nio
    - 翻译
    - 教程
categories: java
---

{% blockquote %}
原文链接：http://tutorials.jenkov.com/java-nio/index.html
{% endblockquote %}
  
  
Java NIO (New IO)

### Java NIO: 通道Channels and 缓存Buffers

在标准IO API中通过streams流和 character streams字节流工作。NIO中通过 channels 和 buffers工作。
数据从channel读入buffer，或者从buffer写入channel。

### Java NIO: Non-blocking IO 非阻塞IO

一个线程可以先通过channel去去读取数据到buffer里，之后去做其他的事情，一旦数据读入完成，该线程可以继续处理读入数据。写也是一个道理

### Java NIO: Selectors 选择器

监控多个选择器的事件，比如建立连接，数据到达等。所以单线程可以监控多个渠道的数据

## Java NIO 概述

核心概念
    - **Channels**
    - **Buffers**
    - **Selectors**
    Pipe and FileLock等实用类是结合上面三个一起用的。
    
### Channels and Buffers

总的来说，NIO中所有IO行为都通过channel开始。channel有点类似于stream流。
channels把数据读入buffer，buffer把数据写出到channel
主要channel实现类：
    - FileChannel
    - DatagramChannel
    - SocketChannel
    - ServerSocketChannel
    
以上覆盖了UDP + TCP的网络IO和文件IO

主要的Buffer实现类：
    - ByteBuffer
    - CharBuffer
    - DoubleBuffer
    - FloatBuffer
    - IntBuffer
    - LongBuffer
    - ShortBuffer

MappedByteBuffer内存映射文件。

### Selectors

    - 一个Selector能通过一个单线程处理多个Channels。如果你的应用有多个连接（channel）是打开的，这会给你带来极大的方便。
    但是每一个连接都只有低流量的channel。
    - 在channel上注册的selector。
    - 调用select（），阻塞，直到有一个注册的channel准备好。一旦方法返回数据，程序可以继续运行

## Java NIO Channel








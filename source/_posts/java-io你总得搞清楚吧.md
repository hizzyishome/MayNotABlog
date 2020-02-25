---
title: java io你总得搞清楚吧
comments: true
date: 2020-02-20 23:13:10
updated: 2020-02-20 23:13:10
tags:
    - java
    - io
categories: 码文
---
### java.io 包 BIO
    - 基于流模型实现
    - File 抽象、输入输出流
    - 交互方式是同步、阻塞的方式
    - 调用是可靠的线性顺序
    - 简单、直观
    - IO 效率和扩展性存在局限性，容易成为应用性能的瓶颈。
    - java.net 下面部分网络 API:Socket、ServerSocket、HttpURLConnection,同步阻塞 IO 类库.网络通信同样是 IO 行为
    - IO 不仅仅是对文件的操作，网络编程中，比如 Socket 通信，都是典型的 IO 操作目标。
    - 输入流、输出流（InputStream/OutputStream）是用于读取或写入字节的，例如操作图片文件。
    - 而 Reader/Writer 则是用于操作字符，增加了字符编解码等功能，适用于类似从文件中读取或者写入文本信息。
    本质上计算机操作的都是字节，不管是网络通信还是文件读取，Reader/Writer 相当于构建了应用逻辑和原始数据之间的桥梁。
    - BufferedOutputStream 等带缓冲区的实现，可以避免频繁的磁盘读写，进而提高 IO 处理效率。这种设计利用了缓冲区，
    将批量数据进行一次操作，但在使用中千万别忘了 flush。
    - 很多 IO 工具类都实现了 Closeable 接口，因为需要进行资源的释放。比如，打开 FileInputStream，它就会获取相应的文件描述符
    （FileDescriptor），需要利用 try-with-resources、 try-finally 等机制保证 FileInputStream 被明确关闭，
    进而相应文件描述符也会失效，否则将导致资源无法被释放。
### java.nio 包 NIO 
    - Channel、Selector、Buffery1018 等新的抽象
    - 多路复用的、同步非阻塞
    - 高性能数据操作
### NIO 2
    - 异步非阻塞 IO 方式
    - 异步 IO 操作基于事件和回调机制，应用操作直接返回，而不会阻塞在那里，当后台处理完成，操作系统会通知相应线程进行后续工作。
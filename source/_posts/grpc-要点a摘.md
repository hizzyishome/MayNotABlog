---
title: grpc 要点å摘
comments: true
date: 2019-12-09 23:14:26
updated: 2019-12-09 23:14:26
tags:
    - rpc
    - grpc
    - go
categories: 码文
---
1. 浅析入门
    1. RPC框架的目标  
    语言中立性原则构建微服务，不同服务采用不同的语言和技术，对于后端并发处理要求高的微服务，比较适合采用 Go 语言构建，而对于前端的 Web 界面，
    则更适合 Java 和 JavaScript。因此，基于多语言的 RPC 框架来构建微服务，是一种比较好的技术选择。
    例如API 服务编排层和后端的微服务之间采用 gRPC 进行通信
        - 远程服务调用更加简单、透明
        - 屏蔽底层的传输方式（TCP 或者 UDP）
        - 屏蔽序列化方式（XML/Json/ 二进制）
        - 屏蔽通信细节
        - 像调用本地接口一样调用远程的服务
    2. RPC 框架原理
        1. 服务消费者通过微服务的动态代理，序列化请求消息，通过rpc client端发现请求消息，
        通过底层网络传输到rpc server端
        2. server端口拿到请求，反序列化请求消息，通过内部路由接口分发到对应逻辑方法中，
        处理后结果再通过上面类似的方法进行返回。
        3. 结构类似于网络分层
        4. 上面说的目标里，屏蔽的几个方面，都抽象成了对应的层结构
    3. 主流框架
        - Google 的 gRPC
        - Apache（Facebook）的 Thrift
        - 新浪的 Motan
        - 阿里的 Dubbo （还支持服务治理的分布式服务框架）
    4.  gRPC 简介
        - 高性能
        - 开源
        - 通用
        - 面向服务端和移动端
        - 基于 HTTP/2 
        1. gRPC 概览
            -  目前主要支持C、Java 和 Go
        2. gRPC 特点
            - 语言中立，支持多种语言
            - 基于 IDL 文件定义服务，通过 proto3 工具生成指定语言的数据结构、服务端接口以及客户端 Stub
            - 通信协议基于标准的 HTTP/2 设计，支持双向流、消息头压缩、单 TCP 的多路复用、服务端推送等特性，
            这些特性使得 gRPC 在移动端设备上更加省电和节省网络流量；
            - 序列化支持 PB（Protocol Buffer）和 JSON，PB 是一种语言无关的高性能序列化框架，
            基于 HTTP/2 + PB, 保障了 RPC 调用的高性能。
2. gRPC 服务端
    1.  服务端创建业务代码  
    定义服务proto（XXXX.proto）
    ```c
   service Greeter {
     rpc SayHello (HelloRequest) returns (HelloReply) {}
   }
   message HelloRequest {
     string name = 1;
   }
   message HelloReply {
     string message = 1;
   }
    ```
   定义服务端Service,并在50051端口开放服务，并把服务的实现加入
   ```
   private void start() throws IOException {
       /* The port on which the server should run */
       int port = 50051;
       server = ServerBuilder.forPort(port)
           .addService(new GreeterImpl())
           .build()
           .start();
   ```
   服务实现类：
   ```java
    static class GreeterImpl extends GreeterGrpc.GreeterImplBase {
        @Override
        public void sayHello(HelloRequest req, StreamObserver<HelloReply> responseObserver) {
          HelloReply reply = HelloReply.newBuilder().setMessage("Hello " + req.getName()).build();
          responseObserver.onNext(reply);
          responseObserver.onCompleted();
        }
      }
    ```
   2. 服务端创建流程
        1. 创建 Netty HTTP/2 服务端
        2. 将需要调用的服务端接口实现类注册到内部的 Registry 中，RPC 调用时，可以根据 RPC 请求消息中的服务定义信息查询到服务接口实现类；
        3. 创建 gRPC Server，它是 gRPC 服务端的抽象，聚合了各种 Listener，用于 RPC 消息的统一调度和处理。
3. gRPC 服务端

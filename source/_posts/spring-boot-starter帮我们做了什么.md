---
title: spring boot starter帮我们做了什么
comments: true
date: 2020-02-06 21:58:26
updated: 2020-02-06 21:58:26
tags:
    - java
    - spring boot
categories: 码文
---
#### Maven tree 分析
    - spring-boot-starter
        - jakarta.annotation-api(提供了一系列声明式编程通用的注解)
        - snakeyaml(yaml文件解析工具)
        - spring-boot
            - spring-context(提供一个运行时的环境，用以保存各个对象的状态。)
                - spring-aop
                    - spring-beans(Bean 的定义、Bean 的创建以及对 Bean 的解析)
                    - spring-core(定义了资源的访问方式)
                - spring-beans
                    - spring-core
                - spring-core
                - spring-expression(Spring Expression Language)
                    - spring-core
            - spring-core
        - spring-boot-autoconfigure(自动配置)
            - spring-boot
        - spring-boot-starter-logging(日志模块)
            - jul-to-slf4j
            - log4j-to-slf4j
            - logback-classic
        - spring-core
            - spring-jcl(日志)
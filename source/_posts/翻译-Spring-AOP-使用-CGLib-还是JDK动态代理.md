---
title: '[翻译]Spring AOP 使用 CGLib 还是JDK动态代理'
comments: true
date: 2020-02-12 22:34:44
updated: 2020-02-12 22:34:44
tags:
    - java
    - 翻译
    - spring
    - aop
    - 动态代理
categories: java
---
{% blockquote %}
原文链接：http://cliffmeyers.com/blog/2006/12/29/spring-aop-cglib-or-jdk-dynamic-proxies.html
{% endblockquote %}
  

即使你不是面向切面编程的拥趸,但是如果你用到了spring 框架中事务管理的功能,那就意味着你用到了面向切面编程中的动态AOP代理,尽管你并没有主动的显式
使用它.Spring框架可以用CGLIB和JDK动态代理,两个不同的技术手段在运行时创建动态代理.  

如果目标类实现了一个或多个接口,那么Spring将创建一个实现了这个类每个接口的JDK动态代理类.如果目标类没有实现任何一个接口,
Spring将使用CGlib创建一个"继承"目标类的新子类.这个不同的实现方式导致了一个重要的区别,一个JDK动态代理类不能(强)转化为原始目标类,因为它只是
一个碰巧和目标类实现了相同接口的类.若果你在你的程序中使用到了JDK的动态代理特性,也间接推动了你面向接口编程,因为通常是通过这些接口去调用代理类.

另一种方式,如果你的程序里完全没用到接口,spring将创建一个大致能被当成目标类本身使用的Cglib代理类.

当然也有一种不论在什么情况下强制创建Cglib代理的方法.[详情见此.](https://docs.spring.io/spring/docs/2.0.0/reference/aop.html#d0e9015)

如果你在使用spring管理你的服务层的时候,报出了奇怪的类型转换错误,希望这篇文章能给你启发.

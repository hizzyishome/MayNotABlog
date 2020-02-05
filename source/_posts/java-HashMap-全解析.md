---
title: java HashMap 全解析
comments: true
date: 2019-12-13 15:16:26
updated: 2019-12-13 15:16:26
tags:
    - java
    - hash
    - 数据结构
    - HashMap
categories: 码文
---
### 我们分析的HashMap主要以分析java8实现为主

### HashMap概述摘要
继承AbstractMap父类
实现Map<K, V>, Cloneable, Serializable接口

### HashMap的内部数据结构
1. 默认初始大小16
```bash
static final int DEFAULT_INITIAL_CAPACITY = 16;
```
2. 最大容量2^30次方
```bash
static final int MAXIMUM_CAPACITY = 1073741824;
```
3. 默认负载因子0.75
```bash
static final float DEFAULT_LOAD_FACTOR = 0.75F;
```
4. 树化临界值8  
执行put操作的时候，会出现桶碰撞的情况，这时候桶索引值相同的键值对会以一个链表的形式存在于hash桶中，但是当链表长度很长的时候，
查找的性能会很低.当链表的长度超过TREEIFY_THRESHOLD（8）的时候，链表会树化，即通过treeifyBin()方法转换成红黑树。
```bash
static final int TREEIFY_THRESHOLD = 8;
```
5. 去树化临界值6  
节点数小于6的时候，从树结构变回链表
```bash
static final int UNTREEIFY_THRESHOLD = 6;
```
6. 去树化临界值6  
节点数小于6的时候，从树结构变回链表
```bash
static final int MIN_TREEIFY_CAPACITY = 64;
```
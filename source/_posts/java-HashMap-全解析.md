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
- 继承AbstractMap父类
- 实现Map<K, V>, Cloneable, Serializable接口
- 数组（Node[] table）和链表结合组成的复合结构
- 数组被分为一个个桶（bucket），通过哈希值决定了键值对在这个数组的寻址；
- 哈希值相同的键值对，则以链表形式存储，如果链表大小超过阈值（TREEIFY_THRESHOLD, 8），链表就会被改造为树形结构。
- HashMap 的性能表现非常依赖于哈希码的有效性

### 对比Hashtable、HashMap、LinkedHashMap、TreeMap
1. Hashtable
    - 早期 Java 类库
    - 同步的(额外的性能开销)
    - 不支持 null 键和值
2. HashMap
    - 不同步
    - 支持 null 键和值
    - 通常put 或者 get 操作，可以达到常数时间的性能
2. LinkedHashMap
    - 遍历顺序符合插入顺序
    - 维护一个双向链表
    - 通过特定构造函数,创建反映访问顺序的实例,类似于实现removeEldestEntry
3. TreeMap
    - 基于红黑树
    - 提供顺序访问,由键的顺序关系决定
    - get、put、remove 之类操作是 O（log(n)）的时间复杂度
    - 具体顺序可以由指定的 Comparator,或根据键的自然顺序
### HashMap的内部数据结构
1. 默认初始大小16
```bash
static final int DEFAULT_INITIAL_CAPACITY = 16;
```
- 容量和负载系数决定了可用的桶的数量
- 空桶太多会浪费空间，如果使用的太满则会严重影响操作的性能.只有一个桶，那么它就退化成了链表，完全不能提供所谓常数时间存的性能。
- 负载因子 * 容量 > 元素数量,同时它是 2 的幂数.所以并不是简单的上一个list.size!!!

2. 最大容量2^30次方
```bash
static final int MAXIMUM_CAPACITY = 1073741824;
```
3. 默认负载因子0.75
```bash
static final float DEFAULT_LOAD_FACTOR = 0.75F;
```
- 建议不要设置超过 0.75 的数值，因为会显著增加冲突，降低 HashMap 的性能。
- 如果使用太小的负载因子，按照上面的公式，预设容量值也进行调整，否则可能会导致更加频繁的扩容，增加无谓的开销，本身访问性能也会受影响。
4. 树化临界值8  
执行put操作的时候，会出现桶碰撞的情况，这时候桶索引值相同的键值对会以一个链表的形式存在于hash桶中，但是当链表长度很长的时候，
查找的性能会很低.当链表的长度超过TREEIFY_THRESHOLD（8）的时候，链表会树化，即通过treeifyBin()方法转换成红黑树。
```bash
static final int TREEIFY_THRESHOLD = 8;
```
- 树化改造，对应逻辑主要在 putVal 和 treeifyBin 中。
- 为什么 HashMap 要树化呢？本质上这是个安全问题。因为在元素放置过程中，如果一个对象哈希冲突，都被放置到同一个桶里，则会形成一个链表，
我们知道链表查询是线性的，会严重影响存取的性能。

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

7. resize 方法兼顾两个职责，创建初始存储表格，或者在容量不满足需求的时候，进行扩容（resize）。
```java
Node[] tab; Node p; int , i; 
if ((tab = table) == null || (n = tab.length) = 0)
 n = (tab = resize()).length;
 if ((p = tab[i = (n - 1) & hash]) == ull)
 tab[i] = newNode(hash, key, value, nll); 
else { /
/ ... 
if (binCount >= TREEIFY_THRESHOLD - 1)
 // -1 for first 
treeifyBin(tab, hash); 
// ... 
}
```
- 门限值等于（负载因子）x（容量），如果构建 HashMap 的时候没有指定它们，那么就是依据相应的默认常量值。
- 门限通常是以倍数进行调整 （newThr = oldThr << 1），我前面提到，根据 putVal 中的逻辑，当元素个数超过门限大小时，则调整 Map 大小。
- 扩容后，需要将老的数组中的元素重新放置到新的数组，这是扩容的一个主要开销来源。


8. 在放置新的键值对的过程中，如果发生下面条件，就会发生扩容。
```java
if (++size > threshold) resize();
```

9. 具体键值对在哈希表中的位置（数组 index）取决于下面的位运算：
```java
i = (n - 1) & hash
```
不是 key 本身的 hashCode，而是来自于 HashMap 内部的另外一个 hash 方法.  
高位数据移位到低位进行异或运算呢?这是因为有些数据计算出的哈希值差异主要在高位，而 HashMap 里的哈希寻址是忽略容量以上的高位的，
那么这种处理就可以有效避免类似情况下的哈希碰撞。
```java
int h; 
return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>>16);
```
### ConcurrentHashMap
1. Hashtable和synchronizedMap对比
    - Hashtable 等同步容器,Collections.synchronizedMap同步包装器（Synchronized Wrapper）都是粗粒度的同步方式，在高并发情况下，
    性能比较低下
    - Hashtable 本身低效,这是将 put、get、size 等各种方法加上“synchronized”,导致了所有并发操作都要竞争同一把锁.
    一个线程在进行同步操作时，其他线程只能等待，大大降低了并发操作的效率。
    - SynchronizedMap同步包装器,操作虽然不再声明成为 synchronized,但是同步对象是synchronized (mutex) {return m.size();}.没有真正意义上的改进
    - 
2. ConcurrentHashMap 分析
    1. 早期 ConcurrentHashMap
        - 分离锁:将内部进行分段（Segment），里面则是 HashEntry 的数组，和 HashMap 类似，哈希相同的条目也是以链表形式存放。
        - HashEntry 内部使用 volatile 的 value 字段来保证可见性，也利用了不可变对象的机制以改进利用 Unsafe 提供的底层能力，
        比如 volatile access，去直接完成部分操作，以最优化性能，毕竟 Unsafe 中的很多操作都是 JVM intrinsic 优化过的。
        - 在进行并发操作的时候，只需要锁定相应段，这样就有效避免了类似 Hashtable 整体同步的问题，大大提高了性能。
        - Segment 的数量由所谓的 concurrentcyLevel 决定，默认是 16,也可以在相应构造函数直接指定.必须是2 的幂数值
        - 
        - ![早期 ConcurrentHashMap](./1.png)  
        - get操作,需要保证的是可见性，所以并没有什么同步逻辑。
        - put 操作，首先是通过二次哈希避免哈希冲突，然后以 Unsafe 调用方式，直接获取相应的 Segment，然后进行线程安全的 put 操作.
        ConcurrentHashMap 会获取再入锁保证数据一致性,Segment 本身就是基于 ReentrantLock 的扩展实现，所以，在并发修改期间，相应 Segment 是被锁定的。
        重复性的扫描，以确定相应 key 值是否已经在数组里面，进而决定是更新还是放置操作.resize会单独对 Segment 进行扩容.
        - size 方法:如果不进行同步，简单的计算所有 Segment 的总值，可能会因为并发 put，导致结果不准确但是直接锁定所有 Segment 进行计算，就会变得非常昂贵。
        分离锁也限制了 Map 的初始化等操作。分段计算两次，两次结果相同则返回，否则对所以段加锁重新计算.
        - ConcurrentHashMap 的实现是通过重试机制（RETRIES_BEFORE_LOCK，指定重试次数 2），来试图获得可靠值。
        如果没有监控到发生变化（通过对比 Segment.modCount），就直接返回，否则获取锁进内部仍然有 Segment 定义行操作。
        -1.7 put加锁 通过分段加锁segment，一个hashmap里有若干个segment，每个segment里有若干个桶，桶里存放K-V形式的链表，
        put数据时通过key哈希得到该元素要添加到的segment，然后对segment进行加锁，然后再哈希，计算得到给元素要添加到的桶，
        然后遍历桶中的链表，替换或新增节点到桶中
        - size 分段计算两次，两次结果相同则返回，否则对所以段加锁重新计算
    2. Java 8 和之后ConcurrentHashMap
        - HashMap 结构非常相似.大的桶（bucket）数组，然后内部也是一个个所谓的链表结构（bin），同步的粒度要更细致一些。
        - 内部仍然有 Segment 定义,仅仅是为了保证序列化时的兼容性而已，不再有任何结构上的用处。segment数量与桶数量一致；
        - 修改为 lazy-load 形式，这样可以有效避免初始开销
        - volatile 来保证可见性。
        - CAS 等操作，在特定场景进行无锁并发操作。
        - Unsafe、LongAdder 之类底层手段，进行极端情况的优化。
        - Key 是 final 的，因为在生命周期中，一个条目的 Key 发生变化是不可能的；与此同时 val，则声明为 volatile，以保证可见性。
        - 1.8 put CAS 加锁 1.8中不依赖与segment加锁，segment数量与桶数量一致；
          首先判断容器是否为空，为空则进行初始化利用volatile的sizeCtl作为互斥手段，如果发现竞争性的初始化，就暂停在那里，等待条件恢复，
          否则利用CAS设置排他标志（U.compareAndSwapInt(this, SIZECTL, sc, -1)）;否则重试
        - 对key hash计算得到该key存放的桶位置，判断该桶是否为空，为空则利用CAS设置新节点
          否则使用synchronize加锁，遍历桶中数据，替换或新增加点到桶中
          最后判断是否需要转为红黑树，转换之前判断是否需要扩容.
        - size 利用LongAdd累加计算
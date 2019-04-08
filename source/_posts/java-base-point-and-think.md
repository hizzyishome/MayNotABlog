---
title: java base point and think
comments: true
date: 2019-03-14 15:50:00
updated: 2019-03-14 15:50:00
tags:
categories:
---
#### 有时候走了太久太远，都忘了为什么出发了。

1. 面向过程和面向对象
    1. 面向过程
        1. 概：面向对象在我印象里是最初在C中获得的，注重顺序思维，结构化编程，即使封装
        函数也是为了复用，而不是降低耦合。
        2. 优点：比面向对象性能更好，不用实例化，节省资源。性能因素占绝对重要性时，
        优考虑面向过程的开发。
        3. 缺点：流式思维，不符合客观世界规律，比起面向对象，更难维护、复用和、扩展。
    2. 面向对象：
        1. 概：是现实世界关系的抽象，符合现实世界的逻辑规律。
        2. 优点：抑郁维护复用和扩展。有封装、继承和多态的特性。
        3. 缺点：资源开销大，性能比面向过程差。
    3. 言之： 现在除了特别对性能有要求的一些项目，对于更多的业务系统讲，机器资源一般是
    较为满足的，更注重的是易维护，易扩展，业务系统大概占据了软件开发项目八成，所以在业务系统上，
    采用面向对象的方式进行开发。（现在的互联网行业人员流动那么大，程序员的编程水平习惯参差不齐，
    如果还不注重维护性和扩展性的话，那大概就是前人挖坑闪后人，后人再挖，闪后后人，
    子子孙孙无穷匮也。）
2. Java的特点
    1. 优点：
        1. 学习成本底。（相比C和C++吧，比Python还差些 :smile:）
        2. 面向对象。（C++也是面向对象的，严格的说面向对象是一种思想）
        3. 平台无关。可移植性好。（只要这个平台有对应的jvm，你只管敲你的Java代码，
        编译成class后，jvm去生产适配各个平台的机器指令。）
        4. 可靠性。（因为强类型？怎么就比其他语言更可靠了？质疑）
        5. 安全性。（和强类型有关，也没有C里指针的各种乱指）
        6. 多线程支持。（C++没有内置的对多线程的支持，需要调用系统的多线程支持）
        7. 方便的网络编程。（简化了网络编程是指对JavaWeb方向的扩展么？确实在C里网络编程中的
        通信一些东西确实比较复杂。）
        8. 编译与解释并存。（Java确实从解释语言里学了很多，这点是我很欣赏的，知道发展自身，
        才是生存下去的道理。）
    2. 缺点：
        1. 初期性能经常与C系比较，确实差一些，但是现在的java性能已经不能被诟病了。
        2.

3. JVM JDK JRE
    1. JVM（Java Virtual Machine）
        - 运行java字节码，字节码class文件jvm能理解
        - 对不同系统有不同实现
        - class文件只面向jvm，各个平台上都是一样的
        - 一方面解决了解释型语言效率低问题
        - 保留了解释性语言可移植性
        - 在不同平台上不需要重新编译，可以直接运行
        - .java文件  -- jdk中的javac --> .class文件
        - .class文件  -- jvm --> 二进制机器码。jvm类加载器首先加载字节码文件，
        解释器逐行解释执行。但是热点代码会多次被解释，所以引进了JIM(Just In Time)编译器。
        运行时编译器，完成一次编译以后，字节码对应的机器码保存，下次调用到直接使用。
        这一部分属于编译后调用，每次重新编译的是解释的部分。
        - HotSpot 惰性评估(Lazy Evaluation)  热点代码是需要JIT编译的部分
        JVM根据每次执行的情况收集信息并且相应的优化 执行次数越多，速度越快
        JDK9引入AOT(Ahead of Time Compilation)编译，直接将字节码编译成机器码，
        避免了JIT模式下的预热开销。
        - 支持分层编译和AOT协作使用 ``？？？``   AOT编译质量不如JIT``？？？``
        - 字节码 和 不同系统的JVM实现 保证了一次编译到处运行
    2. JDK
        - Java Development Kit
        - 包括JRE
        - 编译器
        - 其他工具
    3. JRE
        - Java Runtime Environment
        - 运行已编译java程序
        - 包括jvm java类库 java命令 其他基础构件
        - 包含jsp的web程序，也需要jdk，因为需要将JSP转换为Java servlet，需要jsk编译servlet。
4. Oracle JDK 和 OpenJDK
    1. Oracle JDK
        - 不是完全开源的
        - 更稳定，优化更多，效率可能更高？
    2. OpenJDK
        - 开源
        - GPL许可协议

5. Java和C++的区别
    1. 共通：面向对象，支持继承封装和多态
    2. java: 不提供指针访问内存，更安全   类单继承   接口多继承  内存管理机制
    3. C++： 提供指针   类可以多继承   需要程序员释放内存

6. 字符型常量和字符串常量的区别
    1.
    ```java
    char c = 'c';  String s = "sss";
    ```
    2. 字符相当于ascII值，可以参加运算。字符串代表地址，即在内存中存放位置。
    3. char类型占2个字节 2*8bit = 16bit；字符串至少一个，结束标志（这句并不对，
    在C++中，以\0作为结束，但是在Java中，String是对象，有长度属性，不需要表示结尾）

7. 构造器 Constructor 是否可被 override
    - 父类的私有属性和构造方法并不能被继承
    - Constructor 也就不能被 override（重写）
    - 可以overload（重载）

8. Java 面向对象编程三大特性: 封装 继承 多态
    1. 封装把对象属性私有化，提供可以被外界访问的属性的方法，
    可不提供，但是如果一个类没有提供给外界访问的方法，那这个类也没有什么意义了。
    2. 继承是使用已存在的类的定义作为基础建立新类的技术，
    新类的定义可以增加新的数据或新的功能，也可以用父类的功能，但不能选择性地继承父类。
    通过使用继承能复用以前的代码。
    3. - 子类有父类非private属性和方法
        - 子类可以有自己的属性和方法
        - 子类可以重写父类非private方法（构造方法呢？基于自己的+super()）
    4. 多态指程序中定义的引用变量所指向的具体类型和通过该引用变量发出的方法调用，
    在编程时并不确定，而是在程序运行期间才确定，即一个引用变量到底会指向哪个类的实例对象，
    该引用变量发出的方法调用到底是哪个类中实现的方法，必须在由程序运行期间才能决定。
        - 继承实现（多个子类对同一方法的重写）
        - 接口实现（实现接口并覆盖接口中同一方法）。

9. String StringBuffer 和 StringBuilder
    1. 可变性
        - String -> private final char value[];  不可变
        - StringBuilder 与 StringBuffer 都继承AbstractStringBuilder    char[] value; 可变
    2. 线程安全性
        - String final 常量线程安全
        - StringBuilder 没有对方法加同步锁，非线程安全
        - StringBuffer  加了同步锁，线程安全  ，内部使用 synchronized 进行同步
    3. 性能
        - 对String变量改变赋值，生成新String对象，指针指向新的String对象
        - StringBuffer每次操作自己
        - StringBuilder 有更高的性能
    4. 少量数据为了方便直接String   操作大量数据 单线程StringBuilder  多线程StringBuffer

10. 在一个静态方法内调用一个非静态成员为什么是非法的
    - 静态方法不通过对象去调用方法

11. 在 Java 中定义一个不做事且没有参数的构造方法的作用
    - 继承 子类中的super() 调用父类中无参数构造函数 如果出现这种情况，而父类中没有，报错

12.  import java和javax有什么区别
    - 刚开始的时候 JavaAPI 所必需的包是 java 开头的包，javax 当时只是扩展 API 包来说使用。
    - 然而随着时间的推移，javax 逐渐的扩展成为 Java API 的组成部分。
    - 但是，将扩展从 javax 包移动到 java 包将是太麻烦了，最终会破坏一堆现有的代码。
    - 因此，最终决定 javax 包将成为标准API的一部分。
    - 所以，实际上java和javax没有区别。这都是一个名字。

13. 接口和抽象类的区别是什么
    1. 接口的方法默认是 public，所有方法在接口中不能有实现(Java 8 开始接口方法可以有默认实现），抽象类可以有非抽象的方法实现
    2. 接口中的实例变量默认是 final 类型的，而抽象类中则不一定
    3. 一个类可以实现多个接口，但最多只能实现一个抽象类(java中接口和继承的区别)
    4. 一个类实现接口的话要实现接口的所有方法，而抽象类不一定(不实现默认使用父类)
    5. 接口不能用 new 实例化，但可以声明，但是必须引用一个实现该接口的对象
    从设计层面来说，抽象是对类的抽象，是一种模板设计，接口是行为的抽象，是一种行为的规范。

14. 成员变量与局部变量的区别有那些
    1. 从语法形式上
        - 看成员变量是属于类的，而局部变量是在方法中定义的变量或是方法的参数
        - 成员变量可以被 public,private,static 等修饰符所修饰，而局部变量不能被访问控制修饰符及 static 所修饰；
        - 成员变量和局部变量都能被 final 所修饰；
    2. 从变量在内存中的存储方式来看
        - 如果成员变量是使用static修饰的，那么这个成员变量是属于类的
        - 如果没有使用使用static修饰，这个成员变量是属于实例的。
        - 而对象存在于堆内存，局部变量存在于栈内存
    3. 从变量在内存中的生存时间上看
        - 成员变量是对象的一部分，它随着对象的创建而存在
        - 局部变量随着方法的调用而自动消失。
    4. 成员变量如果没有被赋初值
        - 则会自动以类型的默认值而赋值(一种情况例外被 final 修饰的成员变量也必须显示地赋值)
        - 局部变量则不会自动赋值。

15. 创建一个对象用什么运算符?对象实体与对象引用有何不同?
    - new运算符，new创建对象实例（对象实例在堆内存中）
    - 对象引用指向对象实例（对象引用存放在栈内存中）
    - 一个对象引用可以指向0个或1个对象（一根绳子可以不系气球null，也可以系一个气球）
    - 一个对象可以有n个引用指向它（可以用n条绳子系住一个气球）获取同一个对象

16. 构造方法特性
    - 名字与类名相同
    - 没有返回值，但不能用void声明
    - 生成类的对象自动执行，无需调用

17. 静态方法和实例方法有何不同
    - 在外部调用静态方法时，可以使用"类名.方法名"的方式，也可以使用"对象名.方法名"的方式，
    调用静态方法可以无需创建对象。静态方法在访问本类的成员时，只允许访问静态成员（即静态成员变量和静态方法），
    而不允许访问实例成员变量和实例方法.
    - 实例方法只有"对象名.方法名"的方式，实例方法可以访问所有成员和方法

18. 在调用子类构造方法之前会先调用父类没有参数的构造方法,其目的是帮助子类做初始化工作。

19. == 与 equals
    1. ==
        - 它的作用是判断两个对象的地址是不是相等，两个对象是不是同一个对象。
        - 基本数据类型==比较的是值，引用数据类型==比较的是内存地址
    2. equals()
        - 类没有覆盖 equals() 方法。则通过 equals() 比较该类的两个对象时，等价于通过“==”比较这两个对象。
        - 类覆盖了 equals() 方法。一般，我们都覆盖 equals() 方法来两个对象的内容相等；
        若它们的内容相等，则返回 true (即，认为这两个对象相等)。
        - String 中的 equals 方法是被重写过的，因为 object 的 equals 方法是比较的对象的内存地址，
        而 String 的 equals 方法比较的是对象的值。
        - 创建 String 类型的对象时，虚拟机会在常量池中查找有没有已经存在的值和要创建的值相同的对象，
        如果有就把它赋给当前引用。如果没有就在常量池中重新创建一个 String 对象。
20. hashCode 与 equals
    1. hashCode
        - hashCode() 的作用是获取哈希码，也称为散列码；它实际上是返回一个int整数。
        - 这个哈希码的作用是确定该对象在哈希表中的索引位置。
        - hashCode() 定义在JDK的Object.java中，这就意味着Java中的任何类都包含有hashCode() 函数。
        - 散列表存储的是键值对(key-value)，它的特点是：能根据“键”快速的检索出对应的“值”。
        这其中就利用到了散列码！（可以快速找到所需要的对象）
    2. 为什么要有 hashCode
        - 当你把对象加入 HashSet 时，HashSet 会先计算对象的 hashcode 值来判断对象加入的位置，
        同时也会与其他已经加入的对象的 hashcode 值作比较
        - 如果没有相符的hashcode，HashSet会假设对象没有重复出现。
        - 但是如果发现有相同 hashcode 值的对象，这时会调用 equals（）方法来检查 hashcode 相等的对象是否真的相同。
        - 如果两者相同，HashSet 就不会让其加入操作成功。
        - 如果不同的话，就会重新散列到其他位置。
        - 大大减少了 equals 的次数，相应就大大提高了执行速度。
    3. hashCode（）与equals（）的相关规定
        - 如果两个对象相等，则hashcode一定也是相同的
        - 两个对象相等,对两个对象分别调用equals方法都返回true
        - 两个对象有相同的hashcode值，它们也不一定是相等的
        - 因此，equals 方法被覆盖过，则 hashCode 方法也必须被覆盖
        - hashCode() 的默认行为是对堆上的对象产生独特值。
        如果没有重写 hashCode()，则该 class 的两个对象无论如何都不会相等（即使这两个对象指向相同的数据）

21. 为什么Java中只有值传递
    - 基本类型值，将值拷贝，进行值传递。对象传递的话，将对象的引用（地址）拷贝，进行值传递。
    但是地址的copy值指向同一个对象，方法对对象成员的改动，即改动了对象在内存里的值，会反映在外部。
    然而，如果直接换引用，是换了copy的引用，和外部原来的引用并没有关系。

22. 线程,程序,进程的基本概念
    1. 线程
        - 与进程相似，比进程更小的执行单位。
        - 一个进程在其执行的过程中可以产生多个线程。
        - 与进程不同，多个线程共享同一块内存空间和一组系统资源，所以系统在产生一个线程，
        或是在各个线程之间作切换工作时，负担要比进程小得多，也正因为如此，线程也被称为轻量级进程。
    2. 程序
        - 含有指令和数据的文件，被存储在磁盘或其他的数据存储设备中
        - 程序是静态的代码。
    3. 进程
        - 是程序的一次执行过程，是系统运行程序的基本单位，进程是动态的。
        - 系统运行一个程序即是一个进程从创建，运行到消亡的过程。
        - 一个进程就是一个执行中的程序，它在计算机中一个指令接着一个指令地执行着，
        每个进程还占有某些系统资源如CPU时间，内存空间，文件，文件，输入输出设备的使用权等。
        - 当程序在执行时，将会被操作系统载入内存中。
        - 线程是进程划分成的更小的运行单位。
        - 线程和进程最大的不同在于，基本上各进程是独立的，而各线程则不一定，因为同一进程中的线程极有可能会相互影响。
        - 进程属于操作系统的范畴，主要是同一段时间内，可以同时执行一个以上的程序，
        而线程则是在同一程序内几乎同时执行一个以上的程序段。

23. 线程基本状态

|状态名称|Point|
|---|---|
|NEW|初始状态，已经构建，没有调用start()方法|
|RUNNABLE|运行状态，就绪状态(调用start()方法，但还没有run) 　+　运行中状态|
|BLOCKED|阻塞状态，阻塞于锁？|
|WAITING|等待状态，需要其他线程通知或者中断|
|TIME_WAITING|超时等待状态，指定时间自行返回|
|TERMINATED|终止线程，执行完毕|

24. <a name="final">final</a>
    1. 变量
        - 基本数据类型在初始化之后便不能更改
        - 引用类型初始化之后便不能再让其指向另一个对象
        - 但是被引用的对象本身是可以修改的。
    2. 类
        - 类不能被继承。
        - final类中的所有成员方法都会被隐式地指定为final方法。
    3. 方法
        - 方法锁定，以防任何继承类修改它的含义
        - 早期的Java实现版本中，会将final方法转为内嵌调用。
        但是如果方法过于庞大，可能看不到内嵌调用带来的任何性能提升
        （现在的Java版本已经不需要使用final方法进行这些优化了）。
        - 类中所有的private方法都隐式地指定为final。

25. <a name="Throwable">异常</a>

        ```mermaid
        graph TD;
          Throwable-->Error;
          Throwable-->Exception;
          Error-->VirtulMachineError;
          Error-->AWTError;
          VirtulMachineError-->StackOverFlowError;
          VirtulMachineError-->OutOfMemoryError;
          Exception-->IOException;
          Exception-->RuntimeException;
          IOException-->EOFException;
          IOException-->FileNotFoundException;
          RuntimeException-->ArrithmeticException;
          RuntimeException-->MissingResourceException;
          RuntimeException-->ClassNotFoundException;
          RuntimeException-->NullPointerException;
          RuntimeException-->IllegalArgumentException;
          RuntimeException-->ArrayIndexOutOfBoundsException;
          RuntimeException-->UnknownTypeException;
        ```
    1. Error（错误）
        - 程序无法处理的错误，表示运行应用程序中较严重问题。
        - 大多数错误与代码编写者执行的操作无关，而表示代码运行时 JVM（Java 虚拟机）出现的问题。
        例如，Java虚拟机运行错误（Virtual MachineError），当 JVM 不再有继续执行操作所需的内存资源时，
        将出现 OutOfMemoryError。
        - Error发生时，Java虚拟机（JVM）一般会选择线程终止。
       - 这些错误表示故障发生于虚拟机自身、或者发生在虚拟机试图执行应用时，
       如Java虚拟机运行错误（Virtual MachineError）、类定义错误（NoClassDefFoundError）等。
       - 错误是不可查的，因为它们在应用程序的控制和处理能力之外，而且绝大多数是程序运行时不允许出现的状况。
       - 对于设计合理的应用程序来说，即使确实发生了错误，本质上也不应该试图去处理它所引起的异常状况。
       - 在 Java中，错误通过Error的子类描述。
    2. Exception（异常）
        - 程序本身可以处理的异常。
        - RuntimeException异常由Java虚拟机抛出。
        NullPointerException（要访问的变量没有引用任何对象时，抛出该异常）,
        ArithmeticException（算术运算异常，一个整数除以0时，抛出该异常）,
        ArrayIndexOutOfBoundsException （下标越界异常）。
        - 受检异常 ：需要用 try...catch... 语句捕获并进行处理，并且可以从异常中恢复；
        - 非受检异常 ：是程序运行时错误，例如除 0 会引发 Arithmetic Exception，此时程序崩溃并且无法恢复。

    **异常能被程序本身可以处理，错误无法处理。**

    3. Throwable类常用方法
        - public string getMessage():返回异常发生时的详细信息
        - public string toString():返回异常发生时的简要描述
        - public string getLocalizedMessage():返回异常对象的本地化信息。
        使用Throwable的子类覆盖这个方法，可以声称本地化信息。
        如果子类没有覆盖该方法，则该方法返回的信息与getMessage（）返回的结果相同
        - public void printStackTrace():在控制台上打印Throwable对象封装的异常信息

    4. 异常处理总结
        - **try块：** 用于捕获异常。其后可接零个或多个catch块，如果没有catch块，则必须跟一个finally块。
        - **catch 块：** 用于处理try捕获到的异常。
        - **inally 块：** 无论是否捕获或处理异常，finally块里的语句都会被执行。
        当在try块或catch块中遇到return语句时，finally语句块将在方法返回之前被执行。

        - ***finally块不会被执行***
            - 在finally语句块第一行发生了异常。 因为在其他行，finally块还是会得到执行
            - 在前面的代码中用了System.exit(int)已退出程序。 exit是带参函数 ；若该语句在异常语句之后，finally会执行
            - 程序所在的线程死亡。(后面两个在逗我咩QAQ，这不废话么)
            - 关闭CPU。

    4. 如果try语句里有return，返回的是try语句块中变量值。
        1. 如果有返回值，就把返回值保存到局部变量中；
        2. 执行jsr指令跳到finally语句里执行；
        3. 执行完finally语句后，返回之前保存在局部变量表里的值。
        4. 如果try，finally语句里均有return，忽略try的return，而使用finally的return.
    5. 应该尽量将捕获底层异常类(子类准确类)的catch子句放在前面，同时尽量将捕获相对高层的异常类(父类异常类)的catch子句放在后面。
    否则，捕获底层异常类的catch子句将可能会被屏蔽。（你想啊，你吧Exception放在第一个catch，后面你的ShitException就被短路了）
    6. try语句的嵌套可以很隐蔽的发生。例如，我们可以将对方法的调用放在一个try块中。
    在该方法的内部，有另一个try语句。在这种情况下，方法内部的try仍然是嵌套在外部调用该方法的try块中的。
    7. 程序执行完throw语句之后立即停止；throw后面的任何语句不被执行，
    最邻近的try块用来检查它是否含有一个与异常类型匹配的catch语句。
    如果发现了匹配的块，控制转向该语句；如果没有发现，次包围的try块来检查，以此类推。
    如果没有发现匹配的catch块，默认异常处理程序中断程序的执行并且打印堆栈轨迹。
    8. Throws 仅当抛出了异常，该方法的调用者才必须处理或者重新抛出该异常。
    当方法的调用者无力处理该异常的时候，应该继续抛出，而不是囫囵吞枣。
    9. finally创建的代码块在try/catch块完成之后另一个try/catch出现之前执行。
    finally块无论有没有异常抛出都会执行。如果抛出异常，即使没有catch子句匹配，finally也会执行。
    一个方法将从一个try/catch块返回到调用程序的任何时候，经过一个未捕获的异常或者是一个明确的返回语句，
    finally子句在方法返回之前仍将执行。这在关闭文件句柄和释放任何在方法开始时被分配的其他资源是很有用。
    10. 异常链顾名思义就是将异常发生的原因一个传一个串起来，即把底层的异常信息传给上层，这样逐层抛出。
    当程序捕获到了一个底层异常，在处理部分选择了继续抛出一个更高级别的新异常给此方法的调用者。
    这样异常的原因就会逐层传递。这样，位于高层的异常递归调用getCause()方法，就可以遍历各层的异常原因。
    这就是Java异常链的原理。异常链的实际应用很少，发生异常时候逐层上抛不是个好注意，
    上层拿到这些异常又能奈之何？而且异常逐层上抛会消耗大量资源， 因为要保存一个完整的异常链信息.
    11. 用户自定义异常类，只需继承Exception类即可。
        - 创建自定义异常类。
        - 在方法中通过throw关键字抛出异常对象。
        - 如果在当前抛出异常的方法中处理异常，可以使用try-catch语句捕获并处理；
        否则在方法的声明处通过throws关键字指明要抛出给方法调用者的异常，继续进行下一步操作。
        - 在出现异常方法的调用者中捕获并处理异常。

26. transient
    - 阻止实例中那些用此关键字修饰的的变量序列化；
    - 当对象被反序列化时，被transient修饰的变量值不会被持久化和恢复
    - transient只能修饰变量，不能修饰类和方法。

27. console键盘输入
    - 通过 Scanner
    ```java
    Scanner input = new Scanner(System.in);
    String s  = input.nextLine();
    input.close();
    ```
    - 通过 BufferedReader
    ```java
    BufferedReader input = new BufferedReader(new InputStreamReader(System.in));
    String s = input.readLine();
    ```

1. 数据类型
    1. 基本类型
    
    |keyWord|package|size|range|default|
    |---|---|---|---|---|
    |boolean|Boolean|1byte字节、8bit位|true，false JVM 会在编译时期将 boolean 类型的数据转换为 int，1 true，0 false|false|
    |byte|Byte|1byte字节、8bit位|能存256个数，正负各128个，0放在正数一半 --> -128~127|0|
    |char|Character|2byte字节、16bit位|能存65536个，对应Ascii码表，不需要负数，0~65535|'\u0000'|
    |short|Short|2byte字节、16bit位|能存65536个数，正负各32768个,0放正数一半 --> -32768~32767|0|
    |int|Integer|4byte字节、32bit位|能存4294967296个数，正负各2147483648个,0放正数一半 --> -2147483648~2147483647|0|
    |long|Long|8byte字节、64bit位|能存4294967296个数，正负各一半,0放正数一半 --> 9223372036854775808~9223372036854775807|0L|
    |float|Float|4byte字节、32bit位|符号位（sign）占用1位，用来表示正负数，指数位（exponent）占用8位，用来表示指数，小数位（fraction）占用23位，用来表示小数，不足位数补0。|0.0F|
    |double|Double|8byte字节、64bit位|符号位（sign）占用1位，指数位（exponent）占用11位，小数位（fraction）占用52位，不足位数补0。|0.0D|
    2. 包装类型
        ```java
        Integer x = 2;     // 装箱
        int y = x;         // 拆箱
        ```
    3. 缓存池
        - new Integer(123) 每次都会新建一个对象；
        - Integer.valueOf(123) 会使用缓存池中的对象，多次调用会取得同一个对象的引用。
        - 先判断值是否在缓存池中，如果在的话就直接返回缓存池的内容。
        ```java
        public static Integer valueOf(int i) {
            if (i >= IntegerCache.low && i <= IntegerCache.high)
                return IntegerCache.cache[i + (-IntegerCache.low)];
            return new Integer(i);
        }
        ```
        - 自动装箱过程调用 valueOf() 方法，因此多个值相同且值在缓存池范围内的 Integer 实例使用自动装箱来创建，
        那么就会引用相同的对象。
        ```java
        Integer m = 123;
        Integer n = 123;
        System.out.println(m == n); // true
        }
        ```

2. String
    1. 概：
        - final 不可被继承。
        - Java 8 内部使用 char 数组存储数据
        - Java 9 改用 byte 数组存储字符串，同时使用 coder 来标识使用了哪种编码。
    2. 不可变的好处
        - 缓存 hash 值：因为 String 的 hash 值经常被使用，例如 String 用做 HashMap 的 key。
        不可变的特性可以使得 hash 值也不可变，因此只需要进行一次计算。
        - String Pool 的需要： 如果一个 String 对象已经被创建过了，那么就会从 String Pool 中取得引用。
        只有 String 是不可变的，才可能使用 String Pool。
        - 安全性：String 经常作为参数，String 不可变性可以保证参数不可变。
        例如在作为网络连接参数的情况下如果 String 是可变的，那么在网络连接过程中，String 被改变，
        改变 String 对象的那一方以为现在连接的是其它主机，而实际情况却不一定是。
        - 线程安全:

    3. String Pool
        - 字符串常量池（String Pool）保存着所有字符串字面量（literal strings），
        这些字面量在编译时期就确定。
        - 当一个字符串调用 intern() 方法时，如果 String Pool 中已经存在一个字符串和该字符串值相等
        （使用 equals() 方法进行确定），那么就会返回 String Pool 中字符串的引用；
        否则，就会在 String Pool 中添加一个新的字符串，并返回这个新字符串的引用。
        ```java
         String s1 = new String("aaa");
         String s2 = new String("aaa");
         System.out.println(s1 == s2);           // false
         String s3 = s1.intern();
         String s4 = s1.intern();
         System.out.println(s3 == s4);           // true
        ```
        - 采用字面量的形式创建字符串，会自动地将字符串放入 String Pool 中。
        ```java
        String s5 = "bbb";
        String s6 = "bbb";
        System.out.println(s5 == s6);  // true
        ```
        - 在 Java 7 之前，String Pool 被放在运行时常量池中，它属于永久代。
        而在 Java 7，String Pool 被移到堆中。
        这是因为永久代的空间有限，在大量使用字符串的场景下会导致 OutOfMemoryError 错误。
    4. new String("abc")
        - "abc" 属于字符串字面量，因此编译时期会在 String Pool 中创建一个字符串对象，指向这个 "abc" 字符串字面量
        - 而使用 new 的方式会在堆中创建一个字符串对象。
        ```java
        public class NewStringTest {
            public static void main(String[] args) {
                String s = new String("abc");
            }
        }
        ```
        反编译得到
        ```
        // ...
        Constant pool:
        // ...
           #2 = Class              #18            // java/lang/String
           #3 = String             #19            // abc
        // ...
          #18 = Utf8               java/lang/String
          #19 = Utf8               abc
        // ...

          public static void main(java.lang.String[]);
            descriptor: ([Ljava/lang/String;)V
            flags: ACC_PUBLIC, ACC_STATIC
            Code:
              stack=3, locals=2, args_size=1
                 0: new           #2                  // class java/lang/String
                 3: dup
                 4: ldc           #3                  // String abc
                 6: invokespecial #4                  // Method java/lang/String."<init>":(Ljava/lang/String;)V
                 9: astore_1
        // ...
        ```
        在 Constant Pool 中，#19 存储这字符串字面量 "abc"，
        #3 是 String Pool 的字符串对象，它指向 #19 这个字符串字面量。
        在 main 方法中，0: 行使用 new #2 在堆中创建一个字符串对象，
        并且使用 ldc #3 将 String Pool 中的字符串对象作为 String 构造函数的参数。

        -  String 构造函数
        ```java
        this.value = original.value;
        this.hash = original.hash;
        ```
        将一个字符串对象作为另一个字符串对象的构造函数参数时，并不会完全复制 value 数组内容，而是都会指向同一个 value 数组。

3. 运算
    1. 参数传递 都是值传递，对象也是地址当成值传递
    2. float 与 double
    ```java
    // float f = 1.1; //这个是把double赋值给了float，Java 不能隐式执行向下转型，因为这会使得精度降低。
    float f = 1.1f;
    ```
    3. 隐式类型转换
    ```java
    //字面量 1 是 int 类型，它比 short 类型精度要高，因此不能隐式地将 int 类型下转型为 short 类型。
    short s1 = 1;
    // s1 = s1 + 1;

    //但是使用 += 或者 ++ 运算符可以执行隐式类型转换。
    s1 += 1;
    // s1++;

    s1 = (short) (s1 + 1);
    ```

    4. switch
        - 从 Java 7 开始，可以在 switch 条件判断语句中使用 String 对象。
        - switch 不支持 long，是因为 switch 的设计初衷是对那些只有少数的几个值进行等值判断，
        如果值过于复杂，那么还是用 if 比较合适。
        
4. 继承
    1. 访问权限
        1. private 设计良好的模块会隐藏所有的实现细节,称为信息隐藏或封装.
        因此访问权限应当尽可能地使每个类或者成员不被外界访问。
        2. protected 在继承体系中成员对于子类可见，但是这个访问修饰符对于类没有意义。
        子类的方法重写了父类的方法，那么子类中该方法的访问级别不允许低于父类的访问级别(里氏替换原则)
        3. public 类可见表示其它类可以用这个类创建实例对象。
        成员可见表示其它类可以用这个类的实例对象访问到该成员；
        4. 不加访问修饰符(default) 包级可见
    2. 抽象类与接口
        1. 抽象类
            - 抽象类和抽象方法都使用 abstract 关键字进行声明
            - 抽象类一般会包含抽象方法，抽象方法一定位于抽象类中。
            - 抽象类不能被实例化，需要继承抽象类才能实例化其子类。
        2. 接口
            - Java 8 之前，接口可以看成是一个完全抽象的类，不能有任何的方法实现。
            - Java 8 开始，接口可以有默认的方法实现，因为不支持默认方法的接口的维护成本太高了。
            在 Java 8 之前，如果一个接口想要添加新的方法，那么要修改所有实现了该接口的类。
            - 接口的成员（字段 + 方法）默认都是 public 的，并且不允许定义为 private 或者 protected。
            - 接口的字段默认都是 static 和 final 的。
        3. 比较
            - 从设计层面上看，抽象类提供了一种 IS-A 关系，那么就必须满足里式替换原则，
            即子类对象必须能够替换掉所有父类对象。
            而接口更像是一种 LIKE-A 关系，它只是提供一种方法实现契约，
            并不要求接口和实现接口的类具有 IS-A 关系。
            - 从使用上来看，一个类可以实现多个接口，但是不能继承多个抽象类。
            - 接口的字段只能是 static 和 final 类型的，而抽象类的字段没有这种限制。
            - 接口的成员只能是 public 的，而抽象类的成员可以有多种访问权限。
        4. 使用选择
            1. 使用接口
                - 需要让不相关的类都实现一个方法，
                例如不相关的类都可以实现 Compareable 接口中的 compareTo() 方法；
                - 需要使用多重继承。
            2. 使用抽象类
                - 需要在几个相关的类中共享代码。
                - 需要能控制继承来的成员的访问权限，而不是都为 public。
                - 需要继承非静态和非常量字段。
    3. super
        - 访问父类的构造函数：可以使用 super() 函数访问父类的构造函数，从而委托父类完成一些初始化的工作。
        - 访问父类的成员：如果子类重写了父类的某个方法，可以通过使用 super 关键字来引用父类的方法实现。
    4. 重写与重载
        1. 重写（Override）
            - 继承体系中，指子类实现了一个与父类在方法声明上完全相同的一个方法。
            - 里式替换原则
                1. 子类方法的访问权限必须大于等于父类方法；
                2. 子类方法的返回类型必须是父类方法返回类型或为其子类型。
        2. 重载（Overload）
            - 存在于同一个类中，指一个方法与已经存在的方法名称上相同，但是参数类型、个数、顺序至少有一个不同。
             ```java
            class A {
                public String show(D obj) {
                    return ("A and D");
                }

                public String show(A obj) {
                    return ("A and A");
                }
            }

            class B extends A {
                public String show(B obj) {
                    return ("B and B");
                }

                public String show(A obj) {
                    return ("B and A");
                }
            }

            class C extends B {
            }

            class D extends B {
            }
            ```
            ```java
            public class test {
                public static void main(String[] args) {
                    A a1 = new A();
                    A a2 = new B();
                    B b = new B();
                    C c = new C();
                    D d = new D();

                    // a1为A类，b为B类，先找A类中show(B obj)，没有
                    // 然后A类无父类，
                    // 之后找A类中show(A obj)，因为B的父类为A，找到，显示A and A
                    System.out.println(a1.show(b)); //A and A

                    // a1为A类，c为C类，先找A类中show(C obj)，没有
                    // 然后A类无父类，
                    // 找A类中show(B obj)，因为C的父类是B，没有
                    // 之后找A类中show(A obj)，因为B的父类为A，找到，显示A and A
                    System.out.println(a1.show(c));//A and A

                    // a1为A类，d为D类，先找A类中show(D obj)，找到，显示A and D
                    System.out.println(a1.show(d));//A and D

                    // a2为A类(是以B的基础new一个A，然后地址给a2，但是只有A类中方法，但是A的show(A)被子类B重写，调用这个方法就是子类)，
                    // b为B类，先找A类中show(B obj)，没有
                    // 然后A类无父类，
                    // 然后，找A类中的show(B obj)，没有
                    // 然后找A类中的show(A obj),找到，但是这个方法被B类重写了，所以调用了B类的show(A obj),所以 B and A
                    System.out.println(a2.show(b));//B and A

                    // a2为A类(是以B的基础new一个A，然后地址给a2，但是只有A类中方法，但是A的show(A)被子类B重写，调用这个方法就是子类)，
                    // c为C类，先找A类中show(C obj)，没有
                    // 然后A类无父类，
                    // 然后，找A类中的show(B obj)，没有
                    // 然后找A类中的show(A obj),找到，但是这个方法被B类重写了，所以调用了B类的show(A obj),所以 B and A
                    System.out.println(a2.show(c));

                    // a2为A类(是以B的基础new一个A，然后地址给a2，但是只有A类中方法，但是A的show(A)被子类B重写，调用这个方法就是子类)，
                    // d为D类，先找A类中show(D obj)，有,所以 A and D
                    System.out.println(a2.show(d));//A and D

                    // b为B类，b为B类，先找B类中show(B obj)，找到，显示B and B
                    System.out.println(b.show(b));//B and B

                    // b为B类，c为C类，先找B类中show(C obj)，没有
                    // B的父类为A，找A类中的show(C obj)，没有
                    // 然后找B类中的show（B）找到，显示B and B
                    System.out.println(b.show(c));//B and B

                    // b为B类，d为D类，先找B类中show(D obj)，没有
                    // B的父类为A，找A类中的show(D obj)，找到，显示A and D
                    System.out.println(b.show(d));//A and D
                }
            }
            ```
    涉及到重写时，方法调用的优先级为：
    1. this.show(O)
    2. super.show(O)
    3. this.show((super)O)
    4. super.show((super)O)

5. Object 通用方法
    1. equals()
        1. 等价关系
            1. 自反性
            2. 对称性
            3. 传递性
            4. 一致性 多次调用 equals() 方法结果不变
            5. 与 null 的比较
                - 对任何不是 null 的对象 x 调用 x.equals(null) 结果都为 false
                - 对象是null在调用.equals()方法时会报空指针异常
                - null == null 返回true
        2. 等价与相等
            - 对于基本类型，== 判断两个值是否相等，基本类型没有 equals() 方法。
            - 对于引用类型，== 判断两个变量是否引用同一个对象，而 equals() 判断引用的对象是否等价。
        3. 实现
            - 检查是否为同一个对象的引用，如果是直接返回 true； if (this == o) return true;
            - 传入对象是否为空，空返回false；检查是否是同一个类型，如果不是，直接返回 false；  if (o == null || getClass() != o.getClass()) return false;
            - 将 Object 对象进行转型；EqualExample that = (EqualExample) o;
            - 判断每个关键域是否相等。 判断你定义相等的每个成员变量是否相等

    2. hashCode()
        - hashCode() 返回散列值，而 equals() 是用来判断两个对象是否等价。
        等价的两个对象散列值一定相同，但是散列值相同的两个对象不一定等价。
        - 在覆盖 equals() 方法时应当总是覆盖 hashCode() 方法，保证等价的两个对象散列值也相等。
        - 理想的散列函数应当具有均匀性，即不相等的对象应当均匀分布到所有可能的散列值上。
        这就要求了散列函数要把所有域的值都考虑进来。可以将每个域都当成 R 进制的某一位，
        然后组成一个 R 进制的整数。R 一般取 31，因为它是一个奇素数，如果是偶数的话，
        当出现乘法溢出，信息就会丢失，因为与 2 相乘相当于向左移一位。
        - 一个数与 31 相乘可以转换成移位和减法：`31*x == (x<<5)-x`，编译器会自动进行这个优化。
        ```java
        public int hashCode(char[] chars) {
                int var1 = 0;
                if (var1 == 0 && chars.length > 0) {
                    char[] var2 = chars;

                    for(int var3 = 0; var3 < chars.length; ++var3) {
                        var1 = 31 * var1 + var2[var3];
                        //var1 = (var1 << 5) - var1 +var2[var3];
                    }

                    System.out.println("this.hash = " + var1);
                }

                return var1;
            }
        ```
        - ###### 常见hash算法
            1. Object类的hashCode.返回对象的内存地址经过处理后的结构，由于每个对象的内存地址都不一样，所以哈希码也不一样。
            2. String类的hashCode.根据String类包含的字符串的内容，根据一种特殊算法返回哈希码，只要字符串内容相同，返回的哈希码也相同。
            3. Integer类，返回的哈希码就是Integer对象里所包含的那个整数的数值，
            例如Integer i1=new Integer(100),i1.hashCode的值就是100 。由此可见，2个一样大小的Integer对象，返回的哈希码也一样。
            - 哈希码要完成这么一件事，首先要保证如果equlas出来的结果相等，那么hashCode也相等。
            - 一般的线性表，树中，记录在结构中的相对位置是随机的，即和记录的关键字之间不存在确定的关系，
            因此，在结构中查找记录时需进行一系列和关键字的比较。这一类查找方法建立在“比较“的基础上，
            查找的效率依赖于查找过程中所进行的比较次数。（链表最基础的比较，就是遍历比较，时间都花在了这个上）
            - 理想的情况是能直接找到需要的记录，因此必须在记录的存储位置和它的关键字之间建立一个确定的对应关系f，
            使每个关键字和结构中一个唯一的存储位置相对应。（通过单独识别码去找到该对象，建立联系）
            4. 直接定址法：有一个从1到100岁的人口数字统计表，其中，年龄作为关键字，
            哈希函数取关键字自身或者关键字的某个线性函数。取关键字自身效率不高,时间复杂度是O(1),空间复杂度是O(n),n是关键字的个数。
            5. 数字分析法：重复的可能性大的不取，取的话造成冲突的机会增加，所以尽量不取可能重复的关键字。
            6. 平方取中法： 取关键字平方后的中间几位为哈希地址。
            {421，423，436}，平方之后的结果为{177241，178929，190096}，那么可以取{72，89，00}作为Hash地址。
            7. 折叠法： 将关键字分割成位数相同的几部分（最后一部分的位数可以不同），
            然后取这几部分的叠加和（舍去进位）作为哈希地址，这方法称为折叠法。
            图书的ISBN号为8903-241-23，可以将address(key)=89+03+24+12+3作为Hash地址。
            8. 除留余数法: 取关键字被某个不大于哈希表表长m的数p除后所得余数为哈希地址。H(key)=key MOD p (p<=m)
            在这里p的选取非常关键，p选择的好的话，能够最大程度地减少冲突，p一般取不大于m的最大质数。
            9. 随机数法: 选择一个随机函数，取关键字的随机函数值为它的哈希地址.
            H(key)=random(key) ,其中random为随机函数。通常用于**关键字长度不等**时采用此法。
            - 冲突：对不同的关键字可能得到同一哈希地址。
            - ###### 处理冲突方法
            - 开放定址法：当一个关键字和另一个关键字发生冲突时，使用某种探测技术在Hash表中形成一个探测序列，
            然后沿着这个探测序列依次查找下去，当碰到一个空的单元时，则插入其中。Hi=(H(key)+di) MOD m i=1,2,...,k(k<=m-1)
            比较常用的探测方法有**线性探测法**，比如有一组关键字{12，13，25，23，38，34，6，84，91}，
            Hash表长为14，Hash函数为address(key)=key%11，当插入12，13，25时可以直接插入，
            而当插入23时，地址1被占用了，因此沿着地址1依次往下探测(探测步长可以根据情况而定)，
            直到探测到地址4，发现为空，则将23插入其中。（发现有，则顺延偏移）
            **二次探测再散列**di取值可能为1,-1,2,-2,4,-4,9,-9,16,-16,...k*k,-k*k(k<=m/2).
            **伪随机探测再散列**di取值可能为伪随机数列.
            - 链地址法：采用数组和链表相结合的办法，将Hash地址相同的记录存储在一张线性表中，
            而每张表的表头的序号即为计算得到的Hash地址。如上述例子中，采用链地址法形成的Hash表存储。
            - 再哈希法: 当发生冲突时，使用第二个、第三个、哈希函数计算地址，直到无冲突时。缺点：计算时间增加。
            - 建立一个公共溢出区:假设哈希函数的值域为[0,m-1],则设向量HashTable[0..m-1]为基本表，
            另外设立存储空间向量OverTable[0..v]用以存储发生冲突的记录。
        - Hash表大小的确定也非常关键，如果Hash表的空间远远大于最后实际存储的记录个数，
        则造成了很大的空间浪费，如果选取小了的话，则容易造成冲突。
        在实际情况中，一般需要根据最终记录存储个数和关键字的分布特点来确定Hash表的大小。
        还有一种情况时可能事先不知道最终需要存储的记录个数，则需要动态维护Hash表的容量，
        此时可能需要重新计算Hash地址。
    3. 这里要注意区分三个概念：hashCode值、hash值、hash方法、数组下标
        - hashCode值：是KV对中的K对象的hashCode方法的返回值（若没有重写则默认用Object类的hashCode方法的生成值）
        Object类`public native int hashCode();`native关键字是系统相关的其他语言实现（C/C++）。
        - hash值: 是在hashCode值的基础上又进行了一步运算后的结果，这个运算过程就是*hash方法*。
        - 数组下标: 根据该hash值和数组长度计算出数组下标，计算公式：hash值  &（数组长度-1）= 下标。
        - HashMap中*hash方法*：
            ```java
            static final int hash(Object var0) {
                int var1;
                return var0 == null ? 0 : (var1 = var0.hashCode()) ^ var1 >>> 16;
            }
            ```
    4. toString()
        - Object默认实现
        ```java
        public String toString() {
            return this.getClass().getName() + "@" + Integer.toHexString(this.hashCode());
        }
        ```
    5. clone()
        1. cloneable
        - clone() 是 Object 的 protected 方法，它不是 public，一个类不显式去重写 clone()，
        其它类就不能直接去调用该类实例的 clone() 方法。
        ```java
        public class CloneExample {
            private int a;
            private int b;
        }
        CloneExample e1 = new CloneExample();
        // CloneExample e2 = e1.clone(); // 'clone()' has protected access in 'java.lang.Object'
        ```
        重写 clone() 得到以下实现：
        ```
        public class CloneExample {
            private int a;
            private int b;

            @Override
            public CloneExample clone() throws CloneNotSupportedException {
                return (CloneExample)super.clone();
            }
        }
        ```
        ```java
        CloneExample e1 = new CloneExample();
        try {
            CloneExample e2 = e1.clone();
        } catch (CloneNotSupportedException e) {
            e.printStackTrace();
        }
        ```
        ```bash
        java.lang.CloneNotSupportedException: CloneExample
        ```
        上抛出了 CloneNotSupportedException，这是因为 CloneExample 没有实现 Cloneable 接口。
        - clone() 方法并不是 Cloneable 接口的方法，而是 Object 的一个 protected 方法。
        Cloneable 接口只是规定，如果一个类没有实现 Cloneable 接口又调用了 clone() 方法，
        就会抛出 CloneNotSupportedException。
        ```java
        public class CloneExample implements Cloneable {
            private int a;
            private int b;

            @Override
            public Object clone() throws CloneNotSupportedException {
                return super.clone();
            }
        }
        ```
        2. 浅拷贝
            - 拷贝对象和原始对象的引用类型引用同一个对象。
        3. 深拷贝
            - 拷贝对象和原始对象的引用类型引用不同对象。
            ```java
                @Override
                protected DeepCloneExample clone() throws CloneNotSupportedException {
                    DeepCloneExample result = (DeepCloneExample) super.clone();
                    result.arr = new int[arr.length];
                    for (int i = 0; i < arr.length; i++) {
                        result.arr[i] = arr[i];
                    }
                    return result;
                }
            ```
        4. clone() 的替代方案
            - 使用 clone() 方法来拷贝一个对象即复杂又有风险，它会抛出异常，并且还需要类型转换。
            Effective Java 书上讲到，最好不要去使用 clone()
            - 可以使用拷贝构造函数
            - 拷贝工厂来拷贝一个对象。
6. 关键字
    1. <a href="#final">final</a>
    2. static
        1. 静态变量：又称为类变量，也就是说这个变量属于类的，类所有的实例都共享静态变量，
        可以直接通过类名来访问它。静态变量在内存中只存在一份。
        - 实例变量：每创建一个实例就会产生一个实例变量，它与该实例同生共死。
        2. 静态方法：
        - 静态方法在类加载的时候就存在了，它不依赖于任何实例。
        所以静态方法必须有实现，也就是说它**不能是抽象方法**。
        - 只能访问所属类的静态字段和静态方法，方法中不能有 this 和 super 关键字。
        3. 静态语句块：
        - 静态语句块在类初始化时运行一次。
        4. 静态内部类:
        - 非静态内部类依赖于外部类的实例，而静态内部类不需要。
        - 静态内部类不能访问外部类的非静态的变量和方法。
        5. 静态导包:
        - 在使用静态变量和方法时不用再指明 ClassName，从而简化代码，但可读性大大降低。
        `import static com.xxx.ClassName.*`
        6. 初始化顺序
        - 静态变量和静态语句块优先于实例变量和普通语句块，静态变量和静态语句块的初始化顺序取决于它们在代码中的顺序。
        - 存在继承的情况下，初始化顺序为：
            1. 父类（静态变量、静态语句块）
            1. 子类（静态变量、静态语句块）
            1. 父类（实例变量、普通语句块）
            1. 父类（构造函数）
            1. 子类（实例变量、普通语句块）
            1. 子类（构造函数）
7. 反射
    - 每个类都有一个 Class 对象，包含了与类有关的信息。当编译一个新类时，
    会产生一个同名的 .class 文件，该文件内容保存着 Class 对象。
    - 类加载相当于 Class 对象的加载，类在第一次使用时才动态加载到 JVM 中。
    也可以使用 Class.forName("com.mysql.jdbc.Driver") 这种方式来控制类的加载，
    该方法会返回一个 Class 对象。
    - 反射可以提供运行时的类信息，并且这个类可以在运行时才加载进来，甚至在编译时期该类的 .class 不存在也可以加载进来。
    - Class 和 java.lang.reflect 一起对反射提供了支持，java.lang.reflect 类库主要包含了以下三个类：
        1. Field: 可以使用 get() 和 set() 方法读取和修改 Field 对象关联的字段；
        2. Method: 可以使用 invoke() 方法调用与 Method 对象关联的方法；
        3. Constructor: 可以用 Constructor 创建新的对象。
    - 反射的优点:
        1. 可扩展性: 用程序可以利用全限定名创建可扩展对象的实例，来使用来自外部的用户自定义类。
        2. 类浏览器和可视化开发环境: 一个类浏览器需要可以枚举类的成员。
        可视化开发环境（如 IDE）可以从利用反射中可用的类型信息中受益，以帮助程序员编写正确的代码。
        3. 调试器和测试工具: 调试器需要能够检查一个类里的私有成员。
        测试工具可以利用反射来自动地调用类里定义的可被发现的 API 定义，以确保一组测试中有较高的代码覆盖率。

    - 反射的缺点:
        1. 性能开销 ：反射涉及了动态类型的解析，所以 JVM 无法对这些代码进行优化。
        因此，反射操作的效率要比那些非反射操作低得多。
        我们应该避免在经常被执行的代码或对性能要求很高的程序中使用反射。
        2. 安全限制 ：使用反射技术要求程序必须在一个没有安全限制的环境中运行。
        如果一个程序必须在有安全限制的环境中运行，如 Applet，那么这就是个问题了。
        3. 内部暴露 ：由于反射允许代码执行一些在正常情况下不被允许的操作（比如访问私有的属性和方法），
        所以使用反射可能会导致意料之外的副作用，这可能导致代码功能失调并破坏可移植性。
        反射代码破坏了抽象性，因此当平台发生改变的时候，代码的行为就有可能也随着变化。

8. <a href="#Throwable">异常</a>
9. 泛型
    ```java
    public class Box<T> {
        // T stands for "Type"
        private T t;
        public void set(T t) { this.t = t; }
        public T get() { return t; }
    }
    ```
    1. 泛型类
    ```java
    Box<Integer> integerBox = new Box<Integer>();
    Box<Double> doubleBox = new Box<Double>();
    Box<String> stringBox = new Box<String>();
    ```
    2. 泛型方法
        ```java
        public class Util {
            public static <K, V> boolean compare(Pair<K, V> p1, Pair<K, V> p2) {
                return p1.getKey().equals(p2.getKey()) &&
                       p1.getValue().equals(p2.getValue());
            }
        }
        public class Pair<K, V> {
            private K key;
            private V value;
            public Pair(K key, V value) {
                this.key = key;
                this.value = value;
            }
            public void setKey(K key) { this.key = key; }
            public void setValue(V value) { this.value = value; }
            public K getKey()   { return key; }
            public V getValue() { return value; }
        }
        
        Pair<Integer, String> p1 = new Pair<>(1, "apple");
        Pair<Integer, String> p2 = new Pair<>(2, "pear");
        boolean same = Util.<Integer, String>compare(p1, p2);
        ```
    3. 边界符
        ```java
        public static <T> int countGreaterThan(T[] anArray, T elem) {
            int count = 0;
            for (T e : anArray)
                if (e > elem)  // compiler error 因为除了short, int, double, long, float, byte, char等原始类型，其他的类并不一定能使用操作符>
                    ++count;
            return count;
        }
 
        public interface Comparable<T> {
            public int compareTo(T o);
        }
 
        // 告诉编译器它们都至少实现了compareTo方法
        public static <T extends Comparable<T>> int countGreaterThan(T[] anArray, T elem) {
            int count = 0;
            for (T e : anArray)
                if (e.compareTo(elem) > 0)
                    ++count;
            return count;
        }
        ```
    4. 通配符
        ```java
        public void boxTest(Box<Number> n) { /* ... */ }
        ```
        虽然Integer和Double是Number的子类，但是在泛型中Box<Integer>或者Box<Double>与Box<Number>之间并没有任何的关系
        
        ```java
        class Fruit {}
        class Apple extends Fruit {}
        class Orange extends Fruit {}
        ```
        我们创建了一个泛型类Reader，然后在f1()中当我们尝试Fruit f = fruitReader.readExact(apples);
        编译器会报错，因为List<Fruit>与List<Apple>之间并没有任何的关系。
        
        ```java
        public class GenericReading {
            static List<Apple> apples = Arrays.asList(new Apple());
            static List<Fruit> fruit = Arrays.asList(new Fruit());
            static class Reader<T> {
                T readExact(List<T> list) {
                    return list.get(0);
                }
            }
            static void f1() {
                Reader<Fruit> fruitReader = new Reader<Fruit>();
                // Errors: List<Fruit> cannot be applied to List<Apple>.
                // Fruit f = fruitReader.readExact(apples);
            }
            public static void main(String[] args) {
                f1();
            }
        }
        ```
        按照我们通常的思维习惯，Apple和Fruit之间肯定是存在联系，
        然而编译器却无法识别，那怎么在泛型代码中解决这个问题呢？我们可以通过使用通配符来解决这个问题：
        ```java
        static class CovariantReader<T> {
            T readCovariant(List<? extends T> list) {
                return list.get(0);
            }
        }
        static void f2() {
            CovariantReader<Fruit> fruitReader = new CovariantReader<Fruit>();
            Fruit f = fruitReader.readCovariant(fruit);
            Fruit a = fruitReader.readCovariant(apples);
        }
        public static void main(String[] args) {
            f2();
        }
        ```
        这样就相当与告诉编译器，fruitReader的readCovariant方法接受的参数只要是满足Fruit的子类就行(包括Fruit自身)，
        这样子类和父类之间的关系也就关联上了。
---
title: '[翻译]java io教程'
comments: true
date: 2020-02-22 11:11:15
updated: 2020-02-22 11:11:15
tags:
    - java
    - io  
    - 翻译
    - 教程
categories: java
---
[toc]
---

{% blockquote %}
原文链接：http://tutorials.jenkov.com/java-io/index.html#java-io-api-scope
{% endblockquote %}
  
Java IO 是java自带的为了读入和写出数据而设计的API.绝大所数应用需要进行输入操作然后基于输入数据生成输出数据.
例如:从文件或者网络上读取数据,相对应的,往文件中写入数据或者向网络写出返回一个响应.

Java IO API 位于 Java IO包中.如果你查看在java.io包中的Java IO相关类,繁杂大量的选择会让你迷茫于该选择哪一个来使用.
这些类的是为了应对什么场景?对于给定的特定任务,你应该选择哪一个?你如何创建适合自己业务场景的插件类?等等.

这个教程的目的是对这些IO类是如何组织,如何是使用的给你一个总体的把握.方便你选择正确的类处理对应的任务或者已经有了可以完美解决的类或方法.


A good place to start is the Java IO Overview tutorial. That tutorial gives you a quick overview of the central concepts 
in the Java IO API, and an overview of all the central classes in the Java IO API.
本教程是你吃透Java IO的一个良好指导.教程将带你对JAVA IO API的核心概念和类以及方法,有一个全局的掌握.

## Java IO包的作用域
其实java.io包并没有包括全部类型的输入和输出.例如,从GUI程序或者Web网页的输入和输出就没有包含在java.io包中.这些类型的输入和输出被包含在其他
位置,比如GUI被包括在JFC类在Swing项目中,而Servle 和 HTTP 相关的在J2EE相关包中.

java.io包下的类和方法主要目的在处理文件的输入输出,网络上的流,内存缓冲区等.但是,java.io包不包括网络连接中必须的建立打开网络socket相关类.
由于这个原因,你需要使用Java Networking API.一旦你建立并打开了socke网络连接,之后对数据读写的操作都是通过java IO包中的InputStream类,
和OutputStream类.

## Java NIO —— 另一种IO的选择
Java 也提供了另一种IO API叫做Java NIO。它包含的类和方法和Java IO以及Java Networking APIs提供的功能大致相同，但是Java NIO可以在
**非阻塞**的模式下运行。非阻塞IO相比于阻塞IO在某些情况下会有大幅度的效率提升。

## 更多javaIO的工具，技巧等
本部分教程叫做java有妙招，java 有好物。也包含一些关于java io 的工具。例如：在Stream中替换String，用buffer遍历流等。

## Java IO 类一览表
下面列出的java io 类从输入，输出，以字符为单位，以字节为单位或者更多特定目的，比如缓存，解析等方面分门别类。  

|   | 基于字符输入byte | 输出 | 基于字符输入Character | Output |
| :-----| :----: | :----: | :----: | :----: |
| 基础（Basic） | InputStream | OutputStream | Reader InputStreamReader | Writer OutputStreamWriter |
| 数组（Arrays） | ByteArrayInputStream | ByteArrayOutputStream | CharArrayReader | CharArrayWriter |
| 文件（Files） | FileInputStream RandomAccessFile | FileOutputStream RandomAccessFile | FileReader | FileWriter |
| 管道（Pipes） | PipedInputStream | PipedOutputStream | PipedReader | PipedWriter |
| 缓存（Buffering） | BufferedInputStream | BufferedOutputStream | BufferedReader | BufferedWriter |
| 过滤（Filtering） | FilterInputStream | FilterOutputStream | FilterReader | FilterWriter |
| 解析（Parsing） | PushbackInputStream StreamTokenizer |  | PushbackReader LineNumberReader |  |
| 字符串（Strings） |  |  | StringReader | StringWriter |
| 数据（Data） | DataInputStream | DataOutputStream |  |  |
| 格式化数据（Data-Formatted） |  | PrintStream |  | PrintWriter |
| 对象（Objects） | ObjectInputStream | ObjectOutputStream |  |  |
| 工具（Utilities） | SequenceInputStream |  |  |  |

## Java 5 以及之后版本

教程最开始是基于java5，但是最新的12,13版本也都类似。

## Java IO 总览
在本部分教程将给你Java io 的概述. 特别的，我将通过类和方法的目的给你分类。这个分类会帮助你正确的在将来选择使用对应的类和方法。

### 输入和输出 - 源头和目标

“输入”以及“输出”这两个术语有时候容易引起歧义。一个应用的一部分输入可能是另一个的输出。一个输出流中写入的流，是一个输出流？还是一个来自你
读取的输出流中的流是输出流？毕竟一个输入流输出它从程序中读取到的数据，不是么？我个人觉得当刚接触学习Java IO的时候，这里是有一些困惑的。

出于尝试找到这个困惑的答案，我尝试给input和output赋予不同的名字来从概念上搞清楚，输入流从哪里来，输出流又输出到哪里。

Java中的IO 包几乎只关注从目标原始数据中读取和向目标写原始数据。最典型的源数据和目标数据如下：
    - 文件 Files
    - 管道 Pipes
    - 网络连接 Network Connections
    - 内存缓存区（例如数组） In-memory Buffers (e.g. arrays)
    - System.in, System.out, System.error
    
下图阐述了程序从数据源读取数据和向目标源写入数据的原理：
    ```mermaid
        graph TD;
            Source-->Program-->Destination;
    ```
#### 流（Streams）

IO流是Java IO中的核心概念。流从概念上看是源源不断数据流。你可以从一个流中读取数据，或者写入数据到一个流中。流与数据来源、数据目标密切相关。
Java IO中的流可以基于字节Byte（读取或写入字节），也可以基于字符character（读取或写入字符）。

#### 输入流InputStream, 输出流OutputStream, 读入Reader 和 写出Writer

需要从数据源读入数据的程序需要一个InputStream或者Reader。
需要向目标写入数据的程序需要一个OutputSteam或者Writer。如下所示：
Source --> InputStream / Reader --> Program
Program --> OutputStream / Writer --> Destination

InputStream\Reader与数据源关联.OutputStream\Writer与目标数据有关.

### Java IO 的目的和特性

Java IO 包包含许多InputStream, OutputStream, Reader , Writer类的子类. 
设计如此多子类的原因是为了解决不同的问题，实现不同的目的。详情如下：
    - 访问文件 File Access
    - 访问网络 Network Access
    - 访问内存缓存 Internal Memory Buffer Access
    - 线程间通信(管道) Inter-Thread Communication (Pipes)
    - 缓存 Buffering
    - 过滤器 Filtering
    - 解析器 Parsing
    - 读写文本 Reading and Writing Text (Readers / Writers)
    - 读写基本数据类型 Reading and Writing Primitive Data (long, int etc.)
    - 读写对象 Reading and Writing Objects

当你使用Java IO相关类去读写数据是，最好知道这些类的目的，这能让你更好的理解什么时候该用什么类解决问题。

## Java IO 文件

文件是java应用汇总常见的数据源和目标数据。这部分就带你搞java里的文件。这里不涉及技术细节，但是让你知道什么时候该用什么。单独分页里有详细说明。

### 通过Java IO读取文件

如果你需要从一个端读取文件到另一个端，你可以使用[FileInputStream](#FileInputStream)或者[FileReader](#FileReader)，
通过二进制形式读取用FileInputStream，通过文本化字符流读取用FileReader.这两个类让你一次一个字节或者一次一个字符的从文件的开始读取到文件结尾。
你不用读取整个文件，但是你只能按照文件数据的储存数据顺序读取。

如果你需要跳到文件某一部分，只读取文件某一部分或某几部分数据，可以使用[RandomAccessFile](#RandomAccessFile)。

### 通过Java IO写入文件

通过二进制写入FileOutputStream，通过字符写入FileWriter。一次写一个字节或者一个字符，从文件开始到文件结尾。按照写入顺序存储。

如果你需要跳过某些文件，灵活的写入不同的地方,例如在文件最后写入，你可以用[RandomAccessFile](#RandomAccessFile).

### 通过Java IO随机访问文件

正如我上面提到的，你可通过RandomAccessFile类，随机访问文件。

随机访问并不意味着你真的可以从任意地方读取或写入数据. 
他只是意味着你可以跳过文件，通过任何你想要的方式去同时读取或者写入数据。没有强制特定的访问顺序。这使得覆盖已经存在文件的某一部分，
文件尾写入，从中删除，当然也包括从任何你需要的地方进行读取成为可能。

### 文件和目录信息访问

Sometimes you may need access to information about a file rather than its content. 
For instance, if you need to know the file size or the file attributes of a file. The same may be true for a directory. 
For instance, you may want to get a list of all files in a given directory. 
Both file and directory information is available via the File class.
有时你可能需要访问文件的其他信息，而不是文件内容。例如你需要知道文件大小或者文件属性。对文件目录也一样，你可能需要知道某个目录下所有文件的列表。
文件信息和路径信息都可以通过[File](#File)类获取到。

## Java IO 管道 pipes

    - 管道是java提供给同一个虚拟机上运行的两个线程通信的机制。所以管道可以是数据源也可以使目标数据。
    - 不同jvm不同进程中的线程不能通过管道通信
    - unix、linux中说的管道是运行在不同地址空间的进程通信手段
    - 在java中，通过管道通信必须是运行在统一进程中的不同线程。
    
### 通过Java IO创建管道
    - PipedOutputStream
    - PipedInputStream
    - 一个线程的PipedOutputStream 和 另一个线程的PipedInputStream必须关联
    - 构造方法关联
    -  connect() 方法关联
    ``` java
    import java.io.IOException;
    import java.io.PipedInputStream;
    import java.io.PipedOutputStream;
    
    public class PipeExample {
    
        public static void main(String[] args) throws IOException {
    
            final PipedOutputStream output = new PipedOutputStream();
            final PipedInputStream  input  = new PipedInputStream(output);
    
    
            Thread thread1 = new Thread(new Runnable() {
                @Override
                public void run() {
                    try {
                        output.write("Hello world, pipe!".getBytes());
                    } catch (IOException e) {
                    }
                }
            });
    
    
            Thread thread2 = new Thread(new Runnable() {
                @Override
                public void run() {
                    try {
                        int data = input.read();
                        while(data != -1){
                            System.out.print((char) data);
                            data = input.read();
                        }
                    } catch (IOException e) {
                    }
                }
            });
    
            thread1.start();
            thread2.start();
    
        }
    }
    ``` 
### 管道和线程
    - 当你使用两个关联的管道，传输一个线程的数据流到另一个数据流中时，另一个数据流又传输到再另外一个线程中时，read（）和write（）方法
    在流中调用的时候是阻塞的，如果中间那个线程同时调用read和write方法，可能会造成死锁。
    
### 管道的备选方案
    - 其实大多数情况下，线程间经常直接传输对象，而不是原始字节数据。
    - todo：传对象则如何，在多线程专题中解释。
    
## Java IO 网络

    - 网络知识这里不应该多BB
    - 但是网络连接都是涉及到IO的操作，这几简单BB几句
    - 一旦网络在两个进程之间建立，他们之间的数据传输就像文件传输一样。
    - InputStream 读数据,  OutputStream 写数据
    - 换句话说，通过java 网络api建立的的连接中，还是用java io来读写数据
    - 你能写入文件，就能写入网络。需要改动的只有你写数据的组件是基于OutputStream实现的，而不是FileOutputStream。读入一个鸟样
    ``` java
        public class MyClass {
            
        
            public static void main(String[] args) {
        
                InputStream inputStream = new FileInputStream("c:\\myfile.txt");
            
                process(inputStream);
            
            }
            
        
            public static void process(InputStream input) throws IOException {
                //do something with the InputStream
            }
        }
    ```
    
## Java IO: Byte 和 Char数组

    - 在程序内部，字节和字符数组经常用来临时缓存数据，自然可以统统读进内存数组，然后通过下标访问。
    但是如果你想从InputStream中Reader读取某一部分如何处理。

### 通过InputStream或者Reader读取数组

    - 从数组中读取数据，要通过ByteArrayInputStream或者CharArrayReader包装的字节或者字符。这样array中的字节或字符数据就能被读取到了。
    - CharArrayReader也一样，换个关键字
     ``` java
     byte[] bytes = new byte[1024];
     
     //write data into byte array...
     
     InputStream input = new ByteArrayInputStream(bytes);
     
     //read first byte
     int data = input.read();
     while(data != -1) {
         //do something with data
     
         //read next byte
         data = input.read();
     }
     ```
     
### 通过OutputStream或者Writer写入数组
    - ByteArrayOutputStream或者CharArrayWriter创建，往里面写入数据
    - 写完了之后调用toByteArray()或者toCharArray()
    - CharArrayWriter也一样
     ``` java
        ByteArrayOutputStream output = new ByteArrayOutputStream();
        
        output.write("This text is converted to bytes".getBytes("UTF-8"));
        
        byte[] bytes = output.toByteArray();
     ```
     
## Java中的 System.in, System.out, 和 System.error

    - Java中三种经常用于提供输入输出的三种流
    - System.out输出在控制台
    - 这三个在虚拟机启动时由系统初始化，所以你不必初始化他们（尽管你可以在运行时交换他们）

### System.in
    - 是从控制台读入键盘输入数据的输入流
    - 只能通过控制台或者终端读入与其相关的java程序
    - 不常用，数据经常通过java 运行命令行参数，文件或者网络连接，或者GUI中的输入。
    
### System.out
    - 是一个通过字节写出到控制台或者终端的PrintStream
    - debug用用啥的
### System.err
    - 和out一个狗样，只是文本区别
    - 编译器可能给标个红字意思意思
### 示例
    ``` java
    try {
      InputStream input = new FileInputStream("c:\\data\\...");
      System.out.println("File opened...");
    
    } catch (IOException e){
      System.err.println("File opening failed:");
      e.printStackTrace();
    }
    ``` 
    
### 交换System流
    - 虽然都是 java.lang.System类的静态成员，在jvm启动时就提前实例化了。但是他还是能手动更换。
    ``` java
    OutputStream output = new FileOutputStream("c:\\data\\system.out.txt");
    PrintStream printOut = new PrintStream(output);
    
    System.setOut(printOut);
    ```
    - 这样所写入System.out的数据都被重定向到system.out.txt文件中
    - 记住，这样做你要保证在jvm关闭前，flush System.out流，并且关闭文件。以保证所有数据输入到文件中。
    
## Java IO: 流
    - java io流是你可以读取或者写入的数据流。通常和数据源目标数据相关，比如文件，网络连接等。
    - 流中没有array的索引概念。在流中，你既不可以像在数组中前后移动游标，也不能像在文件操作中使用 RandomAccessFile去随意访问数据。
    流就是一个连续的数据流。
    - 一些流的实现允许你为了之后再次读取，向流中推入数据，比如PushbackInputStream。但是你推入的数据有限，而且你也不能像数组中那样，
    随意遍历数据。数据只能被顺序读取。
    - 基于字节的经常称作流stream，比如InputStream or OutputStream。这些流读取或者写入原始字符数据，
    DataInputStream and DataOutputStream是例外，他们可能读写基础数据类型，比如int, long, float and double values.
    - 基于字符的经常读入器和写出器"Reader" or "Writer"。
    - 基于字符的流可以读入一个Latin1（ISO-8859-1）或者UNICODE编码下的一个字符。
    
### 输入流InputStream
    - java.io.InputStream 是java io输入流的父类。如果你的组件要从流中读入数据，你要继承这个逼，而不是子类。这样能保证你的代码能接受
    所有种类的输入流，而不是某一种具体的子类型。
    - 也不是所有都能基于InputStream，如果你需要向流中回写数据，需要基于PushbackInputStream。
    - 通常调用read()方法。翻译一个int类型的byte值，结束返回-1.
    ``` java
    InputStream input = new FileInputStream("c:\\data\\input-file.txt");
    
    int data = input.read();
    
    while(data != -1){
      data = input.read();
    }
    ```
### 输出流OutputStream
    - java.io.OutputStream是输出流的父类。
    ``` java
    OutputStream output = new FileOutputStream("c:\\data\\output-file.txt");
    output.write("Hello World".getBytes());
    output.close(); 
    ```
    
### 结合流Combining Streams
    - 你可以用缓存流BufferedInputStream包装输入流InputStream， BufferedOutputStream也一样。
    - 你也可以用PushbackStream包装输入流。
    - 或者合并两个输入流到一个 SequenceInputStream时序输入流
    - 自定义类的的包装流
                                                
    ``` java
    InputStream input = new BufferedInputStream(
                            new FileInputStream("c:\\data\\input-file.txt"));
    ```
    
## Java IO: 输入解析
    - String.replace()可以替换，但是每次替换都创建一个对象副本。 O(N * M)，N是String长度，M执行替换次数
    
## Java IO: 读入器和写出器Readers and Writers

    - 和InputStream 、 OutputStream类似，只是基于字符单位
### Reader
    - 包含BufferedReader, PushbackReader, InputStreamReader, StringReader等子类
    ``` java
        Reader reader = new FileReader("c:\\data\\myfile.txt");
    
        int data = reader.read();
        while(data != -1){
            char dataChar = (char) data;
            data = reader.read();
        }
    ```
    - 这并不意味着并不代表每次read读入2个字节的char，这个看具体编码。
    
### 结合InputStreams的Reader
    ``` java
    Reader reader = new InputStreamReader(inputStream);
    ``` 
    
### Writer

    - 包括BufferedWriter 、 PrintWriter
    ``` java
        Writer writer = new FileWriter("c:\\data\\file-output.txt");
        
        writer.write("Hello World Writer");
        writer.close();
    ``` 
    
### 结合Reader和Writers
    ``` java
    Reader reader = new BufferedReader(new FileReader(...));
    
    Writer writer = new BufferedWriter(new FileWriter(...));
    ``` 
## Java IO: 并发IO
    - 如果你不能保证确定每个线程读取多少数据，或者按什么顺序去写出的话，就不应该有一个以上的线程同时从InputStream 或 Reader读取数据，
    也不应该有一个以上的线程同时向OutputStream或 Writer写入数据
    - 如果涉及到不同线程，处理不同数据顺序生命周期，要注意，线程间处理流的代码需要加synchronized
    - 在  Java NIO中，你可以用一个线程通过多个channel渠道读入或者写出数据。例如，你有许多建立的网络连接，但是每个连接上只有少量数据
    （聊天服务器），你可以用一个线程监控多个channel（连接）。
    
## Java IO: 异常处理
    - Streams and Readers / Writers 用完通过调用后close()关闭。 
    - finally中关闭
    - finally中抛出错误怎么办？再try catch
    - close报错呢？异常处理模板。java7以后不用人为处理了？？？
    
## Java InputStream输入流
    - 与数据源相关
    ``` java
        InputStream inputstream = new FileInputStream("c:\\data\\input-text.txt");
        
        int data = inputstream.read();
        while(data != -1) {
          //do something with data...
          doSomethingWithData(data);
        
          data = inputstream.read();
        }
        inputstream.close();
    ``` 
### 子类
    - ByteArrayInputStream
    - FileInputStream
    - PipedInputStream
    - BufferedInputStream
    - FilterInputStream
    - PushbackInputStream
    - DataInputStream
    - ObjectInputStream
    - SequenceInputStream

### read()方法
    - InputStream的read()方法返回int类型的byte值。结尾返回-1的int类型
    -  DataInputStream可以读基础类型
    
### read(byte[])方法     
    - 读入byte数组，返回真实读入的长度。
    
### 读入性能
    - 读数组性能要比单独读入一个字节性能更好
    - 好可能10倍10倍给你凉爽
    - 读取速率决定于你定义的数组长度，操作系统或者硬件等。
    - bugffer 尺寸8kb会更大加速你的读取速率。
    - 但是你的数组容量超过了底层操作或者硬件能力，你可能得不到一个效率提升。
    
### 通过BufferedInputStream的透明缓存
    - Java BufferedInputStream一次读入大量的字符
    - 也有较大速度提升
    - 是InputStream子类，InputStream能用BufferedInputStream就能用
    ``` java
        InputStream input = new BufferedInputStream(
                              new FileInputStream("c:\\data\\input-file.txt"),
                                1024 * 1024        /* buffer size */
            );
    ```
    
### mark() and reset()方法

    - 子类不一定支持，如果支持的话，需要重写markSupported()返回true
    - mark()方法标记当前已经读到数据的位置。可以用read方法从该处继续读取。reset方法再回到mark位置重新读取
    
### 关闭输入流
    - try-catch
###  InputStream 转 Reader
    ``` java
    InputStream inputStream       = new FileInputStream("c:\\data\\input.txt");
    Reader      inputStreamReader = new InputStreamReader(inputStream);
    ```

## Java OutputStream输出流

    - 输出流和目标数据相关
### write(byte)方法

    - 接受int型byte值
    
## Java RandomAccessFile随机访问

    ``` java
    RandomAccessFile file = new RandomAccessFile("c:\\data\\file.txt", "rw");
    ```
    - "rw"读写模式
    
### Access Modes存取模式
| Mode | Description |
|:--:|:--:|
| r | 读. 调用写模式导致IO异常 |
| rw | 读写. |
| rwd | 同步读写模式.所有文件更新同步写入磁盘 |
| rws | 同步读写模式.所有文件或者元数据更新同步写入磁盘 

### 在RandomAccessFile文件中查找

在RandomAccessFile文件中进行读写操作，需要先定位文件指针在要读写的位置，也称作seek

    ``` java
    RandomAccessFile file = new RandomAccessFile("c:\\data\\file.txt", "rw");
    
    file.seek(200);

    ```
    
### 获取文件位置

    ``` java
    long position = file.getFilePointer();

    ```
    
### 读取字节

    ``` java
    RandomAccessFile file = new RandomAccessFile("c:\\data\\file.txt", "rw");
    
    int aByte = file.read();
    
    byte[] dest      = new byte[1024];
    int    offset    = 0;
    int    length    = 1024;
    int    bytesRead = randomAccessFile.read(dest, offset, length);

    ```
    
### 写入

    ``` java
    RandomAccessFile file = new RandomAccessFile("c:\\data\\file.txt", "rw");
    
    file.write(65); // ASCII code for A
    
    byte[] bytes = "Hello World".getBytes("UTF-8");
    file.write(bytes, 2, 5);

    ```
    
### Java File

Xy1018421411563


## <a id="FileInputStream">Java FileInputStream</a>

Java文件输入类FileInputStream（java.io.FileInputStream），支持了从一个文件的字节流中读入内容的功能。FileInputStream是InputStream
的子类。这意味这你用FileInputStream用法类似InputStream。

### Java FileInputStream 用法示例

    ``` java
        InputStream input = new FileInputStream("c:\\data\\input-text.txt");
        
        int data = input.read();
        while(data != -1) {
          //do something with data...
          doSomethingWithData(data);
        
          data = input.read();
        }
        input.close();
    ``` 

提示：为了清晰起见，对应exception的处理这里省去了。想多了解自己滚去看异常去。

### FileInputStream 构造函数

    1. 参数是文件位置路径
    ``` java
        String path = "C:\\user\\data\\thefile.txt";
        
        FileInputStream fileInputStream = new FileInputStream(path);
    ```
    两个```\\```是因为需要转义  
    unix系统下类似这样
        ``` java
            String path = "/home/jakobjenkov/data/thefile.txt";
        ```
    2. 参数是文件对象
        ``` java
            String path = "C:\\user\\data\\thefile.txt";
            File   file = new File(path);
            
            FileInputStream fileInputStream = new FileInputStream(file);
        ```
    3. shit
    
### read()方法
    - 返回包含byte值的int对象
    - 如果返回-1就表示流中再读不到数据了，可以close了
    - 是读到-1的int类型，不是byte类型
    - 用法同InputStream中的read()方法.
        ``` java
            FileInputStream fileInputStream =
                new FileInputStream("c:\\data\\input-text.txt");
            
            
            int data = fileInputStream.read();
            while(data != -1) {
                // do something with data variable
            
                data = fileInputStream.read(); // read next byte
            }
        ```
        while结束循环时，从流中读出所有数据。

### read(byte[])方法
    - 读入byte数组
    - 继承自 Java InputStream 类
    - int read(byte[]) 从流中读取字节直到数组塞满，然后当做参数传入
    - int read(byte[], int offset, int length)从流中读取lengh长度字节，从offset下标开始，放入数组中
    - 返回读入数组的byte数量。如果读入数据的长度小于数组容量，则就读入剩余数据后返回数组长度。
    - 全读入后返回-1
    ``` java
        FileInputStream fileInputStream = new FileInputStream("c:\\data\\input-text.txt");
        
        byte[] data      = new byte[1024];
        int    bytesRead = fileInputStream.read(data, 0, data.length);
        
        while(bytesRead != -1) {
          doSomethingWithData(data, bytesRead);
        
          bytesRead = fileInputStream.read(data, 0, data.length);
        }
    ```
    - read(data, 0, data.length) 等同于 read(data) .
    
### 读入性能
    - 读数组性能要比单独读入一个字节性能更好
    - 好可能10倍10倍给你凉爽
    - 读取速率决定于你定义的数组长度，操作系统或者硬件等。
    - bugffer 尺寸8kb会更大加速你的读取速率。
    - 但是你的数组容量超过了底层操作或者硬件能力，你可能得不到一个效率提升。

### 通过BufferedInputStream的透明缓存
    - Java BufferedInputStream一次读入大量的字符
    - 也有较大速度提升
    - 是InputStream子类，InputStream能用BufferedInputStream就能用
    ``` java
        InputStream input = new BufferedInputStream(
                              new FileInputStream("c:\\data\\input-file.txt"),
                                1024 * 1024        /* buffer size */
            );
    ```
        
### 关闭文件输入流
    - 读完了必须关
    - 调用InputStream中的close()方法 
    ``` java
        FileInputStream fileInputStream = new FileInputStream("c:\\data\\input-text.txt");
        
        int data = fileInputStream.read();
        while(data != -1) {
          data = fileInputStream.read();
        }
        fileInputStream.close();
    ```
    - 不是100%鲁棒的，如果中间出现读取异常，永远不close
    - 所以finally
    ``` java
        try( FileInputStream fileInputStream = new FileInputStream("file.txt") ) {
        
            int data = fileInputStream.read();
            while(data != -1){
                data = fileInputStream.read();
            }
        }
    ```
    
### FileInputStream 转 Reader
    ``` java
        InputStream inputStream       = new FileInputStream("c:\\data\\input.txt");
        Reader      inputStreamReader = new InputStreamReader(inputStream);
    ```

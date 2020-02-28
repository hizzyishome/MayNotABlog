---
title: java 中的动态代理
comments: true
date: 2020-02-10 21:21:47
updated: 2020-02-10 21:21:47
tags:
    - java
    - 设计模式
categories: 码文
---
1. 动态类型语言 静态类型语言: 语言类型信息在运行时检查,还是编译期检查
2. 强类型语言 弱类型语言: 不同类型变量赋值时,是否需要显示的进行类型转换
3. 反射机制是 Java 语言提供的一种基础功能，赋予程序在运行时自省（introspect，官方用语）的能力。通过反射我们可以直接操作类或者对象，
比如获取某个对象的类定义，获取类声明的属性和方法，调用方法或者构造对象，甚至可以运行时修改类定义。
4. 动态代理是一个代理机制。代理可以看作是对调用目标的一个包装，对目标代码的调用不是直接发生的，而是通过代理完成。
其实很多动态代理场景，我认为也可以看作是装饰器（Decorator）模式的应用.
5. 代理可以让调用者与实现者之间解耦.比如进行 RPC 调用，框架内部的寻址、序列化、反序列化等，对于调用者往往是没有太大意义的，通过代理，
可以提供更加友善的界面。
6. JDK Proxy 的优势：
    1. 最小化依赖关系，减少依赖意味着简化开发和维护，JDK 本身的支持，可能比 cglib 更加可靠。
    2. 平滑进行 JDK 版本升级，而字节码类库通常需要进行更新以保证在新版 Java 上能够使用。
    3. 代码实现简单。
7. 基于类似 cglib 框架的优势：
    1. 有的时候调用目标可能不便实现额外接口，从某种角度看，限定调用者实现接口是有些侵入性的实践，类似 cglib 动态代理就没有这种限制。
    2. 只操作我们关心的类，而不必为其他相关类增加工作量。
    3. 高性能。
8. 静态代理：事先写好代理类，可以手工编写，也可以用工具生成。缺点是每个业务类都要对应一个代理类，非常不灵活。
9. 动态代理：运行时自动生成代理对象。缺点是生成代理代理对象和调用代理方法都要额外花费时间。
10. JDK动态代理：基于Java反射机制实现，必须要实现了接口的业务类才能用这种办法生成代理对象。新版本也开始结合ASM机制。
11. cglib动态代理：基于ASM机制实现，通过生成业务类的子类作为代理类。  

#### 项目实操

1. 项目背景: 由于家技术架构改革,强行接入基于grpc的服务.在build请求请求和解析结果的时候十分繁琐,为了快速排查问题,使用动态代理打印请求和结果.
2. 心路历程:
    1. 所有的资料和文档都说通过jdk的实现的动态代理是基于接口的.然后就也假模假式的定义了个接口.
    2. 可是为啥必须定义接口?
    ```java
        public class MyDynamicProxy {
            public static  void main (String[] args) {
                HelloImpl hello = new HelloImpl();
                MyInvocationHandler handler = new MyInvocationHandler(hello);
                // 构造代码实例
                Hello proxyHello = (Hello) Proxy.newProxyInstance(HelloImpl.class.getClassLoader(), HelloImpl.class.getInterfaces(), handler);
                // 调用代理方法
                proxyHello.sayHello();
            }
        }
        interface Hello {
            void sayHello();
        }
        class HelloImpl implements  Hello {
            @Override
            public void sayHello() {
                System.out.println("Hello World");
            }
        }
         class MyInvocationHandler implements InvocationHandler {
            private Object target;
            public MyInvocationHandler(Object target) {
                this.target = target;
            }
            @Override
            public Object invoke(Object proxy, Method method, Object[] args)
                    throws Throwable {
                System.out.println("Invoking sayHello");
                Object result = method.invoke(target, args);
                return result;
            }
        }
    ```
    这是本身的代码,为啥我非得实现那个该死的hello接口?(翻译腔)?? 删除试试,运行
    ```bash
       Exception in thread "main" java.lang.ClassCastException: com.sun.proxy.$Proxy0 cannot be cast to proxy.Hello
       	at proxy.MyDynamicProxy.main(MyDynamicProxy.java:10)
   
    ```
    强转出问题了,代理类0不能强转成Hello类,为何?咱们冷静分析下.
    这还用冷静分析?太智障了,hello和helloImpl在删除了implement hello 之后,根本没关系,你强转Hello那是必定要爆炸的,因为你传入newProxyInstance
    方法中的参数,都是helloImpl的相关类信息.**Proxy.newProxyInstance(HelloImpl.class.getClassLoader(), HelloImpl.class.getInterfaces(), handler);**  
    继续冷静分析,那就是说通过实现了Hello接口,所以我传HelloImpl给newProxyInstance方法,这个方法一定通过**HelloImpl.class.getInterfaces()**
    找到了实现的接口类Hello,之后根据被代理的接口来动态生成代理类的class文件，动态生成的代理类已经继承了Proxy类的，就不能再继承其他的类，
    所以只能靠实现被代理类的接口的形式，故JDK的动态代理必须有接口。超类代理目标类.
    3. spring boot中使用  
    MyInvocationHandler 可以让框架帮你创建而已.
3. 总结:
    - 本质:超类代理目标类.或者目标类的新实例,本身
    - 需要和想要代理的类建立联系
    - java单继承,所以只能实现接口
    - invoke方法中利用jdk反射的方式去调用真正的被代理类的业务方法,需要反射获得代理类的有关参数，必须要通过某个类，反射获取有关方法
    - 成功返回的是object类型，要获取原类，只能继承/实现，或者就是那个代理类.
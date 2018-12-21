---
title: 当一只活的springboot报错Cannot load driver class: com.mysql.jdbc.Driver时
comments: true
date: 2018-12-21 15:27:51
updated: 2018-12-21 15:27:51
tags:
    -springboot
    -java
categories: 干炒springboot心得
---

> 文明人遇到了莫名其妙的错误时，不能骂娘，要和蔼可亲的问候各种组件的祖宗。
> ——《华哗哗的代码修炼之道》

  其实之前接触过一些springboot的东西了，只是这些打算自己搭一次，当一个以后的种子项目。
  结果第一步就踏进了坑里，里面都是钉鸡，鸡儿冲上=。=

  怎么开始我就不赘述了，网上有挺多挺好的引导。总之要是你启动时，报错如下：

  ```
  org.springframework.beans.factory.UnsatisfiedDependencyException:
  Error creating bean with name 'org.springframework.boot.autoconfigure.orm.jpa.HibernateJpaConfiguration':
  Unsatisfied dependency expressed through constructor parameter 0; nested exception is org.springframework.beans.factory.BeanCreationException:
  Error creating bean with name 'dataSource' defined in class path resource [org/springframework/boot/autoconfigure/jdbc/DataSourceConfiguration$Hikari.class]:
  Bean instantiation via factory method failed; nested exception is org.springframework.beans.BeanInstantiationException:
  Failed to instantiate [com.zaxxer.hikari.HikariDataSource]: Factory method 'dataSource' threw exception; nested exception is java.lang.IllegalStateException:
  Cannot load driver class: com.mysql.jdbc.Driver
  ```

  首先，如果`pom.xml` 文件不引用`spring-boot-starter-jdbc`的话，
  只有`spring-boot-starter-web`是不会报错的，因为不涉及数据链接。
  ```
  <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-jdbc</artifactId>
  </dependency>
  ```
  但是加上后，springboot默认你是需要数据链接的，在`src/main/resources/application.properties`
  中就需要添加
  ```
  spring.datasource.url=jdbc:mysql://localhost:3306/test
  spring.datasource.username=dbuser
  spring.datasource.password=dbpass
  spring.datasource.driver-class-name=com.mysql.jdbc.Driver

  ```

  添加了还是报错的话，你就继续看下去吧。

  爆栈上最多的vote回答都是：
  > Looks like the initial problem is with the auto-config.
  If you don't need the datasource, simply remove it from the
  auto-config process:
  `@EnableAutoConfiguration(exclude={DataSourceAutoConfiguration.class})`

  如果到此你在启动类上加了这个注解，你就满意的话，出门右拐不送。

  但是我，真的是要用DB的啊～

  来，按部就班的把这只活的springBoot，放在案板上，裹上面包糠，炸至金黄～算了，看排查不走吧，这个货一看就不好吃

  1. `pom.xml` 文件引用了正确了没？
  - 需要一个带有jdbc驱动的依赖引入，比如`mysql-connector-java`
  ```
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>5.1.46</version>
    </dependency>
  ```

  2. `application.properties`文件的空格问题
  - 看看你各个配置有没有多余的空格
  - 尤其`spring.datasource.driver-class-name=com.mysql.jdbc.Driver`后面有没有

  3. 你要知道你的项目是基于maven构建的，maven这个东西你懂得，是不是也会抽风。
  - 看看你的`.m2`文件夹里面有没有mysql的相关包
  - 没有就clean 然后reimport
  - 保证路径下真的能找到

##### 我就是3.的问题，别忽略了你整个的环境，这才是最要命的，让我们山呼万岁
##### 等待AI代替程序员编码的日子到来，我就能归园田居去种地了。
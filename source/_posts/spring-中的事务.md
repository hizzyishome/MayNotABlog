---
title: spring 中的事务
comments: true
date: 2019-12-08 10:46:58
updated: 2019-12-08 10:46:58
tags:
    - spring
    - 数据库
    - 事务
categories:
---
### spring中的事务抽象
在spring中抽象帮助不同的数据框架ORM都能用相同的方式去做操作。  
1. 一致的事务模型
    -  操作数据的方式：JDBC Hibernate myBatis
    - DataSource JTA
2. 事务抽象类里的核心接口
    - PlatformTransactionManager
        - DataSourceTransactionManager
        - HibernateTransactionManager
        - JtaTransactionManager
    - TransactionDefinitions(事务定义)
        - Propagation(传播特性)
        - Isolation(隔离性)
        - Timeout(超时时间)
        - Read-only status(只读状态)  
        在PlatformTransactionManager中定义了commit rollback 方法实现统一事务提交，回滚操作。
        通过getTransaction方法从TransactionDefinitions获取两个接口的参数TransactionStatus。
    1. 事务的传播特性  
    
     传播性|值|含
     -|-|-
     PROPAFGATION_REQUIRED|0|默认，有事务就用当前事务，没有就创建
     PROPAFGATION_SUPPORTS|1|事务可有可无，不是必须的
     PROPAFGATION_MANDARTORY|2|当前必须要有事务，没有就报错
     PROPAFGATION_REQUIRES_NEW|3|无论是否有事务，都起新事务,之前的挂起
     PROPAFGATION_NOT_SUPPORTED|4|不支持事务，按照非事务方式运行
     PROPAFGATION_NEVER|5|不支持事务，有事务就抛异常
     PROPAFGATION_NESTED|6|内嵌事务，如果有事务，在当前事务中再起一个事务，但是内嵌事务的结果（是否回滚）不影响外部事务的执行
        
    2. 事务的隔离性  
    
    默认-1 需要配置

    隔离性|值|脏读|不可重复读|幻读
    -|-|-|-|-
    ISOLATION_READ_UNCOMMITTED|1|√|√|√
    ISOLATION_READ_UNCOMMITTED|2|×|√|√
    ISOLATION_READ_UNCOMMITTED|3|×|×|√
    ISOLATION_READ_UNCOMMITTED|4|×|×|×
    
3. 编程式事务
    1. Transaction Template(基本简单方式)
        - TransactionCallback
        - TransactionCallbackWithoutResult 
    2. PlatformTransactionManager
        - 可以传入TransactionDefinittion进行定义
        
4. 声明式事务  
    1. 利用aop-proxy在目标方法上操作。  
        1. caller调用proxy代理，而不是直接调用目标方法
        1. 代理掉用Transaction Advisor，此时创建事务
        1. 代理掉用Transaction Advisor调用Custom Advisor
        1. 运行用户interceptors的before操作
        1. 调用目标方法，执行业务逻辑
        1. 执行后，返回结果，也可能没有结果返回void
        1. Custom Advisor运行用户interceptors的after操作，返回给Transaction Advisor
        1. Transaction Advisor事务操作成功commit，失败rollback
        1. 返回给proxy代理
        1. 返回给调用者 caller
    2. 配置方式
        1. 开启方式 注解 `@EnabelTransactionManagement` xml `<tx:annotation-driver>`
        1. 配置
            - proxyTargetClass true false 基于接口还是基于类
            - mode  Aop 默认java 可以改Spj
            - order 事务Aop拦截顺序，默认最低
        1. 方法或类上加`@Transactional`
            - transactionManager
            - propagation
            - isolation
            - timeOut
            - readOnly
            - 判断回滚策略（可配置某些异常类型回滚）
        1. 一个没有配置`@Transactional`的方法，内部调用一个配置了`@Transactional`的方法
        不会有事务的支持（外方法没有调用代理去做操作）。默认情 况下，spring aop同级调用失效，
        因为同级无法创建代理对象，而事务是通过aop代理类实现的。
        最简单是，把自己的实例注入进来，内部直接调用改成，用自己的实例
        调用，因为自己的实例spring帮你创建了一个代理类，我们直接调用就行。也可以通过
        AopContext.currentProxy()获取当前类的代理对象，再调用子方法，其实是增强后的的方法
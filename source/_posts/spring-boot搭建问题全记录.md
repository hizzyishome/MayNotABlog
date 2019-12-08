---
title: spring boot搭建问题全记录
comments: true
date: 2019-12-07 16:11:30
updated: 2019-12-07 16:11:30
tags:
    - java
    - spring boot
    - 记录
categories: 码文
---
1. actuator/beans 访问返回 `Whitelabel Error Page This application has no explicit mapping for /error, so you are seeing this as a fallback.`错误
    你可以试试直接访问 http://localhost:8080/actuator/,返回
    `{"_links":{"self":{"href":"http://localhost:8080/actuator","templated":false},"health":{"href":"http://localhost:8080/actuator/health","templated":false},"health-path":{"href":"http://localhost:8080/actuator/health/{*path}","templated":true},"info":{"href":"http://localhost:8080/actuator/info","templated":false}}}`
    可以看见默认开放的只有`health``info` 和 `health-path`（看某个接口的health状态）。
    在application.properties文件里配置`management.endpoints.web.exposure.include=*` 可以根据自己需要打开某些接口。
    配置后可以发发现可以看项目中的很多信息，包括初始化的bean,caches-cache,caches,conditions,configprops,env,loggers,mappings等等
2. 
3. 
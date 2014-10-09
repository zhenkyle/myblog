---
layout: post
title:  "DAO 层设计方案: 如何减少普通CURD操作的重复"
date:   2014-09-29 20:44
categories: 
---

在普通DAO层设计方案中,存在大量的普通CURD操作重复代码.解决方案如下

###使用泛型Hibernate DAO

###一种直接基于hibernate.SessionFactory的实现
[参考](http://1194867672-qq-com.iteye.com/blog/1159918)

####一种基于Spring Hibernate Support 的普通的实现

[参考](http://blog.csdn.net/superzzz/article/details/6720348)

####一种使用AOP的实现

[参考](http://www.ibm.com/developerworks/cn/java/j-genericdao.html)

###使用Spring data JPA

这是最好的方式

[参考Springside](https://github.com/springside/springside4) 或 Spring data JPA文档
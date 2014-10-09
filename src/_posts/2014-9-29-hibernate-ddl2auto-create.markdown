---
layout: post
title:  "Hibernate ddl2auto create 方式的应用"
date:   2014-09-29 11:46
categories: Hibernate
---

设置`hibernate.hbm2ddl.auto`等于`create`之后,haibernate会:

* 每次启动时drop 掉所有的数据库表，并重新建表

* 如果classpath的根下有`import.sql`，就执行它 (import.sql要用本地编码方式，如在windows下gbk,在mac/linux下utf8)



因此,`hibernate.hbm2ddl.auto`等于`create`，在开发阶段导入测试初始数据时非常有用，但千万不能在生产环境中用，在生产环境中用会导致数据丢失。


与`create`方式类似的是`hibernate.hbm2ddl.auto`的`create-drop`方式会在hibernate程序每次退出时，drop所有表。

在生产环境中应使用`none`或`auto`方式。


[参考](http://stackoverflow.com/questions/673802/how-to-import-initial-data-to-database-with-hibernate)
---
layout: post
title:  "Mysql 在Windows 下中文显示问号问题"
date:   2014-09-25 9:18
categories: 
---

Mysql 5.6, 通过Chacolatey安装,在Windows下mysql 命令和Mysql Workbench中文显示乱码


###问题原因
Mysql 在window下安装后，确实字符集是latin1，这样处理utf8和gbk都有问题，需要改。

###解决办法

在`C:\tools\mysql\current\my.ini`文件，加入：


```
[mysqld]
character_set_server=utf8

[client]
default-character-set=utf8
```

然后重启mysqld

```
net stop mysqld
net start mysqld
```

###测试

分别进入mysql 和 mysql workbench


看所有字符集

```
show character set;
或
show char set;
```

看当前

```
status 或 /s (mysql下)
或
show variables like '%char%'
```

看库
```
show create database dbname;
```

看表
```
show create table tblname /G;
```

[参考](http://blog.csdn.net/dreamthen/article/details/38297003)
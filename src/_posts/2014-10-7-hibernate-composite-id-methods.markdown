---
layout: post
title:  "实现 Hibernate composite id 的几种方式"
date:   2014-10-7 11:27
categories: 
---

网上很多的资料都是过时的或不全面。最新、最全的资料应该来自Hibernate官方文档 5.1.2.1. Composite identifier节。

###对于新项目尽量不要使用composite id

composite id 仅仅是用于对付legacy 数据库的。对于新项目，应该让每个实体都有一个id字段，该字段可以是不代表业务含义的逻辑id。

###实现composite id 有3种大方式

####@EmbeddedId方式

又分两种方式

##### JPA 方式

在@Entity中定义@ManyToOne @OneToOne等关系，@Embeddable 中只能是id字段，不能是@Entity

##### 不兼容JPA方式

在@Embeddable 中定义@ManyToOne @OneToOne等关系，@Embeddable 中可以是@Entity

#### 多@id方式

仅Hibernate支持，很多使用`hbm.xml`的老代码都是这种方式

#### @IdClass 方式

不推荐使用

###参考
[Hibernate 官方文档 5.1.2.1. Composite identifier](http://docs.jboss.org/hibernate/orm/4.3/manual/en-US/html/ch05.html#mapping-declaration-id)

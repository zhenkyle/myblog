---
layout: post
title:  "Hibernate POJO id 的设计策略"
date:   2014-09-29 11:25
categories: Struts Hibernate
---

当在Hibernate 中设计POJO 时,每一个POJO都应有一个id.

如果在前台界面(strtus)中不准备让用户自行维护该id,应将该id设计为自增型.

```
<generator class="identity" />
```


如果在前台界面(struts)中准备让用户自行维护该id，可将该id设计为普通非自增型数据类型,然后再strtus中提供用户界面让用户维护.

更复杂的组合id,应建立一个自增性id作为无业务意义的物理id,再在业务逻辑层面保证组合id的唯一性.
---
layout: post
title:  "Eclipse 报警: must override a supperclass method"
date:   2014-09-27 9:25
categories: 
---

实现接口时,报这个错.

解决方法

项目上点右键 进 `Properties - Java Build Path - Libraries`

将`JRE System Library [JavaSE - 1.5]` 改为 `JRE System Library [JavaSE - 1.7]`

再检查 `Properites - Java Compiler`

应打钩 `Use compliance from execution environment 'JavaSE-1.7' on the 'Java Build Path`

###参考
[参考1](http://stackoverflow.com/questions/11298358/method-must-override-a-superclass-method-latest-sdk)

[参考2](http://mmqzlj.blog.51cto.com/2092359/803060)

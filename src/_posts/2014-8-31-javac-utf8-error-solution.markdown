---
layout: post
title:  "Javac “编码 UTF8 的不可映射字符“ error solution"
date:   2014-08-31 10:34:00
categories: java
---

用ant编译时出现“编码 UTF8 的不可映射字符“，这是因为编译的时候编码跟文件存储的编码方式不一样造成的

用GBK编码的，那么就会出现“编码 UTF8 的不可映射字符“，解决办法：  

```
<javac srcdir="src" destdir="build/classes">
     <compilerarg line="-encoding GBK "/>  
</javac>
```

以上语句告诉ant用GBK编码方式编译源文件

[来源](http://blog.csdn.net/seeds_home/article/details/7327728)

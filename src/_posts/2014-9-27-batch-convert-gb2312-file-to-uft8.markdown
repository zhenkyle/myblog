---
layout: post
title:  "批量装换gb2313文件到utf8"
date:   2014-09-27 9:25
categories: 
---

###在Linux下

```
files=`find . -name '*.java' | xargs enca -L zh | grep GB2312 | cut -d: -f1`
for f in $files; do
  iconv -f GBK -t UTF-8 $f > $f.utf && mv -f $f.utf $f && echo "$f done"
done
```

###参考
[参考1](http://www.cnblogs.com/bamanzi/archive/2010/06/11/2547025.html)
[参考2](http://demi-panda.com/2013/01/12/linux-encoding-convert/)

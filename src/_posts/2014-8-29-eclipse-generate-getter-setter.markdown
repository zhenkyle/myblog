---
layout: post
title:  "Eclipse Trick: generate getter setter"
date:   2014-08-29 9:01:00
categories: eclipse
---

Convert
```
package com.mycompany.app.service;

public class PersonService {
  private String name;

}
```

to

```
package com.mycompany.app.service;

public class PersonService {
  private String name;

  public String getName() {
    return this.name;
  }
  public void setName(String name) {
    this.name = name;
  }
}```

Go Source Menu - Generate Getter and Setters

[Eclipse用法和技巧六：自动生成get和set方法1](http://blog.csdn.net/maybe_windleave/article/details/8852555)
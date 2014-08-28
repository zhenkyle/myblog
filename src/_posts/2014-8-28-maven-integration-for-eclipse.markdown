---
layout: post
title:  "Maven Integration for eclipse"
date:   2014-08-28 17:27:00
categories: maven proxy
---

### About Maven Integration(m2e) for eclipse
m2e is the offical , first class Maven plugin for Eclipse IDE.

[m2e Home](http://www.eclipse.org/m2e/)

### m2e not work behind proxy problem

I'm behind a NTLM proxy, I tried many ways, but I still can't get m2e load online artifacts catalog files.

There is [Bug reported by someone earily](https://bugs.eclipse.org/bugs/show_bug.cgi?id=355774).

#### Work Around 1

I can use maven command line to generate a maven project, the import the exsiting maven project into Eclipse.

#### Work Around 2

Use another way, use `Maven Eclipse Plugin (m-e-p)`. This one is a maven command line plugin, it is not compatible with m2e. Every time you change POM file, you should run this plugin to regenerate eclipse project file.

See [Apache Maven Site](http://maven.apache.org/eclipse-plugin.html) for details.
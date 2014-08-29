---
layout: post
title:  "Setting proxy for Maven"
date:   2014-08-28 17:03:00
categories: proxy
---

### About Maven
Maven is a software project management and comprehension tool with dependency management. On the contrary, `Ant` is a build tools.

[Apache Maven Project Home](http://maven.apache.org/)

[The Maven Central Repository](http://search.maven.org/)

### Setting http proxy for maven command line.

In your user directory (e.g. c:\users\steve or /users/steve), open the .m2 folder (you may need to enable hidden folders on your file browser). open the settings.xml file. If it does not exist, create it.

Add the following lines with the values replaced with the values specific to your proxy:

```
<settings xmlns="http://maven.apache.org/SETTINGS/1.1.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.1.0 http://maven.apache.org/xsd/settings-1.1.0.xsd">
  <proxies>
    <proxy>
      <active>true</active>
      <protocol>http</protocol>
      <host>127.0.0.1</host>
      <port>8123</port>
      <username></username>
      <password></password>
      <nonProxyHosts/>
      <id>1</id>
    </proxy>
  </proxies>
</settings>
```

Now Maven command line should work with proxy.
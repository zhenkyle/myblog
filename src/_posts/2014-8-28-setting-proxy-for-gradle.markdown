---
layout: post
title:  "Setting proxy for gradle"
date:   2014-08-28 15:28:00
categories: proxy
---

### About Gradle
Gradle is a build automation tool. It combines the flexibility of `Ant` and the convention of `Maven`. 

See [Gradle Home Page](http://http://www.gradle.org/) for details.

### Setting http proxy for gradle by Command Line

```
gradlew -Dhttp.proxyHost=127.0.0.1 -Dhttp.proxyPort=8123 build
```

Works for me.

[Reference](http://stackoverflow.com/questions/5991194/gradle-proxy-configuration)

### Setting http proxy for gradle by configure file

In your user directory (e.g. c:\users\steve or /users/steve), open the .gradle folder (you may need to enable hidden folders on your file browser). open the gradle.properties file. If it does not exist, create it.

Add the following lines with the values replaced with the values specific to your proxy:

```
systemProp.http.proxyHost=129.0.0.1
systemProp.http.proxyPort=8123
systemProp.http.proxyUser=userid
systemProp.http.proxyPassword=password
systemProp.http.nonProxyHosts=*.nonproxyrepos.com|localhost
```

Untested.

[Reference](http://codetutr.com/2013/03/27/configuring-gradle-behind-a-proxy/)
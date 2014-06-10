---
layout: post
title:  "Jave EE 开发环境配置"
date:   2014-3-4 16:46:00
categories: javaee
---

以下步骤是Java EE 开发环境在32位Windows环境中的配置。

安装JDK
---

到[Oracle网站][http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html]下载 jdk-7u51-windows-i586.exe

安装

配置环境变量
{% highlight ruby %}
JAVA_HOME = C:\Program Files\Java\jdk1.7.0_51  #JAVA_HOME 代表 JDK安装路径
CLASSPATH = %JAVA_HOME%\lib\tools.jar;%JAVA_HOME%\lib\dt.jar
PATH 中加入 %JAVA_HOME%\bin  #这条好像不做也可以
{% endhighlight %} 
 
安装Tomcat
---

到[Tomcat网站][http://tomcat.apache.org/download-70.cgi] 下载 7.0.52 -- Binary Distribution - Core - [32bit Windows zip][http://mirrors.cnnic.cn/apache/tomcat/tomcat-7/v7.0.52/bin/apache-tomcat-7.0.52-windows-x86.zip]

解压缩到 d:\java_dev\apache-tomcat-7.0.52 中

配置环境变量
{% highlight ruby %}
Tomcat 只需要 JAVA_HOME正常设置
{% endhighlight %} 

启动Tomcat
{% highlight ruby %}
cd d:\java_dev\apache-tomcat-7.0.52\bin
startup.bat
{% endhighlight %} 

停止Tomcat
{% highlight ruby %}
cd d:\java_dev\apache-tomcat-7.0.52\bin
shutdown.bat
{% endhighlight %} 


安装Ant
---

到[Ant 网站][http://ant.apache.org/bindownload.cgi] 下载[apache-ant-1.9.3-bin.zip][http://apache.fayea.com/apache-mirror//ant/binaries/apache-ant-1.9.3-bin.zip]

解压缩到 d:\java_dev\apache-ant-1.9.3 中

配置环境变量
{% highlight ruby %}
ANT_HOME = d:\java_dev\apache-ant-1.9.3
PATH 中加入 %ANT_HOME%\bin
{% endhighlight %} 


安装MySQL 5.1或更高版本
---
此处略

安装Eclipse
---

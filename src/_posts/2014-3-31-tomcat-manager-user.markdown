---
layout: post
title:  "Tomcat 控制台用户设置"
date:   2014-3-4 16:46:00
categories: javaee
---

登录Tomcat控制台“Manager App”，需要输入用户名、密码

设置方法如下，编辑conf\tomcat-users.xml
{% highlight xml %}
<tomcat-users>
<role rolename="manager-gui"/>
<user username="tomcat" password="zhenke1979" roles="manager-gui"/>
</tomcat-users>
{% endhighlight %} 

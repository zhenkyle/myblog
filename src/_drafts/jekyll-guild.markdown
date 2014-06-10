---
layout: post
title:  "jekyll建站指南"
date:   2013-12-27 16:30:02
categories: jekyll
---

简介
---


安装
---

###git 初始设置
{% highlight bash %}
git config --global user.name "Your name"
git config --global user.email youname@example.com
{% endhighlight %}

使用
---

###使用drafts


根据 [jekyll doc: Working with drafts](http://jekyllrb.com/docs/drafts/):

在jekyll根目录下建立`_drafts`目录

在`_drafts`目录下建立不带日期的post,如:`a-draft-post.markdown`

`jekyll server --drafts` 或`jekyll build --drafts` 预览

常见问题
---

###`jekyll server`或 `jekyll build` 报错`jekyll invalid byte sequence in GBK`

{% highlight bash %}
chcp 65001
{% endhighlight %}

### `ssh-keygen -t rsa -C "yourname@example.com"`报错`Could not create directory '//.ssh'`

在Portable Jekyll目录下建`home`目录

{% highlight bash %}
set HOME=%~dp0home
{% endhighlight %}

参考 [Github Help: Generating SSH Keys](https://help.github.com/articles/generating-ssh-keys)

### `git add .` 报`warning: LF will be replaced by CRLF in xxx file`

```
git config core.safecrlf false
```

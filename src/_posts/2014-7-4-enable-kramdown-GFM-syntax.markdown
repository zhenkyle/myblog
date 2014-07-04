---
layout: post
title:  "Enable Jekyll kramdown GFM syntax"
date:   2014-07-04 18:05:02
categories: 
---

From [jekyll doc](http://jekyllrb.com/docs/configuration/)

###Kramdown

In addition to the defaults mentioned above, you can also turn on recognition of Github Flavored Markdown by passing an ```input``` option with a value of “GFM”.

For example, in your ```_config.yml```:

```
kramdown:
  input: GFM
```
---
layout: post
title:  "Convert shadowsocks into an http proxy"
date:   2014-07-04 12:18:02
categories: sublime_text
---

### On Windows

[Download polipo for windows](httpwww.pps.univ-paris-diderot.fr~jchsoftwarefilespolipo)

```
polipo socksParentProxy=localhost:1080

set http_proxy=http127.0.0.1:8123
```

Now you can play with http proxy
```
gem install bundler
wget www.google.com
curl www.google.com

git config --global http.proxy://127.0.0.1:8123
git clone https://github.com/xxx/xxx.git
git xxx
git xxx
git config --global --unset-all http.proxy

```

### On Linux
[Reference](httpsgithub.comclowwindyshadowsockswikiConvert-Shadowsocks-into-an-HTTP-proxy)

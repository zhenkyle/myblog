---
layout: post
title:  "XPEnology NAS install DSM 5.0 4493 Update 4"
date:   2014-08-29 17:34:00
categories: xpenology
---

The same as install 4493 Update 3

Download update but not install using web UI

ssh to dsm

`sed 's/flashupdateDeb/flashupdateDeb1/' /autoupd@te.info > /autoupd@te.info1`

`mv /autoupd@te.info1 /autoupd@te.info`

Now install update using web UI


### Reapply DNSPod.cn patch

```
sed -i -re '/\[DNSPod\.cn\]/{n;s@modulepath=.+@modulepath=/sbin/dnspod.sh@}' /etc.defaults/ddns_provider.conf
sed -i -re '/\[DNSPod\.cn\]/{n;s@modulepath=.+@modulepath=/sbin/dnspod.sh@}' /etc/ddns_provider.conf
```

`/sbin/dnspod.sh` is my dnspod update shell.

[Reference](http://www.xpenology.nl/installatie-dsm-5-0-4493-update-3/)
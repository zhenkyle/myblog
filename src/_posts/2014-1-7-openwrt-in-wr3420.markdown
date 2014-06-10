---
layout: post
title:  "TL-MR3420刷openwrt固件笔记"
date:   2014-1-7 15:53:00
categories: openwrt
---

下载固件 attitude_adjustment 12.09版
---

下载 [openwrt-ar71xx-generic-tl-mr3420-v1-squashfs-factory.bin](http://downloads.openwrt.org/attitude_adjustment/12.09/ar71xx/generic/openwrt-ar71xx-generic-tl-mr3420-v1-squashfs-factory.bin)



通过web 界面刷机
---

通过http://192.168.1.1 admin/admin 上传固件刷机，参考[Openwrt mr3420 wiki](http://wiki.openwrt.org/toh/tp-link/tl-mr3420)

第一次login
---

参考 [Openwrt wiki firstlogin](http://wiki.openwrt.org/doc/howto/firstlogin)

基本配置
---

参考 [Openwrt wiki basic config](http://wiki.openwrt.org/doc/howto/basic.config)

USB配置
---

参考 [Openwrt wiki usb overview](http://wiki.openwrt.org/doc/howto/usb.overview)

exroot
---

MR3420和wr703n一样只有4M flash，需要做exroot。参考 [wr703n做exroot的方式](/jekyll/2013/12/27/openwrt-in-wr703n.html)。

常用软件安装
---

参考 [wr703n安装的软件](/jekyll/2013/12/27/openwrt-in-wr703n.html)

有用的其他资源
---

[Openwrt wiki generic backup](http://wiki.openwrt.org/doc/howto/generic.backup)
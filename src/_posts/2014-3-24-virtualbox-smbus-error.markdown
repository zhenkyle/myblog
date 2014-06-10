---
layout: post
title:  "VirtualBox SMBus error"
date:   2014-3-24 12:34:00
categories: tools
---

VirtualBox4.36安装Ubuntu13.10报
{% highlight ruby %}
piix4_smbus 0000.00.07.0: SMBus base address uninitialized – upgrade bios or use force_addr=0xaddr
{% endhighlight %} 

解决办法：
add "blacklist i2c_piix4" to file
/etc/modprobe.d/modprobe.conf

[source](http://blog.portnumber53.com/2013/08/27/virtualbox-fixing-piix4_smbus-0000-00-07-0-smbus-base-address-uninitialized-upgrade-bios-or-use-force_addr0xaddr/)
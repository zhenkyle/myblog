---
layout: post
title:  "如何用wr703n刷openwrt固件实现迅雷离线下载器"
date:   2013-12-27 11:20:02
categories: openwrt
---

简介
---

最近用wr703n DIY了一个下载器，主要利用家里闲置的带宽来下载高清视频，然后共享给局域网内的其他电脑以及我用Raspberry Pi做的XBMC播放器使用，Raspberry Pi做的XBMC播放器通过HDMI连到电视机上充当高清机顶盒。几天来使用情况令人满意，wr703n完全可以胜任它的工作。在安装的过程中参考了网上很多帖子，这里总结一下。

###拓扑图
电脑/pad/XMBC高清播放器 <---> wr703n下载器（挂500G移动硬盘）<---> 主路由器 <--->  Internet

###功耗
TP-Link WR703N是一款广受欢迎的带USB口的迷你路由器，功耗只有0.5W，一个2.5英寸的500G移动硬盘功耗约2.5W，加在一起3W左右，7X24小时开着下载，差不多半个月才费一度电，很省钱。

###支持迅雷离线
可以支持http/https/ftp/bt等多种协议下载，并且支持下载迅雷离线资源。因为迅雷离线资源实际上是一种需要验证cookie的http资源。

###速度
家里是4M宽带，下载完全可以跑满500K带宽，瓶颈不在wr703n而在资源。迅雷离线的资源单线程下载速度一般在200多K，慢是慢一点，但是比较稳定，可以开2个线程下。

wr703n通过有线连接到我的100M家庭主宽带路由器上，可以稳定的提供读写10MB/s的FTP服务，供其他设备使用，足够Raspberry Pi播放720P视频。

###远程控制
可以通过web界面或者手机客户端远程添加删除任务，很方便，在外面看到好的资源可以随时给家里的下载器添加任务，回到家就下载好了。

###稳定性
已经7X24小时运行了一个多星期，也没见有死机的情况，还比较稳定。

###wr703n的克隆
水星Mercury MW151RM 和 迅捷FAST FW171-3G是wr703n的克隆，也可以刷wr703n的openwrt固件。方法是先刷wr703n的tp-link原厂固件，然后再按以下步骤刷openwrt固件。刷成tp-link原厂固件的方法可以到网上搜索。

安装
---

###原厂固件刷AA版
下载 [这个固件](http://downloads.openwrt.org/attitude_adjustment/12.09/ar71xx/generic/openwrt-ar71xx-generic-tl-wr703n-v1-squashfs-factory.bin) (版本号 12.09， 代号 Attitude Adjustment,  r36088)，将其改名为 openwrt.bin

通过web界面(默认: http://192.168.1.1 / admin /admin)固件上传页上传 openwrt.bin

等待几分钟设备重启，重启后openwrt已经刷好了，需要进行初始设置

更改密码

{% highlight ruby %}
  telnet 192.168.1.1 用户名 root，密码空
  
  passwd 输入两遍新密码
  
  exit 退出
{% endhighlight %} 
 
  这时候telnet服务已经被禁用了，只能用ssh登录
  
接下来设置ip地址

  因为我的主路由器是192.168.1.1，为了避免和主路由器地址冲突，我将wr703n的ip地址设置为:192.168.1.252
  
  ssh root@192.168.1.1

  vi /etc/config/network 编辑成这个样子

{% highlight ruby %}
config interface 'loopback'
        option ifname 'lo'
        option proto 'static'
        option ipaddr '127.0.0.1'
        option netmask '255.0.0.0'

config interface 'lan'
        option ifname 'eth0'
        option type 'bridge'
        option proto 'static'
        option ipaddr '192.168.1.252'
        option netmask '255.255.255.0'
        option gateway '192.168.1.1'
        option dns '192.168.1.1'
{% endhighlight %}


重启后ip地址已变为192.168.1.252

###AA版固件做U盘扩容

wr703n只有4M flash内存，刷完系统就没剩多少了，所以要使用exroot技术做u盘扩容,，这里用的是官方推荐的使用/mnt/sda1 接管/overlay的方法。

移动硬盘分区

  参考了[这篇帖子](http://blog.csdn.net/hexw7/article/details/9864617)，首先在linux下或windows下将一个500G移动硬盘格式化为3个分区。第一个分区512M ext4 用来装软件，第二个分区 128M swap，剩余空间 ext4 用来做下载盘。openwrt读写ext4格式的效率要比ntfs格式高，而且更稳定。

安装usb模块

{% highlight bash %}
opkg update
opkg install kmod-usb-ohci  kmod-usb-uhci
{% endhighlight %}

安装ext4模块

{% highlight bash %}
opkg install  kmod-usb-storage block-mount kmod-fs-ext4
{% endhighlight %}

接下来检查分区

df -h,应能看到有以下两行

{% highlight bash %}
Filesystem                Size      Used Available Use% Mounted on
/dev/sda1               510.0M        0M    510.0M   0% /mnt/sda1
/dev/sda3               464.6G        0G    464.6G   0% /mnt/sda3
{% endhighlight %}

* 拷贝文件

{% highlight bash %}
tar -C /overlay -cvf - . | tar -C /mnt/sda1 -xf -
{% endhighlight %}

* 配置fstab

编辑/etc/config/fstab成这个样子

{% highlight bash %}
config global automount
        option from_fstab 1
        option anon_mount 1

config global autoswap
        option from_fstab 1
        option anon_swap 0

config mount
        option target   /overlay
        option device   /dev/sda1
        option fstype   ext4
        option options  rw,sync
        option enabled  1
        option enabled_fsck 0

config swap
        option device   /dev/sda2
        option enabled  1
{% endhighlight %}

重启

重启后df -h 会变成这个样子

{% highlight bash %}
Filesystem                Size      Used Available Use% Mounted on
rootfs                  510.0M     31.0M    453.4M   6% /
/dev/root                 2.0M      2.0M         0 100% /rom
tmpfs                    14.3M    672.0K     13.6M   5% /tmp
tmpfs                   512.0K         0    512.0K   0% /dev
/dev/sda1               510.0M     31.0M    453.4M   6% /overlay
overlayfs:/overlay      510.0M     31.0M    453.4M   6% /
/dev/sda3               464.6G        0G        0G   0% /mnt/sda3
{% endhighlight %}

看到`/` 文件系统已经变成510M 就成功了。

参考

  [usb.essentials](http://wiki.openwrt.org/doc/howto/usb.essentials) 讲usb驱动
  
  [usb.storage](http://wiki.openwrt.org/doc/howto/usb.storage) 讲usb文件系统选项
  
  [extroot howto](http://wiki.openwrt.org/doc/howto/extroot) 讲openwrt不同版本的命令，需要安装的包，pivot overlay 和pivot root的区别

###安装pure-ftpd充当NAS

{% highlight bash %}
opkg update
opkg install pure-ftpd
{% endhighlight %}

编辑/etc/config/pure-ftpd，将 `option enabled    '0'` 改为 `option enabled   '1'`

{% highlight bash %}
/etc/init.d/pure-ftpd start
/etc/init.d/pure-ftpd enable
{% endhighlight %}


###使用dnspod.sh脚本实现动态域名
家里是ADSL，我使用的是dnspod管理自己的动态域名

到[dnspod.cn API页面](https://support.dnspod.cn/Support/api)下载 [AnripDdns v0.3 by 若海](http://www.anrip.com/post/872),保存为dnspod.sh

使用`scp dnspod.sh root@192.168.1.252/usr/bin/` 上传到wr703n

编辑配置

{% highlight bash %}
# 设置用户参数
arMail="yourname@example.com"
arPass="yourpassword"

# 检查更新域名
arDdnsCheck "youdomain.com" "yoursubdomain"
{% endhighlight %}

安装wget

{% highlight bash %}
opkg update
opkg install wget
{% endhighlight %}

执行`/usr/bin/dnspod.sh`进行测试,成功后维护/etc/crontab/root，让其每10分钟执行一次

{% highlight bash %}
# DNSPod Client
*/10 * * * * /usr/bin/dnspod.sh
{% endhighlight %}

参考

[几个非官方的openwrt软件包](http://linfan.info/blog/2013/10/15/openwrt-packages/)

###安装aria2实现命令行下载
{% highlight bash %}
opkg update
opkg install aria2
{% endhighlight %}


###安装luci-app-aria2实现远程下载

我是到[几个非官方的openwrt软件包](http://linfan.info/blog/2013/10/15/openwrt-packages/)下载的[这个软件包](http://linfan.info/blog/files/luci-app-aria2_1.0_all.ipk)，安装使用下面的命令
{% highlight bash %}
opkg install luci-app-aria2_1.0_all.ipk
{% endhighlight %}

openwrt 12.09 AA版固件缺省是有luci web管理界面的,登录luci后，进入`Services`选项卡-`Aria2 Downloader`，进行配置,需要修改的地方如下：

`Config Directory: /mnt/sda3/download/.aria2`

`Download Directory: /mnt/sda3/download`

`Command-Lines: --file-allocation=none`

最后配置服务为自动启动
{% highlight bash %}
/etc/init.d/aria2 start
/etc/init.d/aria2 enable
{% endhighlight %}

###主路由器设置
在主路由器上

映射6800 端口到 192.168.1.252， 以提供aria2 rpc 访问

映射80 端口到 192.168.1.252，以提供http访问

映射22 端口到 192.168.1.252，以提供ssh访问


使用
---

###从电脑上往下载器添加迅雷离线任务

到chrome 商店安装 迅雷离线助手 插件

配置Aria2 JSON-RPC path为`http://yoursubdomain.yourdomain.com:6800/json/rpc`

在迅雷离线的网页上点`批量导出`-`YAAW`，即可将下载内容添加到家里的下载器

参考[这篇帖子](http://www.right.com.cn/forum/thread-115029-1-1.html)

###从手机上往下载器添加迅雷离线任务

以IOS设备为例，在Appstore中搜索`aria2`，下载`ADM`软件

将HomePage设为迅雷离线下载页面`http://lixian.vip.xunlei.com`

将Host设置为`yoursubdomain.yourdomain.com`
将Port设置为`6800`

同样参考[这篇帖子](http://www.right.com.cn/forum/thread-115029-1-1.html)

###查看下载进度
访问`http://yoursubdomain.yourdomain.com/aria2/`查看下载进度

注意：如果在家里的网络上使用，需要将上面的`yoursubdomain.yourdomain.com`都替换为`192.168.1.252`

常见问题
---

###使用USB2.0 HUB实现稳定的USB连接

wr703n的USB口支持USB1.0有缺陷，会不稳定，在[这篇帖子](https://forum.openwrt.org/viewtopic.php?id=39956)里有详细的讨论。解决的办法简单来说，就是通过一个USB 2.0 HUB来连接USB1.0设备。第206楼，有人给出了r36088直连usb1.0的binary补丁，但我还没试过。这里也有一个[源代码补丁](http://patchwork.openwrt.org/patch/4608/)。

###不要下trunk固件

由于openwrt的开发很快，truck固件很不稳定，经常下载后过几天使用opkg就会报kernel版本不匹配的错，所以最好下稳定版的固件。现时的稳定版是：`12.09， 代号 Attitude Adjustment,  r36088`

###为什么不装smb服务
使用ftp服务实测在有线连接下读写都能达到10MB/s，smb服务可能只有6-7MB/s。smb可能对于wr703n这样轻量级的设备并不适合。

参见[这个帖子](https://forum.openwrt.org/viewtopic.php?id=33387)

###使用其他的ddns服务
如果使用的是花生壳、3322.org等ddns，可以安装`luci-app-ddns`软件包，然后到luci web界面-`Services`-`Dynamic DNS`中进行配置,配置后信息会保存到`/etc/config/ddns`中。

一个花生壳的配置 `/etc/config/ddns`配置文件如下：
{% highlight bash %}
config service 'myddns'
        option force_interval '72'
        option force_unit 'hours'
        option check_interval '10'
        option check_unit 'minutes'
        option retry_interval '60'
        option retry_unit 'seconds'
        option ip_source 'web'
        option enabled '1'
        option interface 'lan'
        option update_url 'http://[USERNAME]:[PASSWORD]@ddns.oray.com/ph/update?hostname=[DOMAIN]&myip=[IP]'
        option domain 'yourdomain.gicp.net'
        option username 'username'
        option password 'password'
        option ip_url 'http://ddns.oray.com/checkip'
{% endhighlight %}


参考[openwrt官方的设置指南](http://wiki.openwrt.org/doc/howto/ddns.client?s[]=dyndns)


###使用yaaw而非webui-airas2
webui-aria2功能更强大，但是配置也更复杂，如果想使用yaaw，可以到[http://binux.github.io/yaaw/](http://binux.github.io/yaaw/)下载ZIP File，解压缩到wr703n 的/www/yaaw 目录中，即可以使用`http://192.168.1.252/yaaw/` 访问

还有一种方式，就是直接使用[yaaw 的 Live Demo](http://binux.github.io/yaaw/demo/)，设置json rpc地址为自己的地址，也可以使用

[webui-aria2的 live demo 地址](http://ziahamza.github.io/webui-aria2/)

###装迅雷固件xware 实现官方远程下载

2013年12月，迅雷发布了其官方远程下载固件 Xware 1.0，下载地址在[这里](http://g.xunlei.com/)。

Xware相当于路由器或NAS上的迅雷客户端程序，可以在很多设备上安装，其`xware_mipseb_32`版可以装在wr703n的openwrt环境中，安装后，可以通过[http://yuancheng.xunlei.com](http://yuancheng.xunlei.com)添加任务，实现远程下载。但是该固件在wr703n上运行很不稳定，存在宕机重启，CPU占用率过高，USB端口丢失等问题，不建议在wr703n上使用。也许在性能更强的openwrt设备上使用更好。

###wiki.openwrt.org打不开
该网站只支持首选语言为英语的浏览器。IE/Firefox/Safari都有解决方法，但是使用Chrome可以直接打开。

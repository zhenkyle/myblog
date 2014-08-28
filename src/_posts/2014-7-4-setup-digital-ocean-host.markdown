---
layout: post
title:  "Setup ditigal Ocean Host"
date:   2014-06-05 12:18:02
categories: 
---

###1、建立主机 ubuntu 14.04 32bit

将ip地址映射到DNS

    adduser demo
    visudo

改

    # User privilege specification
    root  ALL=(ALL:ALL) ALL

为

    # User privilege specification
    demo  ALL=(ALL:ALL) ALL

用demo登录

~~~
sudo cp -r /root/.ssh .
chmod -R demo:demo .ssh
~~~

重新用ssh登录，不用输密码了

###2、安装shadowsockets

使用demo用户，sudo执行

~~~
apt-get install build-essential python-pip python-m2crypto python-dev
pip install shadowsocks
~~~

[https://github.com/clowwindy/shadowsocks](https://github.com/clowwindy/shadowsocks)

~~~
sudo vi /etc/shadowsocks.json

{
    "server":"0.0.0.0",
    "server_port":8830,
    "local_address": "127.0.0.1",
    "local_port":1080,
    "password":"mypassword",
    "timeout":300,
    "method":"aes-256-cfb",
    "fast_open": false,
    "workers": 1
}
~~~

测试  ssserver -c /etc/shadowsocks.json

配置服务器

~~~
apt-get install python-pip python-m2crypto python-gevent supervisor

/etc/supervisor/conf.d/shadowsocks.conf
[program:shadowsocks]
command=ssserver -c /etc/shadowsocks.json
autorestart=true
user=nobody
Add the following line into /etc/default/supervisor
ulimit -n 51200
~~~

Run

~~~
service supervisor start
supervisorctl reload
Control the shadowsocks process:

supervisorctl stop shadowsocks
supervisorctl start shadowsocks
~~~

You can check logs:

~~~
supervisorctl tail -f shadowsocks stderr
~~~

[Reference](https://github.com/clowwindy/shadowsocks/wiki/Configure-Shadowsocks-with-Supervisor)

调优

Edit the limits.conf

~~~
vi /etc/security/limits.conf
Add these two lines
* soft nofile 51200
* hard nofile 51200
~~~

Step 2, Tune the kernel parameters
The priciples of tuning parameters for shadowsocks are
Reuse ports and conections as soon as possible.
Enlarge the queues and buffers as large as possible.
Choose the TCP congestion algorithm for large latency and high throughput.

Here is an example /etc/sysctl.conf of our production servers:

~~~
net.core.wmem_max = 12582912
net.core.rmem_max = 12582912
net.ipv4.tcp_rmem = 10240 87380 12582912
net.ipv4.tcp_wmem = 10240 87380 12582912
net.ipv4.ip_local_port_range = 18000    65535
#net.ipv4.netfilter.ip_conntrack_tcp_timeout_time_wait = 1
net.ipv4.tcp_window_scaling = 1
net.ipv4.tcp_max_syn_backlog = 3240000
#net.core.somaxconn = 3240000
net.ipv4.tcp_max_tw_buckets = 1440000
net.ipv4.tcp_congestion_control = hybla
net.ipv4.tcp_tw_reuse = 1
net.ipv4.tcp_fin_timeout = 15
net.ipv4.tcp_syn_retries = 2
net.ipv4.tcp_synack_retries = 2
net.ipv4.tcp_tw_recycle = 1
~~~

Of course, remember to execute sysctl -p to reload the config at runtime.


安装客户端

[http://shadowsocks.org/en/download/clients.html](http://shadowsocks.org/en/download/clients.html)

###3、安装vncserver

[https://digitalocean.com/community/articles/how-to-setup-vnc-for-ubuntu-12](https://digitalocean.com/community/articles/how-to-setup-vnc-for-ubuntu-12)



###4、安装定制软件

install rvm

[https://www.digitalocean.com/community/articles/how-to-install-ruby-on-rails-on-ubuntu-14-04-using-rvm](https://www.digitalocean.com/community/articles/how-to-install-ruby-on-rails-on-ubuntu-14-04-using-rvm)

使用普通用户

~~~
\curl -sSL https://get.rvm.io | bash -s stable
source /home/zhenke/.rvm/scripts/rvm
rvm install ruby
rvm list
rmv use ruby

sudo apt-get install git

% mkdir my-blog
% cd my-blog
% git init
% rvm --create --ruby-version use 2.0@my-blog
% touch Gemfile

install node.js and npm
sudo apt-get install nodejs
sudo apt-get install npm
sudo ln -s /usr/bin/nodejs /usr/bin/node
[sudo] npm install -g bower grunt-cli

gem update --system
gem update

bundle install
~~~

[https://github.com/joyent/node/wiki/Installing-Node.js-via-package-manager](https://github.com/joyent/node/wiki/Installing-Node.js-via-package-manager)
[http://foundation.zurb.com/docs/sass.html](http://foundation.zurb.com/docs/sass.html)

~~~
% bundle exec jekyll new src
% mv src/_config.yml .

% echo "build" > .gitignore
% echo ".sass-cache" >> .gitignore
~~~

install java 7 oracle

~~~
sudo add-apt-repository ppa:webupd8team/java
sudo apt-get update
sudo apt-get install oracle-java7-installer 
~~~
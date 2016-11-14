---
title: pip的代理设置
date: 2016-11-14 17:35:34
tags:
- Python
---
pip命令只支持http的代理，为pip设置socks代理需要其他的方法。
<! -- more -->
使用proxychains等前置代理工具，安装proxychains
~~~
$ sudo apt-get install proxychains
~~~
配置`/etc/proxychains.conf`,加入
~~~
socks5 127.0.0.1 1080 (你的本地代理地址）
~~~
安装方式
~~~
$ proxychains pip install <package>
~~~

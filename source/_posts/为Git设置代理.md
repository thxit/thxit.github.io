---
title: 为Git设置代理
date: 2016-11-12 22:51:33
tags:
- Git
---
在使用git进行clone或者push操作的时候，由于众所周知的原因，网络速度经常慢到只有几kb/s，所以我们可以通过为Git设置代理的方法加快git的网络速度。
<!-- more -->
## HTTP协议
Git 使用 libcurl 提供 http 支持，所以直接在 git 配置文件中加入
~~~
[http]
proxy = socks5://127.0.0.1:1080
~~~
或者使用git的配置http.config:
~~~
$ git config http.config socks5://127.0.0.1:1080
~~~
也可以使用环境变量
~~~
$ export HTTPS_PROXY=socks5://127.0.0.1:1080
~~~
但是环境变量只能全局使用，虽然也可以根据不同的仓库进行配置，然而需要我们在每次使用的时候加上env
~~~
$ env HTTPS_PROXY=socks5://127.0.0.1:1080 git pull
~~~
## SSH协议
建立`/path/to/soks5proxyssh`文件
shell运行
~~~
#!/bin/sh
ssh -o ProxyCommand="/path/to/socks5proxywrapper %h %p" "$@"
~~~
配置git使用该wrapper
~~~
export GIT_SSH="/path/to/socks5proxyssh“
~~~

修改ssh配置文件.ssh/config
~~~
Host github.com
        ProxyCommand nc -X 5 127.0.0.1:1080 %h %p
~~~
ssh配置参考man ssh_comfig
nc命令参考nc
去掉Host整行的话，SSH代理就成为全局模式。

## GIT 协议
建立 /path/to/socks5proxywrapper 文件，使用 [connect]:(https://bitbucket.org/gotoh/connect) 工具进行代理的转换，各发行版一般打包为 proxy-connect 或者 connect-proxy。
编译并且更改权限
~~~
$ gcc -o connect connect.c
$ sudo chmod +x connect
$ mv connect ~/bin/connect
~~~
shell下运行
~~~
#!/bin/sh
connect -S 127.0.0.1:7070 "$@"
~~~
配置 ~/.gitconfig
~~~
[core]
        gitproxy = /path/to/socks5proxywrapper
~~~
或者直接设置环境变量
~~~
export GIT_PROXY_COMMAND="/path/to/socks5proxywrapper"
~~~

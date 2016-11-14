---
title: Git推送时避免重复输入密码
date: 2016-6-20 11:08:36
tags:
- Git
---
当我们选用https的方式连接git服务器时，每推送一次就要输入一次密码，非常的麻烦。
<!-- more -->
如果不想用ssh连接，我们可以输入以下的命令避免重复输入密码：
~~~
git config --global user.name "your name "
git config --global user.email youremail@exampal.com
git config --global credential.helper store
~~~
该命令存储密码缓存，执行后还需要再输一次。

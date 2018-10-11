---
layout: single
title: 为什么我的账号是 root
date: 2016-01-26 14:15:26 +0800
tags: system
categories: devops
---

## 我只是要sudo

客户的一台服务器, 需要我给配置一下环境, 给我创建了一个用户, 当我使用这个用户登陆系统后发现, 当前用户没有`sudo`权限, 好吧, 我只能要求客户先把我这个用户的加入到`sudo`用户组, 或者使用`visudo`在`/etc/sudoers`替我维护一下当前用户, 等到对方告诉我一切搞定, 我尝试登陆后, 我整个人惊呆了.

<!--more-->

我看到的是`#`, `whoami`的结果是`root`, 好吧, 我明明是用自己的用户登陆的.

## why

我就是一个连5手SA水平都够不上的开发, 没见过这么销魂的事情, 本着好奇的本性, 网上各种查, 但是貌似没找到类似的情况, 但是我不死心, 想起了`/etc/passwd`文件中有相应的信息, 我就随手`cat`了下, 没想到, 我觉得我找到了为什么我登陆后是`root`了.

我的用户的用户组和用户ID都变成了0.

反正我都`root`了, 我也无所谓了, 顺便看看root用户的操作, 果真印证了我的猜测, root的`.bash_history`清楚的记录了客户是直接修改了`/etc/passwd`这个文件, 来达到为我用户提权.

## true or fasle

我不清楚这样好不好, 不过, 我认为我自己不会这么做的, 其实只要简单的为我当前用户加入`sudo group`这些问题都能解决了, 毕竟, 在命令前敲入`sudo`感觉很好.

## 参考
[How To Use passwd and adduser to Manage Passwords on a Linux VPS](https://www.digitalocean.com/community/tutorials/how-to-use-passwd-and-adduser-to-manage-passwords-on-a-linux-vps)
[Understanding /etc/passwd File Format
](http://www.cyberciti.biz/faq/understanding-etcpasswd-file-format/)
[How can I add a new user as sudoer using the command line?](http://askubuntu.com/questions/7477/how-can-i-add-a-new-user-as-sudoer-using-the-command-line)
[How To Edit the Sudoers File on Ubuntu and CentOS](https://www.digitalocean.com/community/tutorials/how-to-edit-the-sudoers-file-on-ubuntu-and-centos)

### PS
[digital ocean](https://www.digitalocean.com/community/tutorials)上面的文章大多比较简单直接, 个人挺喜欢, 很入门.

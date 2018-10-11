---
layout: single
title: 升级 gitlab 异常的处理
date: 2016-02-23 22:26:30 +0800
tags: system
categories: tools
---

## update

开始小规模尝试使用gitlab, 还没玩多少时间, gitlab 8.5 发布了, 看了下发布说明, 大幅提升了系统性能, 就为这个必须赶快尝试下.

gitlab的apt源在墙内用不了, 翻墙下了deb包, 上传到服务器上, `dpkg -i`加个包名轻松安装, 使用体验好的没朋友了...

<!--more-->

悲催的是报错了:
```
PG::ConnectionBad (could not connect to server: No such file or directory
    Is the server running locally and accepting
    connections on Unix domain socket "/var/opt/gitlab/postgresql/.s.PGSQL.5432"?
```
`gitlab-ctl reconfigure`也尝试了, 没能修复问题.

## 解决

google了一下, 找到了gitlab上的一个issue#2005;

参考了下@David-Turbert的方法

> when I migrate from 7.11.4 to 7.14.1 I haved some errors. maybe I can help someone with my "magic" commands:
>
```
gitlab-ctl stop
initctl stop gitlab-runsvdir
```
> here I check if one process was not stoppped with
>
```
ps aux | grep postgresql
```
> and I kill the processes
>
```
kill -9 [procID]
```
> when everything it is correct I restart Gitlab
>
```
initctl start gitlab-runsvdir
gitlab-ctl reconfigure
gitlab-ctl restart
```
> On restart if one process can't restart correctly I restart this procedure
When everything it is ok I reinstall gitlab

我按照这个方法, 确认了postgresql停机, 重新执行了`gitlab-ctl reconfigure`, 之后顺利的安装上了最新版的gitlab, 体验了一下, 服务器配置比较低, 感觉还是流畅了不少.

## 参考
1. [issue#2005 PG::ConnectionBad after fresh install](https://gitlab.com/gitlab-org/gitlab-ce/issues/2005)

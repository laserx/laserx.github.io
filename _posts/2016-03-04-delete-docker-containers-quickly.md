---
layout: single
title: 快速删除多个 docker 容器的方法
date: 2016-03-04 13:39:13 +0800
tags:
  - docker
  - linux
categories: devops
---

学习docker的过程中, 随着每一步的操作, 产生了大量停止工作的容器, 想快速的删除多个容器, 看了下`docker rm --help`, 加上stackoverflow上看到了别人的问答, 总结了2种批量删除容器的方法.

<!--more-->

1. `docker rm $(docker ps -qa)`
2. `docker ps -qa | xargs -n 1 docker rm`

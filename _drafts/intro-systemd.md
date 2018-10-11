---
layout: single
title: 使用 systemd 管理服务
tags:
  - linux
  - systemd
categories: devops
---

## 什么是 systemd

**systemd** 是一套系统 init 软件, 目的是替代 **system V, upstat** 等系统初始化工具.

{% asset_img 1.png systemd %}

<!--more-->

systemd 的出现一直伴随着争议, 大多数声音指明 systemd 设计过度复杂, 没有遵循 Unix 哲学等.

不过现在主流的 linux 发行版基本都使用 systemd 作为其 init 系统, 所以抛开这些声音, 我们不得不掌握和使用 systemd 来使我们的工作更轻松高效.

## unit & target

当我们完成了服务的开发, 将其部署至服务后, 我们需要服务开始运行, 并且在程序崩溃时自动重启服务.

面对这样的场景, 我们可以使用 `supervisor` 等进程管理工具来达到这样的效果. 但是, 为什么不使用 systemd 来替代额外的软件, 同时, 体验 systemd 带来的更多优势呢.

## systemctl


## journalctl

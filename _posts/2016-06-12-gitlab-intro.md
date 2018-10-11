---
layout: single
title: gitlab 入门
date: 2016-06-12 17:12:48 +0800
tags: git
categories: devops
classes: wide
---

## gitlab 初识

![gitlab](/assets/2016-06-12-gitlab-intro/1.png)

`gitlab`作为私有仓库工具, 在部署和更新上都十分方便, 使用`gitlab ce omnibus`可以做到一键部署, 整个过程轻松简单.

<!--more-->

但因为其包托管在`AWS`上, 国内下载困难, 可以使用本地下载后上传至服务器, 手动包安装的方法, 同时, [清华大学TUNA](https://mirror.tuna.tsinghua.edu.cn/)维护着国内的镜像, 让安装的过程更加轻松.

我们在部署的时候将其部署至`aliyun`, 同时分配了域, 开启了ssl, 整体的部署难度和`phabricator`相比轻松了不少.

`gitlab`对硬件要求较高, 在1G内存的机器上(`phabricator`在1G的内存的机器上跑的飞起来), 运行困难, 建议最少2G内存.

`gitlab`给人的第一印象是和`github`很像, 可以`fork`, 简单的issue功能, 项目自带wiki和snippet, 新版本的`gitlab`还添加了docker container registry这个比较独特的功能.

## 我们的工作流

`gitlab`给我们带来最大的最大的收获是, 可以使用MR进行开发了.

之前使用`phabricator`进行开发因为没有使用`arc`工具, 开发基本是按照特性分支开发, 但是代码审查使用的后置审查, 对提交代码的把控较为松懈, 使用`gitlab`之后, 这一现象缓解了不少.

我们在系统中, 创建2个分支, 一个是`master`, 一个是`release`. 这两个分支是受保护分支, 一般的开发人员不可以直接向这两个远程分支进行任何提交, 根据项目的情况, 设定一个2-3周的里程碑(基本上是3周), 在里程碑中根据产品提出的需求, 拆分成大量的issues, 再将issues指派给空闲的开发人员, 通过`gitlab`issue的功能, 直接创建相应的分支.

开发人员领取到issue后, 检出特性分支, 进行开发, 开发完成后自测, 提交MR, 由产品经理检查其功能是否达标, 技术负责人检视代码是否规范, 如果一切正常, 合并代码至`master`, 不符合要求的, 拒绝MR请求, 通知开发者继续修复问题, 待修复完成后, 从新开启MR.

待代码合并至`master`分支后, 测试服自动拉取最新的代码, 供业务人员测试, 当功能完成度达到上线的要求时, 将由技术负责人从`master`提交MR至`release`, `release`分支只提供正式服务器使用.

现在主要的项目是基于`laravel`进行开发的, 集成测试一直是项目成败的关键. 在配置gitlab-ci的时候, `composer`指令超时严重, 至今, 集成测试依然是我们的痛点.

## hits

1. issues 快速创建分支

    在创建issues的时候, 在其title后添加英文, 可以使你更快捷更优雅的创建分支
    ![没有英文title](/assets/2016-06-12-gitlab-intro/2.png)

    ![有英文title](/assets/2016-06-12-gitlab-intro/3.png)

1. 特性过于庞大, 一个issue开发起来较为漫长时

    在开发特性时, 可能会因为预估不足,导致特性分支开发周期超出预期(大于2天, 我认为, 每一个特性的开发时间尽可能控制在一天完成, 如果过大可以拆解成多个issues), 尽可能让开发人员完成一部分可用的功能, 提交MR, 验证无误后, 合并特性分支并删除, 再重新从`master`中创建该issue的特性分支.

1. 合并冲突解决

    我们之前产生遇到了特性过于庞大, 开发持续的时间过长, 提交MR后产生了冲突, 手工解决冲突后, 合并至`master`后, 发生了代码退化. 后期发现, 如果开发的时间过长, 这个时候最好时从`master`分支`merge`至特性分支, 在特性分支中解决冲突, 验证无误后, 再从特性分支MR至`master`.

## 参考

1. [gitlab](https://about.gitlab.com/)
1. [gitlab-flow](https://about.gitlab.com/2014/09/29/gitlab-flow/)
1. [gitlab CI](http://docs.gitlab.com/ce/ci/)

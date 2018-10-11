---
layout: single
title: git 笔记
date: 2015-08-23 10:28:01 +0800
tags: git
categories: tools
---

## 初识 git
在前一家公司, 我的`git`启蒙是一位前端大神, 当时的公司完全没有代码版本控制(这是真的...), 我当时觉得好吧, 没有就自己小心点吧, 每次动手干活前, 都是小心翼翼的备份, 搞的本地文件夹一堆一堆的`*_bak`, 这看起来蠢爆了, 有天我看到前端大神在命令行里敲着些什么, 我随口一问, "这什么鬼", 结果我就认识了`git`...

<!--more-->

`git`给我的感觉是清爽, 尤其是本地开发的时候, 也许我并不需要中心仓库, 这简直太好了, 完美解决了我的难题.

作为分布式代码仓库的代表, `git`并不难以上手, 尤其是当你不需要与他人合作的时候, 仅仅是本地仓库的功能, 就足以让你事半功倍.

## 推动 git
在team中, 我们一直使用的是`SVN`, 尤其是`windows`平台的小伙伴们, [TortoiseSVN](http://tortoisesvn.net/)简直就是神器, 使用便捷, 上手快, 新来的小伙伴人手一套.

推动team切换至`git`我尝试过2次, 第一次, 我试着把`git`的用法给所有开发灌输了一遍, 但是, 当时所有人的反应是, 这玩意操作够复杂的啊. 原来`update & commit`就完的事, 变的复杂好多. 当时的我对`git`也是一知半解, 很可惜, 失败了.

第二次推动`git`切换的契机是, 我尝试把`trello`切换至`phabricator`, 原因是`trello`太灵活了,大家完全不认为自己必须要在`trello`上做什么.

当`phabricator`部署完成了之后, 我想, 是时候切换到`git`了.

## 我的工作流
我的工作流类似`gitflow`, 远程仓库有`master`和`develop`, 要求所有的开发人员的代码只可以推送至`develop`分支, 当然, 当他们试图推送至`master`分支时, `phabricator`会阻止他们.

在本地开发过程中, 我会从`develop`分支上, `git checkout -b WIP`这么一个分支作为的的工作分支, 我认为, 在一个时间段中, 我只会专注于一个开发任务, 所以, 我的特性分支只有一个, 就是`WIP(work in processing)`.

当我将开发任务完成, 我会在`WIP`分支上`git rebase -i develop`, 之后`git checkout develop && git merge WIP`. 这个的最大问题是没有充分利用特性分支进行开发.

但是, 因为我的开发时间并不充裕, 我只需要在`phabricator`确认我现在要做的是哪一个任务, 开始专注于一个任务就够了.

当需要发布的时候, 当我从`develop`上`git checkout -b release`之后, `git rebase -i master && git checkout master && git merge release`. 我觉得这样就足够了.

## 注意
`git`并不简单, 任何一个新的技术出现在你的面前, 真正困难的并不是技术本身, 而是你自己.

当你没有做好准备, 贸然的推动新技术在team中使用的时候, 你的小伙伴们会比你更迷茫.

做好准备, 先说服你自己, 是时候这么干了, 我想, 你离成功不太远了.

## 资源
[Get Git Right by atlassian](https://www.atlassian.com/git/)

[Git教程 by 廖雪峰](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)

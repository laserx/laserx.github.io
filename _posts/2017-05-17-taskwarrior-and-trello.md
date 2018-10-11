---
layout: single
title: 使用 taskwarrior 管理我的日常
date: 2017-05-17 22:45:01 +0800
tags:
  - tools
categories: others
---


## 选择一个适合自己的任务管理工具

工作这么些年, 各种任务管理工具玩了个七七八八, 从最开始的 wonderlist, worktile, 到后来的 trello, phabricator, gitlab 的看板等, 基本没有一个用的很长久. 除去之前工作上使用 gitlab 的 issue board, 和 trello 这2个确实用的时间比较长之外, 别的任务管理工具可以说是, 看上一个, 用用, 过不了几天, 想起来了, 删除账号.

<!--more-->

之前使用 phabricator 的时候, 使用 `arc` 命令行工具的时候发现了 taskwarrior, 当时随便用了下, 感觉和 arc 的功能类似就没再使用, 直到最近, 实在是没什么可以用的工具了, 就又把 taskwarrior 捡了起来.

taskwarrior 是一款命令行工具, 不包含图形界面, 在使用上, 更符合我的工作习惯, 因为 terminal 是我一天使用次数最多的工具之一, 但是, taskwarrior 不具备跨平台的能力, 这使得其在实际使用上略显不足, 不过, 还好有额外的解决方法.

单纯命令行使用起来的感觉已经可以消灭很多任务管理工具了.
```
task add 代码发布准备
```
这样简单的一条指令就创建了一个新的任务, 在我看来, 这样的使用体验, 对比任意一个图形工具都是具有绝对的优势的.

## 基本用法

taskwarrior 使用起来十分简单, 一个简单的 `task add something` 就可以创建一个任务.

使用 `task add something project:someProject.subProject` 就可以创建一个属于 `someProject.subProject` 项目的任务(主项目与子项目使用`.`分割).

额外的属性还有 `priority:H` 等于指定任务的优先级为高, `due:eod` 指定截止时间为今天止.

当添加完任务后可以查看你的任务列表了, 使用`task list` 将输出所有的状态为 `pending` 的任务, 而使用 `task` 或者 `task next` 指令则会按照任务的优先级, 截止时间, 依赖任务等计算出的分值进行排序.

![task next](/assets/2017-05-17-taskwarrior-and-trello/1.png)

`task projects` 可以列印出所有的项目, 便于查看当前执行中的所有项目.

![task projects](/assets/2017-05-17-taskwarrior-and-trello/2.png)

`task edit taskId` 可以使用编辑工具直接修改任务的属性.

taskwarrior 还提供了很多的指令, 例如 `tag`, `calendar`, `burndown` 等等.

![task burndown.daily](/assets/2017-05-17-taskwarrior-and-trello/3.png)

![task calendar](/assets/2017-05-17-taskwarrior-and-trello/4.png)

更多的使用指南和使用参考可以阅读官方的文档, 其中, 强大的 `filter` 让你检索任务更加便捷, 而 `depends` 可以使你更好的组织你的任务.


## 跨平台

taskwarrior 没有可以使用的移动端应用, 所以, 曲线救国, 可以使用 [inthe.am](https://inthe.am/) 提供的工具使其跟 [trello](https://trello.com/) 同步, 这样, 我们就可以使用 trello 将不同平台的 taskwarrior 数据进行同步.

需要注意的是, [inthe.am](https://inthe.am/) 登录使用 **google** 授权登录, 请自备梯子...

你只需要在登录后, 按照 [Configuration & Settings](https://inthe.am/configure) 中的提示, 配置你的 taskwarrior, 集成 trello, 根据个人需求, 可以开启 iCal 等设置.

inthe.am 提供了一些预设 `UDAs`, 可以根据个人的需求, 将其复制到 `~/.taskrc` 中去.

在此之后, 通过 `task sync` 指令同步任务列表, 既可以达到跨平台同步的功能了.

## timewarrior

完成以上的步骤之后, 可以说 taskwarrior 已经可以满足我的需求了. 但是开发者还提供了一个额外的工具, 这就是 timewarrior, timewarrior 的功能主要是进行任务执行时间的统计, 如果你是一个非常有时间观念, 同时, 对每个任务所消耗的时间非常在意的人, 可以尝试下将 timewarrior 和 taskwarrior 集成在一起.

根据官方文档, 使用 [on-modify hook script](https://taskwarrior.org/docs/timewarrior/taskwarrior.html) 可以非常方便的将 taskwarrior 和 timewarrior 集成在一起.

我使用的是 `brew` 安装的 timewarrior, 所以我们需要到
```
/usr/local/Cellar/timewarrior/1.0.0/share/doc/timew/ext
```
这个路径下, 查找对应的脚步, 之后按照官方文档的方法, 配置 hook 即可.

之后, 你只需要 `task start taskId`, 这时, timewarrior 就会自动触发, 开始计时.

## 总结
taskwarrior 是我使用过最适合开发者使用的任务管理工具, 在命令行中轻松整理自己的任务, 安排自己每天的工作.

同时, 因为可以和 trello 同步, 使得我们可以在没有电脑的时候, 通过手机的 trello 客户端添加任务, 回到电脑前, 只需要一个简单的 `task sync` 指令, 就可以将 trello 中的任务同步到本地, 使工作更具有连贯性.

配合 timewarrior, 可以追踪每个任务的时间开销等细节, 连 `timelogger` 这类的工具都可以免了.

inthe.am 的存在使得 taskwarrior 使用个便捷, 同时, 还有额外的集成可以让我们挖掘, 为 taskwarrior 的使用更加亮点.

不过要说的是, trello 集成功能只能满足基本的使用需求, 不适合多人共享看板的集成. 使得 taskwarrior 和 trello 更适合个人使用, 而不是协同办公. 但是, 瑕不掩瑜, taskwarrior 给我的工作和生活带来了很大的帮助.

希望看过这篇文章的你, 也能体验一下 taskwarrior, 尝试一下不一样的任务管理方法.

## ref
1. [taskwarrior official website](https://taskwarrior.org/)
1. [timewarrior official docs](https://taskwarrior.org/docs/timewarrior/index.html)
1. [taskwarrior Q & A](https://answers.tasktools.org/)
1. [inthe.am](https://inthe.am)
1. [trello](https://trello.com)
1. [Getting Things Done with Task Warrior](http://chariotsolutions.com/blog/post/getting-things-done-with-task-warrior/)

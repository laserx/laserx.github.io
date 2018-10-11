---
layout: single
title: 再见 phabricator, 拥抱 gitlab
date: 2016-04-15 10:15:54 +0800
tags: tools
categories: devops
---

## 原因

团队使用`phabricator`大约有小半年的时间, 从使用的情况来看, 效果并不好.

首先, `phabricator`功能强大, 尤其是它的项目管理功能, 非常灵活, 功能丰富. 但是, 我们的团队规模不大, 但项目并行严重, 所以人力上并不充裕, 很多时候, 项目经理担当着开发的角色, 所以, 项目经理对`phabricator`的看法是, 用起来比较复杂.

<!--more-->

其次, `phabricator`的代码审查功能没能推动, 团队大约10人的情况下, 同时开2-3的项目, 每个项目上堆积的任务很多, 代码审查前置堵塞提交, 后置审查对我们来说效果并不理想, 这个问题不是`phabricator`的问题, 是团队管理的问题, 没能切实的做好代码审查的工作.

再次, `phabricator`的持续集成功能不是很健壮, `Harbormaster`使用较为复杂, 使用`jenkins`对我来说又有点太过繁琐了, 所以持续集成这部分完全没有实现.

最后, `phabricator`在我们的团队中只是充当了代码仓库, 维基和简单项目管理的工具.

## 改变

因为`phabricator`的推动是我主导的, 之前代码是托管在`svn`上的, 为了使用`git`, 参考了`phabricator`和`gitlab`, 但是, 当我看到[wikimedia phabricator](https://phabricator.wikimedia.org/)之后, 我觉得`phabricator`更适合我们.

然而, 我的决定是错误的, `phabricator`使用灵活带来的就是更高的管理能力, 或者使用者的自治. 然而以上两点我们都不具备.

现在, 我尝试切换至`gitlab`, 理由很简单, `gitlab`更专注于代码本身, 简单的`milestones`和`issues`足以支撑小型团队的使用, 而且复杂度更低.

## 优势

### 专注于代码

`gitlab`的功能专注于coding, `milestones`和`issues`功能对比`phabricator`的`projects`和`maniphest`功能略显薄弱, 但对小型团队, 已经够用.

更多的惊喜来自于其`gitlab-ci-multi-runner`, 使用起来类似`Travis CI`, 配置简单, 使用起来非常顺手.

### gitlab-flow
对比`phabriactor`, 实现`git`最佳实践的方式是使用`arc`工具, 而对应`gitlab`, 它提供了围绕持续集成模式的`gitlab-flow`.

实际上, `arc`非常强大, 在命令行中可以轻松执行大量的操作, 但是, 在`windows`上体验不佳(尤其是显示中文), `mac`一点问题都没有. 鉴于开发组中, 大量的小伙伴喜欢使用`windows`(别问我为什么, 我也不知道), 所以`arc`的一直就没推动. 没有了`arc`的`phabricator`可玩性少好多...

`gitlab-flow`围绕着`CI`的这种实践给我的感觉就是更加易用, 特性分支开发完成, 提交`MR`(就是`github`中的`PR`), 在确认合并前, 可以代码审查, 以及检视持续集成是否成功等等, 感觉整个流程很合理.

### 更多的第三方集成

`gitlab`的第三方集成更加丰富一些, 例如`sentry`和`mattermost`已经成功的集成.

### 更好的用户体验

`gitlab`的界面更具有亲和力和现代感, 如果和`github`比较的话, 其实`gitlab`也不差.

## 参考

1. [gitlab](https://about.gitlab.com/)
1. [gitlab-flow](https://about.gitlab.com/2014/09/29/gitlab-flow/)
1. [gitlab CI](http://docs.gitlab.com/ce/ci/)
1. [phabricator](https://phabricator.org)

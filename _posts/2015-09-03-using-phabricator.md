---
layout: single
title: 开始使用 phabricator
date: 2015-09-03 21:43:19 +0800
tags: phabricator
categories: tools
---

## 选择
这个问题困扰了我很久, 纠结于`phabricator`和`gitlab`这两者, 前者使用`php`开发, 后者使用`ruby`开发.

`gitlab`的优势是有`pr`有`ci`, 用起来和`github`基本一样, `phabricator`的优势是强大的任务管理和bug追踪管理, 附属工具非常多, 而且都很出色.

<!--more-->

当然, 我主要的开发语言使用的是`php`, 但这不是我选择`phabricator`的主要原因. 主要是强大的任务管理工具, 让我可以把任务管理从`trello`上迁移过来.

## 实践
部署`phabricator`的过程还算顺利, 文档做的非常好, 90%的问题都可以在官方文档中找到.

整体的配置主要分为`web`, `git`, `email`, `file`,  `notifications`这几个部分, 比较好配置, 只是`git`配起来步骤有点多而已.

## 困难
创建项目, 创建任务, 创建代码仓库, 每一步都是那么的顺利. 用起来非常顺手, 这是我的第一个感觉.

不过我还是低估了`phabricator`的复杂度, `Herald`, `Policis`都挺复杂的, `Policis`随处可见, 配着配着就蒙了, 不留神我的小伙伴就看不到代码仓库了.

`Herald`一开始以为就是为了跑通代码审计流程而设计的, 后来发现不是那么回事, 功能强大的很.

当然我觉得比较坑的还是`Audit`和`Differential`功能, 其实这部分也是`phabricator`最强大的地方. 顾名思义, 这部分主要是为了实现代码审查而设计的, 但是强大的`phabricator`提供了两套审查方案.

  * `Differential`代表了`pre-push`我称之为代码审查, 需搭配`Arcanist`

  * `Audit`代表了`post-push`我称之为代码审计, 需搭配`Herald`

这部分绝对是让人最纠结的部分, 我最开始尝试了代码审查, 但是`Arcanist`在`Windows`并不是非常好用, 而且, 类似`pr`的机制需要项目负责人专注于处理代码的提交请求.

而`Audit`轻量很多, 首先, 不需要`Arcanist`. 只需要使用一般的`git`流程就可以了, 当代码`push`的时候, 系统会自动触发`Audit`流程.

## 期望
推动我将`phabricator`以及`git`在team中实践的动力实际上来源于对`TDD & BDD`以及`CI`的向往.

当然, `phabricator`的迭代速度远远超过我的预期, 每周 [changelog](https://secure.phabricator.com/w/changelog/)让我感到自己简直low爆了.

## 结论
我不是一个务实的开发者, 更不是一个理想的技术负责人, 我知道交付逾期的问题出在哪里, 但是我无法控制, 我不希望失控蔓延, 所以我需要`phabricator`.

## 资源
[Phabricator User Documentation](https://secure.phabricator.com/book/phabricator/)

[wikimedia](https://phabricator.wikimedia.org/) (最佳实践素材)

[Khan Academy Development Using phabricator](https://sites.google.com/a/khanacademy.org/forge/for-developers/code-review-policy/using-phabricator)

[Evan Priestley@Quora](http://www.quora.com/Evan-Priestley)

另:

Evan Priestley `phabricator`的负责人, 真心是个大神.
[Is it true that Facebook has no testers?](http://www.quora.com/Is-it-true-that-Facebook-has-no-testers)中的回答
> Ex-Facebook employees have some privileged channels they can use to report issues; I personally report around 13,000 bugs per month

给跪了

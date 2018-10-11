---
layout: single
title: 配置mac指南
date: 2016-03-07 22:43:03 +0800
tags:
  - osx
  - mac
categories: others
---

## 指南

办公室的小伙伴开始尝试使用 mac 了, 做为使用了一段时间的老玩家在此为小伙伴们准备一份起飞指南, 祝愿小伙伴们更加喜爱 coding.

<!--more-->

## 起飞

首先, mac 能使 coder 更加专注于搬砖这件事, 最起码, mac 上没有这个那个的新闻弹窗, 又因为是 unix 家族的, 各种工具也是开心的不要不要的, 所以, 对于搬砖这件事, 还是需要趁手的工具.

当开始配置 mac 的时候, 我们首先需要的是`command line tools`, 只需要在 terminal 中输入`xcode-select --install`, 轻轻回车, 喝杯茶.

当工具安装完毕, 接下来, 开始我们的配置工作.

## zsh
osx 中已经自带了 `zsh`, 所以也就不用再单独安装了, 不过为了玩 `zsh` 玩的更爽, 安装 [oh-my-zsh](https://github.com/robbyrussell/oh-my-zsh) 是必须的, 安装完成之后, 可以开心的配置属于自己的 `zsh` 了, 在`~/.zshrc` 中可以配置主题, 插件等, 插件强烈推荐开启插件 `z`, 它可以让你在目录中跳转飞起来.

## brew
`brew` 相当于 linux 中的 `apt`, 说白了就是软件包管理, `brew` 的存在可以让你轻松的安装各种命令行工具, 在 [brew](https://github.com/Homebrew/homebrew) 的仓库中, 可以查看相关的安装指南.

使用起来非常方便, 刚安完 `brew doctor` 一下,没事 `brew update && brew upgrade` 更新下最新的软件包, 有需求了 `brew tap homebrew/completions` 这样那样的仓库, 安完软件 `brew cleanup` 删除下残余的安装源文件等等.

## atom
`atom` 是我现在使用的主要 editor, 和 `sublime` 相比, 其实没什么优势, 只是 `sublime` 老不更新, `atom` 开源免费, 现在插件也很多, 写写 `markdown` 和 短小的程序代码, 完全没有问题.

安装插件也很方便, 顺便附上自己的插件
```
├── AtomicChar@0.3.11
├── activate-power-mode@0.4.1
├── atom-beautify@0.28.26
├── atom-material-syntax@0.4.3
├── atom-material-ui@1.2.0
├── atom-pair@2.0.10
├── auto-detect-indentation@0.5.0
├── autoclose-html@0.23.0
├── color-picker@2.1.1
├── emmet@2.4.1
├── ex-mode@0.8.0
├── file-icons@1.6.17
├── flex-tool-bar@0.7.3
├── git-control@0.4.0
├── git-log@0.4.1
├── git-plus@5.13.0
├── highlight-selected@0.11.2
├── imdone-atom@1.3.27
├── language-blade@0.20.0
├── language-elixir@0.12.2
├── linter@1.11.3
├── markdown-writer@2.3.1
├── merge-conflicts@1.3.7
├── minimap@4.19.0
├── minimap-bookmarks@0.2.0
├── minimap-cursorline@0.1.0
├── minimap-git-diff@4.1.8
├── minimap-highlight-selected@4.3.1
├── minimap-selection@4.3.1
├── open-recent@5.0.0
├── pigments@0.24.2
├── todo-show@1.3.0
├── tool-bar@0.2.1
├── tool-bar-main@0.0.9
├── vim-mode@0.64.0
└── wakatime@5.0.8
```

## IDEs
这部分其实最复杂了...我觉得还是单开一个日志为妙, `phpstorm`, `webstorm`, `datagrip` 总之 [jetbrains](https://www.jetbrains.com/) 他家的就对了.

## docker
其实一直在使用 `vagrant` 作为开发的主要运行时工具, 不过 `virtualbox` 性能真心给跪了, 用 `vmware fusion` 插件还是收费的, 准备尝试尝试现在最火的 `docker`, 不过本人比较笨拙, 还没玩通...

但是最起码, 工具可以准备好, 最开始使用的是`dockertoolbox` 后来我给卸了, 更新麻烦, 果断使用 `brew` 安装了.

只需要
```
brew install docker docker-compose docker-machine docker-swarm
```
`docker` 大礼包带回家.

## terminal
这个看个人的, 我喜欢 `iterm2`, 比较有特色的功能有, 粘贴历史 `cmd + shift + h`, 立即回放 `cmd + opt + b`, 窗口堆砌 `cmd + opt + e` 等等.

## tools
工具的话简直太多了, `git-extras`, `macvim`, `tree`, `alfred`, `appcleaner`, `xtrafinder`等等都是很好用的工具.

## end
其实, 在不断的使用过程中, 可以发现自己越来越喜欢coding, 快乐的根源来自摸索的过程, 配置属于自己的开发环境才是最重要的, 以上的只是我个人的一点点使用上的心得.

其实有好多工具, 都不知道怎么写, 先这样吧...

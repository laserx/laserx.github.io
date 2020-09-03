---
layout: single
title: ansible playbook 浅尝
date: 2017-01-29 09:52:02 +0800
tags:
  - ansible
  - cm
categories: devops
---

## intro

### 什么是 ansible
`ansible` 是一套完整的系统配置管理工具, 丰富的功能组件, 使运维或者开发人员非常方便的管理线上环境.

<!--more-->

### ansible 能干什么
在我的工作中, `ansible` 对我比较重要, 因为需要维护40-70台(好吧, 最多的时候240台)弹性服务器, 需要批量执行代码更新, 清除数据文件, 查看日志文件等工作, 而使用 `ansible` 可以让工作更轻松.

### 为什么选择 ansible
其实关注 `ansible` 很长时间了, 一直没有什么机会使用, 因为之前需要维护的也就那么一两台服务器, 完全没有使用 `CM` 必要, 但是, 当我发现每日服务器的管理工作需要占用我20分钟以上的时候, 我觉得是使用管理工具的时候了.

其实可以选择的工具非常多, 老牌的 `puppet`, `chef`. 同期的 `saltstack` 等等. 最后选择 `ansible` 的原因是不需要安装 `agent`, 通过 `ssh` 管理更方便一些.

当然, 星多.

## 入门

### 文档

官方文档很翔实, 但是碎片化, 我不喜欢, 看书的话我觉得没太大的必要, 毕竟我就简单用用, 不干什么惊天地泣鬼神的事, 所以我觉得看看别人的代码效果更好一些.

### 最佳实践

`ansible` 的 `playbook` 作为最主要的工具, 如何组织是一个不小的挑战, 不过官方的最佳实践说明我觉得很够用了, 参考下github上别的大牛写的 `playbook` 也是很好的.

```text
.
├── anonymous.yml
├── ansible.cfg
├── authorized
│   ├── id_rsa
│   └── id_rsa.pub
├── authorized.yml
├── deploys.yml
├── hosts
├── maintain.yml
├── proxy.yml
├── readerImage.yml
├── readme.md
└── roles
    ├── anonymize-proxy
    │   ├── configures
    │   │   ├── passwords
    │   │   └── squid.conf
    │   ├── handlers
    │   │   └── main.yml
    │   └── tasks
    │       └── main.yml
    ├── authorized
    │   ├── handlers
    │   │   └── main.yml
    │   ├── tasks
    │   │   └── main.yml
    │   └── templates
    │       └── sshd_config.j2
    ├── code
    │   ├── keys
    │   │   ├── id_rsa
    │   │   └── id_rsa.pub
    │   └── tasks
    │       └── main.yml
    ├── cron
    │   └── tasks
    │       └── main.yml
    ├── maintain
    │   └── tasks
    │       └── main.yml
    ├── post-image
    │   └── tasks
    │       └── main.yml
    ├── proxy
    │   ├── configures
    │   │   └── nginx.conf
    │   ├── handlers
    │   │   └── main.yml
    │   └── tasks
    │       └── main.yml
    ├── runtime
    │   └── tasks
    │       └── main.yml
    └── services
        ├── configures
        │   ├── service1.service
        │   ├── service2.service
        │   ├── service3.service
        │   └── service4.service
        └── tasks
            └── main.yml
```

这是是我整个 `ansible-playbook` 的目录结构, 大体分成了9个`role`, 6个 `playbook`.

在一个 `playbook` 中可以调用多个 `role`, 这样就组成了自动化配置的基础, 每个 `role` 只负责干好自己所定义的任务, 而 `playbook` 负责组合所需的模块.

### inventory

`inventory` 的作用就是定义你需要维护的服务器, 格式也非常简单, 如下:

```
[readers]

[builds]

[proxies]
proxybase ansible_host=192.168.0.4

[anonymous]
anonymbase ansible_host=192.168.0.5

[removes]


[maintains]

service01 ansible_host=192.168.0.1
service02 ansible_host=192.168.0.2
service03 ansible_host=192.168.0.3
...
```

其中, `[]` 定义分组, 你可以根据自己的实际需要, 将服务器分成不同的组, 再在 `playbook` 中调用不同的分组, 同时, 在使用命令行查询所有服务器的日志的时候, 也会更加便捷.

`service01` 相当于给服务器起的别名, 可以使管理更加便捷.

`inventory` 默认的位置是 `/etc/ansible/hosts`, 这样有点麻烦, 我们可以调整 `ansible.cfg`, 来指定 `hosts` 文件.

### ansible.cfg

`ansible` 2.0 配置文件依据以下的顺序检索:

```
* ANSIBLE_CONFIG (an environment variable)
* ansible.cfg (in the current directory)
* .ansible.cfg (in the home directory)
* /etc/ansible/ansible.cfg
```

可以根据个人的使用习惯设置 `cfg` 的路径, 我属于轻度用户, 只有这么一个项目依赖 `ansbile`, 所以的把 `ansible.cfg` 放在了项目中, 纳入版本控制, 我觉得这样也挺方便的, 便于分享代码.

我的配置文件非常简单, 如下:

```
[defaults]
hostfile=./hosts
transport=ssh
host_key_checking=false

[ssh_connection]
ssh_args=-o ForwardAgent=yes
```

其中, `hostfile` 设置 `hosts` 文件的位置
因为使用阿里云的弹性服务器, 有的服务器释放后再次申请的时候, 会分配已经使用过的服务器, 这样, 关闭 `host_key_checking` 可以避免服务器指纹不一致的问题

而我使用 `ssh-agent` 连接服务器, 所以额外配置了 `ssh_args=-o ForwardAgent=yes`.

而一般情况下, 我理解除了指定下 `hosts` 的位置以外, 基本不用设置.

`ansible` 的配置项蛮多的, 可以通过 [Configuration file](http://docs.ansible.com/ansible/intro_configuration.html) 查找更多的配置选项.

### modules

`ansible` 提供了大量的原生模块, 当然, 还有大量的社区维护的模块, 基本可以满足使用的需求. 当然, 对我来说绰绰有余.

在我的项目中, 服务器环境是 `centos 7.1`, 主要使用的模块有 `yum`, `systemd`, `git`, `copy`, `cron`等.

`ansible` 提供了详尽的模块指南, 只需要理解其中的概念基本都可以很快上手.

这里我以简单的 `proxy` 为例,

`proxy` 这个 `role` 实质上是一个 `Nginx forward proxy` 服务, 从之前的 `tree` 可以看到包含以下文件:

**nginx.conf**
```
# For more information on configuration, see:
#   * Official English Documentation: http://nginx.org/en/docs/
#   * Official Russian Documentation: http://nginx.org/ru/docs/

user nginx;
worker_processes auto;
error_log /var/log/nginx/error.log;
pid /run/nginx.pid;

# Load dynamic modules. See /usr/share/nginx/README.dynamic.
include /usr/share/nginx/modules/*.conf;

events {
  worker_connections  2048;
  multi_accept on;
  use epoll;
}

http {
    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  /var/log/nginx/proxy.log  main;

    sendfile            on;
    tcp_nopush          on;
    tcp_nodelay         on;
    keepalive_timeout   65;
    types_hash_max_size 2048;

    server {
        listen       8080;

        location / {
            resolver 8.8.8.8;
            proxy_pass http://$http_host$uri$is_args$args;
        }

        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }
    }

    include             /etc/nginx/mime.types;
    default_type        application/octet-stream;

    # Load modular configuration files from the /etc/nginx/conf.d directory.
    # See http://nginx.org/en/docs/ngx_core_module.html#include
    # for more information.
    include /etc/nginx/conf.d/*.conf;
}
```

**handlers/main.yml**
```yaml
---
- name: restart nginx
  service:
    name: nginx
    state: restarted
```

**tasks/main.yml**
```yaml
---
- name: install nginx
  yum:
    name: nginx
    state: present
    update_cache: yes
  tags: setup

- name: copy nginx configure file
  copy:
    src: ../configures/nginx.conf
    dest: /etc/nginx/nginx.conf
    owner: root
    group: root
    mode: 0644
  notify:
    - restart nginx

- name: enable nginx services
  systemd:
    name: nginx
    enabled: yes
    state: started
  ```


其中, `tasks/main.yml` 为 `role` 的主文件, 其中首先使用 `yum` 模块, 安装 `Nginx`, 接着, 复制配置文件至 `/etc` 下, 同时, 触发 `handers/main.yml` 中的任务, 重启 `Nginx`, 最后, 配置 `systemd`, 使其跟随开机自动启动服务.

这样就免除了手工配置服务器的过程, 当然, 现在云供应商提供镜像功能, 只需要手工配置一次, 就可以批量复制出大量的服务器, 但是, `ansible` 可以让你的工作变的更有序, 更便于管理, 何乐不为呢?

### playbook

`playbook` 相当于把一系列的 `role` 组合成一个任务, 这样, 可以编写细粒度的 `role`, 通过 `playbook` 把一系列的任务组合起来, 使代码更加灵活.

我所编写的 `playbook` 如下所示:

```
---
# this playbook deploys the runner applications on the [builds] hosts

- name: runtime env deploy
  hosts: builds
  remote_user: root

  roles:
    - runtime
    - code
    - services
```

`hosts` 指明所需要构建的服务器组, `remote_user` 指明执行指令的用户, `roles` 显而易见的就是所要执行的 `role`, `ansible playbook` 会按照指定的执行顺序依次执行 `role`.

## 后记

我也只是一个 `ansible` 的初级使用者, 只是在工作中倒逼, 不得不使用 `ansible`. `role` 和 `playbook` 都写的很仓促, 只是为了满足当时的工作(不得不说, `ansible` 和 `systemd` 对我日常维护服务器带来了极大的便利), 以上的内容只是**粗陋**的描述了我**简陋**的使用流程.

当然, 其中缺失了 `ssh-agent`, `systemd` 等内容, 等我整理完成, 可能会写一点吧...

## ref

1. [ansible official docs](http://docs.ansible.com/ansible/index.html)
1. [jlund/streisand(playbook example)](https://github.com/jlund/streisand/)

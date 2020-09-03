---
layout: single
title: 使用 ansible 重启服务并保证后续任务继续执行
date: 2018-01-05 10:30:04 +0800
tags:
  - ansible
---

## version
- ansbile 2.4.2.0

## 表现

当使用 ansible 关闭 selinux 后, 需要重启服务器, 并且能保证后续的任务可以继续执行, 但是每当服务器重启后, ansible 会因与服务器失去连接而导致 unreachable, 后续执行无法继续.

经过各种尝试, 解决该问题.
<!-- more -->

## 解决

直接上代码, 

```yml
- name: turn off selinux
  selinux:
    state: disabled
  register: se
```

当关闭 selinux 时, 将状态寄存入 `se`, 接着

```yml
- name: reboot host and wait for it to return
  shell: sleep 5 && shutdown -r now "reboot for disable selinux"
  ignore_errors: true
  when:
    se.reboot_required == True

- name: wait for the server to finish rebooting
  wait_for_connection:
    delay: 5
```

根据寄存变量 `se` 中 `reboot_required` 的值, 进行服务器的重启动作.

但是, 当走到这一步之后, ansible 会与 host 断开连接, 导致后续的动作无法执行. 也就是无法执行后续的 `wait_for_connection` 模块动作.

最终, 根据 [ansible issue#10472](https://github.com/ansible/ansible/issues/10472#issuecomment-257268841) 解决该问题.

最终代码为以下

```yml
- name: install libselinux-python
  yum:
    name: libselinux-python
    state: present
  tags:
    - optimize
    - selinux

- name: turn off selinux
  selinux:
    state: disabled
  register: se
  tags:
    - optimize
    - selinux
  
- name: reboot host and wait for it to return
  shell: sleep 5 && shutdown -r now "reboot for disable selinux"
  async: 1
  poll: 0
  ignore_errors: true
  when:
    se.reboot_required == True

- name: Wait for the server to finish rebooting
  wait_for_connection:
    delay: 5
```

这样, 就可以保证关闭 selinux 并重启服务器后, 等待服务器重启, 继续进行后续的任务.

## ref
[ansible issue#10472](https://github.com/ansible/ansible/issues/10472#issuecomment-257268841)
---
layout: single
title: 制作 boot2docker 镜像
date: 2016-02-22 02:58:20 +0800
tags:
  - docker
  - ca-certificates.crt
categories: devops
---

## 意义
当我们搭建了自己的docker registry之后, 配置了nginx, 同时添加了自签名的ca证书之后, 制作一个属于自己的boot2docker镜像就显得尤为急切了.

还好boot2docker构建工具能够非常轻松的帮我们做到.

<!--more-->

## 异常
当我写好Dockerfile之后, 执行`docker build -t boot2docker . && docker run --rm boot2docker > boot2docker.iso`将镜像复制到`~/.docker/machine/cache`目录下, 开启一个新的docker-machine后, 无法连接至我的私有registry, 显示的报错指向了我自签名的ca证书.

google一番后发现, 需要对boot2docker进行一些改造.

## 困难
boot2docker提供了完整的[构建工具](https://github.com/boot2docker/boot2docker/blob/master/doc/BUILD.md), 我尝试将ca证书导入ca-certificates.crt中, 各种方法尝试了半天都没成功, 完全被卡住了.

## 解决
看了看issues, 发现这个问题也困扰到了老外, 有大牛给出了[解决方案](https://github.com/boot2docker/boot2docker/issues/347#issuecomment-70950789), 不过我自己做的证书都是`crt`的, 他的工具只能处理`pem`, 我实在懒得改.

正好[楼上](https://github.com/boot2docker/boot2docker/issues/347#issuecomment-70829900)的解决方案我看着也行, 照着他的方案改写了下, 还真的成了.

其实按照[@hairyhenderson](https://github.com/hairyhenderson)他的说法,

> The problem was that /etc/ssl/certs/ca-certificates.crt gets overwritten at boot time, so I added a command to the /etc/init.d/rcS script.

他的脚本重点是
```
echo "cat /usr/local/share/ca-certificates/foo.com/foo.crt >> /usr/local/etc/ssl/certs/ca-certificates.crt" >> $ROOTFS/etc/init.d/rcS
```

所以我的Dockerfile也就出来了

```
FROM boot2docker/boot2docker
COPY data/ca.crt $ROOTFS/etc/docker/certs.d/DOMAIN/ca.crt
RUN echo "cat /etc/docker/certs.d/DOMAIN/ca.crt >> /usr/local/etc/ssl/certs/ca-certificates.crt" >> $ROOTFS/etc/init.d/rcS
RUN /make_iso.sh
CMD ["cat", "boot2docker.iso"]
```

再次执行build, 生成的boot2docker镜像不需要再进去配置, 就可以连接到自签名ca的docker registry服务器上了.

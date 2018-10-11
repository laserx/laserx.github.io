---
layout: single
title: 基于 ubuntu 搭建 docker registry
date: 2016-02-22 02:59:09 +0800
tags: docker
categories: devops
---

## 什么是docker registry
`docker registry`是docker的一个私有docker images存储工具.

鉴于国内网络质量, 以及私有docker images的安全性因素, 搭建一个属于自己团队的docker registry还是很有必要的, 而且, 搭建起来轻松便捷.

<!--more-->

## 准备工作
首先, 需要准备一个域名, 最好还能有一个ca证书, 当然, 没有也是可以的, 像我一样, 自己签一个好了, 不过就是会麻烦很多.

接着你需要一台服务器, 最好是ubuntu, 安着省事, 需要一些工具`docker`是必须的, `apache2-utils`也要有.

接着就可以准备我们的`docker-compose.yml`文件了. 当然, docker registry是python写的, 可以直接安在服务器上, 但是, 既然有现成的docker, 为什么不试试呢.

文件结构是:

```
├── data
├── docker-compose.yml
├── nginx
│   ├── dev-docker-registry.com.csr
│   ├── devdockerCA.crt
│   ├── devdockerCA.key
│   ├── devdockerCA.srl
│   ├── domain.crt
│   ├── domain.key
│   ├── registry.conf
│   └── registry.password
└── readme.md
```

docker-compose.yml
```
nginx:
  image: "nginx:1.9"
  ports:
    - 443:443
  links:
    - registry:registry
  volumes:
    - ./nginx/:/etc/nginx/conf.d

registry:
  image: registry:2
  ports:
    - 127.0.0.1:5000:5000
  environment:
    REGISTRY_STORAGE_FILESYSTEM_ROOTDIRECTORY: /data
  volumes:
    - ./data:/data
```
nginx/registry.conf

```
upstream docker-registry {
  server registry:5000;
}

server {
  listen 443;
  server_name reg.sofans.cn;

  # SSL
  ssl on;
  ssl_certificate /etc/nginx/conf.d/domain.crt;
  ssl_certificate_key /etc/nginx/conf.d/domain.key;

  # disable any limits to avoid HTTP 413 for large image uploads
  client_max_body_size 0;

  # required to avoid HTTP 411: see Issue #1486 (https://github.com/docker/docker/issues/1486)
  chunked_transfer_encoding on;

  location /v2/ {
    # Do not allow connections from docker 1.5 and earlier
    # docker pre-1.6.0 did not properly set the user agent on ping, catch "Go *" user agents
    if ($http_user_agent ~ "^(docker\/1\.(3|4|5(?!\.[0-9]-dev))|Go ).*$" ) {
      return 404;
    }

    # To add basic authentication to v2 use auth_basic setting plus add_header
    auth_basic "registry.localhost";
    auth_basic_user_file /etc/nginx/conf.d/registry.password;
    add_header 'Docker-Distribution-Api-Version' 'registry/2.0' always;

    proxy_pass                          http://docker-registry;
    proxy_set_header  Host              $http_host;   # required for docker client's sake
    proxy_set_header  X-Real-IP         $remote_addr; # pass on real client's IP
    proxy_set_header  X-Forwarded-For   $proxy_add_x_forwarded_for;
    proxy_set_header  X-Forwarded-Proto $scheme;
    proxy_read_timeout                  900;
  }
}
```

大量ca相关的玩意请无视, 接着就可以`docker compose up`来体验一把docker的神奇了.

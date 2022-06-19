---
title: Docker容器没有vim
date: 2021-07-17 19:00:56
tags: docker
categories: 技术资料
---
docker进入容器后，发现没有vim功能，记录解决办法如下：

1. 执行

```
apt-get update
```

2. 安装vim

```
apt-get install vim
```
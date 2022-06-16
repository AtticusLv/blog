---
title: Hexo编辑博客时要用到的命令
date: 2017-05-16 11:00:12
tags:
- hexo
---
记录下在博客中要经常用的hexo相关的tag和命令
# 标签
## 图片
```
{% asset_img issue3.png issue3 %}
```
## 公式
$2^{i-1}$
```
$2^{i-1}$
```
## kdb标签
引入hexo-tag-kbd
{% kbd Ctrl %}
```
{% kbd Ctrl %}
```
## 文章链接
{% post_link Micro-Services 微服务学习 %}
```
{% post_link Micro-Services 微服务学习 %}
```
# 命令
## 新建文章
```
hexo new post "title"
```
## 清理历史静态文件
```
hexo clean
```
## 生成静态文件
```
hexo generate
```
## 本地启动服务查看
```
hexo server
```
## 部署
```
hexo d
```
## 新建新页面
```
hexo new page "title"
```

---
title: HEXO写作命令记录
date: 2020-08-08 17:51:06
tags:
- hexo
---
# HEXO写作命令记录
## Create New Post
```
hexo new '[post_name]'
hexo n '[post_name]'
```

## Create New Page
```
hexo n page ''
```

## Define Custom Page/Post Template
1. Create a new markdown file in the root folder **/scaffolders**, such as **sprint.md**.
2. Use blow commands

```
hexo n sprint '[post_name]'
```

## Debug
```
hexo server
```

## Generate Static Files
```
hexo g
```

## Deploy to remote server/git
```
hexo d
```

## Insert a picture
```
![picture's name](picture's relative path)
```

## Insert a Read More
```
<!-- more -->
```

## Top articles
need to install plugin

```
npm install hexo-generator-index-pin-top --save
```

write **top** attribute in the article

```
top: true

top: 10 # when number is large, the article is the toppest article
```
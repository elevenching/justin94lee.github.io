---
layout: post
title: "Jekyll 配置Github Pages 个人博客遇到的一些问题"
date: 2016-09-11
excerpt: "记录；备忘"
tags: [blog, Jekyll]
feature: https://upload.wikimedia.org/wikipedia/commons/4/42/Jekyll_%28software%29_Logo.png
comments: true
---

## Jekyll 本地调试

修复缺失的包

    bundle install

在本地环境运行

    jekyll serve --watch

然而上面的命令会报错，下面的命令可解决。

    bundle exec jekyll serve -w

## _config.yml 配置

### url 地址

本地调试地址

    url: http://localhost:4000

上传到repo 需要修改为博客地址

    url: https://justin94lee.github.io
    
    需要注意的是，这里是https

### disqus 评论功能

1. 去disqus 注册并配置账号

2. 复制代码到 /_layouts/post.html 底部

3. 在 _config.yml 填写shortname


## 文章规范

以这篇文章为例

    ---
    layout: post
    title: "Jekyll 配置Github Pages 个人博客遇到的一些问题"
    date: 2016-09-11
    excerpt: "记录；备忘"
    tags: [blog, Jekyll]
    feature: https://upload.wikimedia.org/wikipedia/commons/4/42/Jekyll_%28software%29_Logo.png
    comments: true
    ---

1. 标签是tags

2. 不是project,不要写project。

        project: true

    只有文章是project 时才需要加上这个

3. feature: 后面可以添加文章封面图


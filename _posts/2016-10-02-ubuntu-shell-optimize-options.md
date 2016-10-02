---
layout: post
title: "一些ubuntu 的不常用但重要的命令"
date: 2016-10-02
excerpt: "优化Shell 和磁盘速度测速的命令"
tags: [blog]
comments: true
---

### 自动补全

启用shell 的自动补全功能

    sudo apt-get install bash-completion

启用忽略大小写的自动补全功能

编辑~/.inputrc, 在里面添加以下内容
    
    set completion-ignore-case on

### 启用丰富色彩的Shell 命令行界面

编辑 .bashrc,在里面取消注释此行

    force_color_prompt=yes

[参考资料](https://scottlinux.com/2013/07/08/enable-colorful-terminal-in-debian-and-ubuntu/)

### 解决Chrome 字体变成楷体

    sudo apt-get remove fonts-arphic-ukai && sudo apt-get remove fonts-arphic-uming

### 磁盘速度测试

参考以下资料

* https://linux.cn/article-3696-1.html
* http://blog.chinaunix.net/uid-24250828-id-3239100.html

---
layout: post
title: "使用七牛云作为博客的图床"
date: 2016-11-02
excerpt: "不再忍受Push 到GitHub 的速度了，找一个图床寄托你的色彩。"
tags: [教程,博客,备份,同步,图片]
comments: true
---

## 前言

刚写博客的时候，比较少放图片，所以图片文件一直存在本地和博客一起上传，但是在写前一篇博客的时候，发现图片太大push 到GitHub 还真是有困难，压缩了几次才成功。后来才开始恐慌，要是每次都这样，那岂不是要崩溃，于是开始寻找一个可以批量托管图片的地方。

一个好的图片托管工具，应该满足一下几个需求。

* 提供免费试用空间

* 能够使用工具上传，便于批量管理。

* 访问速度要比较快，至少没有被墙。

在比较了Google Photos、imgur、新浪微博、七牛云后发现还是七牛云比较符合要求。

七牛云为实名认证的个人用户提供10Gb 的储存空间，提供命令行工具管理图片，在后台可批量修改图片属性。

看到大部分教程是对旧版的qrsync 命令行同步工具，本文是使用qshell 作为命令行同步工具。

>qshell 是基于七牛API 参考手册实现的一个方便开发者测试和使用七牛API 服务的命令行工具。

## 配置过程

### 注册七牛帐号

注册过程很简单，去[七牛云官网](http://www.qiniu.com/) ，点击*注册* 然后选择账号类型，普通用户一般选择*个人账户*，然后填写资料，根据提示操作即可。

需要注意的是，必须要实名认证后才能获得免费的10Gb 储存空间。个人实名认证可以使用支付宝来快速实名认证，企业认证没试过不清楚。

### 创建图片空间

图片空间就是一个用来存放图片的仓库，或者说是相册。

找到*对象储存* 然后点击**立即添加**

![图片空间][c1]

然后如下图，填写资料即可。

![创建空间][c2]

如果要做图床，*访问控制* 只能选择**公开空间**

记下图片空间的名称和储存区域，在下一步会用到。

### qshell 的配置

* 下载qshell 管理工具

    下载地址：http://developer.qiniu.com/code/v6/tool/qshell.html

* 初始化命令行工具

    下载完成后解压缩，如果是Windows 用户可以直接使用cmd 切换到本地文件夹使用qshell

    Linux 和Mac OS 用户可以重命名qshell_*文件为qshell并复制到/usr/local/bin，如果不清楚如何选择，请参见[此文][ref1]

* 获取密钥

    进入个人主页，然后点击右上角的头像，最后点击密钥管理，进入后你会看到一串“乱码”和一串点，分别是AccessKey、SecretKey 这两个密钥在接下来的过程中很有用。

* 创建本地配置文件

    在本地创建一个配置文件，用于储存密钥和配置信息，并保存为后缀名为json的文件，例如qshell.json

	示例配置

        {
           "src_dir":"/Downloads/Pohtos/",
           "access_key": "Your_Access_Key",
           "secret_key": "Your_Secret_Key",
           "buceet": "你的空间名称",
           "rescan_local": true
        }

    src_dir 就是你本地用来储存图片的地方，也就是要同步到七牛云的文件夹。

    以上5 个参数是必填的，根据需要替换上面冒号后面的内容为自己的，注意要保留双引号。

    **扩展用法**

    "overwrite": false
    表示如果有同名文件时候，不覆盖。

    更多配置参数可参照此[文档][ref2]

### 上传

把上面的配置文件放在一个容易找到的位置，然后把图片放进要同步的文件夹里，后面的就很简单了。

上传命令很简单，就是qshell + 配置文件，示例：

    qshell qupload ~/Downloads/qshell.json

更多高级命令参见[命令][ref3] 参数详解

### 外链

去内容管理可以查看上传图片的外链

![外链][p3]

实际上，外链生成是有规律的，只要记住外链默认域名就能够很容易推算出图片的外链地址，但我想应该还有更好的方法可以挖掘...

## 总结

七牛云的功能还是比较完善的，虽然以后写博客多了几个步骤，但是这也是无法避免的。托管在第三方确实有些风险需要考虑，还好文件在本地也有备份。万一哪天七牛云不能用了，写个工具转移图床应该也不会太难，现在的方法是在折腾和方便之间的一个不完美平衡。

其他方面，因为图片外链不是使用https 协议的，所以副作用就是博客网址前面的小绿锁不见了！

## 参考链接

* http://cnfeat.com/blog/2015/11/30/cli-qiniu/

* https://support.qiniu.com/hc/kb/article/198313/

* http://yixuan.li/geek/2015/11/23/qiniuguideline/

* https://devhub.io/repos/qiniu-qshell

----------
[c1]:http://ofjynnbi3.bkt.clouddn.com/2016-11-01/c1.png
[c2]:http://ofjynnbi3.bkt.clouddn.com/2016-11-01/c2.png
[p3]:http://ofjynnbi3.bkt.clouddn.com/2016-11-01/p3.png
[ref1]:https://github.com/qiniu/qshell
[ref2]:https://github.com/qiniu/qshell/wiki/qupload
[ref3]:https://github.com/qiniu/qshell

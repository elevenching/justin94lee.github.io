---
layout: post
title:  "elementary OS 的安装优化和问题汇集"
date:   2016-07-05
excerpt: "在chromebook 上安装elementary OS 遇到的一些问题和解决方法"
project: false
tag:
- elementary
- 教程
- crouton
- chromebook

comments: false
---

# 安装步骤
## 预备工作
crouton 代理的修改

## chroot 内的修改
修改好后，此时不建议进入桌面环境，否则在chroot 外进入会出错


## 找回语言支持
>sudo apt-get install language-selector-gnome

# 安装过程的问题
## 在chroot 里面使用sudo 命令运行会使得在chroot 外不能进入chroot 桌面
>sudo enter-chroot

>sudo chown -R 1000:1000 "$HOME"

[参考链接](https://github.com/dnschneid/crouton/issues/955)


## 无法安装软件包
需要添加源
>sudo apt-add-repository ppa:ubuntu-on-rails/ppa

>sudo apt-get update

>sudo apt-get install gedit

[参考链接](http://askubuntu.com/questions/287116/gmate-installation-issue)

** 最好去软件中心勾选需要的地址



# 美化及软件包

* Spotify


>sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys BBEBDCB318AD50EC6865090613B00F1FD2C19886

>echo deb http://repository.spotify.com stable non-free | sudo tee /etc/apt/sources.list.d/spotify.list

>sudo apt-get update

>sudo apt-get install spotify-client

* 安装 Atom

>sudo add-apt-repository ppa:webupd8team/atom

>sudo apt-get update

>sudo apt-get install atom

* monoki 编程字体

>https://github.com/madmalik/mononoki/

* Clementine Music Player

>sudo add-apt-repository ppa:me-davidsansome/clementine

>sudo apt-get update

>sudo apt-get install clementine

* 启用所有的软件仓库（sudo software-properties-gtk）

>sudo software-properties-gtk

* 解决网易云音乐客户端无法播放的问题

>sudo apt-get install gstreamer0.10-plugins-good

[原文链接](http://tieba.baidu.com/p/4600131142)


* 在启动项管理显示系统启动项

>sudo sed -i 's/NoDisplay=true/NoDisplay=false/g' /etc/xdg/autostart/*.desktop

* 安装dconf-editor

>sudo apt-get install dconf-tools

* 修改croton 代理地址

打开文件

>vi ~/Downloads/crouton.proxy

找到此段代码

> sed -i '/Execute the main script inline/ased -i '\''s/wget -O /wget -e "https_proxy=http:\\/\\/127.0.0.1:1080\\/" -O /g'\'' $SCRIPTDIR/targets/audio'
"$CACHEFILE"

修改127.0.0.1:8080为你的代理地址，或者pac网址



* 无法调出搜狗拼音

在～/.profile 中添加：

	export XIM="fcitx"
	export XIM_PROGRAM="fcitx"
	export XMODIFIERS="@im=fcitx"
	export GTK_IM_MODULE="fcitx"
	export QT_IM_MODULE="fcitx"

* 移除自带应用程序

* Material Design 主题

>sudo add-apt-repository ppa:snwh/pulp

>sudo apt-get update

>sudo apt-get install paper-gtk-theme paper-icon-theme
>


* 安装shadowsocks-python

**安装pip**

>sudo apt-get install python-pip

>sudo pip install  pip

>sudo pip install shadowsocks

**开启服务**

*方法一*

直接使用命令行开启

>sslocal -s 服务器ip -p 服务器端口 -k 密码

[参考链接](http://bblove.me/2015/03/09/use-ss/)


*方法二*

1. 创建配置文件

在 / etc / 位置下创建配置文件

sudo vi /etc/shadowsocks.json


	{
    	"server":"104.224.130.202",
    	"server_port":1525,
    	"local_address": "127.0.0.1",
    	"local_port":8080,
    	"password":"your_passwd",
    	"timeout":300,
    	"method":"aes-256-cfb",
    	"fast_open": false
	}

2. 启动命令



3. 自动启动

* 去系统设置-应用程序，添加一个启动项，启动程序为 *终端*

* 打开启动项管理

	>gnome-session-properties

* 编辑 *终端* 启动项，把下面的命令行放入 *终端* 的 命令 里面

	>sslocal -c /etc/shadowsocks.json

* 保存

注销后生效


4. 参考链接


	http://shadowsocks.info/ubuntu-shadowsocks-kehuduan/
	http://bblove.me/2015/03/09/use-ss/
	http://huangke.info/chromebookan-zhuang-ubuntu/

# crouton 基本命令

删除已安装的系统
>sudo delete-chroot chrootname

升级crouton
>sudo sh -e ~/Downloads/crouton -u -n chrootname

备份(默认目录)
>sudo edit-chroot -b chrootname

备份到U 盘
>sudo edit-chroot -bf /media/removable/*/ chrootname

从U 盘恢复备份
>sudo edit-chroot -rf /media/removable/*/ chrootname



# 扩展阅读

	http://www.vpsee.com/2013/08/introduction-to-elementaryos/
	http://camo96321.blogspot.com/2015/11/eoselementary-os.html
	http://mylinuxideas.blogspot.com/2015/04/enable-desktop-icons-and-right-click-on.html
	https://wiki.galliumos.org/Additional_Software
	http://chenshen.leanote.com/post/%E5%AE%89%E8%A3%85ElementaryOS-Freya%E5%90%8E%E7%9A%84%E9%85%8D%E7%BD%AE-2
	http://www.binarytides.com/better-elementary-os-luna/
	https://gist.github.com/memoryleakx/7567474
	http://huangke.info/chromebookan-zhuang-ubuntu/
	http://howtoelementaryos.org/how-to-display-hidden-auto-start-entries-in-elementary-os

# 其他参考教程
	https://docs.google.com/document/d/1ZURJITX-eCrhwP1cImwYHMthBXXHJwLrovIfiZh_vwQ/edit?pref=2&pli=1
	http://jeanlouisnguyen.blogspot.com/2014/01/guide-how-to-install-elementary-os-on.html

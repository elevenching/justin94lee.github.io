---
layout: post
title: "Chromebook 安装elementary OS 的优化和一些问题解决"
date: 2016-07-05
excerpt: "在chromebook 上安装elementary OS 遇到的一些问题和解决方法。"
feature: https://raw.githubusercontent.com/elementary/brand/master/851x315_blue.png
tags: [elementary, 教程, crouton, chromebook]
comments: true
---

# 安装步骤

1.下载crouton

可以去crouton 项目官网下载

    https://github.com/dnschneid/crouton

2.下载系统

以安装Ubuntu 系统为例，普通的安装可以输入如下命令：

    sudo sh -e ~/Downloads/crouton -r trusty -n trustyXorg -t xorg,extension,keyboard -m http://mirrors.aliyun.com/ubuntu/ 

-m 后面的代表镜像地址

3.代理问题

**方法一(推荐)**

由于crouton 中的某个包需要连接到谷歌服务器，不使用代理会导致连接超时。

在Chromebook 安装代理后，只需要在上面的命令最后加上：
    
    -p 127.0.0.1:8080

以上分别是代理的地址和端口

[参考链接](https://github.com/dnschneid/crouton/wiki/Crouton-Command-Cheat-Sheet)

**方法二**

之前在国内使用的crouton.proxy 文件由于pac 代理地址失效已不可用，不过可以直接修改crouton 代理地址让它复活。

打开文件

    vi ~/Downloads/crouton.proxy

找到此段代码

     sed -i '/Execute the main script inline/ased -i '\''s/wget -O /wget -e "https_proxy=http:\\/\\/127.0.0.1:1080\\/" -O /g'\'' $SCRIPTDIR/targets/audio'
     "$CACHEFILE"

修改127.0.0.1:8080为你的代理地址，或者pac网址

4. 安装其他桌面环境

如果使用Ubuntu unity环境，在修改代理并运行第一步的代码后，可使用以下命令直接进入系统

    sudo startunity

如果遇到问题可参照crouton [项目官网](https://github.com/dnschneid/crouton)

如果是安装elementary OS 或其他桌面环境，可参照参照此文的方法完成剩余步骤。

* http://blog.chromebook.space/archives/29/

## 安装到可移动设备

Chromebook 的内部储存空间有限，安装Ubuntu
后会占较大一部分空间，在使用一段时间后容易导致系统可用空间不够。

在不能手动给Chromebook 升级内部储存的情况下，安装在可移动设备是个较好的替代选择。

可通过直接安装和从备份恢复来完成安装到可移动设备的步骤

### 直接安装

基本步骤：
    
1.格式化可移动设备为ext4格式 

2.剩余步骤于直接安装无太多差异
    
但需要在下载系统的命令后面加上以下命令，指定安装位置到可移动设备。

    -p /media/removable/USB/

USB 为可移动磁盘的卷标

### 从备份恢复到可移动设备

基本步骤：
    
1.备份系统

2.格式化可移动设备为ext4格式 

3.恢复备份包到可移动设备

### 启动命令

    sudo sh /media/removable/USB/bin/startunity

USB 为可移动磁盘的卷标，startunity 可替换为 /media/removable/USB/bin/
下的对应文件

### 安装在SD 卡的系统休眠问题

Chromebook 在合上盖子后会卸载SD 卡（USB 设备不会)，这样每次都需要重启Chromebook
才能进入。

解决办法是让系统不休眠，缺点是盖上盖子后还是会耗电，极大的降低了续航能力，因此还是建议安装到USB
设备或使用读卡器。

电源管理修改方法参考

* https://github.com/dnschneid/crouton/wiki/Power-manager-overrides

待机和恢复问题

* https://github.com/dnschneid/crouton/issues/288

# 安装过程的问题

### 在chroot 里面使用sudo 命令运行会使得在chroot 外不能进入chroot 桌面
    
    sudo enter-chroot

    sudo chown -R 1000:1000 "$HOME"

[参考链接](https://github.com/dnschneid/crouton/issues/955)

### 无法安装软件包

需要添加源
    
    sudo apt-add-repository ppa:ubuntu-on-rails/ppa

    sudo apt-get update

    sudo apt-get install gedit

[参考链接](http://askubuntu.com/questions/287116/gmate-installation-issue)

**最好去软件中心勾选需要的地址**

# 美化及更多软件包

* 修改系统语言为中文，需要添加语言支持

    sudo apt-get install language-selector-gnome

* Spotify

        sudo ap -key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys BBEBDCB318AD50EC6865090613B00F1FD2C19886

        echo deb http://repository.spotify.com stable non-free | sudo tee /etc/apt/sources.list.d/spotify.list
    
        sudo apt-get update
    
        sudo apt-get install spotify-client

* 安装 Atom 文本编辑器

        sudo add-apt-repository ppa:webupd8team/atom
    
        sudo apt-get update
    
        sudo apt-get install atom

* monoki 编程字体

    参见

        https://github.com/madmalik/mononoki/
    
* Clementine Music Player
    
        sudo add-apt-repository ppa:me-davidsansome/clementine

        sudo apt-get update
    
        sudo apt-get install clementine
    
* 启用所有的软件仓库（sudo software-properties-gtk）

        sudo software-properties-gtk

* 解决网易云音乐客户端无法播放的问题

        sudo apt-get install gstreamer0.10-plugins-good

    [参考链接](http://tieba.baidu.com/p/4600131142)


* 在启动项管理显示系统启动项

        sudo sed -i 's/NoDisplay=true/NoDisplay=false/g' /etc/xdg/autostart/*.desktop

* 安装dconf-editor

        sudo apt-get install dconf-tools

* 无法调出搜狗拼音

在～/.profile 中添加：

        export XIM="fcitx"
    	export XIM_PROGRAM="fcitx"
    	export XMODIFIERS="@im=fcitx"
    	export GTK_IM_MODULE="fcitx"
    	export QT_IM_MODULE="fcitx"

* Material Design 主题

        sudo add-apt-repository ppa:snwh/pulp

        sudo apt-get update

        sudo apt-get install paper-gtk-theme paper-icon-theme
    
* 安装shadowsocks-python

    1.安装pip

        sudo apt-get install python-pip

        sudo pip install  pip

        sudo pip install shadowsocks

2.开启服务

**方法一**

直接使用命令行开启

    sslocal -s 服务器ip -p 服务器端口 -k 密码

[参考链接](http://bblove.me/2015/03/09/use-ss/)


**方法二**

* 创建配置文件

打开shadowsocks.json

    sudo vi /etc/shadowsocks.json


在 /etc/ 位置下创建配置文件

	{
    	"server":"192.168.137.1",
    	"server_port":1793,
    	"local_address": "127.0.0.1",
    	"local_port":8080,
    	"password":"your_passwd",
    	"timeout":300,
    	"method":"aes-256-cfb",
    	"fast_open": false
	}

把上面的server、server_port、local_port、password换成自己的配置

需要注意的是local_port 的端口号建议不要低于1080

* 自动启动

去系统设置-应用程序，添加一个启动项，启动程序为 *终端*

打开启动项管理

    gnome-session-properties

编辑 *终端* 启动项，把下面的命令行放入 *终端* 的 命令 里面

	sslocal -c /etc/shadowsocks.json

保存并退出

注销后生效

* 参考链接

	http://shadowsocks.info/ubuntu-shadowsocks-kehuduan/

	http://bblove.me/2015/03/09/use-ss/

	http://huangke.info/chromebookan-zhuang-ubuntu/

# crouton 基本命令

**chrootname 为安装的版本**

删除已安装的系统

    sudo delete-chroot chrootname

删除可移动设备上的系统

    sudo /media/removable/USB/bin/delete-chroot -y trusty
    # USB 为可移动设备名称

升级crouton

    sudo sh -e ~/Downloads/crouton -u -n chrootname

备份(默认目录)

    sudo sh edit-chroot -b chrootname

备份到U 盘

    sudo sh edit-chroot -bf /media/removable/USB/ chrootname

从U 盘恢复备份

    sudo sh edit-chroot -rf /media/removable/USB/ chrootname

**从可移动设备备份/恢复到另一个可移动设备**

从SD卡备份到U盘

    sudo sh /media/removable/SD Card/bin/edit-chroot -y -f /media/removable/USB/ -b chrootname

从U盘恢复到SD 卡

    sudo sh -e ~/Downloads/crouton -f /media/removable/USB/*.tar.gz -p '/media/removable/SD Card'/

# 参考资料 

* http://www.vpsee.com/2013/08/introduction-to-elementaryos/
* http://camo96321.blogspot.com/2015/11/eoselementary-os.html
* http://mylinuxideas.blogspot.com/2015/04/enable-desktop-icons-and-right-click-on.html
* http://woodenrobot.me/2016/03/15/一天一夜chromebook折腾心得/
* https://wiki.galliumos.org/Additional_Software
* http://chenshen.leanote.com/post/%E5%AE%89%E8%A3%85ElementaryOS-Freya%E5%90%8E%E7%9A%84%E9%85%8D%E7%BD%AE-2
* http://www.binarytides.com/better-elementary-os-luna/
* https://gist.github.com/memoryleakx/7567474
* http://huangke.info/chromebookan-zhuang-ubuntu/
* http://howtoelementaryos.org/how-to-display-hidden-auto-start-entries-in-elementary-os

# 其他参考教程
* https://docs.google.com/document/d/1ZURJITX-eCrhwP1cImwYHMthBXXHJwLrovIfiZh_vwQ/edit?pref=2&pli=1
* http://jeanlouisnguyen.blogspot.com/2014/01/guide-how-to-install-elementary-os-on.html
* https://docs.google.com/document/d/1kfLBq61dzNcQcSRer_mas94qLG4kHEHlK3E6xlXBXcU
 
----------
 题图：<https://github.com/elementary/brand>

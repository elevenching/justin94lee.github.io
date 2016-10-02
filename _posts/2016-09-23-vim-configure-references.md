---
layout: post
title: "VIM Python 编程环境配置"
date: 2016-09-25
excerpt: "配置VIM 成适合用于编写Python 程序和Markdown 文件的编辑器"
tags: [VIM, Config]
feature: https://upload.wikimedia.org/wikipedia/commons/thumb/9/9f/Vimlogo.svg/640px-Vimlogo.svg.png 
comments: true
---

# vim 和Gvim 的安装和更新

安装了vim-tiny，需要先卸载

    sudo apt-get remove vim-tiny

    apt-get update
    
    apt-get install vim

安装Gvim

    sudo apt-get instal vim-gtk

也可根据不同系统选择其他的GVIM 版本，如vim-gtk3，vim-athena 等。

安装后运行以下命令确认支持Python

    vim --version

如果出现+python 或+python 3则支持

*未支持的解决办法*
    
    待补充...

# .vimrc 配置

通过修改 .virmc 得到最佳的操作偏好

使用PEP8的Python 编程规范
    
    au BufNewFile,BufRead *.py
    \ set tabstop=4
    \ set softtabstop=4
    \ set shiftwidth=4
    \ set textwidth=79
    \ set expandtab
    \ set autoindent
    \ set fileformat=unix

显示行号

    set nu

设置语法高亮显示

    syntax on

使用UTF-8 编码
    
    set encoding=utf-8

设置字体，如果是GVIM 建议去 .gvimrc 添加以下内容

    :set guifont=mononoki:14

其中mononoki 代表设置的字体，14 代表字体大小。 

# vim 插件

vundle

用于管理插件的插件

安装方法:
   
    git clone https://github.com/gmarik/Vundle.vim.git ~/.vim/bundle/Vundle.vim 

用vim打开 .vimrc

    vim .vimrc

在 .vimrc 里面的顶部插入以下内容

    set nocompatible              " required
    filetype off                  " required
    
    " set the runtime path to include Vundle and initialize
    set rtp+=~/.vim/bundle/Vundle.vim
    call vundle#begin()
    
    " alternatively, pass a path where Vundle should install plugins
    "call vundle#begin('~/some/path/here')
    
    " let Vundle manage Vundle, required
    Plugin 'gmarik/Vundle.vim'
    
    " Add all your plugins here (note older versions of Vundle used Bundle instead of Plugin)
    
    
    " All of your Plugins must be added before the following line
    call vundle#end()            " required
    filetype plugin indent on    " required

现在开始，你可以把需要Vundle 帮你安装的插件名称以正确的形式放在call vundle#begin()和call vundle#end()之间 

    Plugin 'VundleVim/Vundle.vim'
    Plugin 'altercation/vim-colors-solarized'
    Plugin 'tomasr/molokai'
    Plugin 'jnurmine/Zenburn'
    Plugin 'Lokaltog/vim-powerline'
    Plugin 'nathanaelkane/vim-indent-guides'
    Plugin 'jistr/vim-inerdtree-tabs'
    Plugin 'kshenoy/vim-signature'
    Plugin 'majutsushi/tagbar'
    Plugin 'dyng/ctrlsf.vim'
    Plugin 'terryma/vim-multiple-cursors'
    Plugin 'SirVer/ultisnips'
    Plugin 'Valloric/YouCompleteMe'
    Plugin 'scrooloose/nerdtree'
    Plugin 'gcmt/wildfire.vim'
    Plugin 'sjl/gundo.vim'
    Plugin 'Lokaltog/vim-easymotion'
    Plugin 'suan/vim-instant-markdown'
    Plugin 'lilydjwg/fcitx.vim'
    Bundle 'scrooloose/syntastic'
    Plugin 'rakr/vim-one'
    Plugin 'klen/python-mode'
    Bundle 'Raimondi/delimitMate'
    Plugin 'scrooloose/nerdcommenter'
    Plugin 'pbrisbin/vim-mkdir'
    Plugin 'danro/rename.vim'
    Bundle 'Raimondi/delimitMate'

然后在普通模式下输入：
    
    :w

安装，还是在普通模式下输入：

    ：PluginInstall

## 一点说明

如果不了解上述插件的具体用途，可以去相应的github 页面查看具体用途和用法。

也可上[vimawesome](http://vimawesome.com)搜索相关插件

以下是一些需要在 .vimrc 里面添加的一些代码及说明

**Plugin 'suan/vim-instant-markdown'**

在 .vimrc 里添加以下内容，使得插件支持以markdown 结尾的文件

    autocmd BufNewFile,BufReadPost *.md set filetype=markdown
                        
如果不想在打开markdown 文件后自动打开浏览器，可以添加以下内容。

    let g:instant_markdown_autostart = 0 

**Plugin 'scrooloose/nerdtree'**

在 .vimrc 里添加以下内容，调整文件结构图的位置、大小、忽略的文件类型、快捷键等

    let NERDChristmasTree=0
    let NERDTreeWinSize= 20
    let NERDTreeChDirMode=2
    let NERDTreeIgnore=['\~$', '\.pyc$', '\.swp$']
    let NERDTreeShowBookmarks=1
    let NERDTreeWinPos="left"
    " Automatically open a NERDTree if no files where specified
    autocmd vimenter * if !argc() | NERDTree | endif
    " Close vim if the only window left open is a NERDTree
    autocmd bufenter * if (winnr("$") == 1 && exists("b:NERDTreeType") && b:NERDTreeType == "primary") | q | endif
    " Open a NERDTree
    nmap <F4> :NERDTreeToggle<cr>

**Bundle 'Raimondi/delimitMate'**

在 .vimrc 里添加以下内容

    " 自动补全单引号，双引号等
    Bundle 'Raimondi/delimitMate'
    " for python docstring ", 特别有用
    au FileType python let b:delimitMate_nesting_quotes = ['"']
    " 关闭某些类型文件的自动补全
    "au FileType mail let b:delimitMate_autoclose = 0

[参考链接](http://www.wklken.me/posts/2015/06/07/vim-plugin-delimitmate.html)
    
----------
题图：<https://en.wikipedia.org/wiki/Vim_(text_editor)>

---
layout: post
title: Ubuntu中增加右键终端菜单
category: 笔记
tags: [Ubuntu]
keywords: Ubuntu,右键,终端
description:
---

>其他很多发行版中都有“右键终端”功能，Ubuntu偏偏没有，感觉很不方便，有必要增加该项功能。

首先ctrl+alt+t进入终端，然后键入`sudo apt-get install nautilus-open-terminal`进行安装。

![安装](/post_res/2014-10-31/img/install_terminal.png)

结束后，运行`nautilus -q`命令重新加载文件管理器。

![安装](/post_res/2014-10-31/img/restart_nautilus.png)

效果图如下：

![效果](/post_res/2014-10-31/img/terminal_menu.png)
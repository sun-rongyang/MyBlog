---
title: Arch Linux 安装与配置 Fcitx 搜狗输入法
date: 2016-08-29 15:42:04
updated: 2016-10-30
tags: [arch,中文输入法]
categories:
	- operating system
	- Linux
---

对于中国人来说, [搜狗输入法](http://pinyin.sogou.com/)已经成为"国民级"的汉语输入法了. 通过多年的积累, 搜狗的汉语词库已经达到无可企及的地步, 智能联想用着非常爽. 当然我们的 Arch Linux 也不能少了搜狗输入法呀.
<!--more-->

### Fcitx 输入法框架的安装与配置
Linux 下的搜狗输入法是基于 Fcitx(Flexible Input Method Framework) 的, 所以首先安装 Fcitx.
```shell
$ sudo pacman -S fcitx
```
再安装 Fcitx 对 Gtk+/Qt 提供的输入法模块.
```shell
$ sudo pacman -S fcitx-im
```
在~/.xprofile (如果不存在请新建) 中加入如下代码.
```shell
export GTK_IM_MODULE=fcitx
export QT_IM_MODULE=fcitx
export XMODIFIERS="@im=fcitx"
gsettings set org.gnome.settings-daemon.plugins.xsettings overrides "{'Gtk/IMModule':<'fcitx'>}"
```
安装有图形界面的 Fcitx 配置工具.
```shell
$ sudo pacman -S fcitx-configtool
```
最后注销你的账户, 然后重新登陆, 一切正常的话, Fcitx 会自动启动.

### 安装搜狗输入法
首先在 pacman 中添加 Arch Linux 中文社区仓库/镜像加速源. 在 /etc/pacman.conf 文件末尾添加:
```
[archlinuxcn]
SigLevel = Optional TrustedOnly
Server = http://repo.archlinuxcn.org/$arch
```
然后安装 archlinuxcn-keyring 包导入 GPG key.
```shell
$ sudo pacman -S archlinuxcn-keyring
$ sudo pacman -Sy
```
最后终于可以安装搜狗输入法了.
```shell
$ sudo pacman -S fcitx-sogoupinyin
```

### 配置搜狗输入法
启动 fcitx-configtool 选择添加输入法, 然后照提示配置即可.

### Gnome 3.22 中解决 fcitx 输入法失灵问题
前几天更新了 Arch, 突然发现一些应用程序中不能切换到 fcitx 输入法了, 这可真是件郁闷的事儿. Google 了一通发现了原因, gnome 3.22 中gdm 默认使用的是 Wayland, 但 fcitx 对 Wayland 的支持还不够好, 所以出现问题. 解决办法就是通过修改配置文件使 gdm 默认使用 xorg. 修改如下配置文件 /etc/gdm/custom.conf, 去掉 WaylandEnable=false 行前面的注释即可.

---
title: 为Arch Linux添加网络打印机
date: 2016-08-26 22:48:42
tags: [linux,arch]
categories:
	- operating system
	- Linux
---

Aech Linux不像Ubuntu一样在setting中直接添加打印机即可, Arch用户需要自力更生, 从最基础的系统的打印驱动开始配置打印机. 幸运的是苹果公司为Unix-Like系统开发的标准的开源的打印系统CUPS可以很方便地在Arch中使用.
<!--more-->

### 安装CUPS相关软件
安装过程很简单, 直接pacman
```shell
$ sudo pacman -S cups libcups ghostcript gsfonts
```
我用的是惠普的一款LaserJet网络打印机, 所以要安装惠普的专门驱动
```shell
$ sudo pacman -S hplip
```

### 将CUPS服务加入守护进程并设置开机启动
使用systemd来添加
```shell
$ systemctl start org.cups.cupsd.service
$ systemctl enable org.cups.cupsd.service
```
执行的过程中会要求输入用户密码.

### 通过web界面管理打印机
经过上面的安装, 我们可以搜索和添加所在局域网上的惠普打印机了. 因为要在浏览器中使用web界面进行管理, 所以首先将当前用户加入sys组.
```shell
$ sudo usermod -aG sys [USERNAME]
```
打开浏览器, 访问http://localhost:631 ,选择Administration, 点击 **Add Printer**, 输入当前的用户的用户名和密码, 就可以开始添加打印机了. 选择**AppSocket/HP JetDirect **, 填入"socket://hostname"其中hostname换成你打印机的ip地址. 之后一路按照提示操作即可, 最后print test page. 

以后打开一个文件选择打印时, 你刚才添加的的打印机就会出现在printer list中.


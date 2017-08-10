---
title: 在 Arch Linux 中使用 VirtualBox 安装 Windows7
date: 2016-09-13 12:12:39
tags: [linux,arch,virtualmechine]
categories:
	- operating system
	- Linux
---

本文介绍了如何在 Arch Linux 系统中安装虚拟机 VirtualBox. 并利用 VirtualBox 安装 Windows7 系统.
<!--more-->
## 安装 VirtualBox
### 安装基本软件包
在 Arch 中, 使用 Pacman 安装是方便的.
```shell
$ sudo pacman -S virtualbox
```
**注意:** 当让你选择是 "1 virtualbox-host-dkms" 还是 "2 virtualbox-host-modules-arch" 时一定要选择**2**, 否则 VirtualBox 会应为缺少 Linux 内核模块而启动失败.

### 配置 VirtualBox
为使得在系统启动时自动加载 VirtualBox 的相关内核模块, 创建如下文件
```shell
$ sudo nano /etc/modules-load.d/virtualbox.conf
```
并在文件中添加如下内容.
```
vboxdrv
vboxnetadp
vboxnetflt
vboxpci
```
其中 vboxdrv 是必须的模块, vboxnetadp 和 vboxnetflt 是虚拟机使用网络时的附加模块, vboxpci 是虚拟机使用主机 pci 设备时所需要的模块.

然后将当前用户添加到 vbox 用户组中
```shell
$ sudo gpasswd -a $USER vboxusers
```
如果没什么问题的话就可以顺利启动 VirtualBox 了.
在顺便安装一个扩展以便使用 VirtualBox 的一些高级功能, 使用 yaourt 来安装.
```shell
$ yaourt -S virtualbox-ext-oracle
```

## 安装 Windows7
打开 virtualBox 的 GUI 界面
![](http://oayjon2he.bkt.clouddn.com/vboxstart.png) 

点击 New
![](http://oayjon2he.bkt.clouddn.com/vboxinstallwin7-start.png) 
给虚拟机起个名字, 系统选择 Windows 7 (64-bit), 点击 Next.

![](http://oayjon2he.bkt.clouddn.com/vboxinstallwin7-memory.png) 
设定虚拟机的内存, 一般为实体机内存的一半左右否则设置过大会造成卡顿, 点击 Next.

![](http://oayjon2he.bkt.clouddn.com/vboxinstallwin7-harddisk.png) 
设定虚拟机的硬盘大小, 这个设置多大对速度没什么影响, 点击 Next.

然后一路 Next, 直到创建成功.
![](http://oayjon2he.bkt.clouddn.com/vboxinstallwin7-settingstart.png) 
点击 Settings.

![](http://oayjon2he.bkt.clouddn.com/vboxinstallwin7-settingboot.png) 
选择 System, Boot Order 选项中去掉 Floppy 项前面的勾选.

![](http://oayjon2he.bkt.clouddn.com/vboxinstallwin7-settingcpu.png) 
点击 Processer 选板, CPU数量选实体机的**一半以下**, 不要按截图上的做, 后来卡成狗...... 我改成2后流畅许多.

![](http://oayjon2he.bkt.clouddn.com/vboxinstallwin7-3dacceleration.png) 
在左栏中 Display 中, 点击 Screen 选板, 勾选 Enable 3D Acceleration.

下面添加你想安装的 Win7 系统的 iso 文件.
![](http://oayjon2he.bkt.clouddn.com/vboxinstallwin7-isofile.png) 
在 Storage 右侧的 Attributes 中点击那个小光盘, 选择你的iso文件的路径, 点击 OK. 然后点击 Start.

![](http://oayjon2he.bkt.clouddn.com/vboxinstallwin7-startwin7.png) 
虚拟机开机, 出现 Win7 的安装引导, 按照引导一路安装就OK了, 最后安装成功!

![](http://oayjon2he.bkt.clouddn.com/vboxinstallwin7-success.png) 

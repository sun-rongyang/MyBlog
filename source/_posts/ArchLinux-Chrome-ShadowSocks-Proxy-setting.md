---
title: ArchLinux 中如何使用 ShadowSocks 和 Chrome 插件 SwitchOmega 实现网络的流畅访问
date: 2016-09-07 21:54:09
tags: [linux,arch,proxy]
categories:
	- operating system
	- Linux
---
本文介绍了在 Arch Linux 中(当然稍作修改可以运用到其他操作系统中)配置 ShadowSocks 并借助 Chrome 浏览器的 SwitchOmega 插件实现互联网流畅访问的方法. 当然ShadowSocks 还可以给其他软件提供代理, 这就要看具体软件的设置了.
<!--more-->
> 到底是先有柏林墙还是先有对柏林墙的翻越我们不得而知. 但值得注意的事实是: 只要柏林墙还存在着, 那么对它的翻越就从未停止.

## ShadowSocks 的安装与配置
### 安装命令行版的 ShadowSocks
安装的过程很简单, 直接 pacman 安装即可.
```shell
$ sudo pacman -S shadowsocks
```
### ShadowSocks 配置
将你已经获得的 ShadowSocks 的配置文件命名为 xxx.json 保存在/etc/shadowsocks 文件夹下, 并运行
```shell
$ sslocal -c /etc/shadowsocks/xxx.json &
```
这样 ShadowSocks 就启动了. 一般情况下默认的端口是 localhost:1080, 含义是访问1080端口的数据会自动通过SS服务器进行代理, 这一点是非常重要的, 是后面配置成功的关键. 接下来使用 systemd 的命令将 ShadowSocks 服务转为守护进程并设置开机自动启动.
```shell
$ sudo systemctl start shadowsocks@xxx.service # 转为守护进程
$ sudo systemctl enable shadowsocks@xxx.service # 设置此进程开机启动
```

## Chromium 浏览器及 SwitchOmega 插件的安装与配置
### Chromium & SwitchOmega 的安装
Chromium 浏览器是 Chrome 浏览器的开发版本, 对于一般的使用来说没有什么区别. arch 中可以直接 pacman 安装.
```shell
$ sudo pacman -S chromium
```
SwitchOmega 插件直接去 [GitHub](https://github.com/FelisCatus/SwitchyOmega/releases) 下载最新的 crx 文件进行离线安装. 安装完成后打开插件的设置页面如下图所示
![ ](http://oayjon2he.bkt.clouddn.com/switchomega1.png  "switchOmega 设置页面")
### SwitchOmega 配置
上图中左面的 Import/Export 选项可以用来导出配置文件进行备份也可以导入现成的配置文件. PROFILES 栏中列出了所有设置的代理规则, 我们下面从头开始设置.

首先点击"**+ New profile**", 输入 Profile name, 选择 Proxy Profile, 点击 Create.
![ ](http://oayjon2he.bkt.clouddn.com/newproxyprofile.png "创建 Proxy Profile")
在 Proxy servers 栏下, Protocol 选择 SOCKS5, Sever 填 127.0.0.1, Port 填 1080. 这个1080端口正是 ShadowSocks 监听的端口.
![ ](http://oayjon2he.bkt.clouddn.com/switchomega2.png "Proxy servers 配置")
再次点击"**+ New profile**" 选择 SwitchProfile, 点击Create.
![ ](http://oayjon2he.bkt.clouddn.com/settingswitchprofile1.png  "set switchprofile 1")
在 Import online rule lists 栏下点击 Add a rule list
![ ](http://oayjon2he.bkt.clouddn.com/onlinerulelists1.png  "online rule lists1")
Rule List Fromat 选择 AuroProxy, Rule List URL 填 https://raw.githubusercontent.com/gfwlist/gfwlist/master/gfwlist.txt 然后 Download Profile Now. 最后在 Switch rules 栏中 Rule list rules 行中 Profile 选择你前面设置成功的那个 ShadowSocks 代理. 在左栏中点击 Apply changes 就一切搞定了. 这样列表中的网站就走 ShadowSocks 了, 不在列表中的网站则不会, 这样大大加快了网络的访问速度. 当然在某个网站访问不流畅时可以点击插件图标手动添加到代理列表中.
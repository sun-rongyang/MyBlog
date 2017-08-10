---
title: Hexo 个人博客搭建完全教程:(1) 服务器购买与 http 服务配置
date: 2017-02-06 17:18:16
tags: [blog,hexo,linux,nginx,server]
categories:
    - development
    - blog
---

自己建博客也有半年时间了. 开始的时候采用 Hexo+GitHub page 模式. 直到去年11月的时候我的 GitHub page 突然挂了, 弄了好半天也解决不了. 我也不是计算机专业的, 也没那个多精力放在这上面,所以索性扔下不管. 过年的时候在家没什么事情做, 想想写点博客与大家分享也是不错的事情, 于是决心不依赖 GitHub 自己建一个独立的博客网站. 这篇教程适用于稍微懂些 Linux, 想用 Hexo 搭建自己的博客的非专业的同学. 我将从域名以及服务器的购买, http 服务的搭建, Hexo 的安装与配置, NexT 主题的配置, https 所需 ssl 证书的申请等等. 你在使用这篇教程的时候遇到什么问题或发现什么错误直接留言或给我发邮件都行. 本文是该系列文章的第一篇, 主要讲解服务器购买与 http 服务的搭建.
<!-- more -->

## 服务器购买

### 在 DigitalOcean 上购买服务器
如果你想把服务器部署在国外的话推荐使用 [DigitalOcean **这个链接是我的分享链接, 通过这个购买能抵10刀哦,请放心使用:)**](http://www.digitalocean.com/?refcode=a06d383589f7). DigitalOcean 的好处在于部署非常迅速(网站上号称55秒就可以部署自己的实例).并且可以选择的服务器位置很多, 包括美国, 荷兰, 新加坡, 英国, 德国, 加拿大, 印度. 价格的话也算合理, 我买的最便宜的配置(1CPU/512M/20G/1000G transfor)是每月5刀. 缺点是在国内访问可能不太稳定, 而且如果你的域名没有在国内备案的话可能访问不了(这点我还不太确定).

DigitalOcean 的部署方法非常简单.

- 在[官网](http://www.digitalocean.com/?refcode=a06d383589f7)上注册个账号.
- 点击 Create Droplet 创建新实例.
- 选择你想要的配置, 系统, 地区等等.
- 最后一步 Add your SSH keys 不用配置, 这样 DigitalOcean 会把 root 密码通过邮件发给你.
- 付款的时候可以用信用卡或 PayPal. 如果你没有信用卡的话推荐使用 PayPal, PayPal 现在很方便, 可以直接添加银联的借记卡, 直接实时汇率付美元.

这是[官网](http://www.digitalocean.com/?refcode=a06d383589f7)上的一个指导动图, 非常形象.
![DigitalOcean 部署](http://oayjon2he.bkt.clouddn.com/digitalocean-create.gif)

### 在阿里云上购买服务器
如果你想把网站部署在国内的话推荐使用[阿里云](www.aliyun.com). 阿里云的好处在于国内访问的速度非常快, 而且如果你是高校的学生可以享受教育优惠,每月9.9元, 配置和上面说到的 DigitalOcean 的最便宜的实例差不多. 坏处在于如果不备案的话, 网站不一定能用, 反正我在没加 https 的时候是不行的. 阿里云的部署方法更简单, 直接在官网注册购买就好, 都是中文的网站. 付款的话直接支付宝.

不管在哪里买的, 我们最后只要确定两个东西在手就行, 外网 ip 地址和 root 密码.

## http 服务的配置

有了服务器后, 我们需要使我们的服务器可以在互联网上通过 http 服务被访问. 服务器就是在远处摆着的一个24小时开机的电脑. 网站就是那个电脑上的某些文件, 你可以通过自己的浏览器查看它们. 当你在浏览器中输入一个网址时你的浏览器会向对应的那个远处的电脑发送一个 http 请求, 如果不出什么问题远处的服务器会返回你所请求的对应的文件, 这些文件就组成了你看到的网站界面. 我们需要对服务器进行配置来使其能够处理这些请求. 我用的服务器系统为 Centos7 http 服务用 Nginx 来实现.

### 一些准备工作
对于 Centos7 作为服务器来说, 这里有一篇不错的[新手指南](https://www.digitalocean.com/community/tutorials/initial-server-setup-with-centos-7).

我们用 root 账号登录我们的服务器.
```
$ ssh root@ip-address
```

毕竟 root 账号不能随便玩, 接下来创建一个普通账号并设置一个密码.
```
adduser sunry
passwd sunry
```

把这个账号赋予 root 权限. 在 root 下执行
```
visudo
```
将打开的文件修改如下:
找到    root     ALL=(ALL)      ALL       一行
在该行下方，以相同的格式添加新的一行：
sunry ALL=(ALL)      ALL
最后用 vi 的命令保存退出即可. 再用 sunry 用户登录时 sudo 就可以使用了.

退出 root 账号, 用新建的普通账号再次登录你的服务器.

### 安装 Nginx
添加epel软件仓库.
```
$ sudo yum install epel-release
```

yum 安装 Nginx.
```
$ sudo yum install nginx
```

启动 Nginx 服务.
```
$ sudo systemctl start nginx.service
```

这时在浏览器中输入 http://your-ip-address 如果可以看到 Nginx 的欢迎页面说明你的 Nginx 安装成功并正常启动.

### Nginx 的简单配置
首先说几个 Nginx 的常用命令.
```
$ sudo systemctl enable nginx.service # start nginx service wite the server start

$ sudo systemctl restart nginx.service # restart the nginx service

$ sudo systemctl stop nginx.service # stop the nginx service

$ sudo systemctl status nginx.service # see the status of the nginx service

$ sudo nginx -t # check the syntax of the nginx configuration file

$ sudo nginx -s reload # reload the configuration file

$ nginx -v # show the version of the nginx
```

接下来我们看一下 Nginx 的配置文件夹 /etc/nginx 里都有什么文件
```
.
├── conf.d/
├── default.d/
├── fastcgi.conf
├── fastcgi.conf.default
├── fastcgi_params
├── fastcgi_params.default
├── koi-utf
├── koi-win
├── mime.types
├── mime.types.default
├── nginx.conf
├── nginx.conf.default
├── scgi_params
├── scgi_params.default
├── ssl
├── uwsgi_params
├── uwsgi_params.default
└── win-utf
```
其中 nginx.conf 是 Nginx 的默认配置文件, 所有对 Nginx 的配置本质上都是修改这个文件. conf.d/ 目录下放着 nginx.conf 默认 include 的 server 级别的配置文件段. default.d/ 目录下放着 nginx.conf 默认 include 的 location 级别的配置文件段.

Nginx 默认的网站根目录(所谓根目录就是你的网站的首页的 index.html 之类的文件所在的那个文件夹)是 /usr/share/nginx/html, 该文件夹下的文件有
```
├── 404.html
├── 50x.html
├── index.html
├── nginx-logo.png
└── poweredby.png
```
发现那个 index.html 了吧. 我们为了文件管理的方便性一般将网站放到自己指定的文件夹中. 首先创建一个文件夹.
```
mkdir -p /home/www/your-domain-name
```
然后在这个目录下新建一个 index.html 作为测试.
```
echo "test page" >> /home/www/your-domain-name/index.html
```
接下来我们修改 nginx.conf, 首先看下这个文件中 server 那部分内容, 也是我们要修改的内容.
```
$ sudo nginx.conf

......
server {
        listen       80 default_server;
        listen       [::]:80 default_server;
        server_name  your-domain-name;
        root    /home/www/your-domain-name;
        # Load configuration files for the default server block.
        include /etc/nginx/default.d/*.conf;

       # location / {
       # }

       # error_page 404 /404.html;
       #     location = /40x.html {
       # }

       # error_page 500 502 503 504 /50x.html;
       #     location = /50x.html {
       # }
    }
......
```
其中 server_name 是主机名, 填入你购买的域名, 如何购买域名我会在后面的教程中谈到. root 是根目录的位置, 改成 /home/www/your-domain-name.

在 /etc/nginx/default.d/ 下新建文件 your-domain-name.conf 并填入如下内容.
```
location / {
            index  index.html index.htm;
        }
# 规定了index 文件的格式
        error_page  404              /404.html;

        # redirect server error pages to the static page /50x.html
        #
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html; # 设定 50x.html 的root 文件夹仍是 /usr/share/nginx/html/
        }
```

这时在浏览器中输入 http://your-domain-name 返回 test page 即设置成功.

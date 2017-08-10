---
title: Hexo 个人博客搭建完全教程:(2) 域名购买与 ssl 证书申请
date: 2017-02-08 14:25:03
tags: [blog,hexo,linux,nginx,server]
categories:
    - development
    - blog
---

本文承接上一篇文章继续介绍个人博客的搭建过程, 主要讲解域名购买以及如何让自己的网站开启 https 服务即为自己的网站申请 ssl 证书.
<!-- more -->

## 域名购买

### 在 GoDaddy(狗爹) 上购买域名
如果你不想为自己这个小小的博客也弄什么麻烦的域名备案的话, **不要在国内的域名服务商那里购买域名x3**, 重要的事情说三遍!!! 在全球最大的域名服务商 [GoDaddy](https://sso.godaddy.com/?regionsite=sg&countryview=1&marketid=zh-SG) 购买域名是非常方便的. GoDaddy 有中文主页也有大陆客服, 而且竟然可以直接用支付宝付款!
![GoDaddy 主页](http://oayjon2he.bkt.clouddn.com/godaddy1.png)

- 在官网上注册个账号.
- 在搜索框中搜索你想要的域名.
- 选中你想要的域名, 进入购物车.
- 你会看到提示是否使用¥60.00 /域名/年的域名保护盾. 这个的意思是说用了这个以后你注册域名时的相关个人信息会被隐藏. 如果你自己觉得注册的域名非常珍贵建议用真的个人信息, 如果你只是随便买个玩的话, 个人信息随便填填就好, 因此也没必要使用那个保护盾.
- 下一步选择注册的年限, 选好后直接用支付宝付款即可. 如果你是第一次用狗爹购买域名会提示输入些个人信息.

### 配置 DNS 服务器

登录狗爹主页, 点击右上角用户名, 在快速链接中选择管理域名. 点击要管理域名的右边的小三角, 选择管理DNS, 进入DNS设置界面.
![狗爹-域名管理](http://oayjon2he.bkt.clouddn.com/godaddy2.png)

选择添加记录, 记录类型为 A(主机), 主机一栏填 @, 指向填自己服务器的外网ip, 最后在点完成就可以了. 等着全球 DNS 服务器的递归刷新, 大概一两个小时就能用了. 在浏览器中输入 http://your-domian-name 就可以访问到前文所添加的 test page 了.顺便再说一句(没有完全确定), 这时在大陆访问可能也会返回"您暂时无法访问该网站 blabla"的网页, 如下
![没有备案会返回的](http://oayjon2he.bkt.clouddn.com/beian1.jpg)
这意味着我们和工信部的斗智斗勇还没有结束, 下面才是最关键的一步, **为你的博客开启 https 服务**. 这样除了
~~不可抗拒力~~造成的不能访问其他情况都不会受影响. 而且还可以在浏览器地址栏最左边开启一个绿色的小锁标志, 看起来很安全的样子:)

## ssl 证书申请(开启 https 服务)

在具体操作的过程中主要是参考这篇很棒的文章 [Setting Up HTTPS with Let’s Encrypt SSL Certificate For Nginx on RHEL/CentOS 7/6](http://www.tecmint.com/setup-https-with-lets-encrypt-ssl-certificate-for-nginx-on-centos/). 想要开启 https 服务(也就是加密链接)必不可少的就是获取加密用的 ssl 证书. 对于我们这种个人博客来讲取得一个最低级的 DV 证书就可以了. 对于 ssl 证书下面一段介绍的很清楚.

>SSL证书需要向国际公认的证书证书认证机构（简称CA，Certificate Authority）申请。CA机构颁发的证书有3种类型：
域名型SSL证书（DV SSL）：信任等级普通，只需验证网站的真实性便可颁发证书保护网站, 地址栏只有一个绿色小锁:
企业型SSL证书（OV SSL）：信任等级强，须要验证企业的身份，审核严格，安全性更高；
增强型SSL证书（EV SSL）：信任等级最高，一般用于银行证券等金融机构，审核严格，安全性最高，同时可以激活绿色网址栏。

下面我们要看到的 [Let's Encrypt](https://letsencrypt.org/) 就是一个免费颁发 DV 证书的 CA. ssl 证书是如何工作的呢? 请看下面这张图.![ssl 工作原理](http://oayjon2he.bkt.clouddn.com/nginx-letsencrypt.png)

### 安装 Let's Encrypt 软件
首先在系统里安装 git:
```bash
$ sudo yum install git
```

然后就可以从 GitHub 上 clone Let's Encrypt 的软件了:
```bash
$ su # change to root account
# cd /opt
# git clone https://github.com/letsencrypt/letsencrypt
```
### 申请一个免费 Let's Encrypt ssl 证书
letsencrypt 的工作原理是这个软件通过系统网络的80端口和 Let's Encrypt 的服务器通信, 从而证明网站所在的服务器的真实性. 一般情况下来讲, 80端口默认给 http 服务的, 因此被 Nginx 所占用. 所以首先我们需要停止 Nginx 服务:
```bash
# service nginx stop
# systemctl stop nginx
```
然后在用 ss 命令确定80端口没有被占用:
```bash
# ss -tln
```
下面我们就可以申请证书了:
```bash
# cd /opt
# ./letsencrypt-auto certonly --standalone -d your-domain-name -d www.your-domain-name
```
**注意: 这里一定要填两个域名, 一个是不带 www 的, 一个是带 www 的.** 然后会提示你输入一个有效邮箱, 最后 agree 一下就搞定了. 如果一切顺利就会返回一个 "Congratulations! blabla..." 的一个东西, 那么恭喜你, 成功了!

所有的 ssl 证书都以一个域名一个文件夹的形式放在 /etc/letsencrypt/live/ 中. 去看看那里面到底有什么吧.
```bash
# ls /etc/letsencrypt/live/
# ls /etc/letsencrypt/live/your-domain-name/
```

### 在 Nginx 中开启 https 服务
有了 ssl 证书我们就可以为我们的网站开启 https 了. 想开启 https 当然还是要靠配置 Nginx 啦.

#### 配置 Nginx
首先在 /etc/nginx/ssl/ 目录下生成一个新的 Diffie-Hellman 密码, 这个密码可以在遭受 Logjam 攻击时保护你的服务器:
```bash
# mkdir /etc/nginx/ssl
# cd /etc/nginx/ssl
# openssl dhparam -out dhparams.pem 4096
```
4096 bit 的 key 需要一些时间, 密码生成好后我们就可以配置 nginx.conf 文件了. 在 /etc/nginx/nginx.conf 中如下位置
```nginx
server {
	listen		80 default_server;
    listen		[::]:80 default_server;
    # add ssl configuration at here
}
```
添加下面的配置语句
```nginx
# SSL configuration
        listen 443 ssl default_server;
        ssl_certificate /etc/letsencrypt/live/your-domain-name/fullchain.pem;
        ssl_certificate_key /etc/letsencrypt/live/your-domain-name/privkey.pem;
        ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
        ssl_prefer_server_ciphers on;
        ssl_ciphers 'EECDH+AESGCM:EDH+AESGCM:AES256+EECDH:AES256+EDH';
        ssl_dhparam /etc/nginx/ssl/dhparams.pem;
        ssl_session_timeout 30m;
        ssl_session_cache shared:SSL:10m;
        ssl_buffer_size 8k;
        add_header Strict-Transport-Security max-age=31536000;
```

现在重启 Nginx 服务:
```bash
# systemctl start nginx
# service nginx restart
```
在浏览器中访问你的域名, 绿色的小锁就会出现了. 有一个专业分析网站 ssl 的网站 ssllabs.com 可以用来测试一下自己的网站, 在浏览器中输入:
```
https://www.ssllabs.com/ssltest/analyze.html?d=your-domain-name
```
它会自动分析你的网站的安全性, 然后返回类似如下结果.
![ssllabs 结果](http://oayjon2he.bkt.clouddn.com/ssllabs-result.png)

#### 访问 http 自动跳转 https
在上述配置完成后你会发现如果用 http://your-domain-name 的形式访问你的网站也会返回不加密的结果, 并且有可能被拦截. 为防止吃瓜群众用 http 访问时出现问题, 我们将网站设置成如果有 http 的访问请求自动跳转成 https. 在你刚才配置过的 nginx.conf 文件中接着上面的配置语句加入如下语句:
```nginx
        if ($scheme != https) {
        rewrite ^/(.*) https://$server_name/$1 permanent;

```
重新加载一下配置文件
```bash
$ sudo nginx -s reload
```
然后去浏览器中测试一下.

### 自动跟新 ssl 证书
在 Let's Encrypt 上申请的免费 ssl 证书只有90天的有效期过期要重新申请. 我们可以借助 Python 的 webroot 模块实现不停 web 服务自动更新 ssl 证书, 执行如下命令:
```bash
# cd /opt/letsencrypt
# ./letsencrypt-auto certonly -a webroot --agree-tos --renew-by-default --webroot-path=$YOUR-WEB-SERVICE-ROOT-FOLDER -d your-domain-name -d www.your-domain-name
# systemctl reload nginx
```
其中 webroot-path 填你在 Nginx 配置文件中配置的 root 目录.

要想使更新自动执行我们需要一个 bash 脚本. 在 /usr/local/bin/ 目录下新建 cert-renew 文件:
```bash
# vi /usr/local/bin/cert-renew
```
并填入如下 bash script:
```bash
#!/bin/bash
webpath='$YOUR-WEB-SERVICE-ROOT-FOLDER'
domain=$1
le_path='/opt/letsencrypt'
le_conf='/etc/letsencrypt'
exp_limit=30;
get_domain_list(){
certdomain=$1
config_file="$le_conf/renewal/$certdomain.conf"
if [ ! -f $config_file ] ; then
echo "[ERROR] The config file for the certificate $certdomain was not found."
exit 1;
fi
domains=$(grep --only-matching --perl-regex "(?<=domains \= ).*" "${config_file}")
last_char=$(echo "${domains}" | awk '{print substr($0,length,1)}')
if [ "${last_char}" = "," ]; then
domains=$(echo "${domains}" |awk '{print substr($0, 1, length-1)}')
fi
echo $domains;
}
if [ -z "$domain" ] ; then
echo "[ERROR] you must provide the domain name for the certificate renewal."
exit 1;
fi
cert_file="/etc/letsencrypt/live/$domain/fullchain.pem"
if [ ! -f $cert_file ]; then
echo "[ERROR] certificate file not found for domain $domain."
exit 1;
fi
exp=$(date -d "`openssl x509 -in $cert_file -text -noout|grep "Not After"|cut -c 25-`" +%s)
datenow=$(date -d "now" +%s)
days_exp=$(echo \( $exp - $datenow \) / 86400 |bc)
echo "Checking expiration date for $domain..."
if [ "$days_exp" -gt "$exp_limit" ] ; then
echo "The certificate is up to date, no need for renewal ($days_exp days left)."
exit 0;
else
echo "The certificate for $domain is about to expire soon. Starting renewal request..."
domain_list=$( get_domain_list $domain )
"$le_path"/letsencrypt-auto certonly -a webroot --agree-tos --renew-by-default --webroot-path=”$webpath” --domains "${domain_list}"
echo "Reloading Nginx..."
sudo systemctl reload nginx
echo "Renewal process finished for domain $domain"
exit 0;
fi
```
注意这里 $webpath 变量也要填入你在 Nginx 中配置的 root 目录. 给这个新文件加上可执行权限并给系统安装 bc 计数器:
```bash
# chmod +x /usr/local/bin/cert-renew
# yum install bc
```
测试一下这个新的 bash script 是否设置正确:
```bash
# /usr/local/bin/cert-renew your-domain-name
```
最后将这个脚本加入系统定时任务, 让它每周执行一次
```bash
# crontab -e
```
在打开的文件中填入(调用的是 vi 编辑器)
```bash
@weekly  /usr/local/bin/cert-renew your-domain-name >> /var/log/your-domain-name-renew.log 2>&1
```
至此 ssl 证书的申请和 https 服务的配置终于告一段落, 这也是整个建站过程中最难的部分. 这时个人博客网站搭建的准备工作全部结束, 在下一篇文章中我们的主角 Hexo 将正式出场!

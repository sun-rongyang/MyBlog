---
title: Hexo 个人博客搭建完全教程:(5) 博客的更新,推广与一些补充
date: 2017-03-21 20:25:57
tags: [blog,hexo,git,linux]
categories:
    - development
    - blog
---

今天用了些时间在 Mac 上配置了 HEXO 博客, 索性一鼓作气把拖了很久的这个系列的文章写完. 本文首先介绍了如何配合 Git 实现博客在服务器端的自动更新. 然后介绍了如何让自己的博客被更多的人看见并分析自己博客的访问情况. 最后作为一点补充介绍一下如何在博文中嵌入 PDF, 这一点对写数学或物理理论的博文特别有帮助.
<!--more-->

## 上传你的网站

可以在服务器端建一个 git 仓库并通过 git hook 来自动更新你的网站. 这里有一个地方我没搞出来, 就是这么在 git 上传的时候不需要密码, 配了好半天也没有配出来, 哪位路过的大神菊苣还望指点一下. 其实最笨的方法就是把本地 blog 文件夹下的 public 文件夹中的全部内容拷贝到 /home/www/your-domain-name/ 下, 你的网站就可以正常访问了.

### 使用 scp 命令
在你不想安装任何其他带图形界面的软件时, 你可以使用 scp 命令在命令行中完成复制工作. scp 适合在远程和本地之间复制东西, 你只需要知道 远程系统的 ip 地址和用户名及密码就可以了. 首先在服务器端把之前的网站文件夹删掉
```Shell
# rm -r /home/www/your-domain-name
```
然后在本地的 blog 文件夹下执行:
```bash
$ cp -r public your-domain-name # 先在本地复制一份
$ scp -r your-domain-name REMOTE_USERNAME@REMOTE_IP:/home/www/
```
这样就搞定了, 过程中会提示输入远程服务器的密码. 但这个方法真是特别费力, 只适合测试之用, 我们还是使用 git 比较方便.

### 使用 git 自动跟新博客
#### 在服务器端搭建 git server
我用的服务器系统为 Centos7，直接用 yum 安装即可：
```bash
sudo yum git
```

然后我们切换到 root, 创建一个 git 账户:

```bash
$ adduser git
$ passwd git
```

切换到 git 用户

```shell
$ su git
$ cd ~
```

 然后创建一个 git 裸仓库:

```shell
$ git init --bare blog.git
```

这样一个 git server就搭建好了, 以后我们就向这个裸仓库 post.

#### 使用 ssh-key 配置 git 免密码登录

感谢 [another](https://disqus.com/by/disqus_zv7vrsRMYm/) 在留言中给出的[参考资料](http://yanzai.me/hexo-git-deploy.html), 现在终于配置成功了. 首先在本机的~/.ssh 目录下执行

```shell
ssh-keygen -t rsa
```

这个过程中一路确定就可以了. 这样在 ~/.ssh 下会生成 id_rsa 和 id_rsa.pub 两个文件. 将 id_rsa.pub 这个文件中的内容拷贝到 server 端的 /home/git/.ssh/authorized_keys 中(没有就新建此文件). 并在 git 用户下修改 autorized_keys 文件和 ~/.ssh 目录的权限为:

```shell
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
```

最后在本地测试 ssh 免密码登录.

``` shell
ssh git@REMOTE_IP
```

#### 配置 git hooks 自动更新博客

所谓 git hooks 就是当你完成 git 个某个动作后自动执行的脚本. 我们现在就要编写一个这样的 hooks, 使网站 post 到 blog.git 仓库后,自动 clone 到我们网站的目录下. 首先改变网站的根目录的权限为 git:

```bash
$ chown git:git -R /home/www/your-domain-name
```

然后配置 git hooks, 在刚才创建的 blog.git 目录下执行:

```shell
$ cd /home/git/blog.git/hooks
$ vi post-receive
```

输入如下内容:

```shell
#!/bin/bash
GIT_REPO=/home/git/blog.git #git仓库
TMP_GIT_CLONE=/tmp/hexo
PUBLIC_WWW=/home/www/your-domain-name #网站目录
rm -rf ${TMP_GIT_CLONE}
git clone $GIT_REPO $TMP_GIT_CLONE
rm -rf ${PUBLIC_WWW}/*
cp -rf ${TMP_GIT_CLONE}/* ${PUBLIC_WWW}
```

保存退出后赋予这个文件以执行权限:

```shell
$ chmod +x post-receive
```

这样服务器端就配置结束. 下面在本地的站点配置文件中找到 deploy 项目并做如下修改:

```yaml
deploy: 
  type: git
  repository: git@your-ip-address :/home/git/blog.git
  branch: master
```

最后在博客根目录下执行:

```shell
$ hexo d
```

过程中会让你输入 git 用户的密码. 我没有配置 git 免密码登陆(~~因为没成功😂~~)一切顺利的话在浏览器中访问你网站的域名就会看见网站被更新了. 到此为止网站构建工程以接近尾声, 如果说作为一个网站的建立来说已经结束, 但是建立博客的目的是为了分享与交流, 因此使你的博客让更多的人浏览就变得非常重要了, 下面我们来看看如何推广自己的博客.

## 推广你的博客

### 为网站添加sitemap
sitemap的主要作用是防止搜索引擎在爬你的网站时没有获取完整的网页页面从而影响网站的点击率. HEXO可以通过node.js插件来制作sitemap.
```shell
$ npm install hexo-generator-sitemap --save
```
在站点配置文件中添加
```yml
sitemap:
    path: sitemap.xml
```
作为国内的博客用户, 我们也要就活下百度. 使用node.js的另一个插件来制作适用于百度的sitemap.
```shell
$ npm install hexo-generator-baidu-sitemap --save
```
并在站点配置文件中添加
```yml
baidusitemap:
    path: baidusitemap.xml
```
### 让 Google 搜索收录你的博客
使用[Google站长工具](https://www.google.com/webmasters/) 向Google提交自己的sitemap. 以便Google收录. 在验证网站所有权时, 选择 **HTML Tag** 方式. 这样你会获得一段代码
```html
<meta name="google-site-verification" content="XXXXXXXXXXXXXXXXXXXXXXX" />
```
在站点配置文件中新建google_site_verification字段, 其值就是content的值
```Yaml
google_site_verification: XXXXXXXXXXXXXXXXXXXXXXX
```
这样Google过几天就会收录你的网站了. 百度的站长工具暂时不支持https网站的提交, 所以百度什么时候能收录就看造化了......

### 使用 Google Analysis 分析博客访问量
在 [Google Analysis](https://analytics.google.com/) 上创建一个账号然后添加网站, 并获取跟踪 ID. 在**主题**配置文件中找到 google_analytics 字段并填入刚获得的 ID

```yaml
google_analytics: xxxxxxxxxxxxxxxxxxxxxxx
```

过一段时间在 Google Analysis 上就会统计出你网址被访问的情况. 上面的信息非常多, 可以仔细研究一下.

## 博客文章写作补遗

### 将 PDF 嵌入博客

最近准备开始写一批数学和物理方面的文章, 哈哈, 这些才是我的老本行. 现在这博客看起来像个计算机爱好者的博客. 数学和物理这方面的东西公式非常多, 我先尝试着使用 pandoc 将我用 LaTeX 写的 notes 装换成 markdown 格式的, 无奈公式们太复杂了, 就算 pandoc 这种神器都搞不定, 没办法我只能想办法将pdf嵌到博文中.

当然这在 HEXO 中还是比较好实现的. 在之前的[博客](https://thoroughyoung.info/2017/02/08/hexo-build-blog-complete-guilde-3/)中我介绍了如何给每一篇博文分配各自的资源文件夹. 现在我们就利用这个资源文件夹来嵌入 PDF 文件. 在写博客时只要在文章中嵌入如下代码即可:
```html
<iframe src="{% asset_path YOUR-PDF-FILE-NAME %}" style="width:100%; height:600px;" frameborder="0"></iframe>
```
这样你就可以在博客中嵌入 YOUR-PDF-FILE-NAME 这个 PDF 文件了. 你可以通过调节 width 和 height 这两个参数来调节 PDF 文件显示的大小. 终于可以愉快滴写数理方面的博客了:)

> **结语**: 经过一个月断断续续的写作, 这个 HEXO 构建个人博客的系列文章终于告一段落了. 欢迎大家提出各种问题和建议. 随着博客系统的不断改进, 这个系列的文章在以后还会不断更新的, 也欢迎大家继续关注:)
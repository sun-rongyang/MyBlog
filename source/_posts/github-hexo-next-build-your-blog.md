---
title: GitHub+HEXO 搭建博客
date: 2016-08-03 15:23:39
updated: 2017-02-04
tags: [github,hexo,blog]
categories:
    - development
    - blog
---

GitHub Page 诞生的本来目的是为GitHub上的项目创建项目主页提供方便. 由于其免费, 稳定的特点, 现在越来越多的人选择在其上搭建自己的个人主页. HEXO 是一个快速, 简单, 强大的博客框架同时附带相当多的主题供我们选择.
<!-- more -->

### 在本地搭建HEXO博客框架
HEXO是基于node.js的, 所以首先在本地安装node.js

- 从官网下载node.js源代码, 我下载的是4.4.7
- 本地编译生成(我的系统是Ubuntu16.04)
```bash
$ tar -xzvf node-v4.4.7.tar.gz
$ cd node-v4.4.7
$ ./configure
$ make
$ sudo make install
```
如果不报错的话就算成功. 接下来可安装hexo了
```bash
$ npm install -g hexo
```
然后我们可以初始化一个目录用于存放博客
```bash
$ mkdir blog
$ cd blog
$ hexo init
$ npm install
```
这样一个默认主题的hexo博客就搭建好了. 文件夹中的文件结构如下
```bash
.
├── _config.yml
├── db.json
├── node_modules
├── package.json
├── public
├── scaffolds
├── source
└── themes
```
站点的主要配置文件为 _config.yml 一般站点全局性的配置都在这里完成, 称为站点配置文件. 

### HEXO 配置（基于NexT主题）

主要参考文献:
[官方文档](http://theme-next.iissnan.com/)
[Hexo搭建GitHub博客](http://zhiho.github.io/)

使用不同的主题对于站点配置文件的修改也不同，　这里我使用的是ＮｅｘＴ主题，　因此基于ＮｅｘＴ主题介绍一下ＨＥＸＯ的配置方法．　

#### 安装NexT主题
使用git在github中直接clone是方便的
```bash
$ git clone https://github.com/iissnan/hexo-theme-next themes/next
```
安装之后会在themes/next文件夹下发现另一个_config.yml文件, 这个文件中的内容是直接和NexT主题相关的, 这里称主题配置文件.

#### 使用NexT主题
在站点配置文件中找到 theme: 项, 改成
```
theme: next
```
即可. 这时我们在博客的主文件夹下执行
```bash
$ hexo g #生成博客
$ hexo s --debug #启动本地服务
```
就可以生成博客并在本地启动服务. 在浏览器中输入 http://127.0.0.1:4000 即可浏览效果.

#### 配置NexT主题
#### 基本配置
默认的效果如图

![enter image description here](http://oayjon2he.bkt.clouddn.com/blog-hexo-next-1.png)

通过修改站点配置文件可以修改博客名称, 作者, 描述, 语言等基本信息.
```yaml
# Site
title: Sun Rongyang's blog 
subtitle:
description: How many roads must a man walk down. 
author: Sun Rongyang
language: en  
```
NexT主题提供了三种scheme可选, 在主题配置文件中寻找 scheme: 字段进行配置, 我选择的是 Pisces.

#### 设置菜单
设定菜单内容，对应的字段是 menu.  菜单内容的设置格式是： item name: link. 其中 item name 是一个名称， 这个名称并不直接显示在页面上， 她将用于匹配图标以及翻译. 
```ymal
menu:
  home: /
  categories: /categories #分类页 需要手动创建页面
  tags: /tags #标签页 需要手动创建页面
  archives: /archives #归档页
  #about: /about
  #commonweal: /404.html
```
分类页和标签页需要手动创建页面, 操作如下
```bash
$ hexo new page "categories" 
INFO  Created: blog/source/categories/index.md
```
修改 index.md 文件, 添加 type: 字段
```markdown
---
title: categories
date: 2016-08-03 14:44:04
type: "categories"
---
```
tags页面的创建操作类似. 重新生成博客后在public文件夹下出现了两个新的文件夹 categories 和 tags 
```bash
.
├── 2016
├── archives
├── categories #
├── css
├── images
├── index.html
├── js
├── tags #
└── vendors
```
一般情况下 categories 和 tags 页面并不需要评论, 在上面的 index.md 中加入 comments: false 即可关掉评论.
#### 加入多说评论系统
首先在[多说](http://duoshuo.com/)上注册账号, 然后创建自己的站点. 在站点配置文件中新建 duoshuo_shortname:  字段.
```ymal
duoshuo_shortname: 填入在多说官网创建站点时填写的字段
```
这样博客主题就基本配置完了, 终于可以写一篇自己的博客了:) 在后面的博文中我会进一步介绍NexT主题的进阶配置.

### HEXO编写博客
在博客的主文件夹下执行
```bash
$ hexo new "github-hexo-next-build-your-blog"
```
这样就创建了一篇名为"github-hexo-next-build-your-blog"的博客(这个名字是可以在markdown文件中修改的, 上面命名的原则是能让人和真正的名字对应起来, 因为这篇文章对应的url中会包含这个名字). 刷新浏览器可以在4000端口事实看到这篇文章. 生成的markdown文件头为
```markdown
---
title: GitHub+HEXO 搭建博客 #文章题目
date: 2016-08-03 15:23:39 #日期
tags: [github,hexo,build blog] #标签, 注意多标签的格式
categories: development #文章分类

---
# 正文用markdown语言书写
```

### 部署到GitHub
通过上面的配置我们终于可以在本地浏览自己的博客了, 但是要想让别人看见就要发布到 github page 上, 首先为 hexo 安装git插件
```bash
$ npm install hexo-deployer-git --save
```
在站点配置文件中填写
```yaml
# Deployment
## Docs: https://hexo.io/docs/deployment.html
deploy:
  type: git
  repository: 你的 github page 项目的url
  branch: master
```
这样就可以deploy了
```bash
$ hexo g
$ hexo d #会让你填github的用户名和密码
```
### 为文章添加资源文件夹
最近准备开始写一批数学和物理方面的文章, 哈哈, 这些才是我的老本行. 现在这博客看起来像个计算机爱好者的博客. 数学和物理这方面的东西公式非常多, 我先尝试着使用 pandoc 将我用 LaTeX 写的 notes 装换成 markdown 格式的, 无奈公式们太复杂了, 就算 pandoc 这种神器都搞不定, 没办法我只能想办法将pdf嵌到博文中.

**由于最近将 GitHub 中的博客迁移到自己的服务器上, 本文不再更新. 我将重新写一系列关于 Hexo 搭建博客的文章, 敬请关注.**
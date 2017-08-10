---
title: Hexo 个人博客搭建完全教程:(3) Hexo 博客系统与 NexT 主题的安装与配置
date: 2017-02-08 16:59:01
tags: [blog,hexo,linux]
categories:
    - development
    - blog
---

本文是该系列文章的第三篇,承接上一篇文章继续介绍个人博客的搭建过程. 主要讲解 Hexo 博客系统的安装与配置以及好用的 Hexo 主题 NexT 的安装与配置. 按照这篇文章操作下来可以得到一个功能完整的个人博客.
<!-- more -->

## Hexo 的安装与配置

### 在本地搭建HEXO博客框架
HEXO是基于 node.js 的, 所以首先在本地安装 node.js. 在 Arch Linux 系统中安装 node.js 是非常简单的:
```bash
$ sudo pacman -S nodejs
```
如果不报错的话就算成功. 接下来可安装 hexo 了:
```bash
$ npm install -g hexo
```
然后我们可以初始化一个目录用于存放博客:
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

### 配置 Hexo
所谓配置 Hexo 就是修改 站点配置文件.

#### 最基本配置
通过修改站点配置文件可以修改博客名称, 作者, 描述, 语言等基本信息.
```js
# Site
title: Sun Rongyang's blog
subtitle:
description: How many roads must a man walk down.
author: Sun Rongyang
language: en
```
这样就完成了 Hexo 最基本的配置. 其实对于博客的配置大部分时候都需要基于对应的主题, 大多数配置都是在修改主题的配置文件. 在下一节中我们会全面介绍一种好用的 Hexo 主题 NexT 的配置办法.

#### 开启资源文件夹
[官方教程中对资源文件夹的介绍](https://hexo.io/zh-cn/docs/asset-folders.html)已经很详细了, 我在这里简单介绍下. 资源文件夹对于每一篇博文需要引用的图片, pdf文件比较多时是很有用的. 在站点配置文件中找到 post_asset_folder 字段并开启即可. 这样每次新建一篇 markdown 博文都会生成一个相应的资源文件夹. 把这篇博文中要用到的文件统统放到这个文件夹中. 这样在这篇文章中引用某个文件时就可以使用相对路径了. 但是需要注意的是要使用 html 标签才行. 比如你把一个 example.jpg 放到了资源文件夹中, 想在文章中引用它应使用如下语句:
```markdown
{% asset_img example.jpg This is an example image %}
```

### 写博客
现在终于可以开始写一篇博客了:) 在博客的主文件夹下执行
```bash
$ hexo new "github-hexo-next-build-your-blog"
```
这样就在$BLOG/source/_post 目录下创建了一篇名为"github-hexo-next-build-your-blog"的博客(这个名字是可以在markdown文件中修改的, 上面命名的原则是能让人和真正的名字对应起来, 因为这篇文章对应的url中会包含这个名字). 创建的文件名为 github-hexo-next-build-your-blog.md. 如果你开启了资源文件夹, 还会同时生成目录 github-hexo-next-build-your-blog/. .md 文件中自动填写入文件头:
```markdown
---
title: GitHub+HEXO 搭建博客 #文章题目
date: 2016-08-03 15:23:39 #日期
tags: [github,hexo,build blog] #标签, 注意多标签的格式
categories: development #文章分类

---
# 正文用markdown语言书写
```
其中 tags 和 categories 字段我们下面在 NexT 主题的配置中再详述.

我们可以在本地预览一下自己的博客了, 在博客的主文件夹下执行
```bash
$ hexo s --debug
```
就可以生成博客并在本地启动服务. 在浏览器中输入 http://localhost:4000 即可浏览效果. 生成博客的命令也非常简单
```bash
$ hexo g
```
你的整个博客网站就生成好了, 保存在博客主文件夹下的 public 文件夹中. 你只需要把这个文件夹里的所有内容拷贝到到你服务器的 Nginx 配置的 root 目录下, 你的网站就可以访问了.

## NexT 主题的配置

### 安装 NexT 主题
使用git在github中直接clone是方便的
```bash
$ git clone https://github.com/iissnan/hexo-theme-next themes/next
```
安装之后会在themes/next文件夹下发现另一个_config.yml文件, 这个文件中的内容是直接和NexT主题相关的, 这里称主题配置文件.

### 配置 NexT 主题

#### 使用 NexT 主题
在站点配置文件中找到 theme: 项, 改成

```js
theme: next
```
默认的效果如图所示.
![enter image description here](http://oayjon2he.bkt.clouddn.com/blog-hexo-next-1.png)

#### 设置菜单
在主题配置文件中设定菜单内容，对应的字段是 menu.  菜单内容的设置格式是： item name: link. 其中 item name 是一个名称， 这个名称并不直接显示在页面上， 它将用于匹配图标以及翻译.
```js
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

#### 使用DISQUS

在 [DISQUS](https://disqus.com) 网站上注册并添加你的网站. 在 NexT 版本低于5.1.1时在主题配置文件中 "disqus_shortname" 字段填写你设置的站点短名称. 对于最新的 5.1.1版来说, 找到 disqus 字段, 并修改如下:

```yaml
disqus:
  enable: true
  shortname: your_disqus_shortname
  count: true
```

#### 头像设置
将想要做头像的图片改名为 avatar.extension 放到 themes/next/source/images/ 中并在主题配置文件中找到 avatar 字段并填写如下内容即可.
```js
avatar: /images/avatar.extension
```

#### 侧边栏社交链接与邮件链接
人们在看你的博客时可以顺便去你的其他社交网络上逛逛或者发封邮件给你, 在主题配置文件中找到 social: 字段, 添加社交网络中你自己主页的地址
```js
social:
#LinkLabel: Link
	Email: mailto:sun-rongyang@outlook.com
	GitHub: https://github.com/sun-rongyang
```
然后在下面的 social_icons: 字段中添加相应社交网络的图标.
```js
# Social Links Icons
# Icon Mapping:
#   Map a menu item to a specific FontAwesome icon name.
#   Key is the name of the item and value is the name of FontAwsome icon. Key is case-senstive.
#   When an globe mask icon presenting up means that the item has no mapping icon.
social_icons:
  enable: true
  # Icon Mappings.
  # KeyMapsToSocalItemKey: NameOfTheIconFromFontAwesome
  GitHub: github
  Twitter: twitter
  Weibo: weibo
  Email: envelope
```
可以注意到其中 Email 字段是我添加的. 你可以在 [Font Awsome](http://fontawesome.io/) 上自己搜索想要的图标, 把名字记下来填上即可. 注意这里的一切都是大小写敏感的.

#### 站点建立时间
show一下你玩博客已经几年了. 在主题配置文件中添加since字段
```js
# 站点建立时间
since: 2016
```

#### 代码高亮模式
在主题配置文件中找到 highlight_theme 字段, 可选项有 normal, night, night eighties, night blue, night bright. 选择一种自己喜欢的 theme 即可.

#### 求打赏
如果你的文章写的不错, 很有价值, 那么求各位看官捧个钱场吧^^ 在主题配置文件中新建 reward_comment 字段, 支持支付宝和微信二维码.
```js
reward_comment: 坚持原创技术分享，您的支持将鼓励我继续创作！
wechatpay: /path/to/wechat-reward-image
alipay: /path/to/alipay-reward-image
```

#### 使用MathJax
运用latex强大的公式语言, 写理工科技术博客必备啊. 启用方法很简单, 在主题配置文件中找到 mathjax 字段, 打开即可. 注意其中的 per_page 字段一定要选择 false. 在markdown文档中, 一个\$为插入行内公式: $E=mc^{2}$, 两个\$\$为插入行间公式: 
$$
E=mc^{2}
$$

#### 设置阅读全文功能
在首页中会显示最近post的文章, 如果文章过长的话会使首页显得很难看,  因此可以打开主题配置文件中的 auto_excerpt 字段
```js
auto_excerpt:
  enable: true
  length: 150 #设置预览的文字量
```

#### 在侧边栏添加 搜索框
为自己的博客添加一个本地搜索框, 这样在查找博客中的内容时就非常方便了. 在博客的根目录下执行:
```bash
$ npm install hexo-generator-search --save
```
并在站点配置文件中添加如下字段:
```yaml
search:
  path: search.xml
  field: post
  format: html
  limit: 10000
```

在主题配置文件中添加:

```yaml
# Local search
local_search:
  enable: true
```

#### 文章分享插件

看完一篇文章后觉得不错, 想分享到社交媒体上, 文末的分享插件可以很方便地实现这个功能. 这里使用的是[addthis](https://www.addthis.com)提供的分享插件, 因为它是唯一一个支持 HTTPS 的分享插件. 在主题配置文件中找到`#Share`下的 `#add_this_id`取消注释, 并填入在 addthis 网站上你新创建的某个分享的 profile 的 id 即可.

#### 统计每篇文章的访问量并显示在标题下方
NexT主题使用LeanCloud实现这一功能, 详见 [为NexT主题添加文章阅读量统计功能](https://notes.wanghao.work/2015-10-21-%E4%B8%BANexT%E4%B8%BB%E9%A2%98%E6%B7%BB%E5%8A%A0%E6%96%87%E7%AB%A0%E9%98%85%E8%AF%BB%E9%87%8F%E7%BB%9F%E8%AE%A1%E5%8A%9F%E8%83%BD.html#%E9%85%8D%E7%BD%AELeanCloud) 一文, 这一功能就是该博主添加到NexT主题中的!

#### 显示每一篇文章的更新时间

文章 post 出去以后如有修改会在文章标题下显示最后修改时间, 详见[个人博客搭建hexo+github(7):显示每篇文章的更新时间](http://wuchenxu.com/2015/12/13/Static-Blog-hexo-github-7-display-updated-date/)这篇文章.

#### 添加 About 界面

虽然说喝牛奶不用知道是哪头牛产的, 穿在身上的羊毛衫也不用知道是哪只羊贡献的羊毛, 博客作为你向世界开启的小小窗口, 介绍一下也是不错的. 在博客主文件夹下执行:
```bash
$ hexo new page "about"
```
然后在主题配置文件中将 menu 字段的下面的 about 前的注释去掉即可. 你会发现博客主文件夹中的 source 文件夹下多了一个 about 文件夹, 编辑其中的 index.md 的文件头如下:
```js
---
title: 
date: 2017-01-31 17:32:01
comments: false
---
```
然后在这个文件中用 markdown 语法书写. 生成博客后你就会看见 about 页面了.

经过这样一番配置我们的 Hexo 博客终于算是配置好了. 下一篇文章中我将介绍如何将自己的博客上传到自己的服务器上并让搜索引擎发现你的博客(推广).

[TOC]
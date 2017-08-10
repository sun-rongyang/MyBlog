---
title: Hexo 个人博客搭建完全教程:(4) Mac 上如何使用 HEXO+NexT 写作博客
date: 2017-03-21 17:15:43
tags: [blog,hexo,Mac,OSX]
categories:
    - development
    - blog
---

最近真是太忙了, 导致这个系列的文章更新进展缓慢. 本文是这个系列的第四篇文章. 本打算要写如何通过 Git 来上传自己的博客, 但是最近有人送博主一个 MacBook(Excited!), 我就插播一篇在 Mac 上如何使用 HEXO 来写博客的文章吧.好在 OSX 和 Linux 是差不多的系统, 迁移过来并不太费力气.

<!--more-->

### 安装环境配置

首先安装 Homebrew:

```shell
$ /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

然后安装 Git:

```shell
$ brew install git
```

 再安装 node.js:

```shell
$ brew install node
```

这样 HEXO 的安装环境就配置好了, 接下来我们来安装 HEXO.

### HEXO 安装

直接 npm 安装即可:

```shell
$ sudo npm install -g hexo
```

接下来就可按照我前面写关于本系列的博文来搭建自己的 HEXO 博客了.

### 文件迁移的注意事项

我把之前 Linux 中的博客根目录文件夹整个拷贝到 Mac 上(其实是用 Dropbox 同步的:)~ 只要把以下文件进行替换即可:

- 站点配置文件
- 主题配置文件
- 头像文件(替换themes/next/source/images 中的对应文件)
- categories, tags 还有 about 中的 index.md 文件

大功告成啦!

在本系列的下一篇文章中我继续介绍搭建博客网站的主线: 使用 Git 上传自己的网站到服务器, 敬请期待:)
---
title: HEXO主题NexT进阶配置(二)
date: 2016-08-19 17:27:12
tags: [hexo,next]
categories:
    - development
    - blog
---

这篇文章教你如何在HEXO博客中添加本地搜索功能, 显示每篇文章的访问量(装逼利器, 哈哈哈), 在文章末尾添加分享文章到主流社交媒体的功能, 最后介绍最重要的, 推广你的博客, 让你的博客被搜索引擎收录.
<!-- more -->
### 在侧边栏添加 搜索框
为自己的博客添加一个本地搜索框, 这样在查找博客中的内容时就非常方便了. 在博客的根目录下执行
```shell
npm install hexo-generator-search --save
```
并在站点配置文件中添加如下字段
```yml
search:
  path: search.xml
  field: post
```

### 统计每篇文章的访问量并显示在标题下方
NexT主题使用LeanCloud实现这一功能, 详见 [为NexT主题添加文章阅读量统计功能](https://notes.wanghao.work/2015-10-21-%E4%B8%BANexT%E4%B8%BB%E9%A2%98%E6%B7%BB%E5%8A%A0%E6%96%87%E7%AB%A0%E9%98%85%E8%AF%BB%E9%87%8F%E7%BB%9F%E8%AE%A1%E5%8A%9F%E8%83%BD.html#%E9%85%8D%E7%BD%AELeanCloud) 一文, 这一功能就是该博主添加到NexT主题中的!

### 文章分享插件
看完一篇文章后觉得不错, 想分享到社交媒体上, 文末的分享插件可以很方便地实现这个功能. 这里使用的是多说提供的分享插件. 在站点配置文件中添加如下字段即可.
```yml
duoshuo_share: true
```

### 推广你的博客
#### 为网站添加sitemap
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
#### 向Google提交sitemap
使用[Google站长工具](https://www.google.com/webmasters/) 向Google提交自己的sitemap. 以便Google收录. 在验证网站所有权时, 选择 **HTML Tag** 方式. 这样你会获得一段代码
```html
<meta name="google-site-verification" content="XXXXXXXXXXXXXXXXXXXXXXX" />
```
在站点配置文件中新建google_site_verification字段, 其值就是content的值
```yml
google_site_verification: XXXXXXXXXXXXXXXXXXXXXXX
```
这样Google过几天就会收录你的网站了. 百度的站长工具暂时不支持https网站的提交, 所以百度什么时候能收录就看造化了......

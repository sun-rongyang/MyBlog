---
title: HEXO主题NexT进阶配置
date: 2016-08-04 15:31:21
tags: [hexo,next]
categories:
    - development
    - blog
---

在使用 HEXO 搭建起自己的博客以后, 我们可以充分利用NexT主题本身提供的诸多功能来进一步美化和完善自己的博客. 你可以在博客的侧边栏贴上你的社交网站连接, 也可以让你的博文中用 LaTex 显示公式, 如果你觉得自己的原创文章对别人可能有帮助也可在文末求点赏钱哦~~
<!-- more -->
### 头像设置
将想要做头像的图片转为 gif 格式并改名为 avatar.gif 放到 themes/next/source/images 中即可.

### 侧边栏社交链接
人们在看你的博客时可以顺便去你的其他社交网络上逛逛, 在主题配置文件中找到 social: 字段, 添加社交网络中你自己主页的地址
```yaml
social:
#LinkLabel: Link
	GitHub: https://github.com/sun-rongyang
```

### 多说评论显示UA
在评论区显示评论者的系统和浏览器版本. 在主题配置文件中找到 duoshuo_info: 字段, 并配置如下
```yaml
duoshuo_info:
  ua_enable: true
  admin_enable: true #使博主的评论显示 admin_nickname 字段
  user_id: 1234 #博主的user_id 多说个人主页url最后那串数字
  admin_nickname: Author #博主评论时显示
```

### 站点建立时间
show一下你玩博客已经几年了. 在主题配置文件中添加since字段
```yaml
# 站点建立时间
since: 2016
```

### 建立腾讯公益404页面
腾讯公益404页面，寻找丢失儿童，让大家一起关注此项公益事业！效果如下 http://www.ixirong.com/404.html

使用方法，新建 404.html 页面，放到主题的 source 目录下，内容如下：
```html
<!DOCTYPE HTML>
<html>
<head>
  <meta http-equiv="content-type" content="text/html;charset=utf-8;"/>
  <meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1" />
  <meta name="robots" content="all" />
  <meta name="robots" content="index,follow"/>
</head>
<body>

<script type="text/javascript" src="http://www.qq.com/404/search_children.js"
        charset="utf-8" homePageUrl="your site url "
        homePageName="回到我的主页">
</script>

</body>
</html>
```

###求打赏
如果你的文章写的不错, 很有价值, 那么求各位看官捧个钱场吧^^ 在主题配置文件中新建 reward_comment 字段, 支持支付宝和微信二维码.
```yaml
reward_comment: 坚持原创技术分享，您的支持将鼓励我继续创作！
wechatpay: /path/to/wechat-reward-image
alipay: /path/to/alipay-reward-image
```

### 使用MathJax
运用latex强大的公式语言, 写理工科技术博客必备啊. 启用方法很简单, 找到 mathjax 字段, 打开即可. 在markdown文档中, 一个\$为插入行内公式: $E=mc^{2}$, 两个\$\$为插入行间公式: 
$$
E=mc^{2}
$$

### 设置阅读全文功能
在首页中会显示最近post的文章, 如果文章过长的话会使首页显得很难看,  因此可以打开主题配置文件中的 auto_excerpt 字段
```yaml
auto_excerpt:
  enable: true
  length: 150 #设置预览的文字量
```


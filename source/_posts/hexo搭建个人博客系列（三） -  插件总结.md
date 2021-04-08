---
title: hexo搭建个人博客系列（三） -  插件总结
date: 2021-03-22 18:24:34
tags:
  - hexo
  - next主题
  - 个人博客
categories:
  - hexo
---

## 一、文章加密

### 1、安装插件

```
npm install hexo-blog-encrypt --save
```

### 2、hexo 配置文件\_config.yml 文末添加：

```
# 文章加密
encrypt:
  enable: true
  abstract: 这是一篇加密文章，内容可能是个人情感宣泄或者收费技术。如果你非常好奇，请与我联系。
  message: 输入密码，查看文章。
```

### 3、文章头部加上 password: 123456 即可：

```
---
title: Hello World
password: 123456
---
```

其中：
1.password: 是该博客加密使用的密码
2.abstract: 是该博客的摘要，会显示在博客的列表页
3.message: 这个是博客查看时，密码输入框上面的描述性文字
效果：（此页面是不支持复制功能的 o！）
如果你开启了 字数统计功能 的话，那么本文的统计也会失效。

### 4、加密文章，需要额外隐藏浏览数、评论、作者信息等

在任何需要加密的地方加上一句：

```
<% if (post.encrypt == true) { %>style="display:none" <% } %>
```

<!--more-->

## 二、文章置顶

### 1、安装插件

```
npm install hexo-generator-index-pin-top --save
```

### 2、在需要置顶的文章的 Front-matter 中加上 top: true 即可

```
---
title: Hexo
top: true
---
```

<iframe name="easyXDM_default3069_provider" id="easyXDM_default3069_provider" src="https://embed.widgetpack.com/widget/xdm/index.html?xdm_e=https%3A%2F%2Ftding.top&amp;xdm_c=default3069&amp;xdm_p=1" frameborder="0" style="display: block; margin-left: auto; margin-right: auto; max-width: 100%; color: rgb(0, 0, 0); font-family: &quot;Noto Serif SC&quot;, &quot;PingFang SC&quot;, &quot;Microsoft YaHei&quot;, sans-serif; font-size: 16px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; white-space: normal; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial; position: absolute !important; top: -2000px !important; left: 0px !important;"></iframe>

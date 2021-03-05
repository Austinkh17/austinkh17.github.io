---
title: hexo搭建个人博客系列（二） - 站点配置和主题配置（next7.8版本）
date: 2021-03-03 16:24:34
tags: 
- hexo 
- next主题 
- 个人博客
categories:
- hexo
---

## 一、站点配置

### 1、设置 hexo 的 next 主题

安装next主题，在个人博客目录下，`git clone https://github.com/theme-next/hexo-theme-next themes/next`

我们在站点的配置文件`_config.yml` 中找到 theme 后添加：

```
theme: next  # 配置成刚下载的next主题
```

### 2、配置 hexo 网站相关信息

我们在站点的配置文件`_config.yml` 中，修改：

```
# Site
title:          # 网站标题
subtitle:       # 网站副标题
description:    # 描述，介绍网站的，用于SEO
keywords:       # 网站的关键字，用于SEO
author:         # 博主姓名
language: zh-CN # 语言：zh-CN 是简体中文(next6+版本)
timezone: UTC   # 时区
```

注意：博客框架默认的语言是英文，我们需要到 `/themes/next/languages` 目录下，查看当前 NexT 版本简体中文对照文件的名称是 `zh-Hans` 还是 `zh-CN`。

<!--more-->

### 3、设置 hexo 永久链接

我们在站点的配置文件`_config.yml` 中，修改：

```
url: 绑定的github pages的url
root: /
permalink: :year-:month-:day/:title/
```

### 4、nofollow 标签的使用

减少出站链接能够有效防止权重分散，hexo 有很方便的自动为出站链接添加 `nofollow` 标签的插件。

> hexo-filter-nofollow 会为你的博客中的外链自动添加 rel="external nofollow noreferrer" 属性，从而 改善你的网站的安全性和 SEO。

首先安装 `hexo-filter-nofollow` 插件：

```
npm install hexo-filter-nofollow --save
```

然后我们在站点的配置文件`_config.yml` 中添加配置，将 `nofollow` 设置为 `true`：

```
# 外部链接优化
nofollow:
  enable: true
  field: site
  exclude:
    - 'exclude1.com'
    - 'exclude2.com'
```

其中 `exclude`（例外的链接，比如友链）将不会被加上 `nofollow` 属性。

## 二、主题配置

### 1、配置 hexo 中的 about、tag、categories、sitemap 菜单

默认的主题配置文件`_config.yml` 中，菜单只开启了首页和归档，我们根据需要，可以添加 about、tag、categories、sitemap 等菜单，所以把它们也取消注释。

```
# 菜单设置为 菜单名: /菜单目录 || 菜单图标名字
menu:
  home: / || home
  about: /about/ || user
  tags: /tags/ || tags
  categories: /categories/ || th
  archives: /archives/ || archive
  #schedule: /schedule/ || calendar
  sitemap: /sitemap.xml || sitemap
  commonweal: /404/ || heartbeat

menu_settings:
  icons: true   # 显示图标
  badges: true  # 显示统计信息（徽章显示）
```

注：hexo 所有图标均来自 [fontawesome](https://fontawesome.com/icons)，其中 `||` 后面是你想要设置的图标的名字。除了将fa的图标用于博客侧边栏，还可以直接在`.md`中通过`<i>`标签引用。

### 2、手动生成 hexo 菜单对应文件

新菜单开启后是没有对应文件的，所以要手动生成 about、tags、categories、404 页面。

#### 新建 about 页面

`about` 页面一般用来介绍站点信息和博主信息。

我们可以在博客根目录下输入以下命令新建页面：

```
hexo new page about
```

然后你会发现多了一个 `hexo/source/about` 文件夹，里面有一个 `index.md` 文件，你可以根据自己的需要在这个 Markdown 文件中写一些内容（同文章一样，用 Markdown 语法）。

#### 新建一个 tags 页面

同样的，我们可以新建 `tags` 页面：

```
hexo new page tags
```

在 `tags` 页面文件 `hexo/source/tags/index.md` 的头部修改为：

```
---
title: 标签
date: 2021-03-04 13:13:31   # 时间随意
type: "tags"                # 类型一定要为tags
comments: false             # 提示这个页面不需要加载评论
---
```

#### 新建一个 categories 页面

同样的，我们可以新建 `categories` 页面：

```
hexo new page categories
```

在 `categories` 页面文件 `hexo/source/categories/index.md` 的头部修改为：

```
---
title: 分类
date: 2021-03-04 13:13:31
type: "categories"
comments: false
---
```

#### 新建一个 404 页面

这里我们将 404 替换成腾讯的公益页面。

首先我们在 `hexo/source` 目录下创建自己的 `404.html`：

```
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>404</title>
    </head>
    <body>
        <script type="text/javascript" src="//qzonestyle.gtimg.cn/qzone/hybrid/app/404/search_children.js" charset="utf-8" homePageUrl="/" homePageName="返回"></script> 
    </body>
</html>
```

> 注意：Hexo 博客的基本内容是一些 Markdown 文件，放在 `source/_post` 文件夹下，每个文件对应一篇文章。除此之外，放在 source 文件夹下的所有开头不是下划线的文件，在 `hexo generate` 的时候，都会被拷贝到 public 文件夹下。但是，**Hexo 默认会渲染所有的 HTML 和 Markdown 文件**。

因此我们可以简单地在文件开头加上 `layout: false` 一行来避免渲染：

```
+layout: false
+---

<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>404</title>
    </head>
    <body>
        <script type="text/javascript" src="//qzonestyle.gtimg.cn/qzone/hybrid/app/404/search_children.js" charset="utf-8" homePageUrl="/" homePageName="返回"></script> 
    </body>
</html>
```

这样我们就完成了 404 页面的创建。

注意：本地测试不出来，发布出来就可以了。

### 3、配置 hexo 本地搜索

#### 本地搜索的原理

对于动态网站来说，可以通过 php 实现。但是，Hexo 博客是静态网站，用不了 php。

NexT 主题已经实现这个功能，它用了 Hexo 的拓展包 `hexo-generator-searchdb`，预先生成了一个文本库 `search.xml`，然后传到了网站里面。在本地搜索的时候，NexT 直接用 javascript 调用了这个文件，从而实现了静态网站的本地搜索。

#### 设置过程

安装插件:

```
npm install hexo-generator-searchdb --save
```

然后我们修改站点配置`_config.yml` 文件，添加如下内容：

```
# 本地搜索
search:
  path: search.xml
  field: post
  format: html
  limit: 100
```

- path：索引文件的路径，相对于站点根目录
- field：搜索范围，默认是 post，还可以选择 page、all，设置成 all 表示搜索所有页面
- limit：限制搜索的条目数

然后修改主题配置文件`_config.yml`：

```
local_search:
  enable: true
  trigger: auto
  top_n_per_article: 1
  unescape: false
  preload: false
```

### 4、设置 hexo 中的 rss 订阅

安装插件：

```
npm install hexo-generator-feed --save
```

在站点配置文件`_config.yml` 中添加以下代码：

```
feed:
  type: atom
  path: atom.xml
  limit: 20
  hub:
  content:
  content_limit: 140
  content_limit_delim: ' '
  order_by: -date
  icon: icon.png
  autodiscovery: true
  template:
```

### 5、配置 hexo 站点的 footer 信息

底部 `footer` 可以开关显示 hexo 信息、theme 信息、建站时间等个性化配置：

```
footer:
  since: 2021        # 建站开始时间
  icon:
    name: user       # 设置 建站初始时间和至今时间中间的图标，默认是一个'小人像'，更改user为heart可以变成一个心
    animated: true
    color: "#808080" # 更改图标的颜色，红色为'#ff0000'
  powered:
    enable: false     # 开启hexo驱动
    version: false    # 开启hexo版本号
  theme:
    enable: false     # 开启主题驱动
    version: false    # 开启主题版本号
  custom_text:  # 自定义文字
```

### 6、配置 hexo 中 next 主题样式选择

NexT 一共提供了 4 种首页样式，按照自己喜好选择一个，选择一个其他主题样式后其他的主题前一定要加上注释`#`：

```
# Schemes
#scheme: Muse
#scheme: Mist
#scheme: Pisces
scheme: Gemini
```

### 7、头像信息设置

```
avatar:
  url: /images/avatar.jpg  # 设置头像资源的位置
  rounded: true            # 开启圆形头像
  opacity: 1               # 不透明的比例：0就是完全透明
  rotated: true           # 开启旋转
```

### 8、社交信息和友链配置

这里和菜单设置格式一样，`社交名字: 社交url || 社交图标`，图标信息来自 [fontawesome](https://fontawesome.com/v4.7.0/icons)：

```
social: 
  GitHub: https://github.com/yourname || github
  E-Mail: mailto:yourname@gmail.com || envelope

social_icons:
  enable: true      # 显示社交图标
  icons_only: false # 只显示图标，不显示文字
```

友链配置：

```
# Blog rolls
links_icon: link          # 友链的图标 参考上文
links_title: Links        # 友链的title  比如你可以更改为 友情链接
links_layout: block       # 友链摆放的样式：按块（一行一个）
#links_layout: inline     # 友链摆放的样式：按线摆放（一行很多个），注意，同时只能一种样式
links:
  Title: http://example.com/  # 友链的地址
```

### 9、首页文章不展示全文显示摘要

我们可以在主题配置文件中设置：

```
scroll_to_more: true      # 点击阅读全文后是否跳到<!--more-->标记处,设为false时点击阅读全文可以从头阅读

save_scroll: false        # 自动保存每篇文章或页面上一次滚动的地方

excerpt_description: true # 自动在首页对文章进行摘要描述作为前言文本

auto_excerpt:   # 是否自动截取摘要
  enable: false # 设置为true则自动截取150字当做首页摘要
  length: 150   # 自动截取的字数
```

> 官方公告：`auto_excerpt` 可以自动截断文章内容作为摘要。此功能不是一个 Hexo 主题应当负责的，这为主题的维护者带来了太大压力。自 7.6.0 版本开始，此功能被移除，请自行安装第三方插件，或阅读 Hexo 有关文档。当然，我们仍然建议通过 `<!-- more -->` 来精确控制 Read More 的位置。

### 10、首页文章属性

```
post_meta:
  item_text: false    # 设为true 可以一行显示，文章的所有属性
  created_at: true    # 显示创建时间
  updated_at:
    enabled: true     # 显示修改的时间
    another_day: true # 设true时，如果创建时间和修改时间一样则显示一个时间
  categories: true    # 显示分类信息
```

### 11、页面阅读统计

```
busuanzi_count:
  enable: false              # 设true 开启
  total_visitors: true       # 总阅读人数（uv数）
  total_visitors_icon: user  # 阅读总人数的图标
  total_views: true          # 总阅读次数（pv数）
  total_views_icon: eye      # 阅读总次数的图标
  post_views: true           # 开启内容阅读次数
  post_views_icon: eye       # 内容页阅读数的图标
```

### 12、字数统计、阅读时长

首先安装插件：

```
npm install hexo-symbols-count-time --save
```

主题配置文件`_config.yml` 修改如下：

```
symbols_count_time:
  separated_meta: true  # false会显示一行
  item_text_post: true  # 显示属性名称,设为false后只显示图标和统计数字,不显示属性的文字
  item_text_total: true # 底部footer是否显示字数统计属性文字
  awl: 4                # 计算字数的一个设置,没设置过
  wpm: 275              # 一分钟阅读的字数
```

站点配置文件`_config.yml` 新增如下：

```
symbols_count_time:
 #文章内是否显示
  symbols: true
  time: true
 # 网页底部是否显示
  total_symbols: true
  total_time: true
```

### 13、内容页里的代码块新增复制按钮

```
codeblock:
  copy_button:
    enable: false      # 增加复制按钮的开关
    show_result: false # 点击复制完后是否显示 复制成功 结果提示
```

### 14、配置微信，支付宝打赏

```
# Reward
reward_comment:                   # 打赏描述
wechatpay: /images/wechatpay.png  # 微信支付的二维码图片地址
alipay: /images/alipay.png        # 支付宝的地址
#bitcoin: /images/bitcoin.png     # 比特币地址
```

### 15、相关文章推荐

安装推荐文章的插件：

```
npm install hexo-related-popular-posts --save
```

主题配置信息如下:

```
related_posts:
  enable: true
  title: 相关文章推荐      # 属性的命名
  display_in_home: false # false代表首页不显示
  params:
    maxCount: 5          # 最多5条
    #PPMixingRate: 0.0   # 相关度
    #isDate: true        # 是否显示日期
    #isImage: false      # 是否显示配图
    isExcerpt: false     # 是否显示摘要
```

### 16、文章原创申明

```
creative_commons:
  license: by-nc-sa
  sidebar: false
  post: true       # 默认显示版权信息
  language:
```

### 17、背景动画设置

#### Canvas-nest 风格

进入 `theme/next` 目录，执行命令：

```
git clone https://github.com/theme-next/theme-next-canvas-nest source/lib/canvas-nest
```

实际上就是将一个显示动效的 js 文件 clone 到对应目录。

这时将配置文件`_config.yml` 中的 `canvas_nest: false` 改为 `canvas_nest: true` 才能真正生效。

**个人认为在站点中添加动态背景并没有实际的意义，只会凭空增加页面内存占用及 CPU 消耗**。

### 18、添加 Google 统计

访问 Google Analytics，按照提示填写网站信息开通 GA 服务，获取统计 ID。

然后编辑主题配置文件`_config.yml`，找到关键字 `google_analytics`，删除注释`#`并填写获取到的统计 ID：

```
# Google Analytics
google_analytics:
  tracking_id: 
  localhost_ignored: true
```

### 19、Google Search Console

该版本已经集成了 HTML 标记的验证方式。

- 查看原标记，将其中 content 字段引号内的内容拷贝出来
- 修改主题配置文件`_config.yml`。搜索 `google_site_verification`，将上述拷贝的内容复制在该值后面：

```
# Google Webmaster tools verification setting
# See: https://www.google.com/webmasters/
google_site_verification: uW8bwgMGUwIA01nPfItoty1rmtmmuVkOVTeS9O0nAUg
```

### 20、自定义图标

```
favicon:
  small: /images/favicon-16x16.png
  medium: /images/favicon-32x32.png
  apple_touch_icon: /images/apple-touch-icon.png
  safari_pinned_tab: /images/logo.svg
  #android_manifest: /images/manifest.json
  #ms_browserconfig: /images/browserconfig.xml
```



<iframe name="easyXDM_default3069_provider" id="easyXDM_default3069_provider" src="https://embed.widgetpack.com/widget/xdm/index.html?xdm_e=https%3A%2F%2Ftding.top&amp;xdm_c=default3069&amp;xdm_p=1" frameborder="0" style="display: block; margin-left: auto; margin-right: auto; max-width: 100%; color: rgb(0, 0, 0); font-family: &quot;Noto Serif SC&quot;, &quot;PingFang SC&quot;, &quot;Microsoft YaHei&quot;, sans-serif; font-size: 16px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; white-space: normal; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial; position: absolute !important; top: -2000px !important; left: 0px !important;"></iframe>
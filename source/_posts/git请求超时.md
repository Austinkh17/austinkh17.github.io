---
title: github请求超时
date: 2021-03-05 17:08:16
tags: git
categories: git
---

### github请求超时

#### 一、修改DNS

[站长工具1](http://tool.chinaz.com/dns/)

[站长工具2](http://ping.chinaz.com/github.githubassets.com)

在上面的网站中寻找`github.com`对应的IP地址，复制后进入hosts文件（C/Windows/System32/drivers/etc/hosts），写下`ip  github.com`，并刷新dns缓存（ipconfig /flushdns）

适当的时候可以打开`chrome://net-internals/#dns`刷新一下google浏览器的dns

<!--more-->

#### 二、配置git的最低速度和最低速度时间

```
git config --global http.lowSpeedLimit 0
git config --global http.LowSpeedTime 999999
```

### 三、github1s查看仓库速度快

这个项目名为 github1s，它的使用方法非常简单，**只需要在浏览器地址栏 GitHub 网址链接中的「github 」后面添加 1s ，然后 Enter 键，即可在 VS Code 界面访问该项目的 Repo 代码**。
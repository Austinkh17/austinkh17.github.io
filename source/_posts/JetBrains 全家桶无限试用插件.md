---
title: JetBrains 全家桶无限试用插件
date: 2021-04-28 10:09:30
tags: 工具
categories: 工具
---

### 一、IDE Eval Resetter 介绍
项目地址：https://gitee.com/pengzhile/ide-eval-resetter

项目功能：JetBrains 所有产品都有 30 天试用，这个插件的作用就是重置这个试用时间，让你无限试用，也算是另类的 JetBrains 激活方式。

### 二、使用方法

#### 1、下载插件

下载地址：https://plugins.zhile.io/files/ide-eval-resetter-2.1.6.zip

#### 2、安装插件

直接下载插件 zip 包（macOS 可能会自动解压，然后把 zip 包丢进回收站）
通常可以直接把 zip 包拖进 IDE 的窗口来进行插件的安装。如果无法拖动安装，你可以在Settings/Preferences... -> Plugins 里手动安装插件（Install Plugin From Disk...）
插件会提示安装成功

#### 3、使用插件

如果 IDE 没有打开项目，在Welcome界面点击菜单：Get Help -> Eval Reset
如果 IDE 打开了项目，点击菜单：Help -> Eval Reset
唤出的插件主界面中包含了一些显示信息，2 个按钮，1 个勾选项：

按钮：Reload 用来刷新界面上的显示信息。
按钮：Reset 点击会询问是否重置试用信息并重启 IDE。选择 Yes 则执行重置操作并重启 IDE 生效，选择 No 则什么也不做。（此为手动重置方式）
勾选项：Auto reset before per restart 如果勾选了，则自勾选后每次重启/退出 IDE 时会自动重置试用信息，你无需做额外的事情。（此为自动重置方式）

<!--more-->

---
title: 使用nvm管理node版本
date: 2021-04-20 15:55:47
tags: 
- nvm 
- node
categories: node
---

### 1、nvm 是什么
>（1）nvm(Node.js version manager) 是一个命令行应用，可以协助您快速地 更新、安装、使用、卸载 本机的全局 node.js 版本。
>（2）有时候，我们可能同时在进行多个项目开发，而多个项目所使用的node版本又是不一样的，或者是要用最新的node版本进行试验和学习。这种情况下，对于维护多个版本的node将会是一件非常麻烦的事情，而nvm就是为解决这个问题而产生的，他可以在同一台电脑上进行多个node版本之间的切换，而这正是nvm的价值所在。

<!--more-->

### 2、安装 nvm-windows
nvm下载地址：https://github.com/coreybutler/nvm-windows/releases 点击最新版本的 nvm-setup.zip 下载到本地并安装
安装步骤：以windows10系统为例
注意：nvm的安装目录不能有汉字和空格，否则会报错
注意：电脑之前安装过nodejs的，不需要卸载，nvm在安装的过程中会提示，是否把电脑之前安装过的nodejs交给nvm来管理，点击【是】就可以了

安装完成后，修改settings.txt 在你安装的nvm目录下找到settings.txt文件，打开settings.txt文件后，加上下面两行代码：
node_mirror: https://npm.taobao.org/mirrors/node/
npm_mirror: https://npm.taobao.org/mirrors/npm/
目的是将npm镜像改为淘宝的镜像，可以提高下载速度

### 3、使用 nvm 管理版本（nvm常用命令）
```
nvm install latest 安装最新版本node.js
nvm use 版本号 使用某一具体版本，例如 ：nvm use 14.3.0
nvm list 列出当前已安装的所有版本
nvm ls 列出当前已安装的所有版本
nvm uninstall 版本号 卸载某一具体版本，例如：nvm use 14.3.0
nvm ls-remote Mac版本中,列出全部可以安装的node版本
nvm ls available windows版本,列出全部可以安装的node版本
nvm current 显示当前的版本
nvm alias 给不同的版本号添加别名
nvm unalias 删除已定义的别名
nvm reinstall-packages 在当前版本node环境下，重新全局安装指定版本号的npm包
```

### 4、 nvm 出现的问题
##### 1、全局安装`npm install -g cnpm --registry=https://registry.npm.taobao.org`，`cnpm -v`却提示不是内部或外部命令，也不是可运行的程序

1.修改npm全局安装路径 `npm config set prefix "D:\software\node\node_global"`
2.配置环境变量，在用户变量的path中找到`C:\Users\Administrator\AppData\Roaming\npm`，更改为`D:\software\node\node_global`

##### 2、vscode终端不能执行`cnpm`

vscode属性->兼容性->勾选以管理员身份运行此程序
---
title: 多台设备共享hexo
date: 2021-03-05 17:39:29
tags: hexo
categories: hexo
---

因为更换电脑，之前的hexo本地项目未保存，而github仓库上仅有发布后的文件，因此，还是需要把本地项目也及时的更新到github上去，防止丢失。

## 流程

### 1.电脑A（已经配置好blog的设备）

1. 在github上，对应的blog仓库，创建新分支`hexo` ，并设置为默认分支，github仓库切换到该分支，**注意本地_config.yml文件的deploy配置的分支应该为master**
2. 打开gitbash，使用`git clone git@github.com:YourBlogResposity` ，克隆`hexo` 分支的内容到一个任意空文件夹中，例如`hexo`
3. 显示所有隐藏文件和文件夹，进入刚才 clone 到本地的仓库，删掉除了 .git 文件夹以外的所有内容
4. 命令行 cd 到 clone 的仓库，git add -A ，git commit -m “共享hexo”，git push origin hexo，把刚才删除操作引起的本地仓库变化更新到远程，此时刷新下 github 端博客hexo分支，应该已经被清空
5. 将上述 .git 文件夹复制到本机本地博客根目录下（即含有 themes、source 等文件夹的那个目录），现在可以把上述 clone 的本地仓库删掉了，因为它已经没有用了，本机博客目录已经变成可以和 hexo 分支相连的仓库了**（以上即是使用clone的目的）**
6. 将博客目录下 themes 文件夹下每个主题文件夹里面的 .git .gitignore 删掉。 cd 到博客目录，git add -A ，git commit -m “–”，git push origin hexo，将博客目录下所有文件更新到 hexo 分支。**如果上一步没有删掉 .git .gitignore，主题文件夹下内容将传不上去。**
7. 完成以上步骤后，github上hexo分支应该已经存放了blog的配置文件，mater分支仍然为静态blog文件

### 2.电脑B（添加的设备）

1. 同样`git clone` 远程仓库到本地
2. `cd` 到本地仓库，`npm i hexo -g` 安装hexo，`hexo s -g`测试电脑B能否启动blog
3. 成功添加完毕

## 更新、提交blog的操作

依次执行：

- git pull
- 更新blog内容
- hexo clean
- hexo s -g
- hexo d
- git add -A
- git commit -m “…”
- git push origin hexo
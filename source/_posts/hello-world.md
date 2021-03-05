---
title: hexo搭建个人博客系列（一） - 绑定到github pages
date: 2021-03-02 16:24:34
tags: 
- hexo 
- next主题 
- 个人博客
categories:
- hexo
---
### Github Pages

> Github Pages 是 Github 提供的博客服务。 我们在 Github 中创建一个特定格式的 Repository，Github Pages 就会将里面的信息生成一个网页，展示出来。

具体操作：

- 默认你已经注册了 Github 账号
- 然后需要在 Github 中创建一个以 `用户名.github.io` 结尾的 Repository（每个github账号只有一个github pages）
- 勾选 Initialize this repository with a README
- Create repository
- 打开网页：`https://用户名.github.io/` 就可以看到 README.md 里的内容了。

这个生成好的 Repository 就是用来存放博客内容的地方，也只有这个仓库里的内容，才会被 `https://用户名.github.io/` 这个网页显示出来。

<!--more-->


### Hexo 安装

> Hexo 是一个快速、简洁且高效的博客框架。Hexo 使用 Markdown（或其他渲染引擎）解析文章，在几秒内，即可利用靓丽的主题生成静态网页。

具体操作：

- 使用 Hexo 之前，需要先安装 Node 和 Git（默认你已经安装好了）
- 快捷键 win + r 输入 cmd 并回车，输入 `npm i hexo -g`，回车开始安装 Hexo
- 输入 `hexo -v` 检查版本来检测是否安装成功

### Hexo 创建本地博客

具体操作：

- 在D盘下创建文件夹 `个人博客`
- 进入`个人博客`文件夹，选择 Git Bash Here。 如果没有安装 Git，就不会有这个选项。
- 输入 `hexo init` 将 `个人博客 `文件夹初始化成一个博客文件夹。
- 输入 `hexo g` （generate）生成网页。
- 输入 `hexo s` （server）将生成的网页放在了本地服务器。
- 浏览器里输入 http://localhost:4000/ 。由于我们还没创建任何博客，生成的网页会展示 Hexo 里面自带的一个 Hello World 的博客。
- 回到 Git Bash，按 Ctrl+C 结束进程。此时再看 http://localhost:4000/ 就是无法访问了。

### 将本地 Hexo 博客部署在 Github 上

> - 前面我们已经实现了本地博客，和一个能托管这些内容的线上仓库。
> - 接下来只要把本地博客部署在我们的 Github 对应的 Repository 就可以了。

具体操作：

- 进入到自己 github 中名为 `用户名.github.io` 的仓库，点击 Clone or download，
  复制 `https://github.com/用户名/用户名.github.io.git` 地址待用

- 打开博客的配置文件 /d/个人博客/_config.yml

- 找到 #Deployment，填入以下内容：

  ```
  deploy:  
  	type: git  
  	repository: https://github.com/用户名/用户名.github.io.git  
  	branch: master
  ```

- 回到 Git Bash，输入 `npm i hexo-deployer-git --save` 安装 hexo-deployer-git

- `hexo d（deploy）` 来部署，得到 INFO Deploy done: git 即为部署成功

- 前往 `https://用户名.github.io/` 即可查看博客

### Hexo 发布博客

具体操作：

- 继续在 Git Bash 里，输入 `hexo new "first blog"` 并回车
- 在 D:\blog\source\_posts 路径下，会有一个 first-blog.md 的文件。 编辑这个文件，然后保存
- 回到 Git Bash，输入 `hexo g -d`（生成网页并部署）
- 前往 `https://用户名.github.io/` 查看成果
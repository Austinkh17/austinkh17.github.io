---
title: github提速方案汇总
date: 2021-03-05 17:08:16
tags: github
categories: github
top: 10
---
### 一、修改Hosts

#### 通过 SwitchHosts! 自动更新(推荐)
这里推荐使用 SwitchHosts! 配置hosts，操作很简单，支持跨平台，下载地址(https://github.com/oldj/SwitchHosts/releases)

##### 手动配置
添加一条hosts规则并启用，然后复制前文hosts内容即可。
如果你想保持和云端最新规则同步，可以用下面的配置方式。

##### 定时同步
添加一条规则：

方案名：GitHub（可以自行命名）
类型：远程
URL 地址：https://cdn.jsdelivr.net/gh/ineo6/hosts/hosts
自动更新：1个小时
这样就可以和最新的hosts保持同步。

##### 自定义命令
SwitchHosts!还支持保存后执行自定义命令的功能，点击设置 => 选项 => 命令即可找到。

Windows命令不需要管理员权限，所以直接粘贴`ipconfig /flushdns`内容即可。

#### 手动把 cdn 和 ip 地址绑定

第一步：获取 github 的 global.ssl.fastly

访问地址：http://github.global.ssl.fastly.net.ipaddress.com/#ipinfo 获取 cdn 和 ip 域名
得到：`199.232.69.194 https://github.global.ssl.fastly.net`

第二步：获取 github.com 地址

访问地址：https://github.com.ipaddress.com/#ipinfo 获取 cdn 和 ip
得到：`140.82.114.4 http://github.com`

第三步：修改 host 文件映射上面查找到的 IP

windows系统：

1、修改 C:\Windows\System32\drivers\etc\hosts 文件的权限，指定可写入

右击->hosts->属性->安全->编辑->点击Users->在 Users 的权限 “写入” 后面打勾

2、右击->hosts->打开方式->选定记事本（或者你喜欢的编辑器）->在末尾处添加以下内容：
```
199.232.69.194 github.global.ssl.fastly.net
140.82.114.4 github.com
```

#### 其他获取 ip 地址网站
[站长工具1](http://tool.chinaz.com/dns/)

[站长工具2](http://ping.chinaz.com/github.githubassets.com)

在上面的网站中寻找`github.com`对应的IP地址，复制后进入hosts文件（C/Windows/System32/drivers/etc/hosts），写下`ip  github.com`

#### 修改hosts后刷新dns缓存

修改hosts后，刷新本机dns缓存`ipconfig /flushdns`，刷新google浏览器的dns缓存`chrome://net-internals/#dns`

<!--more-->

### 二、GitHub 镜像访问
这里提供两个最常用的镜像地址：

https://github.com.cnpmjs.org
https://hub.fastgit.org
也就是说上面的镜像就是一个克隆版的 GitHub，你可以访问上面的镜像网

网站的内容跟 GitHub 是完整同步的镜像，然后在这个网站里面进行下载克隆等操作

### 三、GitHub 文件加速
利用 Cloudflare Workers 对 github release 、archive 以及项目文件进行加速，部署无需服务器且自带 CDN

https://gh.api.99988866.xyz
https://g.ioiox.com
以上网站为演示站点

如无法打开可以查看开源项目：gh-proxy-GitHub( https://hunsh.net/archives/23/ ) 文件加速自行部署

### 四、Github 加速下载
只需要复制当前 GitHub 地址粘贴到输入框中就可以代理加速下载！

地址：http://toolwa.com/github/

### 五、加速你的 Github
https://github.zhlh6.cn

输入 Github 仓库地址，使用生成的地址进行 git ssh 等操作

### 六、谷歌浏览器 GitHub 加速插件(推荐)

### 七、GitHub raw 加速
GitHub raw 域名并非 github.com 而是 raw.githubusercontent.com

上方的 GitHub 加速如果不能加速这个域名，那么可以使用 Static CDN 提供的反代服务

将 raw.githubusercontent.com 替换为 raw.staticdn.net 即可加速

### 八、GitHub + Jsdelivr
jsdelivr 唯一美中不足的就是它不能获取 exe 文件以及 Release 处附加的 exe 和 dmg 文件。

也就是说如果 exe 文件是附加在 Release 处但是没有在 code 里面的话是无法获取的

所以只能当作静态文件 cdn 用途，而不能作为 Release 加速下载的用途

### 九、通过 Gitee 中转 fork 仓库下载
网上有很多相关的教程，这里简要的说明下操作。

访问 gitee 网站：https://gitee.com/ 并登录，在顶部选择“从 GitHub/GitLab 导入仓库” 如下：

在导入页面中粘贴你的 Github 仓库地址，点击导入即可：

等待导入操作完成，然后在导入的仓库中下载浏览对应的该 GitHub 仓库代码，你也可以点击仓库顶部的 “刷新” 按钮进行 Github 代码仓库的同步

### 十、配置git的最低速度和最低速度时间

```
git config --global http.lowSpeedLimit 0
git config --global http.LowSpeedTime 999999
```

#### 十一、github1s查看仓库速度快

这个项目名为 github1s，它的使用方法非常简单，**只需要在浏览器地址栏 GitHub 网址链接中的「github 」后面添加 1s ，然后 Enter 键，即可在 VS Code 界面访问该项目的 Repo 代码**。
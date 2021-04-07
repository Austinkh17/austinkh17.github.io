---
title: mongodb知识点
date: 2021-04-07 15:14:16
tags: mongodb
categories: mongodb
---

### 一、安装：
官网下载mongoDB：https://www.mongodb.com/try/download/community?jmp=nav，选择“Custom”自定义 安装路径修改下：D:\mongodb

### 二、先创建数据库文件的存放位置
在MongoDB下创建data，在data下再创建db：D:\mongodb\data\db
因为启动mongodb服务之前需要必须创建数据库文件的存放文件夹，否则命令不会自动创建，而且不能启动成功。

### 三、启动MongoDB服务
1.打开cmd命令行
2.进入D:\mongodb\bin目录（注意：先输入d:进入d盘，然后输入cd mongodb\bin）
3.输入如下的命令启动mongodb服务：mongod --dbpath D:\mongodb\data\db
4.在浏览器输入http://localhost:27017 （27017是mongodb的端口号）查看
但是在本地windows“服务”中，是没有配置上mongodb 服务的，可以打开“服务”看下
注：改为mongod --storageEngine=mmapv1 --dbpath D:\mongodb\data\db
原因是：wiredTiger是数据库引擎，当前版本默认的数据库引擎，它不支持32位系统，命令--storageEngine=mmapv1，
将wiredTiger引擎切换成mmapv1引擎

<!--more-->

### 四、配置本地windows mongodb 服务
这样可设置为 开机自启动，可直接手动启动关闭，可通过命令行net start MongoDB 启动。该配置会大大方便。
1.先在data文件下创建一个新文件夹log（用来存放日志文件）
2.在Mongodb新建配置文件mongo.config
用记事本打开mongo.config ，并输入：
dbpath=D:\software\MongoDB\data\db
logpath=D:\software\MongoDB\data\log\mongo.log
3.用管理员身份打开cmd:
C:\windows\system32\cmd.exe
然后右键，以管理员身份运行。打开后发现在顶端比普通打开的多了”管理员“三个字
4.配置windows服务：
cmd先跳转到 D:\mongodb\bin目录下。
输入：mongod --config "D:\software\Mongodb\mongo.config" --install --serviceName "MongoDB"
即根据刚创建的mongo.config配置文件安装服务，名称为MongoDB。
完成后，再次查看本地的服务。
如果成功的话，会发现本地服务多了”MongoDB"服务。
可以通过：“开机自启动，可直接手动启动关闭，命令行net start MongoDB 启动”。
停止MongoDB：net stop MongoDB
删除MongoDB：sc delete MongoDB

发生服务特定错误: 100
data文件中有两个文件一个mongod.lock和storage.bson，一般删除mongod.lock就可以了，如果服务错误代码100还不能解决，
就把storage.bson一起删掉再启动就可以了！

启动mongodb:
另开一个cmd输入:mongo  
前提是你已经将mongodb添加到环境变量中(path-->D:\mongodb\lib)

```
http://player.youku.com/player.php/sid/XMzE3NjAwNTIwOA==/v.swf
const express = require('express');
const path = require('path');
const favicon = require('serve-favicon');
const logger = require('morgan');
const cookieParser = require('cookie-parser');
const bodyParser = require('body-parser');
const mongoose = require('mongoose');
const movieModel = require('./mongoose/model/movieModel');
const _ = require('underscore');

// mongoose 
mongoose.connect('mongodb://127.0.0.1:27017/movie-website', { useMongoClient: true })
mongoose.Promise = global.Promise;
let db = mongoose.connection;
db.on('error', console.error.bind(console, 'Mongodb connect error !'))
db.once('open', function() {
    console.log('Mongodb started !')
})

let app = express();
```

use imooc,切换到你的项目的数据库下
查询所有数据命令：db.movies.find({}).pretty(),
删除所有命令是：db.movies.remove()
db.users.update({"_id":XXXX},{$set:{role:50}})
show tables

populate()方法：
因为MongoDB是文档型数据库，所以它没有关系型数据库[joins]，Mongoose封装了一个Population功能。使用Population可以实现在一个 
document 中填充其他 collection(s) 的 document(s)。在定义Schema的时候，如果设置某个 field 关联另一个Schema，那么在获取 
document 的时候就可以使用 Population 功能通过关联Schema的 field 找到关联的另一个 document，并且用被关联 document 的内容
替换掉原来关联字段(field)的内容。

SET DEBUG=myapp:* & npm run devstart
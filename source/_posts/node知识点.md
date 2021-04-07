---
title: node知识点
date: 2021-04-07 15:12:16
tags: node
categories: node
---

##  node知识点系列一
nodejs是一个基于Chrome JavaScript运行时建立的平台， 用于方便地搭建响应速度快、易于扩展的网络应用。npm和bower都是web包管理器。 
nodejs 本身自带的框架有限，通过 bower 或者 npm 可以直接调用一些发布到网上的著名的开源模块包。
简而言之：nodejs已经集成了npm，通过npm包管理可以安装bower这些前端包管理工具。通过bower关联安装bootstrap这些前端框架。

### node安装
配置npm全局模块的存放路径和cache的路径。
如果你希望将以上两个文件夹放在nodejs的主目录下，就在nodejs下建立“node_global”及“node_cache”两个文件夹。
然后我们就在cmd中分别键入两行命令，设置路径：
npm config set prefix "D:\nodejs\node_global"
npm config set cache "D:\nodejs\node_cache"

C:\Users\Administrator\AppData\Roaming\npm\node_modules(未设置默认路径)

在系统环境变量添加系统变量NODE_PATH，输入路径D:\nodejs\node_global\node_modules

<!--more-->

windows下使用npm安装vue-cli后，vue还是不可用是什么原因：
用everything找到vue-init所在文件夹，我的是D:\nodejs\node_global，然后把这个加到环境变量Path中

jade模版引擎
特点是语法简洁、简单易学、支持嵌入代码、支持多重继承。
相对于HTML，Jade中的元素（Element）标记（Tag）没有用“<>”包围，其属性（Attribute）是用“()”括起来的。
Jade的元素没有相对应的“开始标记”和“结束标记”。Jade是用“缩进”来描述元素的从属关系（与Python的语法相似）。
jade是一款源于Node.js的HTML模板引擎。
1.模板引擎
2.依赖于JavaScript实现jade到HTML的转换
3.供Node使用
npm install jade

npm安装产生node_modules文件夹，bower安装产生bower_components文件夹

Bower是一个客户端技术的软件包管理器，它可用于搜索、安装和卸载如JavaScript、HTML、CSS之类的网络资源
其他一些建立在Bower基础之上的开发工具，如YeoMan和Grunt
准备工作
1.安装node环境:node.js
npm install bower -g
2.安装Git，bower从远程git仓库获取代码包

GitHub是目前最好用的免费开源项目托管平台，bootstrap等项目都是托管在github上的，所以我们现在需要安装一个GIT客户端来下载bootstrap。

Git Bash，打开进入命令行窗口。
选择Git的下载目录，也是下载bootstrap存放位置。
在Git Bash命令行窗口输入    cd "D:\Imooc"
bower install bootstrap

你不喜欢使用它们的命名 bower_components，那么需要你在你需要的目录下创建一个文件“.bowerrc”并且写入如下信息
{"directory" : "public/libs"}
然后执行：bower install bootstrap
这样你下载的内容就会按照你的指定下载到里面了。
先创建一个文件，然后在dos使用rename命令，将文件重命名，或者直接在dos中创建改文件，
使用命令是：echo >>.bowerrc(windows)

Mongoose是在node.js异步环境下对mongodb进行便捷操作的对象模型工具。
如果使用程序操作数据库，就要使用MongoDB驱动。MongoDB驱动实际上就是为应用程序提供的一个接口，不同的语言对应不同的驱动，
NodeJS驱动不能应用在其他后端语言中
使用require()方法引入mongodb数据库；然后使用MongoClient对象的connect()方法连接mongodb；最后通过node来对mongodb进行异步的增删改查

Mongoose是NodeJS的驱动，不能作为其他语言的驱动。Mongoose有两个特点
1、通过关系型数据库的思想来设计非关系型数据库
2、基于mongodb驱动，简化操作

Mongooose中，有三个比较重要的概念，分别是Schema、Model、Entity。它们的关系是：Schema生成Model，Model创造Document，
Model和Document都可对数据库操作造成影响，但Model比Document更具操作性
Schema用于定义数据库的结构。类似创建表时的数据定义(不仅仅可以定义文档的结构和属性，还可以定义文档的实例方法、静态模型方法、
复合索引等)，每个Schema会映射到mongodb中的一个collection，Schema不具备操作数据库的能力
Model是由Schema编译而成的构造器，具有抽象属性和行为，可以对数据库进行增删查改。Model的每一个实例（instance）就是一个文档document
Document是由Model创建的实体，它的操作也会影响数据库

var mongoose = require('mongoose');
mongoose.connect("mongodb://u1:123456@localhost/db1")

var Schema = mongoose.Schema;
var mySchema = new Schema({})
//通过mongoose.Schema来调用Schema，然后使用new方法来创建schema对象
var MyModel = mongoose.model('MyModel', mySchema);
//model()方法的第一个参数是模型名称,mongoose会将集合名称设置为模型名称的小写版。
var doc1 = new MyModel({})
doc1.save(function (err,doc){})
//将创建的文档保存到数据库的集合中

Middleware中间件
	1 什么是中间件
		中间件是一种控制函数，类似插件，能控制流程中的init、validate、save、remove方法
	2 中间件的分类
		2.1 Serial串行
			串行使用pre方法，执行下一个方法使用next调用
				var schema = new Schema(…);
				schema.pre(‘save’,function(next){
						//做点什么
						next();
				});
		2.2 Parallel并行
			并行提供更细粒度的操作
				var schema = new Schema(…);
				schema.pre(‘save’,function(next,done){
						//下一个要执行的中间件并行执行
						next();
						doAsync(done);
				});
	3 中间件特点
		一旦定义了中间件，就会在全部中间件执行完后执行其他操作
		使用中间件可以雾化模型，避免异步操作的层层迭代嵌套
	4 使用范畴
		1.复杂的验证
		2.删除有主外关联的doc
		3.异步默认
		4.某个特定动作触发异步任务，例如触发自定义事件和通知
简单的说，中间件就相当于java中的过滤器、拦截器，在执行某个方法前，将其拦截住，也有点像AOP中的前置注入。举个简单的例子，
当我们要执行save方法时，我们往往需要对存入的数据进行验证，虽然mongoose提供了safe、strict、schematype、default、validaition验证，
但是这些验证都没有提供完善的错误处理或者拦截机制，而利用中间件，可以对错误的数据进行拦截、错误处理、修订等等。
比如存入的用户名可能带有代码注入，这时候，通过中间件拦截用户名，给与转义，或进行错误提示、日志记录等。经过中间件的拦截，
进入到save方法的数据从理想状态下应该是符合规范且完善的。由此看来，safe、strict、schematype、default、validaition本身就是内部
提供的中间件。
关于path，其实也是一种中间件，如同xml的path解析，mongoose是针对mongodb数据库的一种orm模型，mongodb是javascript的json数据存储，
有的时候，我们并不希望中间件只针对一个操作，而是针对操作对象的某个属性，那么就能使用path快速定位。

var _=require('underscore')
_.extend()方法是Underscore.js库提供的一个方法，作用是将sources对象中的所有属性拷贝到destination对象中，并返回destination对象。
_.extend(destination, *sources) 
从源码可以看出：
1)._.extend()方法的拷贝是有序的，如果有3个参数，首先将第二个参数中的所有属性拷贝到第一参数对象中，然后将第三个参数中的
所有属性拷贝到第一个参数对象中，有相同属性则直接覆盖。
2).每次拷贝，如果属性是一个对象，则直接将这个对象赋给第一个参数对应属性，即第一个参数引用这个对象属性。

bower init --> bower.json
npm init --> package,json

1、解析body不是nodejs默认提供的，你需要载入body-parser中间件才可以使用req.body
此方法通常用来解析POST请求中的数据(或者ajax请求)
2、req.params包含路由参数（在URL的路径部分）
3、req.query包含URL的查询参数（在URL的？后的参数）

req.param在express中的默认规则是
/user/signup/1111?userid=1112
{ userid: 1113 }
优先从路由中获取参数 即1111，
其次路由中无参数，则提交的表单中获取参数，即 1113
最后上述二者均无时，从url的？后查询参数中获取，即1112

bodyParser.json(options)  options可选 ， 这个方法返回一个仅仅用来解析json格式的中间件。这个中间件能接受任何body
中任何Unicode编码的字符。支持自动的解析gzip和 zlib。
bodyParser.urlencoded(options) options可选，这个方法也返回一个中间件，这个中间件用来解析body中的urlencoded字符，
只支持utf-8的编码的字符。同样也支持自动的解析gzip和 zlib
当extended为false的时候，键值对中的值就为'String'或'Array'形式，为true的时候，则可为任何数据类型。

Nodejs是单线程单进程的，但是有了child_process模块，可以在程序中直接创建子进程，并使用主进程和子进程之间实现通信。
1、child_process.spawn(command[, args][, options])
command String 将要运行的命令。
args Array 字符串参数数组。
options 配置对象：
cwd String 子进程的当前工作目录。
env Object 环境变量键值对。
stdio Array|String 子进程的stdio配置。
detached Boolean 这个子进程将会变成进程组的领导。
uid Number 设置用户进程的ID。
gid Number 设置进程组的ID。
返回值: ChildProcess对象
利用给定的命令以及参数执行一个新的进程，如果没有参数数组，那么args将默认是一个空数组。

2、child_process.exec(command[, options], callback)
command String 将要运行的命令，参数使用空格隔开。
options 配置对象：
cwd String 子进程的当前工作目录。
env Object 环境变量键值对。
encoding String 字符编码（默认： 'utf8'）。
shell String 将要执行命令的Shell（默认: 在UNIX中为/bin/sh， 在Windows中为cmd.exe， Shell应当能识别 -c 开关在UNIX中，
或 /s /c 在Windows中。 在Windows中，命令行解析应当能兼容cmd.exe）。
timeout Number 超时时间（默认： 0）。
maxBuffer Number 在stdout或stderr中允许存在的最大缓冲（二进制），如果超出那么子进程将会被杀死 （默认: 200*1024）。
killSignal String 结束信号（默认：'SIGTERM'）。
detached Boolean 这个子进程将会变成进程组的领导。
uid Number 设置用户进程的ID。
gid Number 设置进程组的ID。
callback Function 当子进程执行完毕后将会执行的回调函数，参数有：
error Error
stdout Buffer
stderr Buffer
返回值: ChildProcess对象
在Shell中运行一个命令，并缓存命令的输出。

从文档里可以得出的一些相同点：
1，它们都用于开一个子进程执行指定命令。
2，它们都可以自定义子进程的运行环境。
3，它们都返回一个ChildProcess对象，所以他们都可以取得子进程的标准输入流，标准输出流和标准错误流 。

不同点：
1，接受参数的方式： spawn使用了参数数组，而exec则直接接在命令后。
2，子进程返回给Node的数据量： spawn没有限制子进程可以返回给Node的数据大小，而exec则在options配置对象中有maxBuffer参数限制，
且默认为200K，如果超出，那么子进程将会被杀死，并报错：Error：maxBuffer exceeded，虽然可以手动调大maxBuffer参数，但是并不被推荐。
由此可窥见一番Node.js设置这两个API时的部分本意，spawn应用来运行返回大量数据的子进程，如图像处理，文件读取等。
而exec则应用来运行只返回少量返回值的子进程，如只返回一个状态码。
3，调用对象： 虽然在官方文档中，两个方法接受的第一个参数标注的都是command，即要执行的命令，但其实不然。
spawn接受的第一个参数为文件，而exec接受的第一个参数才是命令。
若一定要使用spwan，则应写成require('child_process').spawn('cmd.exe',['\s', '\c', 'dir'])。
4，回调函数： exec方法相比spawn方法，多提供了一个回调函数，可以更便捷得获取子进程输出。这与为返回的ChildProcess对象的
stdout或stderr监听data事件来获得输出的区别在于：data事件的方式，会在子进程一有数据时就触发，并把数据返回给Node。
而回调函数，则会先将数据缓存在内存中（数据量小于maxBuffer参数），等待子进程运行完毕后，再调用回调函数，并把最终数据交给回调函数。

res.render(file,option)是express中专门渲染视图用的，首先你要在你的app.js或者index.js中设置一下渲染引擎，
比如html,jade,handlebars(我自己使用的)，mustache等。然后将视图模板的文件位置放入file,将传入的模板数据放入option对象中，
模板引擎就能自己渲染出视图。一般数据是JSON，模板是views目录下的模板文件。

.bowerrc
{
"directory":"public/libs"
}
directory就指定了将包安装到何处，有了这个文件，运行bower install jquery时，就会把jquery安装到public/libs目录下

app.use(express.static(path.join(__dirname, 'public')));这句话的意思是制定程序的静态文件目录
这个__dirname 已经是获取当前模块文件所在目录的完整绝对路径
css与页面加载需要的.js等静态资源文件放到 public 里
express 里用 express.static 将这些静态资源的请求指向到 public 目录.
public --> libs --> bootstrap/jquery
       --> js --> admin.js
jade里写script(src="/js/admin.js") script(src="/libs/jquery/dist/jquery.min.js")

Provisional headers are shown" 是什么意思?
这个警告的意思是说：请求的资源可能会被（扩展／或其他什么机制）屏蔽掉。
用 chrome://net-internals 来帮助你查找被屏蔽的请求以及可能的原因。

作为模板语言，Jade支持文件的包含include和扩展extend的，分别说明：include比较符合正常思维，
什么地方缺某部分包含进来即可；extend则使用先给出整体，再替换局部的模式。
一个块就是一个 Jade 的 block ，它将在子模板中实现，同时是支持递归的。
Jade 块如果没有内容，Jade 会添加默认内容，下面的代码默认会输出 block scripts, block content, 和 block foot.

PORT=4000 node app或者入口文件app.js来修改端口号

看下npm版本，2.6.1以上才支持npm update -g
更新npm版本的方法是
npm install npm@latest -g

npm init [-f|--force|-y|--yes]
init指令会询问一系列的问题，并将你的配置写成一个package.json文件。如果使用了-f|--force|-y|--yes这些参数，
那么会生成一个默认的package.json文件。

## node知识点系列二
win+r --> cmd --> d:--> cd nodejs\pro --> node server.js

npm install express 
以上命令会将 Express 框架安装在当前目录的 node_modules 目录中， node_modules 目录下会自动创建 express 目录。
以下几个重要的模块是需要与 express 框架一起安装的：
body-parser - node.js 中间件，用于处理 JSON, Raw, Text 和 URL 编码的数据。
cookie-parser - 这就是一个解析Cookie的工具。通过req.cookies可以取到传过来的cookie，并把它们转成对象。
multer - node.js 中间件，用于处理 enctype="multipart/form-data"（设置表单的MIME编码）的表单数据。

npm install body-parser 
npm install cookie-parser 
npm install multer 

npm install express -g//全局安装
npm install express-generator -g//express控制器

由于http协议是无状态的，所以服务器就无法跟踪会话了，采用cookie和session来弥补不足
当发起http请求，客户端发送当前域下的cookie，服务器解析cookie拿到信息，这些cookie存储在服务器里
而session是存储在服务端，当客户端发送过来的带有唯一标识就是sessionid，则去找出这个session，没有则创建一条会话session，
然后将cookie和请求的数据返回给客户端

name: 设置 cookie 中保存 session id 的字段名称，默认为connect.sid
secret: 通过设置 secret 来计算 hash 值并放在 cookie 中，使产生的 signedCookie 防篡改
resave: 如果为true，则每次请求都重新设置session的 cookie，假设你的cookie是10分钟过期，每次请求都会再设置10分钟
saveUninitialized: 如果为true, 则无论有没有session的cookie，每次请求都设置个session cookie

npm install morgan --save
var logger = require('morgan'); 
if ('development' === app.get('env')) //获取环境变量判断是开发环境{
  app.set('showStackError', true); // 显示错误信息
  app.use(logger(':method :url :status'); // 打印express路由的信息并预置格式
  app.locals.pretty = true; // 设置网页源码格式为非压缩，可读
  mongoose.set('debug', true);  //打开mongoDb调试模式
}

Query.populate(path, [select], [model], [match], [options])

exec()方法用于检索字符串中的正则表达式的匹配。

doctype html
html(lang='en')
  head
    title= hellojade
    style.
      section {
        margin: 20px auto;
        border:1px #eaeaea solid;
        width:80%;
        height:200px; }
      section div {
        margin: 10px; }
  body
  
npm install nrm -g
开发的npm registry 管理工具 nrm,  能够查看和切换当前使用的registry
$ nrm ls

* npm ---- https://registry.npmjs.org/
  cnpm --- http://r.cnpmjs.org/
  eu ----- http://registry.npmjs.eu/
  au ----- http://registry.npmjs.org.au/
  sl ----- http://npm.strongloop.com/
  nj ----- https://registry.nodejitsu.com/
$ nrm use cnpm //switch registry to cnpm

  Registry has been set to: http://r.cnpmjs.org/
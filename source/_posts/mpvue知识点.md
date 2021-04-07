---
title: mpvue知识点
date: 2021-04-07 14:49:16
tags: mpvue 微信小程序
categories: mpvue 微信小程序
---

### 1.第一次进入时的生命周期执行顺序
(1).created
(2).onload(option)，option作为页面路径中的参数，一个页面只会调用一次，接收页面参数，监听页面加载
(3).onshow
(4).onready，页面初次渲染完成，一个页面只会调用一次，代表页面已经准备妥当，可以和视图层进行交互
(5).mounted

在所有 页面 的组件内可以通过 this.$root.$mp.query 进行获取小程序在 page onLoad 时候传递的 options。要注意：写到created报错。

在所有的组件内可以通过 this.$root.$mp进行获取小程序在 app onLaunch/onShow 时候传递的 options。

<!--more-->

### 2.onload和onshow的区别
onLoad是在当前页面第一次加载的时候调用的，一个页面只会调用一次，而此时onShow也会调用的，所以执行onload一定会执行onshow,但执行onshow不一定就执行onload

### 3.关于后台销毁等情况的生命周期钩子函数执行
当 后台运行小程序然后再打开小程序，执行onhide和onshow；
当 杀死小程序再打开小程序，执行onunload和onload,以及onhide和onshow；
当 小程序onunload后，vue实例并没有被销毁，但是小程序的page实例被销毁了，所以vue内部的状态会被保存

### 4.常用的全局事件
onPullDownRefresh监听用户下拉动作
onReachBottom页面上拉触底事件的处理函数
onPageScroll 监听页面滚动事件
onShareAppMessage 用户点击右上角分享事件

### 5.view-scroll必须给固定的高度，不可以flex:1

### 6.拥有slot的组件无法自动编译成功

### 7.mpvue快速搭建项目
```
# 全局安装 vue-cli
$ npm install --global vue-cli

# 创建一个基于 mpvue-quickstart 模板的新项目
$ vue init mpvue/mpvue-quickstart my-project

# 安装依赖
$ cd my-project
$ npm install
# 启动构建
$ npm run dev
# 打包
$ npm run build
# 生成分析报告
$ npm run build --report
```
- 将dist/wx导入微信开发者工具

### 8.改造vuex，flyio
- src下新建store/index.js
````
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)

const store = new Vuex.Store({
  state: {
    count: 0
  },
  mutations: {
    increment: (state) => {
      const obj = state
      obj.count += 1
    },
    decrement: (state) => {
      const obj = state
      obj.count -= 1
    }
  }
})

export default store
````
- main.js下挂载到全局
````
import store from './store'
// App.store = store新版本不可用
Vue.prototype.$store = store
````
- 使用less语法
npm i less less-loader -D
- mpvue-entry: 集中式页面配置，自动生成各页面的入口文件，优化目录结构，支持新增页面热更新
- mpvue-router-patch: 在 mpvue 中使用 vue-router 兼容的路由写法
npm i mpvue-entry mpvue-router-patch -D

- npm i vant-weapp -S

- 使用flyio进行数据交互
npm i -S flyio

- this.$route.query获取的参数为字符串，当传递数组或对象时，需要用JSON.parse解析出来
- 小程序进入后台会清空vuex
- 路由文件修改需要npm run dev

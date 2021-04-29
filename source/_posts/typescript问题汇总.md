---
title: typescript问题汇总
date: 2021-04-22 17:58:23
tags: typescript
categories: typescript
---

###

![img](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/1e88098e19b74b79b8a51b6e16254cf1~tplv-k3u1fbpfcp-watermark.image?imageslim)

把 `"build": "vue-tsc --noEmit &&vite build"` 改成 `"build": "vite build"`, 就可以了,应该是 element-plus 没有将用到的@types 打包

### TypeScript 错误 property does not exist on type Object
在TypeScript中如果按JS的方式去获取对象属性，有时会提示形如Property 'value' does not exist on type 'Object'的错误。具体代码如下：
```
var obj: Object = Object.create(null);
obj.value = "value";//[ts] Property 'value' does not exist on type'Object'.
```
这是因为Typescript在执行代码检查时在该对象没有定义相应属性，遇到该报错有以下几种解决方式：

#### 1.将对象类型设置为any
这是是一种非常效率的解决办法，可以访问修改任何属性不会出现编译错误。具体代码如下：
```
var obj: any = Object.create(null);
obj.value = "value";
```

#### 2.通过字符方式获取对象属性
这种方式有些hack的感觉，但是依然能解决编译错误的问题。具体代码如下：
```
var obj: Object = Object.create(null);
obj["value"] = "value";
```

#### 3.通过接口定义对象所具有的属性
虽然较为繁琐，但却是最提倡的一种解决方式。通过接口声明对象后，所具有的属性值一目了然。具体代码如下：
```
var obj: ValueObject = Object.create(null);
obj.value = "value";

interface ValueObject {
  value?: string
}
```

#### 4.使用断言强制执行
声明断言后，编译器会按断言类型去执行。具体代码如下：
```
var obj: Object = Object.create(null);
(obj as any).value = "value";
```

<!--more-->

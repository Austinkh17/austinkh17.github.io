---
title: 基于vite搭建的vue3项目
date: 2021-04-26 09:38:18
tags:
- vue
- vite
categories: vue
---

### vite的优缺点

Vue CLI 优点|Vue CLI 缺点
:-|:-
经历过战斗考验，可靠|开发服务器速度与依赖数量成反比
与Vue 2兼容
可以捆绑任何类型的依赖关系	
插件生态系统	
可以针对不同的目标进行构建

Vite 优点|Vite 缺点
:-|:-
开发服务器比Webpack快10-100倍|只能针对现代浏览器（ES2015+）
将code-splitting作为优先事项|与CommonJS模块不完全兼容
|最小的脚手架不包括Vuex、路由器等
|不同的开发服务器与构建工具

<!--more-->

### vite的坑
#### 1.require不能使用
在使用vue-cli的时候,出于业务需要我们可能需要这样引用图片
```
<img :src="imgUrl" alt="">
  
imgUrl: require("../assets/images/bg.png")
```
如果图片的路径是动态的,我们也需要使用require引用,从而让框架在打包的时候分析出正确的路径
但这种图片引用方案在vite中并不能用,浏览器中会报require相关错误

这种报错自然可以理解,因为vite使用的是浏览器自带的module去解析js的,而require语法是node语法,自然报错,但是vite并没有给出合理的解决方案。

import导入图片，方案如下：
```
<template>
    <img :src="item.icon || defaultIcon" alt="icon" class="nav-icon" />
</template>

<script lang="ts">
import { defineComponent } from 'vue'
import defaultIcon from '@/assets/logo.png'

export default defineComponent({
  name: 'index',
  setup: () => {
    return { defaultIcon }
  }
});
</script>
```
### vue3实现Modal组件
https://segmentfault.com/a/1190000038928664
https://github.com/febobo/web-interview/issues/50  对比vue2实现

```
实现 API 形式
那么组件如何实现API形式调用Modal组件呢？

在Vue2中，我们可以借助Vue实例以及Vue.extend的方式获得组件实例，然后挂载到body上

import Modal from './Modal.vue';
const ComponentClass = Vue.extend(Modal);
const instance = new ComponentClass({ el: document.createElement("div") });
document.body.appendChild(instance.$el);
虽然Vue3移除了Vue.extend方法，但可以通过createVNode实现

import Modal from './Modal.vue';
const container = document.createElement('div');
const vnode = createVNode(Modal);
render(vnode, container);
const instance = vnode.component;
document.body.appendChild(container);
在Vue2中，可以通过this的形式调用全局 API

export default {
    install(vue) {
       vue.prototype.$create = create
    }
}
而在 Vue3 的 setup 中已经没有 this 概念了，需要调用app.config.globalProperties挂载到全局

export default {
    install(app) {
        app.config.globalProperties.$create = create
    }
}
```

```
// index.ts
import { App, createVNode, render } from 'vue';
import Modal from './Modal.vue';
import config from './config';

// 新增 Modal 的 install 方法，为了可以被 `app.use(Modal)`（Vue使用插件的的规则）
Modal.install = (app: App, options) => {
  // 可覆盖默认的全局配置
  Object.assign(config.props, options.props || {});

  // 注册全局组件 Modal
  app.component(Modal.name, Modal);
  
  // 注册全局 API
  app.config.globalProperties.$modal = {
    show({
      title = '',
      content = '',
      close = config.props!.close
    }) {
      const container = document.createElement('div');
      const vnode = createVNode(Modal);
      render(vnode, container);
      const instance = vnode.component;
      document.body.appendChild(container);
        
      // 获取实例的 props ，进行传递 props
      const { props } = instance;

      Object.assign(props, {
        isTeleport: false,
        // 在父组件上我们用 v-model 来控制显示，语法糖对应的 prop 为 modelValue
        modelValue: true,
        title,
        content,
        close
      });
    }
  };
};
export default Modal;
```
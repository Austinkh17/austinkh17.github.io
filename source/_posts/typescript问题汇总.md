---
title: typescript问题汇总
date: 2021-04-22 17:58:23
tags: typescript
categories: typescript
---

### 1.vue3打包报错提示nullable

![img](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/1e88098e19b74b79b8a51b6e16254cf1~tplv-k3u1fbpfcp-watermark.image?imageslim)

把 `"build": "vue-tsc --noEmit &&vite build"` 改成 `"build": "vite build"`, 就可以了,应该是 element-plus 没有将用到的@types 打包

### 2.TypeScript 错误 property does not exist on type Object
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

### 3.typescript 提示 Object is possibly ‘null‘ 的解决方法
`document.querySelector('.main-table').setAttribute('height', '300px')`
如上，我要设置某元素的高度，但typescript提示 Object is possibly ‘null’，是因为可能不存在选择元素的情况。

#### 解决方案一
最正确的解决方案，就是加null的判断
```
const table = document.querySelector('.main-table');
if (table) {
  table.setAttribute('height', '300px');
}
```
#### 解决方案二
使用断言方式，当然这是你能保证元素必定存在的情况
```
(document.querySelector('.main-table') as Element).setAttribute('height', '300px');
```
#### 解决方案三
这和解决方案原理一样，要判断null情况，但写法简单点，当然这是关闭eslint的情况下，否则eslint会提示错误
```
document.querySelector('.main-table')?.setAttribute('height', '300px');
```
这里使用了 ?. 符号，相当于&&，意思是先判断?前面的对象是否存在，存在情况下再执行后面的方法；
使用下面代码也是可以的：
```
const table = document.querySelector('.main-table');
table && table.setAttribute('height', '300px');
```

### Property 'innerText' does not exist on type 'Element'.Vetur(2339)
```
console.log('打印2', (dom as HTMLElement)?.innerText);
```

### 如何使用ref获取dom节点
```
<template>
  <div ref="divRef" />
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue';

export default defineComponent({
  setup() {
    const divRef = ref<HTMLElement>();
    return {
      divRef
    };
  },
});
</script>
```

### 如果使用Element-Plus的form组件，使用resetFields或者validate方法时，提示没有该属性
```
<template>
    <el-form
        :model="dialog.data"
        :rules="dialogRules"
        ref="formRef"
        label-width="120px"
    ></el-form>
</template>

<script lang="ts">
import { ElForm } from 'element-plus';
import { defineComponent, ref } from 'vue';

export default defineComponent({
  setup() {
    const formRef = ref<InstanceType<typeof ElForm>>();

    const resetForm = () => {
      formRef.value?.resetFields();
    };
    const confirm = () => {
      formRef.value?.validate((valid) => {
        if (valid) {
          // do
        }
      });
    };
    return {
      resetForm,
      confirm,
      formRef
    };
  },
});
</script>
```
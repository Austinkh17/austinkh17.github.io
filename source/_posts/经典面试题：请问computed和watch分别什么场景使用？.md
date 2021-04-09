---
title: 经典面试题：请问computed和watch分别什么场景使用？
date: 2021-04-08 14:11:06
tags: vue
categories: vue
---

回答：当模板中的某个值需要通过一个或多个数据计算得到时，就可以使用计算属性；监听属性主要是监听某个值发生变化后，对新值去进行逻辑处理。

<!--more-->

### computed和watch的区别
- 计算属性是基于它们的响应式依赖进行缓存的，只在相关响应式依赖发生改变时它们才会重新求值。相比之下，每当触发重新渲染时，调用方法将总会再次执行函数。注：下面的计算属性不会更新
```
computed: {
  now: function () {
    return Date.now()
  }
}
```
- computed的setter和getter
运行 vm.fullName = 'John Doe' 时，setter 会被调用，vm.firstName 和 vm.lastName 也会相应地被更新。
```
computed: {
  fullName: {
    get: function () {
      return this.firstName + ' ' + this.lastName
    },
    set: function (newValue) {
      var names = newValue.split(' ')
      this.firstName = names[0]
      this.lastName = names[names.length - 1]
    }
  }
}
```
- 当需要在数据变化时执行异步或开销较大的操作时，watch这个方式是最有用的。

### watch其他用法

#### 随时监听，随时取消
```
getFormInfo() {
    if(this.pageType !== 'add'){
        this.rawFormData = {
            username: 'kk',
            telephone: '13134345678'
        };
        this.formData = JSON.parse(JSON.stringify(this.rawFormData));
    }
    //  等表单数据回填之后，监听数据是否发生变化
    this.unwatch = this.$watch('formData', () => {
        this.isFormDataChange = true;
        this.updateCount++;
    }, {deep: true});
    this.pageType === 'detail' && this.unwatch();
}
```

#### 监听对象中的某个属性
```
watch: {
    'form.shareType':{
        handler(val) {
            if(val !== ''){
                this.$refs.form.validateField('shareCondition');
            }else{
                this.form.shareCondition = '';
            }
        },
        deep: true,
        immediate: true
    }
}
```


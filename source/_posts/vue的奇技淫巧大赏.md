---
title: vue的奇技淫巧大赏
date: 2021-06-15 10:46:00
tags: vue
categories: vue
---

### 1.子组件改变props变量
当父组件传来的props变量为引用类型时，子组件直接修改其属性，是可以改变父组件变量的，此时不会报错，但vue推荐的是组件间数据的单向流动，所以慎用！！

### 2.自定义props校验
```
props: {
    num: {
      default: 1,
      validator: function (value) {
          // 返回值为true则验证不通过，报错
          return [
            1, 2, 3, 4, 5
          ].indexOf(value) !== -1
    }
    }
  }
```
<!--more-->
### 3.watch的高级用法
#### 1.immediate:true的作用
在watch声明该变量时，就执行其handler方法，其效果相当于created初始化时就执行一次方法
#### 2.随时监听，随时关闭
```
getFormInfo() {
    if(this.pageType !== 'add'){
        this.rawFormData = {
            username: 'kk',
            telephone: '13134345678'
        };
        this.formData = JSON.parse(JSON.stringify(this.rawFormData));
    }
    //  编辑状态时，等表单数据回填之后，再监听数据是否发生变化
    this.unwatch = this.$watch('formData', () => {
        this.isFormDataChange = true;
        this.updateCount++;
    }, {deep: true});
    this.pageType === 'detail' && this.unwatch();
}
```

### 4.`computed`如何传参？
```
<div>{{a1(1)}}</div>

computed: {
    a1() {
        return function(a) {
            return a + 1;
        }
    }
}
```
但是，computed传入了额外的参数，失去了缓存的效果，不如直接使用methods

### 5.`hook`的使用场景
#### 1.同一组件内使用
```
const timer = setInterval(() => {}, 1000);
this.$once('hook:beforeDestroy', () => {
    clearInterval(timer);
    timer = null;
});
```

#### 2.在父子组件间使用
```
<child @hook:mounted="childMounted"></child>
```

### 6.`$emit`传递函数给父组件
```
子
this.$emit('change', data, (res) => {
    this.$message.success(res);
    this.loading = false;
});

父
async change(data, cb){
    await api();
    cb();
}
```

### 7.动态指令和动态参数
```
<template>
    <aButton @[someEvent]="handleSomeEvent()" :[someProps]="1000" />...
</template>
<script>
  ...
  data(){
    return{
      ...
      someEvent: someCondition ? "click" : "dbclick",
      someProps: someCondition ? "num" : "price"
    }
  },
  methods: {
    handleSomeEvent(){
      // handle some event
    }
  }  
</script>
```

### 8.el和$mounted的优先级
el优先级大于$mounted

### 9.data初始化数据
```
Object.assign(this.$data, this.$options.data.call(this));
```
data()中若使用了this来访问props或methods，在重置$data时，注意this.$options.data()的this指向，最好使用this.$options.data.call(this)
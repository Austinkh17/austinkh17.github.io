---
title: webstorm常用
date: 2021-04-07 14:48:16
tags: 工具
categories: 工具
---

## webstorm 快捷键

| 快捷键              | 说明                                               |
| ------------------- | -------------------------------------------------- |
| ctrl+f3             | 跳到光标处单词其他处，跳到最后处用 f3 来跳（必记） |
| ctrl+alt+左右箭头   | 后退/前进（必记）                                  |
| ctrl+shift+N        | 通过文件名快速查找工程内的文件（必记）             |
| alt+箭头            | 切换代码选项卡（必记）                             |
| alt+up/down         | 上一个/下一个方法（必记）                          |
| alt+鼠标选中一列    | 同时多行编辑（必记）                               |
| alt + shift + 上/下 | 可以移动整行代码                                   |
| shift+上下左右箭头  | 选中区域                                           |
| backspace           | 删除前面的                                         |
| delete              | 删除后面的                                         |
| ctrl+[/]            | 跳到对应标签的前后                                 |
| ctrl+shift+[/]      | 选中对应标签前后区域                               |
| 左箭头              | 按字母向左移动                                     |
| ctrl+左箭头         | 按单词向左移动                                     |
| shift+左箭头        | 选中左边区域                                       |
| ctrl+shift+z        | 撤销 ctrl+z 的操作                                 |
| ctrl+shift+alt+N    | 通过一个字符快速查找位置                           |
| ctrl+shift+A        | 快速查找并使用编辑器所有功能                       |
| ctrl+shift+F12      | 最大区域显示代码（会隐藏其他的功能界面模块）       |
| ctrl+alt+s          | 打开配置窗口                                       |
| ctrl+tab            | 切换代码选项卡（还要进行此选择，效率差些）         |
| ctrl+F4             | 关闭当前代码选项卡                                 |
| ctrl+alt+L          | 格式化代码                                         |
| shift+enter         | 重新开始一行（无论光标在哪个位置）                 |
| ctrl+G              | 到指定行的代码                                     |
| alt+f12             | 打开终端                                           |
| ctrl+shift+i        | 快速查看方法定义处                                 |

<!--more-->

- git annotate 查看 git 修改记录
- vcs->Local History -> Show History/ctrl+shift+A history 查看历史
- 右击代码选项卡上的文件，然后右键 -> spilt vertically(左右两屏)或者 spilt horizontally(上下两屏)
- webstorm 中 eslint 报错解决(eslint 安装包指向 node global 或者项目中的)
- TypeError: this.CliEngine is not a constructor,需要降低 eslint 版本`cnpm i -g eslint@3.19.0`
- Error: Failed to load plugin vue: Cannot find module 'eslint-plugin-vue'`cnpm i -g eslint-plugin-vue`
- Error: Failed to load plugin html: Cannot find module 'eslint-plugin-html'`cnpm i -g eslint-plugin-html`
- Error: eslint-plugin-html error: It seems that eslint is not loaded.`cnpm i -g eslint-plugin-html@3.0.0`
- 注意重启 webstorm
- file -> new -> edit file templates(可以修改新建文件模板，如下的 vue 文件)

```
<template>
    <div class="${COMPONENT_NAME}">#[[$END$]]#</div>
</template>

<script>
export default {
    name: "${COMPONENT_NAME}",
    components:{},
    data() {
        return {}
    },
    watch: {
        data:{
          handler(newValue, oldValue){
            console.log('this is watching data:',newValue, oldValue);
          },
          //deep:true,
          immediate:true
        }
    },
    computed: {
        data() {
          return this.data;
        }
    },
    methods: {
        name() {}
    },
    mounted(){
        console.log('this is mounted:',this.data);
    },
    onShow(){
        console.log('this is onShow:',this.data);
    },
    onUnload(){
        this.data = '';
        console.log('this is onUnload:',this.data);
    }
}
</script>

<style lang="less" scoped>
</style>
```

- live template(自定义动态模板)

```
log -> console.log('this is console test: ',this.data);
zs -> <!--公用组件：数据表格
      /**
      * 组件名称
      * @module 组件存放位置
      * @desc 组件描述
      * @author 组件作者
      * @date 2020年12月05日17:22:43
      * @param {Object} [title]    - 参数说明
      * @param {String} [columns] - 参数说明
      * @example 调用示例
      *  <hbTable :title="title" :columns="columns" :tableData="tableData"></hbTable>
      */
      -->
api -> api.unBind({}).then(res => {
           console.log('获取成功： ', res);
       }).catch(err => {
           console.log('获取失败： ', err);
       });
```

- file -> add to favorites 将该文件添加至收藏夹 alt+2 展示收藏夹列表
- webstrom 中 JavaScript 代码样式设置中启用 Use Tab 字符 ->解决 eslint no mixed spaces and tabs 问题
- 获取数据时，申明临时变量，一次性赋值，防止多次渲染

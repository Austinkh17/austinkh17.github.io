---
title: vscode新建快捷片段
date: 2021-04-07 17:10:57
tags: vscode 工具
categories: vscode
---

### vsocde 新建快捷片段 snippets

打开文件--首选项--用户片段，添加快捷片段在 json 文件中

```
"console.log": {
    "scope": "javascript,typescript,vue",
    "prefix": "log",
    "body": [
        "console.log('111', $1);"
    ],
    "description": "console.log"
},
"vue": {
    "scope": "vue",
    "prefix": "vue",
    "body": [
        "<template>",
        "	<div class=\"index\">$1</div>",
        "</template>\n",
        "<script>",
        "export default {",
        "	name: 'index',",
        "	data() {",
        "		return {};",
        "	},",
        "	methods() {}",
        "};",
        "</script>"
    ],
    "description": "vue snippets代码片段"
},
"api": {
    "scope": "javascript,typescript,vue",
    "prefix": "apii",
    "body": [
        "let params = {",
        "	date: this.date",
        "};",
        "let res = await api.getListApi(params);",
        "if (res.code == 0) {",
        "	let res = res.data;",
        "} else {",
        "	this.message.error(res.message);",
        "}"
    ],
    "description": "api获取接口"
},
"note": {
    "scope": "javascript,typescript,vue",
    "prefix": "note",
    "body": [
        "/**",
        "* @description 表格行添加类",
        "* @param {Object} row 行对象",
        "* @param {Number} rowIndex 行序号",
        "* @return {String}  类名",
        "*/"
    ],
    "description": "注释"
}
```

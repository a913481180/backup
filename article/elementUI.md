---
title: element UI
date: 2021-11-12 20:23:33
categories:
  - web
tags:
  - web
---

## element 的表单校验输入框有值但校验未通过

- `:model="ruleForm"` 绑定的`ruleForm`值是否挂载成功并且操作的是否是这个表单。

- `:rules="rules"` 校验的规则格式绑定的`rules`是否定义并且格式正确为对象数组。

- `el-form-item中的`prop="name"`是否和rules中的`name: [ { required: true, message: '请输入活动名称', trigger: 'blur' }, ], `的名称一致，两个`name`相同的，element 的校验就是根据这个 prop 找对应的输入框的。

- `<el-input v-model="ruleForm.name"></el-input>` 的`v-model="ruleForm.name"`确保对象`ruleForm`中有`name`这个属性！

## el-table 格式化 el-table-column 内容

- v-if 判断

```html
<template slot-scope="scope">
  <span v-if="scope.row.xxxx == 0"></span>
</template>
```

- 使用 formatter 属性

```html
<el-table-column
  prop="xxx"
  :formatter="function(row, column){xxxx}"
></el-table-column>
```

## ElementUI 的 el-table 默认内容不换行，让 el-table-column 根据占位符\n 换行

添加 css 样式

```css
.el-table >>> .cell {
  white-space: pre-line;
}
```

## 点击时获取列表的值，可通过给节点添加 data 自定义属性，通过点击事件 e.currentTarget.dataset 获取属性值

## element 获取 table 表格行数据，可通过 template 设置 slot-scope=“scope”属性，在传参时使用 scope.row.xxxx 拿到当前行的数据

## 当只有⼀个 el-input 的时候，可以⽤ elementUI 的⾃带的回车键触发提交事件但是有时候会同时触发刷新页⾯，这样可以在 el-form 上添加@submit.native.prevent 来解决

## element el-input 的 autofocus 失效问题解决

> autofocus 是 input 的原生属性,饿了么组件也支持这种方法, 但是 input 外面还有其他组件, 导致 autofocus 失效, 只能手动调用 focus 方法:组件上绑定 ref，手动调用 focus()

## elementUI 选择器 select 获取选中对象

```html
<!-- 有些情况下需要获取选中对象的全部数据，添加 value-key="id" ，value-key 为唯一性标识, 将 value 绑定为item -->
<el-select v-model="area" value-key="id" placeholder="请选择区域">
  <el-option
    v-for="item in areaLsit"
    :key="item.id"
    :label="item.areaName"
    :value="item"
  >
  </el-option>
</el-select>
```

## vue css >>> /deep/穿透，深度查询样式在 chrome89 edge 新版本中无效问题

大概理解应该是`/deep/`基本上无效了，直接替换成“”空格，或者`>>>`代替处理；

于是，在项目中全局替换`/deep/`为空格，但是部分页面会有错乱问题，于是将替换为空格后，仍然无效的样式部分，添加`>>>`,居然可以了，但是对于 Sass Less 之类的预处理器是无法正确解析 `>>>`的，所以要保证样式是 css 文件；

还有一点要强调，`>>>`在 html 单页面下貌似无效，在 vue 项目中有效；

项目问题基本解决后，做一下汇总的情况下，上网查看关于样式穿透，发现除了`>>>`、`/deep/`外，还有一个`::v-deep`，写法如下：

```html
<style lang="scss" scoped>
  /用法1/ .a {
    ::v-deep .b {
    }
  }
  /用法2/ .a ::v-deep .b {
  }
</style>
```

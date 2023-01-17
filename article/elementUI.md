---
title: element UI
date: 2021-11-12 20:23:33
categories: 
- web
---

#  element UI

## element的表单校验输入框有值但校验未通过：

- `:model="ruleForm"` 绑定的`ruleForm`值是否挂载成功并且操作的是否是这个表单。

- `:rules="rules"` 校验的规则格式绑定的`rules`是否定义并且格式正确为对象数组。

- `el-form-item中的`prop="name"`是否和rules中的`name: [ { required: true, message: '请输入活动名称', trigger: 'blur' }, ], `的名称一致，两个`name`相同的，element的校验就是根据这个prop找对应的输入框的。

- `<el-input v-model="ruleForm.name"></el-input>` 的`v-model="ruleForm.name"`确保对象`ruleForm`中有`name`这个属性！

## el-table格式化el-table-column内容：

- v-if判断
```
<template slot-scope="scope">
        <span v-if="scope.row.xxxx == 0"></span>
    </template>
```
- 使用formatter属性
```
<el-table-column prop="xxx"   :formatter="function(row, column){xxxx}">
```
## ElementUI的el-table 默认内容不换行，让el-table-column根据占位符\n换行：

添加css样式
```
.el-table >>>.cell {
    white-space: pre-line;
}
```

## 点击时获取列表的值，可通过给节点添加data自定义属性，通过点击事件e.currentTarget.dataset获取属性值
## element获取table表格行数据，可通过template设置slot-scope=“scope”属性，在传参时使用scope.row.xxxx拿到当前行的数据
## 当只有⼀个el-input的时候，可以⽤elementUI的⾃带的回车键触发提交事件但是有时候会同时触发刷新页⾯，这样可以在el-form上添加@submit.native.prevent来解决

## element el-input的autofocus失效问题解决
>autofocus是input的原生属性,饿了么组件也支持这种方法, 但是input外面还有其他组件, 导致autofocus失效, 只能手动调用focus方法:组件上绑定ref，手动调用focus()

## elementUI选择器select获取选中对象

```
有些情况下需要获取选中对象的全部数据，添加 value-key="id" ，value-key 为唯一性标识, 将 value 绑定为item
<el-select v-model="area" value-key="id" placeholder="请选择区域">
    <el-option v-for="item in areaLsit" :key="item.id" :label="item.areaName" :value="item">
    </el-option>
</el-select>
```
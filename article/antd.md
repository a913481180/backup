---
title: Ant-Design
date: 2021-02-12 20:11:33
categories:
  - web
tags:
  - web
---

## antd

### Table

- 缺少 key，提示错误

添加 rowkey 属性指定唯一 key

```html
<Table rowkey={'id'}>
</Table>
```

### Checkbox

- Checkbox.Group 初始值无效
  useState()初始为空，Checkbox.Group 绑定后不再更新

### 路由懒加载

- lazy
  Suspense 中的 fallback 渲染的组件不能使用懒加载

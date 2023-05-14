---
title: Ant-Design
date: 2021-02-12 20:11:33
categories:
- web
---

## antd

### Table

- 缺少key，提示错误

添加rowkey属性指定唯一key

```html
<Table rowkey={'id'}>
</Table>
```

### Checkbox

- Checkbox.Group 初始值无效
useState()初始为空，Checkbox.Group 绑定后不再更新

### 路由懒加载

- lazy
Suspense中的fallback渲染的组件不能使用懒加载

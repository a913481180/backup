---
title: selenium
date: 2022-11-12 20:23:33
categories: 
- web
---

#  selenium

## 在使用selenium对当前窗口操作，报错no such window: target window already closed

- `:model="ruleForm"` 绑定的`ruleForm`值是否挂载成功并且操作的是否是这个表单。

- `:rules="rules"` 校验的规则格式绑定的`rules`是否定义并且格式正确为对象数组。

- `el-form-item中的`prop="name"`是否和rules中的`name: [ { required: true, message: '请输入活动名称', trigger: 'blur' }, ], `的名称一致，两个`name`相同的，element的校验就是根据这个prop找对应的输入框的。

- `<el-input v-model="ruleForm.name"></el-input>` 的`v-model="ruleForm.name"`确保对象`ruleForm`中有`name`这个属性！

## selenium 中执行使用js脚本 
- 使用JS参数传参方式
在使用Javascript语句时，还可以动态传入参数或元素对象，Js 语句中使用占位符“argument[n]”来表示取第几个参数，如

```
js = "arguments[0].setAttribute('style', arguments[1]);
element = driver.find_element_by_id("kw")
style = "background: red; border: 2px solid yellow;"
driver.execute_script(js, element, style)
```
这里埋设了两个参数，一个是元素对象，另一个是样式字符串。


- 获取js返回值
```
let a=driver.execute_script('return arguments[0].value')
```


## selenium 运行报错：element not interactable

- 解决一：
写自动化时，没有考虑到等待时间，有时候代码运行过快，web界面反应不过来，在这个报错的元素前添加一个等待时间，根据个人时间定等待时间
这个元素被隐藏了
- 解决二:
1.打个比方，有一个退出按钮，你找到元素了，也定位了，但无法click（），这个可能是被隐藏了，要将鼠标显示放在这个界面的位置，元素才能被找到click（）
2.元素被隐藏了，web界面css元素被隐藏
## Selenium如何使用句柄方式切换窗口
获取当前页面句柄：driver.current_window_handle

　　· 获取所有页面句柄：driver.window_handles

　　通过句柄，我们可以进行窗口的切换。

　　· 切换窗口：
  driver.switch_to.window(handle)
## element获取table表格行数据，可通过template设置slot-scope=“scope”属性，在传参时使用scope.row.xxxx拿到当前行的数据
## 当只有⼀个el-input的时候，可以⽤elementUI的⾃带的回车键触发提交事件但是有时候会同时触发刷新页⾯，这样可以在el-form上添加@submit.native.prevent来解决

## element el-input的autofocus失效问题解决
>autofocus是input的原生属性,饿了么组件也支持这种方法, 但是input外面还有其他组件, 导致autofocus失效, 只能手动调用focus方法:组件上绑定ref，手动调用focus()
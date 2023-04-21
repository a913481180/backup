---
title: selenium
date: 2022-11-12 20:23:33
categories:
  - web
---

# selenium

## 在使用 selenium 对当前窗口操作，报错 no such window: target window already closed

关闭了新窗口后，浏览器默认显示了百度首页，但是当我们操作百度首页的时候，报了错误：没有窗口，目标窗口已关闭，因为当前窗口句柄在第二个上面，但是我们给他删掉了，在操作第二个，就会报错，将用 switch_to.window()句柄切换到第一个标签即可

## selenium 中执行使用 js 脚本

- 使用 JS 参数传参方式
  在使用 Javascript 语句时，还可以动态传入参数或元素对象，Js 语句中使用占位符“argument[n]”来表示取第几个参数，如

```js
js = "arguments[0].setAttribute('style', arguments[1]);
element = driver.find_element_by_id("kw")
style = "background: red; border: 2px solid yellow;"
driver.execute_script(js, element, style)
```

这里埋设了两个参数，一个是元素对象，另一个是样式字符串。

- 获取 js 返回值

```js
let a = driver.execute_script("return arguments[0].value");
```

## selenium 运行报错：element not interactable

- 解决一：
  写自动化时，没有考虑到等待时间，有时候代码运行过快，web 界面反应不过来，在这个报错的元素前添加一个等待时间，根据个人时间定等待时间
  这个元素被隐藏了
- 解决二:
  1. 打个比方，有一个退出按钮，你找到元素了，也定位了，但无法 click（），这个可能是被隐藏了，要将鼠标显示放在这个界面的位置，元素才能被找到 click（）
  2. 元素被隐藏了，web 界面 css 元素被隐藏

## Selenium 如何使用句柄方式切换窗口

获取当前页面句柄：`driver.current_window_handle`

· 获取所有页面句柄：`driver.window_handles`

通过句柄，我们可以进行窗口的切换。

· 切换窗口：
`driver.switch_to.window(handle)`

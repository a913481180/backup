---
title: JS实例
date: 2021-05-22 20:22:11
categories:
- web 
---

# 实例

## 判断是否是pc设备

```
    function IsPC() {
        var userAgentInfo = navigator.userAgent;
        console.log(userAgentInfo);
        var Agents = ["Android", "iPhone","SymbianOS", "Windows Phone", "iPod"];
        var flag = true;
        for (var v = 0; v < Agents.length; v++) {
            if (userAgentInfo.indexOf(Agents[v]) > 0) {
                flag = false;
                break;
            }
        }
        if(window.screen.width>=768){
             flag = true;
        }
        return flag;
    }
```


## 轮播图

```
window.onload()=function(){
var oUl1=document.getElementById("ul1");
var oDiv1=document.getElementById("div1");
//将图片添加到末尾
oUl1.innerHTML+=oUl1.innerHTML;
//重新设置一下ul的宽
oUl1.style.width=220*8+"px";
setInterval(function(){
//让ul向左运动一个图片位置
startMove(oUl1,{left:oUl1.offsetLeft-220},function(){
if(oUl1.offsetLeft<=-oUl1.offsetWidth/2){
oUl1.style.left="opx";
}
})
},2000);
```


### 拖拽

- mousedown 

记录鼠标按下的位置和被拖拽物体相对距离

```
//clientX为鼠标位置，offsetLeft为盒子位置
var offsetX=e.clientX-node.offsetLeft;
var offsetY=e.clientY-node.offsetTop;
```


- mousemove

```
//鼠标移动的距离减去相对距离=盒子移动的距离
node.style.left=e.clientX-offsetX+'px';
node.style.top=e.clientY-offsetY+'px';
```

- mouseup

取消拖拽

*实例：*

```
//父盒子与子盒子要有定位
window.onload=function(){
var oDiv=document.getElementById("div1");
oDiv.onmousedown=function(ev){
var e=ev||window.event;
var offsetX=e.clientX-oDiv.offsetLeft;
var offsetY=e.clientY-oDiv.offsetTop;

document.onmousemove=function(ev){
	var e=ev||window.event;
oDiv.style.left=e.clientX-offsetX+"px";
oDiv.style.top=e.clientY-offsetY+"px";
}}
//绑定到document上，防止鼠标移动过快,离开节点
document.onmouseup=function(){
document.onmousemove=null;
}
}
```
- 动态创建表单并提交
```
// JavaScript 构建一个 form 
function MakeForm() 
{ 
  // 创建一个 form 
  var form1 = document.createElement("form"); 
  form1.id = "form1"; 
  form1.name = "form1"; 
  // 添加到 body 中 
  document.body.appendChild(form1); 
  // 创建一个输入 
  var input = document.createElement("input"); 
  // 设置相应参数 
  input.type = "text"; 
  input.name = "value1"; 
  input.value = "1234567"; 
  // 将该输入框插入到 form 中 
  form1.appendChild(input); 
  // form 的提交方式 
  form1.method = "POST"; 
  // form 提交路径 
  form1.action = "/servlet/info"; 
  // 对该 form 执行提交 
  form1.submit(); 
  // 删除该 form 
  document.body.removeChild(form1); 
} 
```



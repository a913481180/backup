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

document.onmouseup=function(){
document.onmousemove=null;
}
}
```



---
title: JS实例
date: 2020-11-22 20:22:11
catergories:
- study
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


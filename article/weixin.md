---
title: 微信小程序
date: 2021-01-11 20:22:22
categories:
- study
---

# 结构

- app.json      //配置

```
{"page":[
    "page/index/index“，
    "page下页面路径“
]}
"window":{
   // 导航配置
}
```

- app.js  //注册小程序应用

```
App({.....})
```

- app.wxss    //全局公共样式

```
 page{       //小程序会在所有页面加一个page ，因此应设置页面高度为100%
            height:100%;
        }
```

- 页面：
page  
\\index  
\\-index.js  

```
     page({
     //初始化数据
         data: {
             msg: 'hello',
             userInfo:{
                 //存放用户信息
             },
         },

        //事件绑定
        handleParent(){            
            console.log('parent')
        },
        handleChild(){
            console.log('child')
        },

        //跳转至logs页面
        toLogs(){                
            //需要使用api
            wx,navigateTo({         //保留当前页面跳转到另一个页面
                url:'/page/logs/logs',   //相对路径
            })
        },

         //生命周期函数：监听页面加载，最先执行
         onLoad:function(options){
             console.log('onLoad()')        //打印

             //修改msg数据：this.setData
             consile.log(this)  //this表示当前页面的实例对象
             consile.log(this.data.msg)   
              this.setData({
                 msg:'after'
             })

             //授权以后获取用户信息
             wx.gerUserInfo({
                 succes:() => {
                     condole.log(res);
                     //成功后更新数据
                     this.setData({
                         userInfo: res.userInfo
                     })
                 },
                 fail:() =>{
                     condole.log(err);
                 },
             })
       },

    //获取用户信息
       handleGetUserInfo(res){      //形参用于判断
           console.log(res)
        if(res.detail.userInfo){
            //修改userInfo的状态数据
            thsi.setData({
                userInfo: res.detail.userInfo
            })
        }
        
       },
.....
                 })
```
-index.wxml

```
      <view class="indexcontainer">   <!--//样式-->
        
        <image wx:if='{{userInfo.avatarUrl}}' class="avatarUrl" src="{{userInfo.avatarUrl}}"> </image>    <!--图片路径-->
        <botton wx:else bindgetuserinfo='handleGetUserInfo' open-type="getUserInfo">获取用户信息</botton>
        <text class="userName"> 文字文字 </text>
        <text>{{msg}}</text>    <!--//使用数据-->
        <!--  事件绑定  -->
    <!--    <view class="hello" bindtap="handleParent">  <text bindtap='handleChild'>hello</text>   </view>  -->

      <view class="hello" bindtap="toLogs">  
        <text>hello</text>  
       </view> 
    </view>
```

-index.wxss      //样式

```
             .indexContainer{
  display: flex;
  flex-direction: column;
  align-items: center;
  background: #f0f;       /*只有16进制*/
  height: 100%;
}
.avatarUrl{         /*头像尺寸样式*/
  width: 200rpx;
  height:200rpx;
  border-radius: 50%;      /*圆角*/
  margin: 100rpx 0;
}
.usrName{
  font-size: 32rpx;
  margin: 100rpx 0;
}
.hello{
  width:300rpx;
  height: 80rpx;
  line-height: 80rpx;
  text-align: center;
  font-size: 28rpx;
  border: 1rpx solid #333;
  border-radius:10rpx
}
```
-index.json  

- 日志：
log/logs.json
```
{
    "usingComponens":{},
    "navigationBarTitleText": "日志“    //最顶部栏文字
}
```

# 绑定事件
bind：冒泡事件
catch：非冒泡

# 条件渲染

```
wx:if='条件'
wx:else
wx:elif='条件'
```

---
# 功能

1. 导航栏
在app.json文件内：
```
{
    "pages":[
        "路径",
        "路径"
    ],
    "window":{
        ...
    },
    "tabBar":{
        "backgroundColor":"#f6f6f5",
        "list":[{
            "pagePath":"路径",
            "text":"文字"
        },
        {
            "pagePath":"路径",
            "text":"文字"
 
        }
        ]
    }
}

```

2. input输入计算

在index.js文件夹内
```
page({
    data:{
        first:"",
        second:""
    },
    first:function(e){
        var n=e.detail.value;
        if(!isNaN(n)){
            this.setData({
                first:n
            })
        }
    },
 calculate:function(){
    var n=this.data.first+1;
    this.setData({
        second:n
    }
    )
},
....
})
```

在index.wxml文件中
```
<input bindinput="first" placeholder="文字"></input>
<input value="second" placeholder="文字"disabled></input>
.....
```

3. 布局

在index.wxml文件内
```

```

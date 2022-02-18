---
title: 微信小程序
date: 2021-01-11 20:22:22
categories:
- web
---

## 结构

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
- index.wxml

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

- index.wxss      //样式

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
- index.json  

- 日志：
log/logs.json
```
{
    "usingComponens":{},
    "navigationBarTitleText": "日志“    //最顶部栏文字
}
```

### 绑定事件
bind：冒泡事件
catch：非冒泡

### 条件渲染

```
wx:if='条件'
wx:else
wx:elif='条件'
```

---
### 布局

### 常用功能

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

2. input

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
//placeholder：提示
//bindiniput:输入
//bindconfirm:回车触发事件
//value:输入框内容
<input class="search_input" placeholder="Search" bindinput="text_input" bindconfirm="searchtap"value="{{search_key}}"/>

.....
```

- textarea

```
//auto-focus: 自动聚焦
//maxlength="-1":不限长度
 <textarea auto-focus placeholder="Text"maxlength="-1" bindinput="text_input" value="{{record}}"/>
```

- 选择照片

```
 wx.chooseImage({
      count: 1, //数量 默认9
      sizeType: ['original', 'compressed'], //图片质量：原图，压缩
      sourceType: ['album', 'camera'],      //拍照，相册
      success: (res) => {
        //成功后把地址放入全局变量中,
        app.data.picpath = res.tempFilePaths[0];
        //跳转到图片裁剪界面，文字识别界面
        wx.navigateTo({
          url: '/pages/ocr/ocr',
        })

      }
    })

```

- 跳转界面 

```
 //返回界面
            wx.navigateBack({
              delta: 0,     //0表示上一层
            });


 //关闭当前页面跳转到
  wx.redirectTo({
    url: '/pages/index/index',
  })
```

- 获取列表index

wx:for控制属性绑定一个数组进行列表渲染。
>（当前项的下标变量名默认为 index，数组当前项的变量名默认为 item，改名：使用 wx:for-item=“jjj" 可以指定数组当前元素的变量名，
>使用 wx:for-index =”aa“可以指定数组当前下标的变量名。
这个注意了。wx:key还是要加上的，不然一直报这个提示错误：如果列表中项目的位置会动态改变或者有新的项目添加到列表中，并且希望列表中的项目保持自己的特征和状态（如 <input/> 中的输入内容，<switch/> 的选中状态），需要使用 wx:key 来指定列表中项目的唯一的标识符。

>wx:key 的值以两种形式提供
字符串，代表在 for 循环的 array 中 item 的某个 property，该 property 的值需要是列表中唯一的字符串或数字，且不能动态改变。
>保留关键字 *this 代表在 for 循环中的 item 本身，这种表示需要 item 本身是一个唯一的字符串或者数字，如：
当数据改变触发渲染层重新渲染的时候，会校正带有 key 的组件，框架会确保他们被重新排序，而不是重新创建，以确保使组件保持自身的状态，并且提高列表渲染时的效率。如不提供 wx:key，会报一个 warning， 如果明确知道该列表是静态，或者不必关注其顺序，可以选择忽略。）

```
wxml:
 <view class="touch-item {{item.isTouchMove ? 'touch-move-active' : ''}}" data-index="{{index}}" bindtouchstart="touchstart" bindtouchmove="touchmove" wx:for="{{item1}}"   wx:key="i">
 <view class="del" catchtap="del" style="border-radius:20rpx 0 0 20rpx;"  data-detail="{{item.content.text}} " data-index="{{index}}">删除</view></view>
js:
 //列表删除
del(e){
    this.data.item1.splice(e.currentTarget.dataset.index, 1)
    this.setData({
     item1: this.data.item1
    })}
```

- 搜索

```
for(var i=0,j=0;i<app.data.item2.length;i++){
   if(app.data.item2[i].delete==0){
     //模糊搜索
  if(app.data.item2[i].content.text.indexOf(this.data.search_key)!=-1){
    this.data.item1[j]=app.data.item2[i];
    j++;
  }
}
}
```

- 复制

```
/*#######复制功能########################*/
copy: function (e) {
  var that = this;
  wx.setClipboardData({
   data: e.currentTarget.dataset.detail,
   success: function (res) {
   }
  });
 },
```

- 云数据库

```

wx.cloud.init({         //初始化
  env:"cloud1-3gerlsz2682ceb37",
  traceUser:true,
});
const db=wx.cloud.database();

Page({ 
    var that = this;//定义到这里，让that先获取到外面方法的this
    db.collection('my_data').where({_openid:app.data.useropenid}).get({
      success: function(res) {
    //    that.setData({item2:res.data[0].array})
    //用户数据放入全局变量中
    app.data.item2=res.data[0].array; 
    
    //更新数据 ，添加新条目
  db.collection('my_data')
  .doc(app.data.useropenid)     //_id
  .update({
    data:{
     array: _.unshift([{
      content:{
        text: this.data.record,
        time: this.data.record_time,
      },
      isTouchMove:false,
      delete:0
    }])}
  })
})
```

- 获取时间

```
util.js:
const formatTime = date => {
  const year = date.getFullYear()
  const month = date.getMonth() + 1
  const day = date.getDate()
  const hour = date.getHours()
  const minute = date.getMinutes()
  const second = date.getSeconds()

  return [year, month, day].map(formatNumber).join('-') + ' ' + [hour, minute, second].map(formatNumber).join(':')
}

const formatNumber = n => {
  n = n.toString()
  return n[1] ? n : '0' + n
}

module.exports = {
  formatTime: formatTime
}
 js:
 var util = require('../../util/util.js');
 Page({
  //获取时间
  const _ = db.command;
  var time = util.formatTime(new Date());
  this.setData({
    record_time:time
  })
  
  })
```

3. 布局

- 固定按钮

```
wxml:
<view class="round-click" bindtap="new_goto">+</view>
wxss:
  .round-click{

    height: 120rpx;

    width: 120rpx;
    background-color:rgb(246, 246, 246,0.9);
    border-radius: 100%;
    position: fixed;
    bottom: 100rpx;
    right: 20rpx;

    display: flex;
    align-items: center;
	font-size:70rpx;
    justify-content: center;
    z-index: 9;

}
```


wx.request发起 HTTPS 网络请求,data请求的参数,success接口调用成功的回调函数	

AudioContext对象用于与audio组件绑定，通过它可以在逻辑层操作视图层的audio组件。可以用wx.createAudioContext(audioId)接口创建并返回audio的上下文对象AudioContext。


## 跳转

1. 利用小程序提供的 API 跳转：

```
// 保留当前页面，跳转到应用内的某个页面，使用wx.navigateBack可以返回到原页面。
// 注意：调用 navigateTo 跳转时，调用该方法的页面会被加入堆栈，但是 redirectTo 
wx.navigateTo({
  url: 'page/home/home?user_id=111'
})
复制代码
// 关闭当前页面，返回上一页面或多级页面。可通过 getCurrentPages() 获取当前的页面栈，决定需要返回几层。

wx.navigateTo({
  url: 'page/home/home?user_id=111'　　// 页面 A
})
wx.navigateTo({
  url: 'page/detail/detail?product_id=222'　　// 页面 B
})
// 跳转到页面 A
wx.navigateBack({
  delta: 2
})
复制代码
// 关闭当前页面，跳转到应用内的某个页面。
wx.redirectTo({
  url: 'page/home/home?user_id=111'
})
// 跳转到tabBar页面（在app.json中注册过的tabBar页面），同时关闭其他非tabBar页面。
wx.switchTab({
  url: 'page/index/index'
})
// 关闭所有页面，打开到应用内的某个页面。
wx.reLanch({
  url: 'page/home/home?user_id=111'
})
```

原理：wxml中不能直接使用较高级的js语法，如‘.toFixed’，‘toString()’，但可以通过引入wxs模块实现效果

```
1.新建`filter.wxs`
var filters = {    
    toFix: function (value) {       
        return value.toFixed(2) // 此处2为保留两位小数，保留几位小数，这里写几    
    },
    toStr: function (value) {       
        return value.toString()
    },
    toNum: function (value) {       
        return value.toNumber()
    },
}
 
module.exports = {   
    toFix: filters.toFix,
    toStr: filters.toStr,
    toNum: filters.toNum,//暴露接口调用
}
2.WXML中引入WXS
<wxs module="filters" src="../../utils/filters.wxs"></wxs>
3.在WXML中使用
<view>
{{ filters.toFix(price) }}
</view>
　　其他如toString(),toNumber()也可用此类似方法
```

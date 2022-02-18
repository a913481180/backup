---
title: echart
date: 2021-11-12 22:23:33
categories: 
- web
---

# echart

1. echarts的图表是画在canvas上的， 可以通过grid设置来调整图表与canvas的间距：grid:{ x:50, y:50,x2:50,  y2:60,}
2. 通过回调函数判断是否是第一个数据（params.dataIndex 的值）来决定是否显示相应的内容：formatter:(n)=>{return n.dataIndex}
3. echarts3.0已支持多标题的设置

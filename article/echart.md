---
title: echarts/d3.js
date: 2022-12-12 22:23:33
categories:
  - web
---

## echarts

1. echarts 的图表是画在 canvas 上的， 可以通过 grid 设置来调整图表与 canvas 的间距：grid:{ x:50, y:50,x2:50, y2:60,}
2. 通过回调函数判断是否是第一个数据（params.dataIndex 的值）来决定是否显示相应的内容：formatter:(n)=>{return n.dataIndex}
3. echarts3.0 已支持多标题的设置
4. Echarts 设置 tooltip 层级 z-index
   tooltip 有一个 extraCssText 属性。所以可以设置该属性来修改它的样式：

```
tooltip:{extraCssText:'z-index:2'}
```

## svg 可缩放矢量图形（Scalable Vector Graphics）

### SVG CSS 属性

SVG 元素具有以下可以设置的 CSS 属性。并非所有元素都具有所有这些 CSS 属性。因此，CSS 属性被分为针对不同元素的多个表。

#### 形状 CSS 属性

path 元素和其他形状元素的 CSS 属性：

| CSS 属性          | 描述                                                                     |
| ----------------- | ------------------------------------------------------------------------ |
| fill              | 设置形状的填充颜色。                                                     |
| fill-opacity      | 设置形状的填充不透明度。                                                 |
| fill-rule         | 设置形状的填充规则。                                                     |
| marker            | 设置沿此形状的线（边）使用的标记。                                       |
| marker-start      | 设置沿此形状的线（边）使用的开始标记。                                   |
| marker-mid        | 设置沿此形状的线（边）使用的中间标记。                                   |
| marker-end        | 设置沿此形状的线（边）使用的结束标记。                                   |
| stroke            | 设置用于绘制此形状轮廓的笔触（线条）颜色。                               |
| stroke-dasharray  | 设置用于绘制此形状轮廓的笔划（虚线）。                                   |
| stroke-dashoffset | 设置用于绘制此形状轮廓的笔划（直线）破折号偏移量。                       |
| stroke-linecap    | 设置用于绘制此形状轮廓的笔划（线）线帽。有效值为 round，butt 和 square。 |
| stroke-miterlimit | 设置用于绘制此形状轮廓的笔划（直线）斜接限制。                           |
| stroke-opacity    | 设置用于绘制此形状轮廓的笔触（直线）不透明度。                           |
| stroke-width      | 设置用于绘制此形状轮廓的笔触（线条）宽度。                               |
| text-rendering    | 设置用于绘制此形状轮廓的文本渲染。                                       |

文字 CSS 属性
text 元素的 CSS 属性：

| CSS 属性                     | 描述                                   |
| ---------------------------- | -------------------------------------- |
| alignment-baseline           | 设置文本与其对齐方式 x 和 y 坐标。     |
| baseline-shift               | 设置用于呈现文本的基线偏移。           |
| dominant-baseline            | 设置主要基线。                         |
| glyph-orientation-horizontal | 设置水平字形方向。                     |
| glyph-orientation-vertical   | 设置垂直字形方向。                     |
| kerning                      | 设置渲染文本的字距（字距是字母间距）。 |

渐变 CSS 属性
SVG 渐变的 CSS 属性：

| CSS 属性     | 描述                                               |
| ------------ | -------------------------------------------------- |
| stop-color   | 设置在渐变中使用的 stop 元素中使用的终止色。       |
| stop-opacity | 设置 stop 在渐变中使用的元素中使用的停止不透明度。 |

#### class

```html
<svg
  xmlns="http://www.w3.org/2000/svg"
  xmlns:xlink="http://www.w3.org/1999/xlink"
>
  <style type="text/css">
      <![CDATA[

        circle.myGreen {
           stroke: #006600;
           fill:   #00cc00;
        }
       circle.myRed {
       stroke: #660000;
       fill:   #cc0000;
    }

      ]]>
  </style>

  <circle class="myGreen" cx="40" cy="40" r="24" />
  <circle class="myRed" cx="40" cy="100" r="24" />
</svg>
```

#### style

```html
<svg version="1.1" viewbox="0 0 1000 500" xmlns="http://www.w3.org/2000/svg">
  <rect
    height="300"
    width="600"
    x="200"
    y="100"
    style="fill: red; stroke: blue; stroke-width: 3"
  />
</svg>
```

### svg

属性

- `width`宽
- `height`高
- `x`水平位置
- `y`垂直位置
- `version` 属性用于指明 SVG 文档遵循规范。它只允许在根元素<svg> 上使用。它纯粹是一个说明，对渲染或处理没有任何影响。虽然它接受任何数字，但是只有 1.0 和 1.1.这两个有效的选择。
- `viewBox` 属性允许指定一个给定的一组图形伸展以适应特定的容器元素(裁剪画面)。属性的值是一个包含 4 个参数的列表 min-x, min-y, width and height，以空格或者逗号分隔开，在用户空间中指定一个矩形区域映射到给定的元素,不允许宽度和高度为负值，0 则禁用元素的呈现。这个属性会受到 `preserveAspectRatio` 的影响。
- `preserveAspectRatio`:否强制进行统一缩放。

### defs

SVG 允许我们定义以后需要重复使用的图形元素。建议把所有需要再次使用的引用元素定义在 defs 元素里面。这样做可以增加 SVG 内容的易读性和无障碍。在 defs 元素中定义的图形元素不会直接呈现。你可以在你的视口的任意地方利用 `<use>`元素呈现这些元素。

### g

元素 g 是用来组合对象的容器。添加到 g 元素上的变换会应用到其所有的子元素上。添加到 g 元素的属性会被其所有的子元素继承。此外，g 元素也可以用来定义复杂的对象，之后可以通过`<use>`元素来引用它们。

### rect

rect 元素是 SVG 的一个基本形状，用来创建矩形，基于一个角位置以及它的宽和高。它还可以用来创建圆角矩形

- x
- y
- width
- height
- rx 圆角
- ry 圆角

### patterns

在我看来 patterns（图案）是 SVG 中用到的最让人混淆的填充类型之一。它的功能非常强大，所以我认为他们值得讨论一下并且我们应至少对他们有最基本的了解。跟渐变一样，`<pattern>` 需要放在 SVG 文档的 `<defs>` 内部。

```html
<?xml version="1.0" standalone="no"?>
<svg width="200" height="200" xmlns="http://www.w3.org/2000/svg" version="1.1">
  <defs>
    <linearGradient id="Gradient1">
      <stop offset="5%" stop-color="white" />
      <stop offset="95%" stop-color="blue" />
    </linearGradient>
    <linearGradient id="Gradient2" x1="0" x2="0" y1="0" y2="1">
      <stop offset="5%" stop-color="red" />
      <stop offset="95%" stop-color="orange" />
    </linearGradient>

    <pattern id="Pattern" x="0" y="0" width=".25" height=".25">
      <rect x="0" y="0" width="50" height="50" fill="skyblue" />
      <rect x="0" y="0" width="25" height="25" fill="url(#Gradient2)" />
      <circle
        cx="25"
        cy="25"
        r="20"
        fill="url(#Gradient1)"
        fill-opacity="0.5"
      />
    </pattern>
  </defs>

  <rect
    fill="url(#Pattern)"
    stroke="black"
    x="0"
    y="0"
    width="200"
    height="200"
  />
</svg>
```

关于 pattern 容易混淆的事是，pattern 定义了一个单元系统以及他们的大小。上例中，我们在 pattern 元素上定义了 width 和 height 属性，用于描述在重复下一个图案之前应该跨过多远。如果你想要在绘制时偏移矩形的开始点，也可以使用 x 和 y 属性，原因如下。

就像前面使用了 gradientUnits 属性，同样的 pattern 也有一个属性 patternUnits 用于描述我们使用的属性单元。这同之前使用的 objectBoundingBox 默认值一样，所以当一个值为 1 时，它被缩放到应用 pattern 对象的宽高值。因此，我们希望 pattern 垂直和水平的重复 4 次，所以宽高被设置位 0.25，这一位置 pattern 的宽高仅为总外框大小的 0.25。

与渐变不同，pattern 有第二个属性 patternContentUnits，它描述了 pattern 元素基于基本形状使用的单元系统，这个属性默认值为 userSpaceOnUse，与 patternUnits 属性相反，这意味着除非你至少指定其中一个属性值（patternContentUnits 或 patternUnits），否则在 pattern 中绘制的形状将与 pattern 元素使用的坐标系不同，当你手写这部分时会容易混淆。为了使上例生效，我们必须考虑我们的边框（200 像素）大小和我们实际希望 pattern 垂直和水平重复 4 次的需求。这意味着每个 pattern 单元应该是 50x50 的方形，pattern 中的两个矩形和圆形的大小会被缩放适应到一个 50x50 的边框里，任何我们绘制在边框外的内容都不会显示。因为我们希望 pattern 从边框的左上角里开始，所以 pattern 也必须偏移 10 像素，也就是 pattern 的 x 和 y 属性需要调整为 10/200=0.05。

如果对象改变了大小，pattern 会自适应其大小，但是对象里面的内容不会自适应。所以当我们在 pattern 中还是放置 4 个重复的 pattern 时，组成 pattern 的对象将不会保持相同的大小，同时在他们之间会有大片空白区域。通过改变 patternContentUnits 属性，我们可以把所有的元素放到相同的单元系统中：

```html
<pattern
  id="Pattern"
  width=".25"
  height=".25"
  patternContentUnits="objectBoundingBox"
>
  <rect x="0" y="0" width=".25" height=".25" fill="skyblue" />
  <rect x="0" y="0" width=".125" height=".125" fill="url(#Gradient2)" />
  <circle
    cx=".125"
    cy=".125"
    r=".1"
    fill="url(#Gradient1)"
    fill-opacity="0.5"
  />
</pattern>
```

现在，因为 pattern 内容与 pattern
本身处于相同的单元系统中，所以我们不用偏移边框以使 pattern
在正确的位置上开始，并且即使对象变大，pattern 也会自动的缩放以保证 pattern
内部的对象数目和重复不变。这与 userSpaceOnUse 系统不同，userSpaceOnUse
系统中如果对象改变大小，pattern 本身会保持不变，只是重复更多次去填满边框。
它有一点点的副作用，在 Gecko 中的圆如果半径设置得小于
0.075（尽管半径应该设置的比这个值大得多。这个可能是 pattern 元素中的一个
bug，或者也不算
bug，我也不太清楚）的话绘制的时候可能会出现问题，为了规避这个问题，可能最好的办法是尽量避免在
objectBoundingBox 单元中绘制图形。 在你想要使用 pattern
的时候，可能你并不中意这些方法中的任何一个，Pattern
通常都是有确认的大小并且重复他们自己，与对象形状独立开来。要想创建这种
pattern，pattern
和它的内容必须在当前用户空间中绘制，这样当对象在做如下操作时他们才不会改变形状：

```html
<pattern
  id="Pattern"
  x="10"
  y="10"
  width="50"
  height="50"
  patternUnits="userSpaceOnUse"
>
  <rect x="0" y="0" width="50" height="50" fill="skyblue" />
  <rect x="0" y="0" width="25" height="25" fill="url(#Gradient2)" />
  <circle cx="25" cy="25" r="20" fill="url(#Gradient1)" fill-opacity="0.5" />
</pattern>
```

当然，这意味着如果你后续改变了对象大小，pattern 也不会缩放。上述三个举例在下图中放在一个矩形中展示，高度被轻微拉伸到 300px，但是我注意到这不是完整的图片，并且有些其他选项可能你的应用不支持。

## d3.js

> <https://github.com/d3/d3/blob/main/API.md#dragging-d3-drag>

### 安装

`npm install d3`命令
`<script src="https://d3js.org/d3.v4.min.js"></script>`

### 选择元素

> 采用了链式语法。就是说，每次打点调用 d3 提供的函数方法，都会返回一个 d3 对象，这样可以继续调用 d3 的其他函数方法

- `select` 返回 css 选择器所匹配的第一个元素。
- `selectAll` 返回 css 选择器匹配的所有元素。
  这两个函数返回的结果称为选择集。

### 操作元素

- 增加 ( append )元素后添加
- 插入 ( insert )元素前添加
- 删除元素 ( remove )
- 设置获取元素属性 ( attr )。
- 设置获取元素样式 ( style )。

### 数据绑定

> D3 有一个很独特的功能：能将数据绑定到 DOM 上，也就是绑定到文档上。这么说可能不好理解，例如：网页中有段落元素 `<p>` 和一个整数 5，于是可以将整数 5 与 `<p>`绑定到一起。绑定之后，当需要依靠这个数据才操作元素的时候，会很方便。数据就会添加到 DOM 元素的`__data__`属性中

- `datum(value)` 绑定一个数据到选择集上

> 这个方法不会进行数据绑定的计算(update、enter、exit)，只能在现有的元素上绑定数据。如果 value 是数组会自动转成字符串。
> 　　假设有一字符串 China，要将此字符串分别与三个段落元素绑定，代码如下：

```js
var str = "China";

var body = d3.select("body");
var p = body.selectAll("p");

p.datum(str);

p.text(function(d, i){
    return "第 "+ i + " 个元素绑定的数据是 " + d;
});

/*
绑定数据后，使用此数据来修改三个段落元素的内容，其结果如下：

第 0 个元素绑定的数据是 China
第 1 个元素绑定的数据是 China
第 2 个元素绑定的数据是 China

　　在上面的代码中，用到了一个无名函数 function(d, i)。当选择集需要使用被绑定的数据时，常需要这么使用。其包含两个参数，其中：

　　（1）d 代表数据，也就是与某元素绑定的数据。

　　（2） i 代表索引，代表数据的索引号，从 0 开始。

　　例如，上述例子中：第 0 个元素 apple 绑定的数据是 China。
 * /
```

- `data([value])` 绑定一个数组到选择集上，数组的各项值分别与选择集的各元素绑定

```js
var dataset = ["I like dogs", "I like cats", "I like snakes"];
var body = d3.select("body");
var p = body.selectAll("p");

p.data(dataset).text(function (d, i) {
  return d;
});

/*
当 i == 0 时， d 为 I like dogs。

　　当 i == 1 时， d 为 I like cats。

　　当 i == 2 时， d 为 I like snakes。
*/
```

- selection.data([data[, key]]) 1.如果选择集中只有一个分组，则入参 data 应该是一个数组，该数组会与选择集中的元素进行绑定。 2.如果选择集中含有多个分组（使用方法 selection.selectAll() 嵌套选择生成的选择集可能含有多个分组），则入参 data 应该是一个返回数组的函数，每一个分组都会调用该方法，并依次传入三个参数：

当前所遍历的分组的父节点所绑定的数据 datum`d`
当前所遍历的分组的索引 index `i`
选择集的所有父节点 parent nodes `nodes`

其中函数内的 `this` 指向当前所遍历的分组的父节点，即与 nodes[i] 相同
最后该函数返回一个数组，然后该数组的元素就用于与该分组的元素进行绑定

---

默认基于索引来对选择集中的元素和数组中的数据进行匹配 join-by-index，即选择集的第一个元素和数组的第一个数据进行绑定，并依次类推，但我们可以通过第二个入参 key 来自定义匹配规则。

```js
const data = [
  { name: "Locke", number: 4 },
  { name: "Reyes", number: 8 },
  { name: "Ford", number: 15 },
  { name: "Jarrah", number: 16 },
  { name: "Shephard", number: 23 },
  { name: "Kwon", number: 42 },
];

d3.selectAll("div")
  .data(data, function (d) {
    // key 函数，如果传入的 d 存在，则取其属性 name 作为键
    // 否则取元素的 id 属性（这是一个回退操作，因为在初始时，元素还没有绑定数据，所以 d 是 undefined）
    return d ? d.name : this.id;
  })
  .text((d) => d.number);
```

---

元素在绑定数据时，并不一定是一一对应的，可能会出现节点和数据元素个数不匹配的问题，针对这个问题，D3 提出三个概念：

如果 DOM 节点多出来，则未绑定数据的节点会进入名为 `exiting` 选择集（准备从页面「离去」的节点，一般在后续操作中删除）
如果数据元素多出来了，则对应多出来的「虚拟节点」会进入名为 `entering` 选择集（一般会在后续操作中实例化这些 DOM 节点，并插入在页面的相应位置）
可与数据对应上的 DOM 节点，进入名为 `updating`选择集，它是默认选择集，即 selection.data() 方法返回的对象就是 `update` 选择集（而 `enter` 选择集和 `exit` 选择集需要调用该对象的 enter() 和 exit() 方法才能获得）

---

## 在绑定数据后，D3 没有立即更新（增删）页面节点，而是生成 3 个选择集，这样为数据可视化提供了更大的灵活度和可定制性，例如对于 exiting 选择集的节点，可以在删除时设置一些淡出的动效；对于 entering 选择集的节点可以设置不一样的颜色，高亮出来它们是新增到页面上的

选择集绑定数据后，返回的三个选择集 entering、exiting、updating 其中的元素次序会有所不同：

1.其中选择集 entering 和 updating 中元素会根据其绑定的数据在新数组中的索引顺序进行排列，而且会在相应的位置「留空」（使用 null 作为占位符），便于后续两者合并 merge 2.而 exiting 选择集会根据其原来绑定的数据在原来的数组中的位置来排序，也是在相应的位置「留空」，以便保持元素原有的索引值。
然后将 entering 选择集的「虚拟节点」添加 append 到页面时，元素会基于索引顺序「混入」页面的 updating 选择集的元素中（作为兄弟元素），这样可以确保最后页面的元素顺序与它们所绑定的数据在数组顺序一致。
但是由于 D3 会复用 updating 选择集的元素（以便提高性能），对于 updating 选择集中的元素，如果在新绑定的数组中，它们所对应的数据的索引发生了变化时，页面的相应元素的次序并不会更新，如果不进行「重排」，可能造成最后页面的元素无法与它们所绑定的数据在数组顺序一致。因此一般在选择集绑定新数据后调用 selection.order() 方法，以更新 updating 选择集在页面的元素的顺序

[more](https://blog.csdn.net/RRie1/article/details/119315788#:~:text=%E8%A7%A3%E5%86%B3%E6%96%B9%E5%BC%8F%20-%3E%20%E5%AE%9E%E7%8E%B0%E6%95%B0%E6%8D%AE%E7%BB%91%E5%AE%9A%E7%9A%84%E7%BB%91%E5%AE%9A%E5%87%BD%E6%95%B0%E4%B8%BAdata%EF%BC%88data%2C,keyFunction%EF%BC%89%EF%BC%8C%E4%BD%BF%E7%94%A8keyFuntion%20%E4%B8%BA%E6%95%B0%E6%8D%AE%E7%BB%91%E5%AE%9Akey%E3%80%82%20%2F%2F%E6%AD%A3%E7%A1%AE%E7%BB%91%E5%AE%9A%E6%96%B9%E5%BC%8Fd3.selectAll%28%27rect%27%29.data%28data%2Cd%3D%3Ed.name%29.attr%28%27width%27%2Cd%3D%3Ed.value%29)
[more](https://juejin.cn/post/6999509011687014414)

### 绑定事件

```js
d3.select("svg rect").on("click", (event, data) => {
  d3.select(event.target);
});
```

### Panning 平移和 Zooming 缩放

#### 创建缩放器

> 会和 mouseup 事件冲突

使用方法 d3.zoom() 创建一个缩放器（以下称为 zoom）
它既是一个方法，可以接收选择集作为参数 zoom(selection)，为选择集中的元素（一般是包含数据的容器 <g> 元素）添加相应的缩放事件监听器，并为它们设置变换 transform 的初始值

```js
// 一般通过 selection.call() 方法调用缩放器创建函数
// 这样 selection 选择集就会作为参数传递给缩放器创建函数
selection.call(d3.zoom().on("zoom", zoomed));
function zoomed(event) {
    g.attr("transform", event.transform);
}

如果希望移除缩放相关事件的监听器，可以为将相应事件回调函数设置为 null
selection.on(".zoom", null);

// 也可以只移除特定的缩放事件的监听器，如滚轮缩放
selection.on("wheel.zoom", null);
```

> d3.zoomIdentity
> 在 d3.js 进行缩放的时候，有一个缩放的中心点，我们在 demo 进行测试的时候就会发现，这个缩放的中心点就是鼠标所在的位置，在鼠标所在的位置局部放大和缩小。
> 在默认情况下，这种中心点的位置在 svg 的[width/2,height/2]的位置，例如调用 scaleTo,scaleBy 方法的时候，就是已 svg 的中心为缩放的中心进行缩放；如果是调用 d3.zoomIdentity.scale(2)进行缩放，那么缩放的中心点则为[0,0]:
> 因为 d3.zoomIdentity 表示的是坐标位置[0,0]，当前缩放为 1 的一种状态，因此它将缩放中心的位置定在了[0,0]。

```js
//直接缩放
svg.call(
    zoom.transform,
    d3.zoomIdentity.translate(100，100)
);
//多少时间内完成缩放
svg.transition().duration(1000).call(
    zoom.transform,
    d3.zoomIdentity.translate(100，100)
);
```

#### 移除缩放

```js
d3.select("svg").on(".zoom", null);
//重新绑定
d3.select("svg").call(d3.zoom());
```

#### 约束缩放

- zoom.constrain([constrain]) 对缩放平移操作进行约束。
  其入参是一个函数，该函数依次接收三个参数，最后返回一个经过约束的变换值：

transform 原本需要执行的缩放变换对象
extent 视图范围
translateExtent 平移的范围

默认的约束函数如下，其作用是确保视图范围不大于可平移的范围，这样用户就可以通过移动，将图像的各个部分移到视图的任何地方

```js
function constrain(transform, extent, translateExtent) {
  var dx0 = transform.invertX(extent[0][0]) - translateExtent[0][0],
    dx1 = transform.invertX(extent[1][0]) - translateExtent[1][0],
    dy0 = transform.invertY(extent[0][1]) - translateExtent[0][1],
    dy1 = transform.invertY(extent[1][1]) - translateExtent[1][1];
  return transform.translate(
    dx1 > dx0 ? (dx0 + dx1) / 2 : Math.min(0, dx0) || Math.max(0, dx1),
    dy1 > dy0 ? (dy0 + dy1) / 2 : Math.min(0, dy0) || Math.max(0, dy1)
  );
}
```

- zoom.extent([extent]) 设置视图范围 viewport extent。
  参数 extent 是一个数组 [[x0, y0], [x1, y1]]，其中 [x0, y0] 表示视图范围的左上角，而 [x1, y1] 表示视图范围的右下角。
  参数 extent 也可以是一个返回数组的函数，它会被选择集中的元素依次调用，并传递当前所遍历的元素所绑定的数据 datum d 作为参数。而函数内的 this 指向当前所遍历的元素节点。
  💡 如果缩放器绑定的是普通的 HTML 元素，则视图范围的默认值 [[0, 0], [width, height]] 即与元素的宽高大小相同；如果缩放器绑定的是 SVG 类型的元素，则视图范围的默认值是 SVG 的 viewBox（如果没有设置 viewBox 属性，就使用 SVG 的宽高，即 viewport）。
  视图的范围 viewport extent 对于一些函数有影响，如通过 zoom.scaleBy() 和 zoom.scaleTo() 方法触发的变换，其视图的中心会保持不变；视图中心和大小会影响使用插值器 d3.interpolateZoom 创建的过渡动画的路径；而平移范围 translate extent 的约束需要依赖视图范围（平移范围应该大于视图范围）。

- zoom.translateExtent([extent]) 设置平移范围 translate extent。
  参数 extent 是一个数组 [[x0, y0], [x1, y1]]，其中 [x0, y0] 表示平移范围的左上角，而 [x1, y1] 表示平移范围的右下角。默认值是 [[-∞, -∞], [+∞, +∞]]
  💡 该方法虽然约束的是平移操作，但可能造成缩小时平移的发生。
  ⚠️ 该约束在通过 zoom.scaleBy()、zoom.scaleTo()、zoom.translateBy() 方法执行变换时生效；但是通过 zoom.transform() 方法执行变换时不生效

以上三个方法提及的两个特殊的范围：viewport extent 视图范围、translate extent 平移范围。通过设置视图范围的大小，以及通过平移范围来约束视图（范围，一个数组）可以修改的位置，可以间接来限制元素可以平移的位置，通过这两个特殊范围的配合可以实现特定的元素不被移出画面外这一需求。

- zoom.scaleExtent([extent]) 设置缩放比例的范围。参数 extent 是一个的数组 [k0, k1]，表示缩放比例的范围，其中 k0 是可设置的最小缩放比例，k1 是可设置的最大缩放比例。默认范围是 [0,∞][0,\infty ][0,∞]
  如果达到了约束的缩放比例极限时，即使用户继续滑动鼠标滚轮，缩放变换也会被忽略。
  💡 以上方法限制视图的缩放，但可能会造成一个「副作用」，即当视图缩放达到了约束的缩放比例极限时，用户还继续滚动就会造成页面的滚动（如果当时页面是可滚动的），如果希望修正这个「副作用」，可以在相应的选择集上监听 wheel 事件并取消它的默认行为

```js
selection
  .on(zoom) // 为选择集的元素设置了缩放监听器后，取消 wheel 事件的默认行为
  .on("wheel", (event) => event.preventDefault());
```

该约束在通过 zoom.scaleBy()、zoom.scaleTo()、zoom.translateBy() 方法执行变换时生效；但是通过 zoom.transform() 方法执行变换时不生效

- zoom.filter[(filter)] 用于判断是否执行缩放变换操作。参数 filter 是一个返回布尔值的函数，当返回的是 falsy 时忽略变换操作。它用以限制特定条件下不响应缩放变换操作。
  函数 filter 接收当前的缩放事件 event 和当前调用缩放器的选择集的元素所绑定的数据 datum d 作为参数，而函数内的 this 指向当前元素节点。
  其默认值如下，因此按下 Ctrl（但是可以在滚动鼠标滑轮时按下 Ctrl）或使用鼠标的次级按键（对于右手用户，次级按键一般是指鼠标的右键）时，默认是无法进行缩放平移操作，因为这些操作一般有其他用途

```js
function filter(event) {
  // 对于设置为右手操作的鼠标
  // 当使用左键时，event.button 为 0
  // 当使用右键时，event.button 为 2
  return (!event.ctrlKey || event.type === "wheel") && !event.button;
}
```

- zoom.wheelDelta([delta]) 设置鼠标滚轮的每次滚动的 delta 值，参数 delta 是一个函数，最后返回修改后的 delta 值 Δ\Delta Δ。
  参数 delta 默认值如下

```js
function wheelDelta(event) {
  return (
    -event.deltaY * (event.deltaMode === 1 ? 0.05 : event.deltaMode ? 1 : 0.002)
  );
}
```

该值用于在鼠标滚轮时计算缩放比例 k×2Δk\times 2^{\Delta}k×2Δ（其中 kkk 是原始的缩放比例），例如当 Δ=1\Delta = 1Δ=1 时，视图的元素会缩小一半；当 Δ=−1\Delta = -1Δ=−1 时，视图的元素会放大一倍

- zoom.clickDistance([distance]) 设置点击事件中，鼠标按下与放开鼠标之间允许的最大距离（该距离通过点击事件的 event.clientX、event.clientY 计算得到），如果大于等于该距离，就不会抛出点击事件。
  💡 可以想象以鼠标按下点为圆心，以参数 distance 为半径，在该圆内释放鼠标，都会抛出点击事件，在圆外（或圆周上）放开鼠标，点击事件都会被忽略（因为此时更应该触发拖拽事件）。参数默认值 distance 是 0
  该方法可用于优化点击放大的场景，而原始画面中有大量较小的元素，从鼠标的按下到释放可能会发生微小的移动，避免识别为对该元素的拖拽操作，可以通过设置「可移动式点击的最大距离」，来区分点击事件和拖拽事件。

- zoom.tapDistance([distance]) 对于触屏设备在双击时，两次点击允许的最大距离（该距离通过首次点击的 touchstart 和第二次点击的 touchend 事件的 event.clientX、event.clientY 计算得到），如果大于等于该距离，就不会抛出 dblclick 双击事件。
  💡 其应用场景和前一个方法类似，一般是为了区分双击事件和拖拽事件，参数默认值 distance 是 10

### 比例尺

#### 线性比例尺

```js
let scale = d3.scaleLinear().domain([0, 10000]).range([0, 100]);
```

其中 scaleLinear() 对应法则，domain() 是定义域，range() 是值域。

### 坐标轴

D3 中提供了专门的坐标轴模块 d3-axis，只需要几行代码就可以生成各种各样的坐标轴。一般情况下，坐标轴要与比例尺一起使用。记住这句话，离开比例尺的坐标轴就是耍流氓。

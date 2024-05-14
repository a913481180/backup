---
title: markdown语法
date: 2020-12-18 20:00:00
categories:
  - blog
---

## 标题

```md
# 这是一级标题

## 这是二级标题

### 这是三级标题

#### 这是四级标题

##### 这是五级标题

###### 这是六级标题

井号与标题间有空格
```

**效果：**

# 这是一级标题

## 这是二级标题

### 这是三级标题

#### 这是四级标题

##### 这是五级标题

###### 这是六级标题

---

## 字体

```md
**这是加粗的文字**

_这是倾斜的文字_

**_这是斜体加粗的文字_**

~~这是加删除线的文字~~
```

**效果：**
**这是加粗的文字**
_这是倾斜的文字_`
**_这是斜体加粗的文字_**
~~这是加删除线的文字~~

---

## 引用

```md
> 这是引用的内容

> > 这是引用的内容

> > > 这是引用的内容
> > > 在被引用的文本前加上>符号，以及一个空格就可以了
```

**效果：**

> 这是引用的内容
>
> > 这是引用的内容
> >
> > > 这是引用的内容

---

## 分隔线

```md
---
---
```

## 超链接

```md
[简书](http://jianshu.com)
[百度](http://baidu.com)
```

**效果:**
[简书](http://jianshu.com)
[百度](http://baidu.com)

---

## 排序

```md
- 列表内容

* 列表内容

- 列表内容

1.  列表内容
    \+ 列表内容
    前面加反斜线\即可显示符号本身
2.  列表内容
    注意：序号跟内容之间要有空格
3.  列表内容
4.  列表嵌套
    上一级和下一级之间敲三个空格即可
```

**效果：**

- 列表内容

- 列表内容

- 列表内容

1. 列表内容
   \+ 列表内容
2. 列表内容
3. 列表内容 4. 列表嵌套

---

## 代码

````md
`代码内容`

单行代码：代码之间分别用一个反引号包起来

\```

代码块

\```

代码块：代码之间分别用三个反引号包起来，且两边的反引号单独占一行或者缩进 4 个空格或是 1 个制表符
````

## 列表

```md
| 商品   | 数量 |  单价  |
| ------ | ---: | :----: |
| :苹果: |   10 |  \$1   |
| :电脑  |    1 | \$1000 |

冒号在左边表示左对齐，右边表示有对齐，两边都有表示居中。
```

**效果：**

| 商品   | 数量 |  单价  |
| ------ | ---: | :----: |
| :苹果: |   10 |  \$1   |
| :电脑  |    1 | \$1000 |

---

## 链接

## `链接写法：<http://baidu.com>`

## 图片

```md
![文本](图片地址 "可选标题")
开头一个感叹号 !
接着一个方括号，里面放上图片的替代文字
接着一个普通括号，里面放上图片的网址，最后还可以用引号包住并加上选择性的 'title' 属性的文字。
```

## 画图

> [Mermaid 中文网](https://mermaid.nodejs.cn/syntax/flowchart.html)

包括流程图、顺序图、类图、状态图、实体关系图、用户旅程图、甘特图、饼图、象限图、需求图、Gitgraph 图、思维导图和时间线图等

### 流程图

````txt
\```Mermaid
  graph LR
    A[节点A] --> B{条件}
    B --> C{条件}
    %% 注释 ----------------------
    C --> D
\```
````

#### 在 Mermaid 中，标识符可以包括字母、数字和下划线，但必须以`字母`或`下划线`开头

标识符是用来表示节点名称的，例如在流程图中使用 A、B、C 等表示不同的节点。除了标识符，Mermaid 还支持使用文本和样式来描述节点的内容和属性，例如使用`{text}`表示节点内的文本内容，使用`{style}`表示节点的样式属性。
`style A fill:#f9f,stroke:#333,stroke-width:4px`

定义一类样式并将该类附加到应该具有不同外观的节点。

```txt
    A:::xxx --> B
    classDef xxx fill:#f96

```

#### 节点之间的链接

带箭头的链接:`A-->B`
不带箭头的链接:`A---B`
带箭头和文本的链接:`A-->|xxxx|B`或`A--|xxxx|-->B`
不带箭头和文本的链接:`A--|xxxx|---B`或`A---|xxxx|B`
虚线的链接:`A-.->B`
虚线和文本的链接:`A-.xxxx.->B`
粗线的链接:`A==>B`
粗线和文本的链接:`A==xxxx==>B`
看不见的链接:`A ~~~ B`

可以在同一行中声明多个链接`a --> b --> d`
还可以在同一行中声明多个节点链接`a --> b & c--> d`,`A & B--> C & D`

多方向多类型箭头:`A o--o B`,`B <--> C`,`C x--x D`

链接的最小长度
默认情况下，链接可以跨越任意数量的等级，但你可以通过在链接定义中添加额外的`-`来要求任何链接比其他链接更长。`B ---->|No| E[End]`
当链接标签写在链接中间时，必须在链接右侧添加额外的`-`。`B -- No ----> E[End]`
对于点链接或粗链接，要添加的字符是等号或点

#### Mermaid 支持多种节点类型，这些类型主要通过文本表示。例如

默认节点：使用 A、B、C 等大写字母表示，如`A[默认节点]`。
文本节点：使用`B[bname]`的格式表示，其中“bname”是节点的名字。
平行四边形：使用`B[\bname\]`的格式表示，其中“bname”是节点的名字。
平行四边形：使用`B[/bname/]`的格式表示，其中“bname”是节点的名字。
梯形：使用`B[/bname\]`的格式表示，其中“bname”是节点的名字。
梯形：使用`B[\bname/]`的格式表示，其中“bname”是节点的名字。
圆角节点：使用`C(cname)`的格式表示，其中“cname”是节点的名字。
圆角节点：使用`C([cname])`的格式表示，其中“cname”是节点的名字。
圆柱节点：使用`C[(cname)]`的格式表示，其中“cname”是节点的名字。
圆形节点：使用`D((dname))`的格式表示，其中“dname”是节点的名字。
非对称节点：使用`E>ename]`的格式表示，其中“ename”是节点的名字。
菱形节点：使用`F{fname}`的格式表示，其中“fname”是节点的名字。
六角形节点：使用`G{{六角形节点}}`的格式表示。
平行四边形节点：使用`H{{平行四边形节点}}`的格式表示。

#### 子图

```txt
flowchart TB
    c1-->a2
    subgraph one
    a1-->a2
    end
    subgraph two
      direction RL
      b1-->b2
    end
    subgraph three
    c1-->c2
    end
```

#### 点击事件

触发 js 函数

```html
<script>
  const callback = function () {
    alert("A callback was triggered");
  };
</script>
```

`click C call callback() "Tooltip for a callback"`

链接在同一浏览器选项卡/窗口中打开
`click D href "https://www.github.com" "Open this in a new tab" _blank`

### 画序列图

```txt
sequenceDiagram
    participant A as Alice
    participant J as John
    A->>J: Hello John, how are you?
    J->>A: Great!
```

创建和销毁参与者

```txt
sequenceDiagram
    Alice->>Bob: Hello Bob, how are you ?
    Bob->>Alice: Fine, thank you. And you?
    create participant Carl
    Alice->>Carl: Hi Carl!
    create actor D as Donald
    Carl->>D: Hi!
    destroy Carl
    Alice-xCarl: We are too many
    destroy Bob
    Bob->>Alice: I agree
```

分组/框

```txt
    sequenceDiagram
    box Purple Alice & John
    participant A
    participant J
    end
    box Another Group
    participant B
    participant C
    end
    A->>J: Hello John, how are you?
    J->>A: Great!
    A->>B: Hello Bob, how is Charly?
    B->>C: Hello Charly, how are you?
```

可以向序列图添加注释

```txt
sequenceDiagram
    participant John
    Note right of John: Text in note
    Note left of John: Text in note
    Note over Alice,John: A typical interaction<br/>But now in two lines
```

临界区

```txt
sequenceDiagram
    critical Establish a connection to the DB
        Service-->DB: connect
    option Network timeout
        Service-->Service: Log error
    option Credentials rejected
        Service-->Service: Log different error
    end
```

平行线

```txt
sequenceDiagram
    par Alice to Bob
        Alice->>Bob: Hello guys!
    and Alice to John
        Alice->>John: Hello guys!
    end
    Bob-->>Alice: Hi Alice!
    John-->>Alice: Hi Alice!
```

背景高亮

```txt
sequenceDiagram
    participant Alice
    participant John

    rect rgb(191, 223, 255)
    note right of Alice: Alice calls John.
    Alice->>+John: Hello John, how are you?
    rect rgb(200, 150, 255)
    Alice->>+John: John, can you hear me?
    John-->>-Alice: Hi Alice, I can hear you!
    end
    John-->>-Alice: I feel great!
    end
    Alice ->>+ John: Did you want to go to the game tonight?
    John -->>- Alice: Yeah! See you there.
```

### 思维导图

```txt
mindmap
Root
    A
      B
      C
```

### xy 图

```txt
xychart-beta
    title "Sales Revenue"
    x-axis [jan, feb, mar, apr, may, jun, jul, aug, sep, oct, nov, dec]
    y-axis "Revenue (in $)" 4000 --> 11000
    bar [5000, 6000, 7500, 8200, 9500, 10500, 11000, 10200, 9200, 8500, 7000, 6000]
    line [5000, 6000, 7500, 8200, 9500, 10500, 11000, 10200, 9200, 8500, 7000, 6000]
```

### 时间线图

```txt
timeline
    title History of Social Media Platform
    2002 : LinkedIn
    2004 : Facebook
         : Google
    2005 : Youtube
    2006 : Twitter
```

```txt
timeline
        title MermaidChart 2023 Timeline
        section 2023 Q1 <br> Release Personal Tier
          Buttet 1 : sub-point 1a : sub-point 1b
               : sub-point 1c
          Bullet 2 : sub-point 2a : sub-point 2b
        section 2023 Q2 <br> Release XYZ Tier
          Buttet 3 : sub-point <br> 3a : sub-point 3b
               : sub-point 3c
          Bullet 4 : sub-point 4a : sub-point 4b
```

### 需求图

```txt
    requirementDiagram

    requirement test_req {
    id: 1
    text: the test text.
    risk: high
    verifymethod: test
    }

    element test_entity {
    type: simulation
    }

    test_entity - xxxx -> test_req
```

### 甘特图

```txt
gantt
    title A Gantt Diagram
    dateFormat YYYY-MM-DD
    section Section
        A task          :a1, 2014-01-01, 30d
        Another task    :after a1, 20d
    section Another
        Task in Another :2014-01-12, 12d
        another task    :24d
```

### 饼图

```txt
pie showData
title xxxx
    "Dogs" : 386
    "Cats" : 85
    "Rats" : 15
```

### 画类图

定义类

```txt
classDiagram
    class Animal["Animal with a label"]
    class Car["Car with *! symbols"]
    Animal --> Car
```

定义类的成员

```txt
classDiagram
%% 带有 () 的被视为函数/方法，所有其他被视为属性。
class BankAccount{
    +String owner
    +BigDecimal balance
    +deposit(amount)
    +withdrawal(amount)
}
```

定义关系

```txt
classDiagram
classA <|-- classB:xxxxxxxx
classC *-- classD:xxxxx
classE o-- classF
classG <-- classH
classI -- classJ
classK <.. classL
classM <|.. classN
classO .. classP
%%双向关系
Animal <|--|> Zebra

```

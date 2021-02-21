---

title: CSS 布局
date: {{ date }}
updated: {{date}}
tags: CSS布局
categories: CSS

---
## CSS布局
### 布局总览:

- 正常布局流

- display属性

- Grid (CSS网格)

- Flexbox(弹性盒子)

- position (定位)

- float(浮动)

- 多列布局

- 表格布局

  

### Flexbox(弹性盒子) 

- 专门设计出来用于创建横向或是纵向的**一维页面布局** 

-  弹性盒子的布局围绕两个轴来布局: 主轴 和 交叉轴  , 在弹性盒子中的元素称为flex 项

  > 主轴和交叉轴 不是一成不变的, 它根据`flex-direction`布局方向变化,属性指定主轴方向,另一个轴就是交叉轴 
  >
  > `align-items `:  控制交叉轴的对齐方式 ,因为它将所有直接子节点上的`align-self`值设置为一个组
  >
  > ` align-content` : 浏览器如何沿着弹性盒子布局的纵轴和网格布局的主轴在内容项之间和周围分配空间。
  >
  > `align-self` 控制单个flex项的对齐方式
  >
  > `justify-content`: 控制 flex 项在主轴上的位置
  >
  > > space-around ——它会使所有 flex 项沿着主轴均匀地分布，在任意一端都会留有一点空间。 space-between 则不会再两端留有空间    align-content中的属性同样的方式

  

- 以下为弹性盒子的主要属性:

  > `flex-direction `指定主轴方向 ,默认为row
  >
  > `flex-wrap`   空间不够时是否让 flex项进行换行
  >
  > - 弹性元素属性：
  >
  > `flex-flow` : flex-direction flex-wrap  的缩写
  >
  > `flex-grow `: 指定了flex容器中剩余空间应该分配给项目的比例。
  >
  > `flex-basis:` 元素大小的基准值,空间允许情况下会尽量满足该值 , 比width的优先级大
  >
  > `flex-shrink`: 空间不足时,缩放比例的多少
>
  > `flex`:  flex-grow flex-shrink flex-basis   缩写   
  
  


- 利用`order` 属性进行flex 项排序 ,默认值为0, 可以为负值,  order小的显示顺序在前

- *弹性盒子中的`文本节点`也可以进行弹性布局*

### Grid布局

- 被设计用于同时在两个维度上把元素按行和列排列整齐。

- 定义栅格系统:

   > `grid-template-rows `:规定每行的高度,有几行就声明几个值
   >
   > `grid-template-columns `:和rows 一样使用,区别在于,多出的元素只会溢出行,不会溢出列
   >
   > > **值的几种特殊类型:**
   > >
   > > 还可以给某一列使用 auto 值,自动获得剩余所有的宽/高度
   > >
   > > 使用repeat(3/auto-fill,1fr /px/minmax(50px,100px)/%...) 声明栅格系统 
   > >
   > > auto-fill :  根据值大小,自动分出列 (值不能是fr)    
   > >
   > > fr : 可用空间按比例分配        minmax()函数规定了尺寸的范围
   >
   > `grid-tempalte`:  grid-template-rows、grid-template-columns、grid-template-areas 属性值的缩写
   >
   > 

- **间距(gutter)定义:**

   > `column-gap `  :别名: grid-column-gap
   >
   > ` row-gap `    别名: grid-row-gap
   >
   > `gap` 规则可以一次定义行、列间距

   

- **栅格线命名:**

   >独立命名: 
   >
   >```text
   >grid-template-rows:
   > [r1-start] 100px [r1-end r2-start] 100px [r2-end r3-start] 100px [r3-end];
   >```
   >
   >单元格的每个方向给两条边   例如 :r1-star r1-end    (r1-end r2-start是一条线)
   >
   >自动命名:  命名从1开始  r-start 1 ... 
   >
   >```text
   > grid-template-rows: repeat(3, [r-start] 100px [r-end]);
   >```

- **区域命名:**

   > 使用` grid-template-areas`可以定义栅格区域，并且栅格区域必须是矩形的。

   > ```text
   >  grid-template-areas: 
   >  "header header header"
   >  "nav main main"
   >  "footer footer footer";
   >  定义三行三列,每个栅格都有名字, 系统会为区域自动命名, 例如 header-start    header-end  来表示header栅格区域  
   > ```

   > `grid-template`简写模式:

   >  grid-template:
   >       '栅格名称 栅格名称 栅格名称 栅格名称' 行高
   >       '栅格名称 栅格名称 栅格名称 栅格名称' 行高
   >       '栅格名称 栅格名称 栅格名称 栅格名称' 行高/列宽 列宽 列宽 列宽;
   >
   > `区域占位` :使用一个或多个 连续的. 定义区域占位。

- **填充栅格:**    

- 使用栅格线:    （这些属性是用在子元素上的）

   > `grid-row-start/end`     行开始/结束的栅格线
   >
   >  `grid-column-start/end `   列开始/结束的栅格线
   >
   > 样式属性可以使用以下值:  `栅格线名字`、`span`  、`auto`
   >
   > span例子 :  grid-row-end/star: span 2 向下/上包含两行  (bootstarp 基于此开发栅格系统)

- > **方法简写::**  `grid-row` 设置行开始栅格线，使用 `grid-column` 设置列结束栅格线。
   >
   > 例如 :  grid-row: 2/4;  grid-column: 2/4;

   

- 使用区域:

   > grid-area`更加简洁是同时对 `grid-row` 与 `grid-column` 属性的组合声明。
   >
   > 语法结构1: grid-row-start/grid-column-start/grid-row-end/grid-column-end。
   >
   > 语法结构2: 区域名字  例如 :   
   >
   > grid-area: nav;   grid-area: header-start/nav-start/main-end/main-end

- 填充栅格时,不要设置宽高 两个发生冲突,宽高属性优先级更高

   ------

   

- **栅格流动** : 

- 设置`grid-auto-flow` 属性可以改变单元格排列方式。

   > 参数说明: 
   >
   > column 按列排列    row	按行排列
   >
   > dense    元素使用前面空余栅格

- **对齐管理:**

- | justify-content | 所有栅格在容器中的水平对齐方式，容器有额外空间时 | 栅格容器 |
   | --------------- | ------------------------------------------------ | -------- |
   | align-content   | 所有栅格在容器中的垂直对齐方式，容器有额外空间时 | 栅格容器 |
   | align-items     | 栅格内所有元素的垂直排列方式                     | 栅格容器 |
   | justify-items   | 栅格内所有元素的横向排列方式                     | 栅格容器 |
   | align-self      | 元素在栅格中垂直对齐方式                         | 栅格元素 |
   | justify-self    | 元素在栅格中水平对齐方式                         | 栅格元素 |

- **自动排列:**

  >当栅格无法放置内容时，系统会自动添加栅格用于放置溢出的元素，我们需要使用以下属性控制自动添加栅格的尺寸。
  >
  >`grid-auto-rows`控制自动增加的栅格行的尺寸，
  >
  >grid-auto-flow:row时(默认值)	针对对象:容器
  >
  >`grid-auto-columns`控制自动增加的栅格列的尺寸，
  >
  >grid-auto-flow: column时;	针对对象:容器

  

- 终极简写: `grid`是简写属性

- 在不使用gird的情况下怎么创建网格布局: 可以利用div块 子元素的浮动属性,构建一个网格系统布局,但是这是固定的宽度,创建流体网格可以使用百分比设置宽度   flex也可以用来创建网格系统,但是flex仍然是一维设计的,用来处理网格还是需要计算width

  

### position定位

- **相对定位:**

- 相对定位(relative positioning)让你能够把一个正常布局流中的元素从它的默认位置按坐标进行相对移动。

- **绝对定位:**
- 元素会被移出正常文档流，并不为元素预留空间，通过指定元素相对于最近的非 static 定位祖先元素的偏移(如果没有就是html标签)，来确定元素位置。

- **固定定位:**

  > 与绝对定位的工作方式完全相同,但是只有一个区别:
  >
  > 固定定位固定元素是相对于浏览器视口本身

- **粘性定位(sticky position)** 

  > 它基本上是相对位置和固定位置的混合体，它允许被定位的元素表现得像相对定位一样，直到它滚动到某个阈值点, 根据视口定位
  >
  > 例如，从视口顶部起10像素为止，此后它就变得固定了

  

- 绝对定位的元素可以通过指定top和bottom ，来填充可用的垂直空间。
- 绝对定位元素的 top...值如果是百分比,则是非static定位的祖先元素的宽度 

- **定位层级z-index：**
  
- 对于**定位元素**来说通过定义层级来决定谁的层级高, 默认后者高于前者 
  
  > **同级的元素才有可比性**,父子元素之间的z-index是失效的(除非子元素设置负值父元素设置auto)
  >
  > 对于同级元素说,z-index根据数值大小进行排列
  
- z-index 也支持过渡效果,它没有在每一步改变它的值(没有渐变效果)，所以你认为它没有过渡效果，但实际上是有的



- 对于float元素 z-index还有更复杂的状况.详细查询资料

### 多列布局 (multicol)
- `column-count`    `column-width` 属性把块变为多列容器

  > column-width  浏览器将按照你指定的宽度尽可能多的创建列；任何剩余的空间之后会被现有的列平分。 这意味着你可能无法期望得到你指定宽度，除非容器的宽度刚好可以被你指定的宽度除尽。

- Multicol 创建的列无法单独的设定样式。 不存在让单独某一列比其他列更大的方法，同样无法为某一特定的列设置独特的背景色、文本颜色。
   -  `column-gap `改变列间间隙。
   -  `column-rule `在列间加入一条分割线。

- 多列布局的内容因为空间问题(分配内容是按一列一列的填充)被拆成多个内容(称为内容折断)  

  > 通过属性break-inside: avoid 在内容(子元素)中添加防止

### float 浮动

- 浮动会改变该元素本身和在正常布局流中跟随它的其他元素的行为

  > 行级元素浮动后,可以设置宽高有了块级元素的特征 
  >
  > 浮动元素会提升层级压住标准元素, 元素向指定方向移动，直到遇见边框。 
  >
  > 浮动会脱离正常文档流,所以容器高度由最后一个非浮动元素的位置决定,

- 浮动元素的外边距可以推开周围元素,但是周围元素无法设置外边距推开浮动元素,

- > 这里的周围元素仅限于 块级元素    原因: 块级元素不能推开在于,本质上块级元素还是独占一行,应用了外边距,但是是针对容器边框的,而行内元素的margin 是针对周边元素的
  >
  > 
  >
  > 行内元素可以通过左右的外边距推开  inline-block 可以通过所有外边距推开

- 浮动对块级元素并没有影响,只是对块级元素中的文本产生影响,而作用到块级元素,换句话说如果没有文本,块级元素会忽略浮动元素,出现在最初的位置

  

- **清除浮动:**

-  [`clear`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/clear) 属性。它将非浮动块的边框边界移动到`所有相关浮动元素`外边界的下方

- 容器自适应浮动元素

  > 1. 给父容器使用**clearfix** 方法,
  >
  > #container::after {
  >   content: ' ';
  >   display: block;
  >   clear: both;
  > }
  >
  > 2. 在容器的最后使用空的div块, 应用celar:both  自适应
  > 3.  设置父元素 `overflow: auto` 或者设置其他的非默认的 `overflow: visible` 的值。 创建BFC

  

### 表格布局

- (通常用于老的不支持 弹性盒子的浏览器)

- display: table  display: table-row ..
-------------------

### 响应式设计

- 响应式设计是三种技术的混合使用。

  > `流动布局`    `响应式多媒体`     `媒体查询`

- **流动布局:**

  > 关键在于`流动性`,在任何宽度下都可以很好地适应, 而不是内容被隐藏
  >
  > float  flex  grid 默认情况下都是流动的
  >
  > 在设计布局时,为了流动性,我们尽量不适用 固定的像素数值 ,而是百分比  em rem  vh vw..

- 响应式排版的问题(主要是字体大小),不要使用像素值写死,这样在不同设备上的体验不好,使用 em 视口单位会更好

  

- **媒体查询:**

- [媒体查询](https://developer.mozilla.org/zh-CN/docs/Web/Guide/CSS/Media_queries)可以应用于各种元素：

  >  在< link style source>元素的media属性中定义了待应用的资源
  >
  > 在JavaScript中，您可以使用[`Window.matchMedia()](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/matchMedia) 方法根据媒体查询测试窗口,[查询方法](https://developer.mozilla.org/zh-CN/docs/Web/Guide/CSS/Testing_media_queries)

- 语法格式 :

- ```
  @media media-type and (media-feature-rule) {
    /* CSS rules go here */
  }
  常用的  media-type  all  print  screen
  ```

- 媒体查询的三种逻辑 与 或 非

- 媒体查询中的断点:  引入媒体查询的点就叫做**断点**。

- 媒体查询可以通过js css实现布局的变化,例如:导航栏在移动端收起为一个图标



- 你真的需要媒体查询吗？

  > 流动布局在设计中是优先级最高的,媒体查询当成辅助来使用
  >
  > 弹性盒、网格和多栏布局都给了你建立可伸缩的甚至是响应式组件的方式，而不需要媒体查询。

- 响应式设计方法: 

  > **第一种方法:** 你可以从桌面或者最宽的视图开始，然后随着视口变得越来越小，加上断点，把物件挪开；桌面端向下的方式
  >
  > **第二种方法:**  你也可以从最小的视图开始，随着视口变得越来越大，增添布局内容。被叫做移动优先的响应式设计， 移动端向上
  >
  > - 尽量使用移动端向上的方式进行，让那些不支持媒体查询的设配也可以获得一个较好的布局
  >
  > ​    

  

- **响应式的多媒体:**

- 响应式图像:

  > 图像的二个问题:一个图像裁剪转换的问题(小设备看不到主要信息,需要裁剪出重要信息) 
  >
  >  图像分辨率的问题(小设备上不需要加载高分辨率的图像,浪费带宽)

- > 解决方法:    
  >
  > 1. 准备两套图片  img元素通过 srcset  sizes属性根据尺寸切换
  >
  > 2. 利用< picture> 中的source标签  通过他的media srcset属性实现
  >
  >    为什么不可以通过css  js 来实现响应式?
  >
  >    因为浏览器预加载 任意的图片 , 你不能先加载好 img 元素后, 再用 JavaScript 检测可视窗口的宽度，动态地加载小的图片替换已经加载好的图片，这就加载了两次

- 有选择性的为用户提供图片:

  > 使用 display: none 图片也会在浏览器请求时加载,会发生隐藏但是浪费带宽
  >
  > 可以通过js  data-属性来控制 ,一开始不给与图片src地址, 在页面加载完之后再进行选择性的加载图片

- css像素  和 设备像素的区别

- 媒体查询的断点问题

  >  传统的断点是 320px 768px 1024px... 但是正确的方法应该是根据内容设置断点，通过对浏览器缩小放大，找到不合适的布局进行设置断点 

### 传统的布局方法

### 旧浏览器支持
- **特性查询  :** 允许测试一个浏览器是否支持特定的一个css特性

- > ```
  > @supports (display: grid) {
  >   .item {
  >       width: auto;
  >   }
  > }
  > ```

  
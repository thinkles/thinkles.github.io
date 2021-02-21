---

title: sass基础
date: {{ date }}
updated: {{date}}
tags: sass
categories: CSS

---
## Sass

- `引入` 允许你创建多个文件，然后在一个文件中引入。
- `变量`  不在进行重复写一个值 ,而是直接使用变量
- `嵌套`  在嵌套中写css规则 , 更方便..
- `继承`  通过继承, 我们可以写一些基本样式, 然后继承他

- sCSS是 sass 的升级版本  scss使用花括号表示关系, sass则是使用空白符和缩进

#### 变量
- 使用`$`符号来标识变量,任何可以用作css属性值的赋值都 可以用作sass的变量值，甚至是以空格分割的多个属性值

- 当变量定义在css规则块内，那么该变量只能在此规则块内使用。 变量之间也可以相互引用


#### 嵌套
- 使用嵌套处理重复的书写选择器

  > CSS在处理嵌套过程中,会用空格使用后代选择器, 但是有些时候比如 :first :hover 等情况, 我们不希望有空格, 这时候使用 &:的符号

- > & 本质上代表外层的父元素, 所以可以在嵌套中还可以使用后代选择
  >
  > 例如: p &{}  但是尽量只定义 后代选择器, 这样导入时比较安全

- 对于 > + ~的处理来说, 可以放在外层选择器后边，或里层选择器前边

- 嵌套处理重复的属性  例如 : border-radius  -style ...

#### 导入SASS文件
- CSS@import规则，允许在一个CSS文件中导入其他CSS文件。然而，只有执行到@import时，浏览器才会去下载其他CSS文件，这导致页面加载起来特别慢。

- SCSS@import规则不同, 在生成CSS文件时就进行导入,把所有样式整合到一起,而无需发送多个下载请求

  > 所有被导入文件中定义的变量和混合器均可在导入文件中使用。
  >
  > 使用sass的@import规则可以省略.sass或.scss文件后缀 
  >
  > 允许同时导入多个文件

-  局部文件:

   > 把Sass 样式 分成多个文件进行管理,然而只想生成几个CSS文件, 那么就要使用局部文件 : 名字以`下划线开头`的sass文件 

- 在引入文件时,防止被覆盖已经声明的变量, 

  > 变量声明的后面使用!default  关键字, 如果这个变量被声明赋值了，那就用它声明的值，否则就用这个默认值。

- sass允许@import命令写在css规则内。这种导入方式会直接导入css规则内,成为嵌套语法




- 原生的CSS导入
- >被导入文件的  名字以.CSS结尾；或者URL值,文件名以 http:// 开头, 或者通过URL()
  >
  >这里推荐 @import url() 的方式 , 用css结尾导入可能会补上多余的前缀(vscode中) 
  >
  >换句话说,你不能用sass的@import直接导入一个原始的CSS文件，因为sass会认为你想用CSS原生的@import。


- 静默注释,在生成的CSS文件中不会产生注释 格式: //


#### 混合器
- 处理重复的大段样式代码
- > 通过@mixin name 定义  @include引入 

- 避免滥用混合器 ,在何时使用混合器

  > 判断一组属性是否应该组合成一个混合器，一条经验法则就是你能否为这个混合器想出一个好的名字。

- CSS类名应该是表述标签的含义,  混合器名字应该是展示性描述,应该具备应用样式的表述含义
- 混合器传参

  > @mixin name($name), include时传递参数
  >
  > 参数默认值  使用$name: default-value

#### 继承
- 选择器继承  @extend语法实现,选择器可以继承另一个选择器定义的所有样式

  > 继承被编译后是 使用`,`的并集选择器

- 选择器不仅会继承所有样式，任何该选择器有关的组合选择器样式 也会以组合选择器的形式继承

  > //.seriousError从.error继承样式
  > .error a{  //应用到.seriousError a
  >   color: red;
  >   font-weight: 100;
  > }

- 像#main .error这种后代选择器序列是不能被继承的。但可以反过来继承,我们也应该避免这种情况

- 继承 交集选择器:

  > @extend.important.error ， 那么.important.error 和h1.important.error 的样式都会被.seriousError继承

  

- 继承只会在生成CSS时复制选择器，而不会复制大段的CSS属性。比起混合器更加节省空间,但是如果继承的选择器较复杂,可能会复制很多选择器

  > 避免这种情况出现的最好方法就是不要在CSS规则中使用后代选择器（比如.foo .bar）去继承CSS规则。 为了匹配可能的情况,它会复制大量的选择器



#### sassScript

- CSS规则块内可以进行运算

- > 如果需要使用变量，同时又要确保 / 不做除法运算而是完整地编译到 CSS文件中，只需要用 #{} 插值语句将变量包裹。

- > font: #{$font-size}/#{$line-height};
  >
  > 通过 #{} 插值语句可以在选择器或属性名中使用变量

  

#### 指令

- Sass 中 @media 指令与 CSS中用法一样，只是增加了一点额外的功能：允许其在 CSS规则中嵌套。如果 @media 嵌套在 CSS规则内，编译时，@media 将被编译到文件的最外层，包含嵌套的父选择器。


- @extend-Only 选择器  Sass 引入了“占位符选择器” (placeholder selectors)，看起来很像普通的 id 或 class 选择器，只是 # 或 . 被替换成了 %。可以像 class 或者 id 选择器那样使用，当它们单独使用时，不会被编译到 CSS文件中。

- !optional 声明 要求 @extend 不生成新选择器
- 在指令中使用 @extend 时（比如在 @media 中）有一些限制,这能继承在media规则内的选择器

- @at-root  指令使嵌套在父元素的规则,编译时作为独立的样式,而不是后代选择器

- @debug 把表达式打印出来 @ warn 指令将 SassScript 表达式的值打印到标准错误输出流。
- @ error 将 SassScript 表达式的值作为一个致命错误抛出,对于验证 mixin 和函数的参数非常有用


- @if指令  @while指令
- @for指令 
- > @for $i from 1 through 3  @for $i from 1 to 3  ,区别在于 to不包括end

- @each 指令
- > @each $animal in puma, sea-slug, egret, salamander 


- Sass 支持自定义函数，并能在任何属性值或 Sass script 中使用：
- >@function grid-width($n) {
  >@return $n * $grid-width + ($n - 1) * $gutter-width;} 
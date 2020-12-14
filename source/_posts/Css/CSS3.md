---
title: CSS3
date: 2020-12-13 20:03:45
tags: Css3
categories: 前端Css
---

### CSS3
- 边框
    -  border-radius    圆角
       
       > 默认一个值应用在四个方向,如果想单独设置,可以各四个值分别代表 从左上顺时针到左下的角
    -  box-shadow   边框阴影 
       
        >四个值分别   x偏移量 | y偏移量 | 阴影模糊半径 | 阴影颜色 
-  border-image    用图片创建边框
    
    -  >三个值 /* border-image: image-source image-height image-width image-repeat */
    
 - 背景  
   - background-size  规定背景图片的尺寸进行缩小扩大使用
   - background-origin  规定背景图片的定位区域
    > 参数:content-box、padding-box 或 border-box  以这几个区域为参考放置图片

- 文本
  
   - text-shadow 文本应用阴影 (和box-shadow 格式一样)
   - >四个值分别 :水平阴影(px)、垂直阴影 (px)、模糊距离(px)，以及阴影的颜色
   - word-break  指定了单词怎样断行
   - >有些英语单词单纯断行影响阅读
- 字体
  
  - @font-face 规则   应用时设置font-family: name 使用自己设置的名字
```
@font-face {
  font-family: 'ciclefina';
  src: url('fonts/cicle_fina-webfont.eot');
  src: url('fonts/cicle_fina-webfont.eot?#iefix') format('embedded-opentype'),
         url('fonts/cicle_fina-webfont.woff2') format('woff2'),
         url('fonts/cicle_fina-webfont.woff') format('woff'),
         url('fonts/cicle_fina-webfont.ttf') format('truetype'),
         url('fonts/cicle_fina-webfont.svg#ciclefina') format('svg');
  font-weight: normal;
  font-style: normal;
}
```

- 渐变
  -  linear-gradient()函数创建  
  - >  background: linear-gradient(to bottom right, blue, pink);  第一个参数可选 也可以替换成角度  颜色参数也可以是多种颜色  可以控制结束为止,渐变中心... 在使用角度的时候, 0deg 代表渐变方向为从下到上, 90deg 代表渐变方向为从左到右
  
###  CSS 变形 (transforms)
- transform-origin 指定原点的位置,默认值为元素的中心，可以被移动

- 适用于三维的属性
 > 首先需要设置的属性是透视值（perspective）。透视正是三维空间的立体感的源泉。元素与观察者之间的距离越远，他们就越小。  CSS 属性 perspective 可以更加真实3d的变换效果,一般放在变换图像外的容器写

 > 通过 perspective-origin 属性来设置。默认透视值中，为观察者被置于中心，但是这并不总是适当的。
 > CSS 属性 transform-style 设置元素的子元素是位于 3D 空间中还是平面中。如果选择平面，元素的子元素将不会有 3D 的遮挡关系。
 >  backface-visibility 指定当元素背面朝向观察者时是否可见。


 - 以下为transform利用函数
  
    - translate()   偏移
    -  >元素从其当前位置移动，根据给定的 left（x 坐标） 和 top（y 坐标）  translatex()  translatey()
    - rotate() 平面旋转 参数给与度数
    - >元素顺时针旋转给定的角度。允许负值，元素将逆时针旋转 rotateX() 围绕x轴旋转多少度  rotateY()  以上两个旋转都是围绕某个轴的**立体旋转**

    - > 这里所有的3d坐标系, 都是y轴是竖直的,z轴则是向屏幕里的方向
    - scale()  
    - > 元素的尺寸会增加或减少，根据给定的宽度（X 轴）和高度（Y 轴） 当超出 [-1, 1]范围外时，缩放将在坐标方向上放大元素 如果为负值会反射(反方向投射)
    - skew() 
     >一个元素在二维平面上的倾斜转换
     >
     >- transform-origin  指明元素变形的远点

###  CSS过渡 (transitions)
  - transition 提供了一种在更改CSS属性时控制动画速度的方法
  - 想让一个元素的属性 transition起来 , 必须要先有预设值, 比如div元素 想要变换width, 那么div元素必须首先定义width,然后通过覆盖/ 添加类名的方式改变width 才可以, 如果一开始div没有width值, 通过添加类给与width值, 这时候是不发生变幻的
  >  定义 transition:width 2s, height 2s, transform 2s 定义的样式后面书写时间  简写属性

  - transition-delay
    指定延迟，即属性开始变化时与过渡开始发生时之间的时长。
  - transition-duration 变换时间
    -transition-timing-function 指定一个函数，定义属性值怎么变化
  - 当过渡完成时触发一个事件，在符合标准的浏览器下，这个事件是 transitionend
  - >如果需要循环，查看 animation
  
- transition 是非常好的工具，可以让 JavaScript 效果平滑而不用修改 JavaScript
- 书写多个过渡效果时，每个完整的要用逗号隔开
- 触发过渡效果,只需要某个属性更改值,这时就会触发,比如 hover写新值,每次移入移出时都能触发过渡,js给标签添加新类也行

> 当过渡完成时触发一个事件，在符合标准的浏览器下，这个事件是 transitionend, 在 WebKit 下是 webkitTransitionEnd. propertyName字符串，指示已完成过渡的属性。 elapsedTime 浮点数，指示当触发这个事件时过渡已运行的时间


### 动画
- 使用keyframes定义动画序列:@keyframes中补写关键帧 from{} to{},
- animation 简写属性
      animation-duration  设置动画一个周期的时长
      animation-timing-function 定义CSS动画在每一动画周期中执行的节奏(匀速/先快后慢...)
      animation-direction  设置动画在每次运行完后是反向运行还是重新回到开始位置重复运行
      animation-delay 延迟
      animation-iteration-count 设置动画重复次数， 可以指定infinite无限次重复动画
      animation-name  指定由@keyframes描述的关键帧名称    
>  animation: 3s ease-in 1s 2 reverse both paused slidein    animation属性的最后slidein一定为动画名字由@keyframes 定义

- 哪些 CSS 属性可以动画?  关注可动画属性列表 对CSS转换同样适用
  



---

title: CSS3 指南
date: {{ date }}
updated: {{date}}
tags: CSS3
categories: CSS

---
### CSS3
- 边框
    -  border-radius    圆角
       
       > 默认一个值应用在四个方向,如果想单独设置,可以各四个值分别代表 从左上顺时针到左下的角
    -  box-shadow   边框阴影 
       
        >四个值分别   x偏移量 | y偏移量 | 阴影模糊半径 | 阴影颜色 

    - border-image    用图片创建边框

      > 三个值 /* border-image: image-source image-height image-width image-repeat */
- 文本
  
   - text-shadow 文本应用阴影 (和box-shadow 格式一样)
   
     > 四个值分别 :水平阴影(px)、垂直阴影 (px)、模糊距离(px)，以及阴影的颜色
   
   - @font-face 规则   应用时设置font-family: name 使用自己设置的名字
- 渐变
  -  linear-gradient()函数创建  
  - >  background: linear-gradient(to bottom right, blue, pink);  第一个参数可选 也可以替换成角度  颜色参数也可以是多种颜色  可以控制结束为止,渐变中心... 在使用角度的时候, 0deg 代表渐变方向为从下到上, 90deg 代表渐变方向为从左到右
  
- filter 属性 完成图像模糊 (滤镜效果) 颜色转变的想过 
###  CSS 变形 (transforms)

- 行级元素不产生变形效果，将其转为 `inline-block` 或 `block` 以及弹性元素时都可以产生变化效果


 - **transform属性,变形函数**:

 - 偏移

    - `translate()`  控制X、Y轴偏移   `translate3d() ` 空间内偏移  , 给 x y z轴的坐标

    - >从其当前位置移动，根据给定的坐标值
      >
      >还有,  `translateX() ` `translateY()`   `translateZ()`  
      >
      >**坐标值的类型:**
      >
      >百分比 :  100%代表元素本身的尺寸(z轴不能给百分比值,因为z轴可以无限纵深,没有参考值)
      >
      >负值: 反方向偏移
      >
      >标准值
      
    
      

  - 缩放: 

    - `scale()` 设置 X/Y 轴的缩放     ` scale3d()`  沿X/Y/Z三个轴绽放元素。

      > `scaleX()  scaleY()  scaleZ()`   

      > z轴的缩放放大,视觉上是距离远近
      >
      > 放大和缩放  , 1 代表本身的大小, 负值进行反方向变换


  - 旋转:
    

    - `rotate()`  在平面内旋转,效果与使用 `rotateZ` 相同。  `rotate3d(tx,ty,tz,angle)` 同时设置X/Y/Z轴的旋转向量值来控制元素的旋转

      > 允许负值，元素将逆时针旋转

      >`rotateX()   rotateY()    rotateZ()`  元素按照不同坐标轴进行旋转
      >
      >可以同时设置多个旋转规则，顺序不同结果也会不同。

  - 倾斜:

    - skew()同时设置X/Y轴倾斜操作，不指定第二个参数时Y轴倾斜为零

      > `skewX( )  skewY()`   
      >
      > 通过倾斜还可以做出立体的图像

    

- 变形基点:

- 使用 `transform-origin` 设置元素的X/YZ操作的基点，用于控制旋转、倾斜等操作

  > - **旋转默认以元素中心**进行旋转，改变基点后可控制旋转点位置
  >
  > - **元素移动不受变形基点所影响**
  > - 基点是元素原始空间位，而不是translate移动后的空间位,基点存在于元素本身,而不是移动的空间位上

- 透视:

  > 使用 `perspective` 来控制元素的透视景深   (简单理解 数值越小观察位置越近)
  >
  > 给单个元素设置 tansform:perspective() ,使视角向后靠,让3d感更强,(近距离看不到3d的)

  

  > `perspective` **规则**用于将父级整个做为透视元素，会造成里面的每个子元素的透视是不一样的。对父元素没有影响, 一般容器内是一个立方体时使用,用同一个位置观察不同的元素
  >
  > `perspective()` 函数用于为元素设置单独透视，每个元素的透视效果是一样的。相当于把每个元素拿到同一个位置去看

  -  使用 `transform-style` 用于控制3d透视。参数:flat   preserve-3d

    > 应用于舞台即变形元素的父级元素
    > 设置 overflow:visible 时 preserve-3d 才无效



- 透视视角:

  > 通过 `perspective-origin` 属性来设置。默认透视视角为 观察者视角不变 物体变换位置
  >
  > 可以理解眼镜看物体的位置,看物体的左边或者右边
  >
  > 需要设置 `perspective` 透视后才可以看到效果。

  

- 使用 `backface-visibility` 用于控制是否可以看到元素的背面。

- > 一般设置在元素上而不是舞台元素上
  >
  > 需要舞台元素（父级元素）设置 `transform-style: preserve-3d`
  
- `transform-style: preserve-3d` 当z轴参与 需要3d感时需要给父元素加上



###  CSS过渡 (transitions)
  - transition 提供了一种在更改CSS属性时控制动画速度的方法

    > transition-property` 规定哪些属性进行改变
    >
    > `transition-duration` 变换时间
    >
    > `transition-delay`  指定延迟 
    >
    > 简写属性  `transition`
    >
    > transition: width 2s 1s(delay) linear, height 2s, transform 2s 



  - `transition-timing-function `指定一个函数，定义属性值变化快慢函数
    
    > 关键字 或者自定义 cubic-bezier()
    
    

- > 当过渡完成时触发一个事件，`transitionend`
  >
  > 想让一个元素的属性 transition起来 , 必须要先有预设值,  
  >
  > 部分属性是没有过渡属性的

### 动画

- 使用keyframes定义动画序列

  > @keyframes name {
  >
  > from{....} 
  >
  > to{....},
  >
  > 或者 关键帧
  >
  > 25%{}  75%{}...
  >
  > } 
  >
  > 帧动画也可以和form to混合使用
  >
  > 多个帧动画有共同的属性可以组合书写
  >
  > 25%,75%{
  >
  > }
  
- animation 属性:

  > `animation-duration`  设置动画中的属性的运行时长
  >
  >  `animation-timing-function`  动画变化速率 贝塞尔曲线 和transition一样
  >  `animation-direction`  设置动画方向  (方向指 动画的0% 100%)
  >
  >  `animation-delay` 延迟
  >  `animation-iteration-count` 设置动画重复次数， 可以指定infinite无限次重复动画
  >    ` animation-name`   指定应用的动画
  >
  > `animation-play-state`  指定动画暂停开始 
  >
  > ``animation-fill-mode` :用于定义动画播放结束后的处理模式，是回到原来状态还是停止在动画结束状态

- 简写属性 : animation

  > animation: 3s ease-in 1s 2 reverse both paused 动画名字
  >
  > 动画名字由@keyframes 定义

- 没有中间值的属性,没办法进行动画变化, 例如:solid  dashed 边框样式是没有中间值的,他会一瞬间变化没有动画



### 媒体查询和响应式

- style link 标签的 media属性,针对各设备的样式化 ,默认为 all

- 您可以在`media`属性中提供媒体类型或查询; 然后，只有在媒体条件为true时，才会加载此资源。

- @import url() media名字  查询条件   给各设备导入样式化文件

  > 利用import 把每个不同设备的css文件 进行导入同一个css文件

- only 关键字 : 支持媒体查询的浏览器应用样式
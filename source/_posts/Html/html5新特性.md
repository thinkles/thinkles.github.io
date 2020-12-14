---
title: html5基础
date: 
tags: html5
categories: HTML
---


##### 多媒体
- video audio属性
 
    controls : 为网页中的音频显示标准的HTML5控制器。
    autoplay : 使音频自动播放。
    loop : 使音频自动重复播放。
    muted  这个属性会导致媒体播放时，默认关闭声音。
    poster:这个属性指向了一个图像的URL，这个图像会在视频播放前显示。通常用于粗略的预览或者广告。
    preload 这个属性被用来缓冲较大的文件
- 可以用 < source> 标签来指定多个文件，以为不同浏览器提供可支持的编码格式
- < audio> 标签与 < video> 标签的使用方式几乎完全相同 < audio> 上式使用方式几乎一样，不支持宽高设置 同时也不支持poster属性
- 播放控制: 当你已经用新的元素将媒体嵌入 HTML 文档以后，你就可以用 JavaScript 代码 采用编程的方式来控制它们。  方法在实现媒体元素的接口中定义 常用的有 play() pause()....
- 停止媒体播放很简单，只要调用 pause() 方法即可，然而浏览器还会继续下载媒体直至媒体元素被垃圾回收机制回收。通过移除媒体元素的 src 属性（或者直接将其设为一个空字符串——这取决于具体浏览器）， 你可以摧毁该元素的内部解码，从而结束媒体下载
- 媒体元素支持在媒体的内容中从当前播放位置移到某个特定点。 这是通过设置元素的属性currentTime的值来达成的；有关元素属性的详细信息请看HTMLMediaElement


- < video> 标签不被支持时可以使用Flash播放Flash格式的影像。video 内嵌 object param标签

- 错误处理: 最后一个source元素上增加一个错误处理器。
```
<video controls>
  <source src="dynamicsearch.mp4" type="video/mp4"></source>
  <a href="dynamicsearch.mp4">
    <img src="dynamicsearch.jpg" alt="Dynamic app search in Firefox OS">
  </a>
  <p>Click image to play a video demo of dynamic app search</p>
</video>
```

- 给那些听不懂音频语言的人们提供一个音频内容岂不是一件很棒的事情吗？所以，感谢 HTML5< video>使之成为可能，有了WebVTT格式，你可以使用< track>标签。

#### svg
- SVG 是用于描述矢量图像的XML语言。 它基本上是像HTML一样的标记，只是你有许多不同的元素来定义要显示在图像中的形状，以及要应用于这些形状的效果。
- 引用svg   要通过 <img>元素嵌入SVG，你只需要按照预期的方式在 src 属性中引用它 缺点是无法通过js css操作图像
- 直接引用svg代码块
#### 区块 段落元素
- header article ..  这些区块标签在实际开发中很少用,主要功能是对文档内容进行分块
#### MathML
- 是一组标签,可以再内容中直接嵌入复杂的数学公式(有工作需要才学习)  
#### cavans

-----------------------
 以下内容作为h5前端不太有联系, 以下主要就是些API学习实现各个功能,和JsDOM  后端关系更大
 下列内容的学习放在后端学习/ JSDOM
** (具体参考现代 javascript教程文档) **
#### 本地存储
-  localStorage   sessionStorage  indexedDB
#### 设备(移动)访问
- 定位 检测方向 触控事件 使用摄像机 指针锁定
#### 通信 (属于后端接触范围 和网络还有关系 前端暂时不需要) 
-  WebSockets 是一种先进的技术。它可以在用户的浏览器和服务器之间打开交互式通信会话。使用此API，您可以向服务器发送消息并接收事件驱动的响应，而无需通过轮询服务器的方式以获得响应。
#### 拖放api  History API  全屏api

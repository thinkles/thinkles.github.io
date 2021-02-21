---

title:  HTML基础
date: {{ date }}
updated: {{date}}
tags: HTML基础
categories: HTML

---
## HTML基础：

#### 基本标签使用:

1. 段落标题：   h  p

2. 列表:  ul li  ol li  (列表的嵌套也是可以的)   描述列表: dl dt dd

   > 描述列表,浏览器的默认样式会在描述列表的描述部分< dd>和描述术语< dt>缩进

3. 强调语义：strong  em (更强的可访问性 语义性)   b i u(仅仅影响表象而且没有语义，被称为表象元素)

4. 超链接:  a    属性title :补充信息    通过id还可以设置锚点

> ```html
> <a href="https://download.mozilla.org..." download="firefox-installer.exe">下载</a>   
> // 当链接是下载资源时 使用download属性给一个资源名字
> <a href="mailto:nowhere@mozilla.org">向 nowhere 发邮件</a>
> //可以通过mailto: 来给一个邮件地址
> ```



5. < abbr title="" > 包裹一个缩略语或缩写,title属性里面是详细介绍, 
   < address>  标记联系方式   < sup> 和< sub>上标和下标。< code>标记计算机代码
   < pre>用来保存空白字符, 默认浏览器是忽略缩进和空格
6. < time datetime="">  将时间和日期标记为可供机器识别的格式

7. 块级内容引用:  blockquote 包裹起来表示，并且在cite属性里用URL来指向引用的资源 行内引用: q 包裹起来, 写cite属性

   > 块级引用:浏览器在渲染块引用时默认会增加缩进
   >
   > 行内引用:默认将其作为普通文本放入引号内表示引用
   >
   > cite属性默认不会显示,更好的做法是使用cite元素 附上a链接,元素默认的字体样式为斜体



8. 文档结构标签 < header> < footer > < aside>  < nav>  < main>

   > main 主题部分中  可用article   section  div等元素表示。
   >
   > 标签本质上没有特别,主要有语义的作用,   他会让可访问性更强

9. 换行和切割线 : hr  br

10. ins  del 元素, 删除文本和标记文本分别出现  文字划线 和 下划线 可以通过css改变样式,变成漂亮的标注 

------



#### 表格 

1. < table>  < tr>< td>  < th>: 样式加粗 一般设置小标题, < caption>

2. colspan 横跨 rowspan 竖跨  

3. 表格内结构标签：< thead>  < tbody> < tfoot>：没有特效，只是格式标签 . 

   scope 属性,添加在< th> 元素中 用来说明是列标题还是行标题  scope="col" scope="row"  (id headers也可以标记但是太麻烦了)

4. < colgroup>  < col>为一列添加样式 ,每个col 代表一列 ,这里是第二列应用样式,
   使用 span 可以指定列数  ,这里可以改成 一个< col span=2>  (为每一行设置样式是比较简单的,但是为每一列设置的话使用这种方法比较简单 或者 nth-child)

  ```
 <table>
 <colgroup>
    <col>
    <col style="background-color: yellow">
  </colgroup>
 <table>
  ```



------



#### HTML多媒体

- **img 图片标签:**    

- > title 鼠标悬停的信息
  >
  > alt 在图片无法显示时, 显示的描述信息
  >
  > width height  
  >
  > srcset 用于浏览器根据宽、高和像素密度来加载相应的图片资源

- html5 中的< figure> 和 < figcaption >元素 （放置图片的说明信息）：为图片提供一个语义容器，在标题和图片之间建立清晰的关联。

- > 注意 figure 里不一定要是一张图片，只要是一个这样的独立内容单元：图片、一段代码、音视频、方程、表格..  
  >
  > 它本身没有一个实质的作用,是一个语义化的容器

- **如何选择img 还是 background背景:**

- > 如果图像对您的内容里有意义，则应使用HTML图像。
  >
  > 如果图像纯粹是装饰，则应使用CSS背景图片。这样插入的图片完全没有语义上的意义, 也不能被屏幕阅读器识别。

- 像 img 和 video  ifram这样的元素有时被称之为**替换元素**，这样的元素的内容和尺寸由外部资源所定义，而不是元素自身。

- > 替换元素: 当资源加载出来后,元素将被替换掉,所以尺寸是外部资源所定义的,但是我们可以进行设置元素的大小

  ------

  

  

##### 自适应图片

- > 解决方法:    
  >
  > 1. 准备两套图片  img元素通过 srcset  sizes属性根据尺寸切换
  >
  > 2. 利用< picture> 中的source标签  通过他的media srcset属性实现
  >
  >    为什么不可以通过css  js 来实现响应式?
  >
  >    因为浏览器预加载 任意的图片 , 你不能先加载好 img 元素后, 再用 JavaScript 检测可视窗口的宽度，动态地加载小的图片替换已经加载好的图片，这就加载了两次

  ------
  
  

- **音视频标签: audio  video**

- > 属性值: 
  >
  > width 和 height 控制尺寸,如果没有保持纵横比,那么没有内容填充的部分默认为背景色
  >
  > controls : 为网页中的音频显示标准的HTML5控制器。
  > autoplay : 使音频自动播放。
  > loop : 使音频自动重复播放。
  > muted  这个属性会导致媒体播放时，默认关闭声音。
  > poster:这个属性指向了一个图像的URL，这个图像会在视频播放前显示。通常用于粗略的预览或者广告。
  > preload 这个属性被用来缓冲较大的文件
  >
  > < audio> 上使用方式几乎一样，但不支持宽高设置 同时也不支持poster属性

- video 标签内为备选内容, 用 < source  type=''> 标签来指定多个文件，以为不同浏览器提供可支持的编码格式

- 关于这种媒体元素,会有响应的事件和函数可以调用,详情查阅MDN ,常用的有 play() pause()....

- < track>标签, 提供一个音频内容的文本(字幕)

------



- **html 中的向量图**---svg图片,通过img标签添加,或者通过svg标签添加(这成为内联svg)

- > 矢量图形相较于同样的位图，通常拥有更小的体积，因为它们仅需储存少量的算法，而不是逐个储存每个像素的信息。在放大时不会产生失真
  >
  > 缺点 : 
  >
  > 无法使用JavaScript操作图像。
  > 如果要使用CSS控制SVG内容，则必须在SVG代码中包含内联CSS样式。 

- 可以使用SVG作为CSS背景图像

- 内联svg优缺点:

- > 将 SVG 内联减少 HTTP 请求，可以减少加载时间。
  >
  > 浏览器不能像缓存普通图片一样缓存内联SVG。



- < iframe>元素旨在允许您将其他Web文档嵌入到当前文档中。< embed>和< object>元素的功能不同于< iframe>—— 这些元素是用来嵌入多种类型的外部内容的通用嵌入工具,不太使用



#### URL和Path

- URL: 统一资源定位符（英文：**U**niform **R**esource **L**ocator）是一个定义了在网络上的位置的一个文本字符串, URL使用路径Path查找文件。路径指定文件系统中您感兴趣的文件所在的位置。URL分为:**绝对URL**和**相对URL**
  - **绝对URL**: 指向由其在Web上的绝对位置定义的位置，包括 protocol（协议） 和 domain name（域名）
  - **相对URL**:指向与您链接的文件相关的位置, 相对的基准默认是index.html主入口

- > 当链接到同一网站的其他位置时，你应该使用相对链接,当链接到另一个网站时，你需要使用绝对链接  
  >
  > 相对URL，浏览器只在同一服务器上查找被请求的文件。如果使用绝对URL会不断地让你的浏览器做额外的工作



#### 关于meta元数据

- 视口元标签:

  > ```
  > <meta name="viewport" content="width=device-width">
  > //除了这项选项,还有别的可用的选项
  > ```
  >
  > 这行代码会强制地让手机浏览器采用它们真实可视窗口的宽度来加载网页
  >
  > **原因 :**这个元标签的存在，是由于原来iPhone发布以后，人们开始在小的手机屏幕上阅览网页，而大多数站点未对移动端做优化的缘故。移动端浏览器因此会把视口宽度设为960像素, 渲染出缩小版本的网页, 但是你的媒体查询可能在960像素下不会生效
  >
  > 所以不加这行代码,可能只会渲染出缩小版本的网页

  

- `meta`元素包含了 name 和 content特性：

- > name  指定了 meta 元素的类型
  >
  > content  指定了实际的元数据内容

- 常见的  name ='description'     meta charset="utf-8"

> 关于有些meta元数据。可以是自定义的，它旨在向某些网站 (如社交网站) 提供可使用的特定信息。例如，Facebook 编写的元数据协议 [Open Graph Data](http://ogp.me/) 为网站提供了更丰富的元数据,  Twitter 还拥有自己的类型的专有元数据协议







#### 页面添加图标的方式

- 将图片保存在与网站的索引页面相同的目录中，以.ico格式保存（大多数浏览器将支持更通用的格式，如.gif或.png，但使用ICO格式将确保它能在如Internet Explorer 6一样久远的浏览器显示）, 在head中引用

> <link rel="shortcut icon" href="favicon.ico" type="image/x-icon">

#### 为文档设定主语言

> <html lang="zh-CN">

> 你还可以将文档的分段设置为不同的语言。例如，我们可以把日语部分设置为日语，
>
> ```
> <p>日语实例: <span lang="jp">ご飯が熱い。</span>.</p>
> ```



#### 内联元素和块级元素

- 以块的形式出现,对于在前面的内容它会出现新的一行,后面的内容也会被挤到下一行

- > address  article aside audio 
  > blockquote  canvas dd div dl 
  >
  > fieldset form figcaption figure  footer
  > h1~6  header hgroup hr noscript 
  > ol output p pre section
  > table ul  tfoot video

- 内联元素通常出现在块级元素中并环绕文档内容的一小部分,内联元素不会导致文本换行

- > b, big, i, small, tt
  > abbr, acronym, cite, code, dfn, em, kbd, strong, samp, var
  > a, bdo, br, img, map, object, q, script, span, sub, sup
  > button, input, label, select, textarea






#### 其他

- 如何将一张图片的不同区域链接到不同页面。: [图像映射](https://developer.mozilla.org/zh-CN/docs/learn/HTML/Howto/Add_a_hit_map_on_top_of_an_image)

- 数据属性 data-   还可以被css访问  

  > article::before {
  > content: attr(data-parent);
  > }
  > article[data-columns='3'] {
  > width: 400px;
  > }
- 在 < img> 元素中定义的图片可以从外部来源加载并在 < canvas> 元素中使用，就像是从本地源加载一样。在HTML5中，一些 HTML 元素提供了对 [CORS](https://developer.mozilla.org/en-US/docs/HTTP/Access_control_CORS) 的支持， 例如 < audio>、< img>、< link>、< script> 和 < video>均有一个跨域属性 (crossOrigin  property)，它允许你配置元素获取数据的 CORS 请求。 : [需要配置跨域](https://developer.mozilla.org/zh-CN/docs/Web/HTML/CORS_enabled_image)

-  < link> 元素的 rel 属性的属性值preload 可以实现在页面加载初期进行预加载

  

#### 怪异模式和标准模式  : <!doctype html>  

- 这个声明的目的是防止浏览器在渲染文档时，切换到我们称为“**怪异模式(兼容模式)**”的渲染模式。“<!DOCTYPE html>" 确保浏览器按照最佳的相关规范进行渲染，而不是使用一个不符合规范的渲染模式。

- 兼容模式下 会出现很多问题, 比如 css应用问题, 加上标签和不加标签可能会出现差异,不如各个浏览器排版会出现问题

  

  

---
title: html基础
date: 
tags: html基础
categories: HTML
---




## HTML基础：

#### 标签
1. 段落标题：< hn>< p>    强调语义：< strong 加粗> < em斜体> 为了阅读识别时使用，表现形式和< b> < i>没有区别           < span>没有语义  < div>  
2. < b> < i> < u> 粗 斜 下划线  用于标记联系方式的元素——< address> 斜体表示内容

3. 超链接 < a href target title> 不仅可以链接外部网页，还可以链接到同一份文档的另一部分
4. 描述列表 dl dd dt浏览器的默认样式会在描述列表的描述部分< dd>和描述术语< dt>缩进
5. < abbr title="" > 当光标指向文字用来对某文字进行解释
6. 使用上标和下标。< sup> 和< sub>元素

7. br hr    如果一个块级内容（一个段落、多个段落、一个列表等）从其他地方被引用，你应该把它用< blockquote>元素包裹起来表示，并且在cite属性里用URL来指向引用的资源  与之相对有行内引用《 q》

8. 文档结构标签 < header> < footer > < article> < aside> < section> < nav>:导航栏 < main>:主体内容
#### 事件 字符集

#### 全局属性

#### 表格 
1. < table> < td>  < th>: 样式加粗 一般设置小标题, < caption>
2.  colspan横跨 rowspan 竖跨  
3. 表格内结构标签：< thread:页眉 >  < tbody:定义表格主题> < tfoot:定义表格页脚>：没有特效，只是格式标签  
4. < colgroup>  < col>为一列添加样式 ,每个col 代表一列 ,这里是第二列应用样式,如果想应用某行某列
  span 表示该 < col> 元素横跨的列数
  ```
  <colgroup>
    <col>
    <col style="background-color: yellow">
  </colgroup>
  ```

5. scope 属性，可以添加在< th> 元素中 用来说明是列标题还是行标题  scope="col" scope="row"


#### 多媒体
1. < img src= alt= width height title= >   title鼠标悬停的信息   alt它的值应该是对图片的文字描述
- > 如果图像对您的内容里有意义，则应使用HTML图像。如果图像纯粹是装饰，则应使用CSS背景图片。
  
- html5 中的< figure> 和 < figcaption >元素 （放置图片的说明信息）：为图片提供一个语义容器，在标题和图片之间建立清晰的关联。
```
<figure>
  <img src="https://raw.githubusercontent.com/mdn/learning-area/master/html/multimedia-and-embedding/images-in-html/dinosaur_small.jpg"
     alt="一只恐龙头部和躯干的骨架，它有一个巨大的头，长着锋利的牙齿。"
     width="400"
     height="341">
  <figcaption>曼彻斯特大学博物馆展出的一只霸王龙的化石</figcaption>
</figure>
```

- < iframe>元素旨在允许您将其他Web文档嵌入到当前文档中。< embed>和< object>元素的功能不同于< iframe>—— 这些元素是用来嵌入多种类型的外部内容的通用嵌入工具,不太使用




### HTML 5
####  区块段落元素
- HTML5新增了几个新元素使得开发者可以用标准语义去描述web文档的结构。

        <section>, <article>, <nav>, <header>, <footer>, <aside> 和 <hgroup>
        新的HTML5标记 <time>元素，使用pubdate布尔值，表示整个文档的发布时间
        <blockquote> 元素是一个外部引用 ,表示该文字是引用内容
####  新的元素
>  < mark>， < figure>， < figcaption>， < data>， < time>， < output>， < progress>， 或者 < meter>和< main>，这增加了有效的 HTML5 元素的数量。
          


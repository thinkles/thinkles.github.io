---

title:  DOM 基础
date: {{ date }}
updated: {{date}}
tags: jsDOM
categories: Javascript

---
### Document

#### 浏览器环境
- JavaScript 语言已经发展成为一种具有多种用途和平台的语言。平台可以是一个浏览器，一个 Web 服务器，或其他主机（host）, 如果它能运行 JavaScript 的话。它们每个都提供了特定于平台的功能。JavaScript 规范将其称为 **主机环境**。
- > 主机环境提供了自己的对象和语言核心以外的函数。Web 浏览器提供了一种控制网页的方法。Node.JS 提供了服务器端功能 等

- window 代表一个"根"对象,是 JavaScript 代码的全局对象,又代表“浏览器窗口”，并提供了控制它的方法。(查看窗口高度等)

#### DOM  BOM
- 文档对象模型（Document Object Model），简称 DOM，将所有页面内容表示为可以修改的对象。
- > document 对象是页面的主要“入口点”。我们可以使用它来更改或创建页面上的任何内容。

- 浏览器对象模型（Browser Object Model），简称 BOM，表示由浏览器（主机环境）提供的用于处理文档（document）之外的所有内容的其他对象。
- > location 对象允许我们读取当前 URL , navigator 对象提供了有关浏览器和操作系统的背景信息。
- > 函数 alert/confirm/prompt 也是 BOM 的一部分,它代表了与用户通信的纯浏览器方法。


#### DOM 树
- 根据文档对象模型（DOM），每个 HTML 标签都是一个对象(元素节点)

- 文本节点中,空格和换行符都是完全有效的字符，就像字母和数字
- > **所以在标签和标签之间的空格 换行符会被解析成文本节点 **
  >
  > 例外 : < head> 之前的空格和换行符均被忽略 , 
- > HTML规范要求所有内容必须位于 < body> 内. 所以 </ body>之后不能有空格, body之后的内容会被自动移动到body中,并处在最下面
- 与 DOM 一起使用的浏览器工具,通常不会在文本的开始/结尾显示空格，并且在标签之间也不会显示空文本节点（换行符）。


- 自动修正 : 如果浏览器遇到格式不正确的 HTML，它会在形成 DOM 时自动更正它。
- > 表格是一个有趣的“特殊的例子”。按照 DOM 规范，它们必须具有 < tbody>,在形成DOM时,如果没有会给加上

- 常用的节点 :  

        document — DOM 的“入口点”。
        元素节点 — HTML 标签，树构建块。
        文本节点 — 包含文本。
        注释 


#### 遍历DOM
- 对 DOM 的所有操作都是以 document 对象开始。它是 DOM 的主“入口点”。从它我们可以访问任何节点。

        <html> = document.documentElement
        <body> = document.body
        <head> = document.head

- > document.body有可能为空,脚本无法访问在运行时不存在的元素。尤其是，如果一个脚本是在 < head> 中，那么脚本是访问不到 document.body 元素的(其他元素也访问不到。获得null值)
- 在 DOM 中，null 值就意味着“不存在”或者“没有这个节点”



- **遍历节点属性**
  
    > 子节点 :
    > childNodes : 返回集合(一个类数组的可迭代对象)列出了所有子节点，包括文本节点。 
    > firstChild 
    > lastChild     
    >
    > 兄弟节点和父节点:
    > parentNode
    > nextSibling  
    > previouSibling
    
    


- > 遍历DOM属性都是只读的，只能通过函数对节点进行操作，不能直接通过 “=” 赋值进行更改节点
    >
    > 几乎所有的 DOM 集合都是 **实时** 的。换句话说，它们反映了 DOM 的当前状态。当保留引用时,节点更新自动会出现在集合中

- > elem.hasChildNodes() 用于检查节点是否有子节点。





- **遍历元素节点属性 ：**
  
     > children — 仅那些作为元素节点的子代的节点。 返回实时更新的DOM集合
     > firstElementChild，lastElementChild — 第一个和最后一个子元素。
> previousElementSibling，nextElementSibling — 兄弟元素。
     > parentElement — 父元素。

- > parentElement 属性返回的是“元素类型”的父节点，而 parentNode 返回的是“任何类型”的父节点,这些属性通常来说是一样的 
  >
  > 唯一的例外就是 document.documentElement  因为document.documentElement.parentNode       //document   
>
  >  parentElement 属性则返回null  因为document不是一个元素节点

- 某些类型的 DOM 元素还可能会提供特定于其类型的其他属性。表格（Table）是一个很好的例子  。

        table.rows — <tr> 元素的集合。
        table.caption/tHead/tFoot — 引用元素 <caption>，<thead>，<tfoot>。
        table.tBodies — <tbody> 元素的集合
        
        <thead>，<tfoot>，<tbody> 元素提供了 rows 属性
        
        tr.cells — 在给定 <tr> 中的 <td> 和 <th> 单元格的集合。
        tr.sectionRowIndex — 给定的 <tr> 在封闭的 <thead>/<tbody>/<tfoot> 中的位置（索引）。
        tr.rowIndex — 在整个表格中 <tr> 的编号（包括表格的所有行）。
        td.cellIndex — 在封闭的 <tr> 中单元格的编号。
> HTML 表单（form）还有其它导航（navigation）属性

#### 节点搜索

- 节点查找方法

```
getElementById()
getElementsByTagname() 返回一个类数组,类数组只能使用下标 和length不具备其他功能
getElementsByClassName() 返回一个类数组
getElementsByName() 返回在文档范围内具有给定 name 特性的元素,返回类数组
querySelector()
querySelectorAll() 采用了css选择器的格式, 当元素只有一个,使用两个方法效果一样
```

- 可以直接使用和元素id相同名字的全局变量 来访问元素

  > 这种容易和相同命名的全局变量冲突,不建议使用

- 所有的 `getElementsBy` 方法都会返回一个 `实时的（live） 集合`。这样的集合始终反映的是文档的当前状态，并且在文档发生更改时会“自动更新”。

  >  相反，`querySelectorAll `返回的是一个 静态的 集合。就像元素的固定数组



- `elem.matches(css)` 不会查找任何内容，它只会检查 elem 是否与给定的 CSS 选择器匹配。它返回 true 或 false。

- > 当我们遍历元素（例如数组或其他内容）并试图过滤那些我们感兴趣的元素时，这个方法会很有用。

- `elem.closest(css) `  向上查找与 CSS 选择器匹配的最近的祖先。elem 自己也会被搜索。与选择器匹配，则停止搜索并返回该祖先。

- `elemA.contains(elemB)`  用来检查子级与父级之间关系的方法 ,返回Boolen

  

#### 节点属性：type，tag 和 content
- DOM 节点类 : 不同的 DOM 节点可能有不同的属性 例如文本节点与元素节点不同,但是所有这些标签对应的 DOM节点之间也存在共有的属性和方法，每个 DOM 节点都属于相应的内建类。
-  层次结构（hierarchy）的根节点是 EventTarget，Node 继承自它，其他 DOM 节点继承自 Node。

        EventTarget-> Node -> Element -> HTMLElement -> HTMLBodyElement 
                       |  \          \
                     Text  Comment   SVGElement


- >EventTarget — 是根的“抽象（abstract）”类。该类的对象从未被创建。它作为一个基础，以便让所有 DOM 节点都支持所谓的“事件（event）”.  为事件（包括事件本身）提供支持，
  
- >Node — 也是一个“抽象”类，充当 DOM 节点的基础。它提供了树的核心功能：parentNode，nextSibling，childNodes 等（它们都是 getter）。 提供通用 DOM 节点属性，

- >Element — 是 DOM 元素的基本类。它提供了元素级的导航（navigation），例如 nextElementSibling，children，以及像 getElementsByTagName 和 querySelector 这样的搜索方法,Element 类充当更多特定类的基本类：SVGElement，XMLElement 和 HTMLElement。   提供通用（generic）元素方法

- >HTMLElement — 最终是所有 HTML 元素的基本类。各种 HTML 元素均继承自它 ,它提供了通用（common）的 HTML 元素方法（以及 getter 和 setter）

- 给定节点的全部属性和方法都是继承的结果。  

- 正如我们所看到的，DOM 节点是常规的 JavaScript 对象。它们使用基于原型的类进行继承




- 查看 DOM 节点类名:

- > 对象通常都具有 constructor 属性。
  >
  > alert( document.body.constructor.name ); // HTMLBodyElement
  >
  > 或者我们可以对其使用 toString 方法
  >
  > 我们还可以使用 instanceof 来检查继承


- > console.log(elem) 显示元素的 DOM 树。
  >
  > console.dir(elem) 将元素显示为 DOM 对象适合探索其属性。  

  

  

  ##### 节点属性 :nodetype nodeName 和 tagName属性

- nodeType 属性返回数值表示节点类型, 我们只能读取 nodeType 而不能修改它。它是一种过时的方法

- 从 nodeName 或者 tagName 属性中读取它的标签名：

  > tagName 属性仅适用于 Element 节点。nodeName 是为任意 Node 定义的：对于元素，它的意义与 tagName 相同。
  >
  > 对于其他节点类型（text，comment 等），它拥有一个对应节点类型的字符串。#text #comment...

  

  ##### 节点内容: inner/outerHTML属性 

- `innerHTML` 属性允许将元素内的 HTML 获取为**字符串形式**。我们也可以修改它。(替换原来的元素)

- “innerHTML+=” 会进行完全重写,移除旧的内容。然后写入新的 innerHTML（新旧结合）。

- >移除内容的副作用: 因为内容已“归零”并从头开始重写，因此所有的图片和其他资源都将重写加载

- innerHTML 不包括外围的元素标签,只在元素内中作用

- `outerHTML` 属性包含了元素的完整 HTML。就像 innerHTML 加上元素本身一样。

- > 与 `innerHTML` 不同，写入 `outerHTML` 不会改变元素。而是在 DOM 中替换它。 换句话说 div.outerHTML = '< p>..< /p>'   这时Dom被替换了,但是div.outerHTML的值还是不会改变




- **nodeValue/data**：读取/设置文本节点内容 , **仅对文本节点有效**

  > 和innerHTML  的区别: innerHTML 属性仅对元素节点有效

  ##### textContent: 纯文本

- > textContent  对所有节点有效, 获取文本,去掉所有 < tags>

- 写入 textContent 要有用得多，因为它允许以“安全方式”写入文本。 所有符号（symbol）均按字面意义处理。例如可以写入 < b> 等标签像普通字符一样


- > innerText 和textContent  区别, textContent 可以输出所有文本内容,innerText只能输出哪些在页面渲染出来的文本, 如果部分文本隐藏,无法显示
  >
  > innerHTML 和textContent 区别在于, innerHtml会连同标签一起输出

  

- “hidden” 特性（attribute）和 DOM 属性（property）指定元素是否可见。我们可以在 HTML 中使用它，或者使用 JavaScript 进行赋值

#### 特性和属性（Attributes and properties）
- 特性（attribute）— 写在 HTML 中的内容。属性（property）— DOM 对象中的内容。

- 对于元素节点，大多数标准的 HTML 特性（attributes）会自动变成 DOM 对象的属性（properties）, 

- > 但是 特性 — 属性映射并不是一一对应的

- DOM 属性 :  DOM 节点是常规的 JavaScript 对象,我们可以创建一个新的属性(属性值 函数...),还可以修改内建属性的原型，例如修改 Element.prototype 为所有元素添加一个新方法

- HTML特性 :  在HTML中，标签可能拥有特性（attributes）。当浏览器解析HTML文本，并根据标签创建 DOM 对象时，浏览器会辨别 标准的 特性并以此创建 DOM 属性。但是非 标准的 特性则不会。

  > 如果一个特性不是标准的，那么就没有相对应的 DOM 属性

- > 一个元素的标准的特性对于另一个元素可能是未知的。例如 "type" 是 < input> 的一个标准的特性（HTMLInputElement），但对于 < body>（HTMLBodyElement）来说则不是。

  

    ##### 特性操作 (所有特性都可以通过使用以下方法进行访问):
  
        elem.hasAttribute(name) — 检查特性是否存在。
        elem.getAttribute(name) — 获取这个特性值。
      elem.setAttribute(name, value) — 设置这个特性值。
        elem.removeAttribute(name) — 移除这个特性。
        elem.attributes 读取所有特性, 属于内建 Attr 类的对象的集合,具有 name 和 value 属性。(可迭代对象)
  
  


- HTML特性有以下几个特征：它们的名字是大小写不敏感的（id 与 ID 相同）。它们的值总是字符串类型的(使用alert输出总是字符串)。
- >   使用get...获取时大小写不敏感 ; 
  >
  >   将任何东西赋值set..给特性，这些东西会变成字符串类型 ;
  
  

- 当一个标准的特性被改变，对应的属性也会自动更新，（除了几个特例）反之亦然。
- > 例如 input.value 只能从特性同步到属性，反过来则不行：
  >
  > 这个“功能”在实际中会派上用场，因为用户行为可能会导致 value 的更改，然后在这些操作之后，如果我们想从 HTML 中恢复“原始”值，那么该值就在特性中。



- DOM 属性不总是字符串类型的 :   input.checked 属性（对于 checkbox 的）是布尔型的。 style 属性是一个对象
- > 尽管大多数 DOM 属性都是字符串类型的。有一种非常少见的情况，即使一个 DOM 属性是字符串类型的，但它可能和 HTML 特性也是不同的。 href 属性 : 一直是一个完整的 URL  href特性 : 标签中的值

  

  ##### 自定义(非标准)特性

- 有时，非标准的特性常常用于将自定义的数据从 HTML 传递到 JavaScript，或者用于为 JavaScript “标记” HTML 元素。但是自定义的特性也存在问题,名字之间可能会有冲突。

- 如果我们出于我们的目的使用了非标准的特性，之后它被引入到了标准中并有了其自己的用途，该怎么办？ 为了避免冲突，存在 data-* 特性。

- >所有以 “data-” 开头的特性均被保留供程序员使用。它们可在 dataset 属性中使用。如果一个 elem 有一个名为 "data-about" 的特性，那么可以通过 elem.dataset.about 取到它


- 在大多数情况下，最好使用 DOM 属性。仅当 DOM 属性无法满足开发需求，并且我们真的需要特性时，才使用特性

  > 例如：我们需要一个非标准的特性。但是如果它以 data- 开头，那么我们应该使用 dataset。
  >
  > 我们想要读取 HTML 中“所写的”值。对应的 DOM 属性可能不同，例如 href 属性一直是一个 完整的 URL，但是我们想要的是“原始的”值。
  
- 自定义属性可以和 样式结合起来,  通过css属性选择符,规定特定的属性有特定的样式

--------------
#### 修改文档(document)

- 创建元素 : 
  
- > document.createElement(tag) //元素节点
  >
  > document.createTextNode(text) //文本节点
  
- 插入元素 :
      node.append(...nodes or strings) — 在 node 末尾 插入节点或字符串，
        node.prepend(...nodes or strings) — 在 node 开头 插入节点或字符串，
        node.before(...nodes or strings) — 在 node 前面 插入节点或字符串，
        node.after(...nodes or strings) — 在 node 后面 插入节点或字符串，
        node.replaceWith(...nodes or strings) — 将 node 替换为给定的节点或字符串。
  
- > 这些方法可以在单个调用中一次**插入多个节点列表**和文本片段。
    >
    >  div.before('<p>Hello</p>',document.createElement('hr')); 这里的文字都被“作为文本”插入，而不是“作为 HTML 代码”
    >
    > 这些方法只能用来插入 DOM 节点或文本片段。

  
  
- 如果我们想要将内容“作为 HTML 代码插入”，像使用 `elem.innerHTML` 所表现的效果一样,我们可以使用另一个非常通用的方法：elem.insertAdjacentHTML(where, html)。

- **elem.insertAdjacentHTML(where, html): **

- 第一个参数是代码字（code word），指定相对于 elem 的插入位置。第二个参数是 HTML 字符串，该字符串会被“作为 HTML” 插入

    ```java
    "beforebegin" — 将 html 插入到 elem 前插入，
    "afterend" — 将 html 插入到 elem 后。
    "afterbegin" — 将 html 插入到 elem 开头，
    "beforeend" — 将 html 插入到 elem 末尾，
    ```
    
- > elem.insertAdjacentText(where, text) — 语法一样，但是将 text 字符串“作为文本”插入而不是作为 HTML，    
>
    > elem.insertAdjacentElement(where, elem) — 语法一样，但是插入的是一个元素。  

    > 它们的存在主要是为了使语法“统一”。因为对于元素和文本，我们有 append/prepend/before/after 方法 


- 节点移除:
- 想要移除一个节点 :  node.remove()。
- > 如果我们要将一个元素移动到另一个地方,不需要删除后再添加,插入方法会自动从旧位置删除该节点。在新的位置添加

- 克隆节点：
- >  调用 `elem.cloneNode(true)` 来创建元素的一个“深”克隆 — 具有所有特性（attribute）和子元素。     
  >
  >   如果我们调用` elem.cloneNode(false)`，那克隆就不包括子元素。


- DocumentFragment 是一个特殊的 DOM 节点，用作来传递节点列表的包装器（wrapper）。....之所以提到 DocumentFragment ，主要是因为它上面有一些概念，例如 [template](https://zh.javascript.info/template-element) 元素，我们将在以后讨论。

  

  

  ##### 老式的 insert/remove 方法

- 由于历史原因，还存在“老式”的 DOM 操作方法。我们在这儿列出这些方法的唯一原因是，你可能会在许多就脚本中遇到它们。

  > parentElem.appendChild(node)  将 node 附加为 parentElem 的最后一个子元素。
  >
  > parentElem.insertBefore(node, nextSibling)  在 parentElem 的 nextSibling 前插入 node。
  >
  > parentElem.replaceChild(node, oldChild)  将 parentElem 的后代中的 oldChild 替换为 node。
  >
  > parentElem.removeChild(node) 从 parentElem 中删除 node（假设 node 为 parentElem 的后代）。

- >所有这些方法都会返回插入/删除的节点。换句话说，parentElem.appendChild(node) 返回 node。

  

- 还有一个非常古老的向网页添加内容的方法：document.write 调用 document.write(html) 意味着将 html “就地马上”写入页面。document.write`调用只在页面加载时工作,如果我们稍后调用它，则现有文档内容将被擦除。

- > 调用 `document.write` 方法来写入一些东西，浏览器会像它本来就在 HTML 文本中那样使用它。所以它运行起来出奇的快，因为它 **不涉及 DOM 修改**。它直接写入到页面文本中，而此时 DOM 尚未构建。


#### 样式和类

- JavaScript 既可以修改类，也可以修改 style 属性。相较于将样式写入 style 属性，我们应该首选通过 CSS 类的方式来添加样式。仅当类“无法处理”时，才应选择使用 style 属性的方式。

- className 和 classList 
  
    ```java
    elem.className 对应于 "class" 特性（attribute）。
    类似 setAttribute()操作,整体替换
    
    如果我们对 elem.className 进行赋值，它将替换类中的整个字符串。有时，这正是我们所需要的，但通常我们希望添加/删除单个类。 (某个元素可能有多个类名),我们既可以使用 className 对完整的类字符串进行操作，也可以使用使用 classList 对单个类进行操作
    
    elem.classList 是一个特殊的对象，它具有 add/remove/toggle 单个类的方法。
    ```
    
- >elem.classList.contains(class) — 检查给定类，返回 true/false。

- classList 是可迭代的 可以使用for of 进行输出所有类名

    ##### 元素样式

- elem.style 属性是一个对象，它对应于 "style" 特性（attribute）中所写的内容

- > 对于多词（multi-word）属性，使用驼峰式 . 前缀属性像 -moz-border-radius 和 -webkit-border-radius 这样的浏览器前缀属性，也遵循同样的规则

- **重置样式属性** : 

    - 不应该使用 delete elem.style.display，而应该使用 elem.style.display = "" 将其赋值为空。

- **设置多个样式属性** : 
  
  - 我们不能像这样的 div.style="color: red; width: 100px" 设置完整的属性，因为 div.style 是一个对象，并且它是只读的

  - 想要以字符串的形式设置完整的样式，可以使用特殊属性 style.cssText ,这样的赋值会删除所有现有样式：它不是进行添加样式，而是替换所有样式。
  
  - > 可以通过设置一个特性（attribute）来实现同样的效果：div.setAttribute('style', 'color: red...')。



- style 属性仅对 "style" 特性（attribute）值起作用,不能获取在style外应用的样式,如果想要获得**当前应用的css样式**使用以下 :


- **计算样式**：getComputedStyle(element, [pseudo])  

- > 结果是一个具有样式属性的对象 ,通过 . 调用获得当前应用样式
  >
  > pseudo : 表示一个伪元素  ::before    要添加引号


- 计算值和解析值

  > 计算 (computed) 样式值是所有 CSS 规则和 CSS 继承都应用后的值，它看起来像 height:1em 或 font-size:125%。

  > 解析 (resolved) 样式值是最终应用于元素的样式值值。浏览器将使用计算（computed）值，并使所有单位均为固定的,且为绝对单位，例如：height:20px 或 font-size:16px

- > 现在 getComputedStyle 实际上返回的是属性的解析值（resolved）。


- getComputedStyle 需要完整的属性名,我们应该总是使用我们想要的确切的属性，例如 paddingLeft、marginTop 或 borderTopWidth。否则，就不能保证正确的结果。

- > 如果有 paddingLeft/paddingTop 属性，那么对于getComputedStyle(elem).padding，我们会得到什么？什么都没有，或者是从已知的 padding 中“生成”的值？这里没有标准的规则。

-   getComputedStyle 没有办法是用伪类获取 样式

- > JavaScript 看不到 `:visited` 所应用的样式。此外，CSS 中也有一个限制，即禁止在 `:visited` 中应用更改几何形状的样式

#### 元素大小 滚动

- JavaScript 中有许多属性可让我们读取有关元素宽度、高度和其他几何特征的信息。

    

- 一些浏览器（并非全部）通过从内容大小中获取空间来为滚动条保留空间。 如果有padding在padding中出现滚动条  

- > google 内容宽度计算会减去滚动条的宽度   火狐则是 内容大小不变(但实际滚动条仍是占据了内容宽度)   不管有无padding都遵循上述内容

- > 如果元素中有很多文本，并且溢出了，那么浏览器会在 padding-bottom 处显示“溢出”文本，这是正常现象。

- 如果一个元素（或其任何祖先）具有 display:none 或不在文档中，则所有几何属性均为零（或 offsetParent 为 null）。 我们可以用它来检查一个元素是否被隐藏 :    

- ```javascript
return !elem.offsetWidth &&!elem.offsetHeight;
  ```

    

    

     **offsetParent，offsetLeft/Top ,offsetWidth/Height  :**

- offsetParent 返回一个最接近的祖先

    ```html
    最近的祖先为下列之一：
    CSS 定位的（position 为 absolute，relative 或 fixed），
    或 <td>，<th>，<table>，
    或 <body>。
    ```

- > 属性 offsetLeft/offsetTop 提供相对于 offsetParent 左上角的 x/y 坐标。
    >
    > offsetWidth/Height 提供了元素的完整大小（包括边框）。

    **clientTop/Left,clientWidth/Height :**

- clientTop/Left  : 在元素内部，我们有边框（border)。为了测量它们，可以使用 clientTop 和 clientLeft。

- > 但准确地说 — 这些属性不是边框的 width/height，而是内侧与外侧的相对坐标。当文档从右到左显示（操作系统为阿拉伯语或希伯来语）时，影响就显现出来了。此时滚动条不在右边，而是在左边，此时 clientLeft 则包含了滚动条的宽度。

- clientWidth/Height : 这些属性提供了元素边框内区域的大小。 它们包括了 “content width” 和 “padding”，但不包括滚动条宽度（scrollbar）：

- >如果这里没有 padding，那么 clientWidth/Height 代表的就是内容区域，即 border 和 scrollbar（如果有）内的区域。

**scrollWidth/Height, scrollLeft/scrollTop :**


- scrollWidth/Height :  这些属性就像 clientWidth/clientHeight，但它们还包括滚动出（隐藏）的部分：

- >scrollHeight : 是内容区域的完整内部高度，包括滚动出的部分。  
  >
  >scrollWidth  : 是完整的内部宽度，没有水平滚动时，它等于 clientWidth。

- scrollLeft/scrollTop :  是元素的隐藏、滚动部分的 width/height。 换句话说，scrollTop 就是“已经滚动了多少”。

- >除了 scrollLeft/scrollTop 外，所有属性都是只读的。如果我们修改 scrollLeft/scrollTop，浏览器会滚动对应的元素。




- 不要从 CSS 中获取 width/height
  1. 首先，CSS width/height 取决于另一个属性：box-sizing，它定义了“什么是” CSS 宽度和高度。出于 CSS 的目的而对 box-sizing 进行的更改可能会破坏此类 JavaScript 操作
  2. 其次，CSS 的 width/height 可能是 auto，例如内联（inline）元素： 在 JavaScript 中，我们需要一个确切的 px 大小，以便我们在计算中使用它。因此，这里的 CSS 宽度没什么用
  3. 滚动条问题:使用 getComputedStyle(elem).width 时，有些返回实际宽度, 有些返回的宽度中包含滚动条

#### Window 大小和滚动

- **获取窗口（window）的宽度和高度**:，我们可以使用 document.documentElement 的 clientWidth/clientHeight 
- > **clientWidth/clientHeight** 会提供没有滚动条（减去它）的 width/height。换句话说，它们返回的是可用于内容的文档的可见部分的 width/height
  >
  > **window.innerWidth/innerHeight** 属性。 则包括了滚动条的宽度高度

- **获取文档的 width/height:**, 从理论上讲，由于根文档元素是 document.documentElement，并且它包围了所有内容，因此我们可以通过使用 documentElement.scrollWidth/scrollHeight 来测量文档的完整大小。
但是在该元素上，对于整个文档，这些属性均无法正常工作。

> 为了可靠地获得完整的文档高度，我们应该采用以下这些属性的最大值：
```javascript

let scrollHeight = Math.max(
  document.body.scrollHeight, document.documentElement.scrollHeight,
  document.body.offsetHeight, document.documentElement.offsetHeight,
  document.body.clientHeight, document.documentElement.clientHeight
);

```

- **获得当前滚动** :
- DOM 元素的当前滚动状态在 elem.scrollLeft/scrollTop 中。
- >对于文档滚动，在大多数浏览器中，我们可以使用 document.documentElement.scrollLeft/Top，但在较旧的基于 WebKit 的浏览器中则不行
  >
  >我们根本不必记住这些特性，因为滚动在 `window.pageXOffset/pageYOffset` 中可用
  >
  >这些属性是只读的


- **更改滚动**：
- > scrollTo，scrollBy，scrollIntoView
  >
  > window.scrollBy(x,y),将页面滚动至相对于当前位置(针对窗口)的 (x, y) 位置  
  >
  > window.scrollTo(pageX,pageY)。将页面滚动至绝对坐标(针对文档)        这些方法适用于所有浏览器。
  >
  > 对 elem.scrollIntoView(top) 的调用将滚动页面以使 elem 可见 ,  top=true（默认值） 页面滚动，使 elem 出现在窗口顶部  top=false，页面滚动，使 elem 出现在窗口底部。

  


-  禁止滚动 :要使文档不可滚动，只需要设置 document.body.style.overflow = "hidden"。该页面将冻结在其当前滚动上。 document.body.style.overflow = ‘’ 恢复滚动
- > 我们还可以使用相同的技术来“冻结”其他元素的滚动，而不仅仅是 document.body。

####  坐标
- 大多数 JavaScript 方法处理的是以下**两种坐标系**中的一个：

- > 相对于窗口 — 类似于 position:fixed，从窗口的顶部/左侧边缘计算得出。我们将这些坐标表示为` clientX/clientY`
  >
  > 相对于文档 — 与文档根中的 position:absolute 类似，从文档的顶部/左侧边缘计算得出。我们将它们表示为 `pageX/pageY`。


- **元素坐标**：elem.getBoundingClientRect() 

- > 方法 `elem.getBoundingClientRect()` 返回最小矩形的窗口坐标，该矩形将 `elem` 作为内建 [DOMRect](https://www.w3.org/TR/geometry-1/#domrect) 类的对象

- > 返回值是一个 DOMRect 对象
  >
  > 主要 DOMRect 属性 :
  >
  > x/y — 矩形原点相对于窗口的 X/Y 坐标，
  > width/height — 矩形的 width/height（可以为负）。
  > 此外，还有派生（derived）属性：
  >
  > top/bottom — 顶部/底部矩形边缘的 Y 坐标，
  > left/right — 左/右矩形边缘的 X 坐标。    
  >
  > 窗口的所有坐标都从左上角开始计数 ,与CSS position top/bottom属性不同



- elementFromPoint(x, y), 会返回在窗口坐标 (x, y) 处嵌套最多（the most nested）的元素。

- > 方法 document.elementFromPoint(x,y) 只对在可见区域内的坐标 (x,y) 起作用。

  

- **文档坐标:**

- 文档相对坐标从文档的左上角开始计算，而不是窗口。

- 这两个坐标系统通过以下公式相连接：

- > pageY = clientY + 文档的垂直滚动出的部分的高度。  pageX = clientX + 文档的水平滚动出的部分的宽度。

- > 窗口坐标非常适合和 position:fixed 一起使用，文档坐标非常适合和 position:absolute 一起使用。

### 事件简介

#### 浏览器事件

- 常用事件:      

  > Document 事件 : DOMContentLoaded —— 当 HTML 的加载和处理均完成，DOM 被完全构建完成时。
  >
  > CSS 事件：transitionend —— 当一个 CSS 动画完成时。
  >
  > 键盘事件  鼠标事件  表单事件

  

  ##### 事件处理  

  1. 事件处理程序可以设置在 HTML 中名为 on< event> 的特性（attribute）中。

     > 如果在HTML特性中写处理程序,this表示全局对象 
     >
     > window 在严格模式中为 undefined。 

  - 不要对处理程序使用 setAttribute。
    
    >  document.body.setAttribute('onclick', function() { alert(1) });   //失效

  - 如果一个处理程序是通过 HTML 特性（attribute）分配的，浏览器读取特性的内容创建一个新函数，并将这个函数写入 DOM 属性（property）

    


  2. 我们还可以使用 DOM 属性（property）on< event> 来分配处理程序。

     > elem.onclick = function() {}  
     >
     > 要移除一个处理程序 —— 赋值 elem.onclick = null。
     >
     > 使用dom属性分配处理程序, 如果为此事件分配两个处理程序,新的 DOM 属性将覆盖现有的 DOM 属性  
  3. addEventListener 和 removeEventListener 来管理处理程序的替代方法,可以为一个事件分配多个处理程序

     > `element.addEventListener(event, handler[, options]);`  
     >
     > 和`removeEventListener`语法相同
     >
     > options具有以下属性的附加可选对象：
     > once：如果为 true，那么会在被触发后自动删除监听器。
     > capture：事件处理的阶段，true :捕获阶段处理程序
     > passive：如果为 true，那么处理程序将不会调用 preventDefault()，
     >
     > 由于历史原因，options 也可以是 false/true，它与 {capture: false/true} 相同。

  - 要移除处理程序，我们需要传入与分配的函数完全相同的函数

  - > 直接给于匿名函数(或箭头函数)来移除处理程序无法实现,过程中相当于创建了两个相同内容的不同的函数,
    >
    > **所以如果我们不将函数存储在一个变量中，那么我们就无法移除它**
    >
    > 对于某些事件，只能通过 addEventListener 设置处理程序,例如 transtionend 和 DOMContentLoaded

  

- HTML特性中处理函数带括号,而其余的处理方式不带括号,带括号相当于直接函数调用

  

- 为什么要使用 `addEventListener`?

- > 它允许给一个事件注册多个监听器
  >
  > 它提供了一种更精细的手段控制 `listener` 的触发阶段。（即可以选择捕获或者冒泡）
  >
  > 它对任何 DOM 元素都是有效的

  

  ##### 事件对象
  
- 当事件发生时，浏览器会创建一个 event 对象，将详细信息放入其中，并将其作为参数传递给处理程序。

- event 对象的一些属性：

- > type  事件类型     
  >
  > currentTarget   处理事件的元素  
  >
  > .event.clientX / event.clientY  指针事件中指针的窗口相对坐标
  >
  > 
  >
  > 还有很多属性。其中很多都取决于事件类型：键盘事件具有一组属性，指针事件具有另一组属性

- event 对象在 HTML 特性中处理程序中也可用

  

  ##### 对象处理程序

- 我们不仅可以分配函数，还可以使用 addEventListener 将一个对象分配为事件处理程序。当事件发生时，就会调用该对象的 handleEvent 方法。 我们还可以为此使用类创建一个对象

- 此类中必须要有一个handleEvent(event) 方法

  > handleEvent 方法不必通过自身完成所有的工作。它可以调用其他特定于事件的方法

  > 这样会使事件处理程序分离出来,处理多个事件时更加方便, 利于后期维护   : 事件委托章节中的实例诠释了这种写法


#### 冒泡和捕获
- **冒泡**（bubbling）原理很简单。当一个事件发生在一个元素上，它会首先运行在该元素上的处理程序，然后运行其父元素上的处理程序，然后一直向上到其他祖先上的处理程序。


- > event.target —— 是引发事件的“目标”元素，它在冒泡过程中不会发生变化。  计划运行的元素
  >
  > this —— 实际上处理事件的"当前"元素.  ( == event.currentTarget)  
  
- 冒泡事件从目标元素开始向上冒泡 ,它们会调用路径上所有的处理程序。

- **停止冒泡的方法**是 event.stopPropagation()。 如果一个元素在一个事件上有多个处理程序，即使其中一个停止冒泡，其他处理程序仍会执行, event.stopImmediatePropagation() 方法，可以用于停止冒泡，并阻止当前元素上的其他处理程序运行

- > 通常，没有真正的必要去阻止冒泡。一项看似需要阻止冒泡的任务，可以通过其他方法解决。

- **捕获** : 

- DOM 事件标准描述了事件传播的 3 个阶段：

  > 捕获阶段（Capturing phase）—— 事件（从 Window）向下走近元素。      
  >
  > 目标阶段（Target phase）—— 事件到达目标元素。
  >
  > 冒泡阶段（Bubbling phase）—— 事件从元素上开始冒泡。

- > 为了在捕获阶段捕获事件，我们需要将处理程序的 capture 选项设置为 true 如果为 false（默认值），则在冒泡阶段设置处理程序。

- 要移除处理程序，`removeEventListener` 需要同一阶段


- 如果我们在同一阶段有多个事件处理程序，并通过 `addEventListener` 分配给了相同的元素，则它们的运行顺序与创建顺序相同

> 冒泡 和捕获 的两种方式, 实际上在影响事件的执行顺序, 冒泡是子元素首先执行事件,  捕获 是父元素首先执行事件



#### 事件委托

- ```javascript
  event.target.closest('td')  针对td中有嵌套元素时,用来获取td元素的方法 
  ```

- 委托示例：标记中的行为  看看实例的书写 https://zh.javascript.info/event-delegation 通过自定义特性进行标记, 写一个类, 通过获取自定义属性执行相应的函数, 把该类绑定到父元素上事件处理

- 行为模式 :将描述其行为的自定义特性添加到元素。 用文档范围级的处理程序追踪事件(通过事件委托),如果事件发生在具有特定特性的元素上,则执行行为   

- > 这可能变得非常方便 —— 无需为每个这样的元素编写 JavaScript。只需要添加自定义特性。文档级处理程序使其适用于页面的任意元素。  可能为库 框架提供一个方便的调用


#### 浏览器默认行为

- > 点击一个链接 —— 触发导航（navigation）到该 URL。
  >
  > 点击表单的提交按钮 —— 触发提交到服务器的行为。
  >
  > 在文本上按下鼠标按钮并移动 —— 选中文本。

- 某些事件会相互转化。如果我们阻止了第一个事件，那就没有第二个事件了。



- 有两种方式来告诉浏览器我们不希望它执行默认行为：
  
- >  主流的方式是使用 event 对象。有一个 event.preventDefault() 方法。
  >
  >  如果处理程序是使用 on< event>（而不是 addEventListener）分配的，那返回 false 也同样有效。
  >
  >  从处理程序返回 false 是一个例外, 事件处理程序返回的值通常会被忽略。唯一的例外是从使用 on<event> 分配的处理程序中返回的 return false。
  
  
  
- addEventListener 的可选项 passive: true 向浏览器发出信号，表明处理程序将不会调用 preventDefault()。

  > 为什么需要?  

- 如果默认行为被阻止，那么 `event.defaultPrevented 属性`为 true，否则为 false。

- > 有时我们可以使用 event.defaultPrevented 来代替，来通知其他事件处理程序，该事件已经被处理。 不再使用event.stopPropagation() , 来阻止冒泡  
  >
- event.stopPropagation() 和 event.preventDefault()是两个不同的东西。它们之间毫无关联。

  
#### 创建自定义事件
- 内建事件类形成一个层次结构（hierarchy），类似于 DOM 元素类。根是内建的 Event 类。

-  **创建自定义事件过程** :

  - new Event(type[, options]); 

  > type —— 事件类型，可以是像这样 "click" 的字符串，或者我们自己的像这样 "my-event" 的参数。
  >
  > options —— 具有两个可选属性的对象：
  >
  > bubbles: true/false —— 如果为 true，那么事件会冒泡
  >
  > cancelable: true/false —— 如果为 true，那么“默认行为”就会被阻止。
  >
  > 默认情况下，以上两者都为 false：必须设置 bubbles:true，否则事件不会向上冒泡。

  - 事件对象被创建后，使用 elem.dispatchEvent(event) 调用,在元素上''运行''。然后,就可以像常规事件一样捕获该事件 进行处理

  - > 我们应该对我们的自定义事件使用 addEventListener，因为 on< event> 仅存在于内建事件中，document.onhello 则无法运行。

  

- 有一种方法可以区分“真实”用户事件和通过脚本生成的事件。

- > 对于来自真实用户操作的事件，event.isTrusted 属性为 true，
  >
  > 对于脚本生成的事件，event.isTrusted 属性为 false

  
  
- 对于UI事件, 像 MouseEvent 和 KeyboardEvent ,如果我们想要创建这样的事件，我们应该使用它们而不是 `new Event`。例如，`new MouseEvent("click")`。

- > 正确的构造器允许为该类型的事件指定标准属性。就像鼠标事件的 `clientX/clientY` 一样：
  >
  > ```javascript
  > let event = new MouseEvent("click", 
  >         {
  >          bubbles: true, 
  >          cancelable: true,  
  >          clientX: 100,  
  >          clientY: 100 
  >         });
  > ```

  


- 对于我们自己的全新事件类型，例如 "hello"，我们应该使用 new CustomEvent()。

- > 从技术上讲，CustomEvent 和 Event 一样。除了一点不同。 在第二个参数（对象）中，我们可以为我们想要与事件一起传递的任何自定义信息添加一个附加的属性 detail。以避免与其他事件属性的冲突。
  >
  > ```javascript
  > <h1 id="elem">Hello for John!</h1>
  > 
  > <script>
  >   // 事件附带给处理程序的其他详细信息
  >   elem.addEventListener("hello", function(event) {
  >     alert(event.detail.name);
  >   });
  > 
  >   elem.dispatchEvent(new CustomEvent("hello", {
  >     detail: { name: "John" }
  >   }));
  > </script>
  > ```

  

- 对于新的，自定义的事件，绝对没有默认的浏览器行为，但是绑定此类事件的代码可能有自己的计划，触发该事件之后应该做什么。

- > 通过调用 event.preventDefault()，事件处理程序可以发出一个信号，指出这些行为应该被取消。函数结束
  >
  > 在这种情况下，elem.dispatchEvent(event) 的调用会返回 false




- 通常事件是在队列中处理的。根据顺序进行调用

- >值得注意的例外情况就是，一个事件是在另一个事件中发起的,例如使用 `dispatchEvent`。这类事件将会被立即处理
  >
  >如果不想改变顺序,可以把另一个触发事件的调用包装到零延迟的 setTimeout 中

  

### UI事件


   ##### 鼠标事件

- 此类事件不仅可能来自于“鼠标设备”，还可能来自于对此类操作进行了模拟以实现兼容性的其他设备，例如手机和平板电脑。

- 在单个动作触发多个事件时，事件的顺序是固定的。也就是说，会遵循 mousedown → mouseup → click 的顺序调用处理程序。

    ##### 键位获取属性

- event.button 属性 获取鼠标按钮键位 ,左中右键,分别返回0 1 2 ...

- > 旧代码可能会使用event.which属性获取鼠标键盘键位，这是获取按钮的一种旧的非标准方式， 现在不推荐使用(考虑兼容性可以使用)

- 所有的鼠标事件都包含有关按下的组合键的信息。事件属性：

        shiftKey：Shift
        altKey：Alt（或对于 Mac 是 Opt）
        ctrlKey：Ctrl
        metaKey：对于 Mac 是 Cmd
    ##### 坐标获取属性
    
- 所有的鼠标事件都提供了两种形式的坐标：

        相对于窗口(浏览器内)的坐标：clientX 和 clientY。
        相对于文档的坐标：pageX 和 pageY。
        相对于屏幕的坐标 :ScreenX ScreenY

- 如果我们想禁用选择以保护我们页面的内容不被复制粘贴，那么我们可以使用另一个事件：oncopy。return false

- 如何防止 ,鼠标左键不松情况的文本选择,最合理的方式是防止浏览器对 `mousedown` 进行操作,mousedown事件返回false
##### 移动鼠标事件  

- 对于 mouseover：

        event.target —— 是鼠标移过的那个元素。
        event.relatedTarget —— 是鼠标来自的那个元素（relatedTarget → target）。
    
- mouseout 则与之相反：

        event.target —— 是鼠标离开的元素。
        event.relatedTarget —— 是鼠标移动到的，当前指针位置下的元素（target → relatedTarget）

- > relatedTarget 属性可以为 null。这是正常现象，仅仅是意味着鼠标不是来自另一个元素，而是来自窗口之外。或者它离开了窗口。

- ##### **跳过元素**

- 当鼠标移动时，就会触发 mousemove 事件。但这并不意味着每个像素都会导致一个事件。如果访问者非常快地移动鼠标，那么某些 DOM 元素就可能被跳过：

- > 这对性能很有好处，因为可能有很多中间元素。我们并不真的想要处理每一个移入和离开的过程
    >
    > 但是如果mouseover 被触发了，则必须有 mouseout事件

    

- 鼠标转到另一个元素（甚至是一个后代），那么它将离开前一个元素,所以当鼠标指针从元素移动到其后代时触发 mouseout 事件

   > 如果离开父元素时有一些行为，当鼠标指针深入子元素时，我们并不希望发生这种行为。
   >
   > 我们可以在mouseout 处理程序中检查` relatedTarget`，如果鼠标指针仍在元素内，则忽略此类事件, 在处理程序中return结束处理 。

- 事件 mouseenter 和 mouseleave

   > 元素内部与后代之间的转换不会产生影响。当鼠标移入更深入的子元素时，不会触发离开父元素的事件
   > 事件 mouseenter/mouseleave 不会冒泡。（因为不会冒泡所以不能事件委托）

   

##### 鼠标拖放事件

- 在现代 HTML 标准中有一个 关于拖放的部分，其中包含了例如 dragstart 和 dragend 等特殊事件

- >  原生的拖放事件也有其局限性。例如，我们无法阻止从特定区域的拖动。并且，我们无法将拖动变成“水平”或“竖直”的。还有很多其他使用它们无法完成的拖放任务。并且，移动设备对此类事件的支持非常有限。

- 参考[MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/DragEvent )原生拖放事件



- 基础的拖放算法 ： mousedown  mousemove  mouseup

- > 我们在 document 上跟踪 mousemove，而不是在 ball 上 因为在快速移动鼠标后，鼠标指针可能会从球上跳转至文档中间的某个位置（甚至跳转至窗口外）。

- >因为浏览器有自己的对图片和一些其他元素的拖放处理。它会在我们进行拖放操作时自动运行，并与我们的拖放处理产生了冲突。 解决冲突 ： ball.ondragstart = function() { return false;};




- 潜在的放置目标: 在实际中，我们通常是将一个元素放到另一个元素上,当我们拖动时，可拖动元素一直是位于其他元素上的。而鼠标事件只发生在顶部元素上，而不是发生在那些下面的元素。所以无法使用鼠标事件实现
- > 使用 document.elementFromPoint 检测鼠标指针下的 “droppable” 的元素
- > document.elementFromPoint(clientX, clientY) 的方法。它会返回在给定的窗口相对坐标处的嵌套的最深的元素



##### 指针事件

- 指针事件是一种现代方式，可以处理来自各种指针设备（例如鼠标，钢笔/手写笔，触摸屏等）的输入。

- > 待扩展...

##### 键盘事件

- 当一个按键被按下时，会触发 **keydown** 事件，而当按键被释放时，会触发 **keyup** 事件。

- > 如果按下一个键足够长的时间，它就会开始“自动重复”：keydown 会被一次又一次地触发，然后当按键被释放时，我们最终会得到 keyup。因此，有很多 keydown 却只有一个 keyup 是很正常的。
  >
  > 对于由自动重复触发的事件，event 对象的 event.repeat 属性被设置为 true。 是一个只读属性

- **event.code 和 event.key** 

- > 事件对象的 key 属性允许获取按键的字符，
  >
  > 事件对象的 code 属性则允许获取“物理按键代码”。 


-    物理按键代码
  
     >   字符键的代码为 "Key<letter>"："KeyA"，"KeyB" 等。 大小写敏感："KeyZ"，不是 "keyZ"
     >   数字键的代码为："Digit<number>"："Digit0"，"Digit1" 等。
     >    特殊按键的代码为按键的名字："Enter"，"Backspace"，"Tab" 等。

- 选择按键处理方式 :

- > 例如有些键盘布局不同,但我们只绑定按到"z"键才触发,不管键盘怎样布局 ---- 使用event.key
  >
  > 就算键盘布局不同, 只要物理位置正确,无所谓是不是按到"z",都可以执行  ---- 使用event.code

  

- 阻止对 keydown 的默认行为可以取消大多数的网页行为

- > keydown的默认行为:
  >
  > 出现在屏幕上的一个字符（最明显的结果）。
  >
  > 一个字符被删除（Delete 键）。
  >
  > 滚动页面（PageDown 键）。
  >
  > 浏览器打开“保存页面”对话框（Ctrl+S）

  - 过去曾经有一个 keypress 事件，还有事件对象的 keyCode、charCode 和 which 属性。大多数浏览器对它们都存在兼容性问题，现在不再使用

  > 以致使该规范的开发者不得不弃用它们并创建新的现代的事件（本文上面所讲的这些事件），除此之外别无选择。旧的代码仍然有效，因为浏览器还在支持它们，但现在完全没必要再使用它们。

##### 滚动
- scroll 事件允许对页面或元素滚动作出反应

- 我们如何使某些东西变成不可滚动？

  > 我们不能通过在 onscroll 监听器中使用 event.preventDefault() 来阻止滚动，因为它会在滚动发生之后才触发。
  >
  > 但是我们可以在导致滚动的事件上，例如在 pageUp 和 pageDown 的 keydown 事件上，使用 event.preventDefault() 来阻止滚动。

- 通过设置 overflow 可以控制页面滚动 不滚动

#### 表单控件

##### 表单属性和方法

- 表单（form）以及例如 input 的控件（control）元素有许多特殊的属性和事件。

- > 获得文档中的表单:
  >
  > document.forms.name      name: 表单的name属性
  >
  > document.forms[0]             文档中第一个表单
- > 获得表单中的元素
  >
  > form.elements.name      name : 表单中空间的name属性 
  >
  > form.elements[0]            表单中第一个
  >
  > 可能会有多个名字相同的元素，这种情况经常在处理单选按钮中出现,在这种情况下，form.elements[name] 将会是一个集合，

  

- < fieldset> 元素。它们也具有 elements 属性，通过*form.elements.name* 获得

- 我们可以将 form.elements.login 写成 form.login 或者 form[login]
- > 这样写会有一个小问题 , 如果我们访问一个元素，然后修改它的 name，之后它仍然可以被通过旧的 name 访问到（当然也能通过新的 name 访问） 
  >
  > 但是通过 form.elements 来调用只能使用新名字



- **反向引用** : 
- > 表单引用了所有元素，元素也引用了表单
    >
    > 其对应的表单都可以通过 `element.form` 访问到    element :表单内元素

- **表单控件**: 
- > 我们可以通过 `input.value`（字符串）或 `input.checked`（布尔值）来访问复选框（checkbox）中的它们的 `value`。
    >
    > 使用 textarea.value 访问而不是textarea.innerHTML
- select 和 option: 
  
        select.options —— <option> 的子元素的集合，
        select.value —— 当前所选择的 <option> 的 value，
        select.selectedIndex —— 当前所选择的 <option> 的编号。
           
        设置选中状态,改变value值 :
        找到对应的 <option> 元素，并将 option.selected 设置为 true。
        将 select.value 设置为对应的 value。
        将 select.selectedIndex 设置为对应 <option> 的编号。

- > 如果 <select> 具有 multiple 特性（attribute），则允许多选。这个功能很少使用。在这种情况下，我们需要使用第一种方式：从 <option> 的子元素中添加/移除 selected 属性

- 有一个很好的简短语法可以创建 <option> 元素：option = new Option(text, value, defaultSelected, selected);
- > text —— <option> 中的文本，
    >
    > value —— <option> 的 value，  
    >
    > defaultSelected —— 如果为 true，那么 selected HTML-特性（attribute）就会被创建，  
    >
    > selected —— 如果为 true，那么这个 <option> 就会被选中


- <option> 元素具有以下属性：
  
        option.selected :  <option> 是否被选择。
        option.index : <option> 在其所属的 <select> 中的编号。
        option.text  : <option> 的文本内容（可以被访问者看到）。


##### 表单事件 聚焦
- 当用户点击某个元素或使用键盘上的 Tab 键选中时，该元素将会获得聚焦（focus）。当网页加载时，HTML-特性（attribute）autofocus 也可以让一个焦点落在元素上

- **focus/blur 事件  focus/blur 方法** 

- > elem.focus() 和 elem.blur() 方法可以设置和移除元素上的焦点。
  >
  > 请注意，我们无法通过在 onblur 事件处理程序中调用 event.preventDefault() 来“阻止失去焦点”，因为 onblur 事件处理程序是在元素失去焦点 之后 运行的。

  **不适用情况:**

- 默认情况下，很多元素不支持聚焦。列表（list）在不同的浏览器表现不同，但有一件事总是正确的：focus/blur 保证支持那些用户可以交互的元素：<button>，<input>，<select>，<a> 等。

- 另一方面，为了格式化某些东西而存在的元素像 <div>，<span> 和 <table> —— 默认是不能被聚焦的。elem.focus() 方法不适用于它们，并且 focus/blur 事件也绝不会被触发。

  

- 使用 HTML特性 tabindex可以允许在任何元素上聚焦

- > 任何具有 `tabindex` 特性的元素，都会变成可聚焦的。该特性的 `value` 是当使用 Tab（或类似的东西）在元素之间进行切换时，元素的顺序号。
  >
  >  值为-1 元素被忽略   = 0 默认排在最后
  >
  > .tabIndex 有对应的DOM属性

  

- **focus/blur 委托 : focus 和 blur 事件不会向上冒泡**

- > 解决方法 : 
  >
  > 方案一，有一个遗留下来的有趣的特性（feature）：focus/blur 不会向上冒泡，但会在捕获阶段向下传播。
  >
  > 方案二，可以使用 focusin 和 focusout 事件 —— 与 focus/blur 事件完全一样，只是它们会冒泡。必须使用 elem.addEventListener 来分配它们，而不是 on<event>。
>
  > 

- 可以通过 `document.activeElement `来获取当前所聚焦的元素。

##### 事件：change，input，cut，copy，paste

- > 当元素更改完成时，将触发 change 事件。
  >
  > 对于文本输入框，当其失去焦点时，就会触发 change 事件

- 每当用户对输入值进行修改后，就会触发 input 事件。
- >如果我们想要处理对 < input> 的每次更改，那么此事件是最佳选择。  
  >
  >**无法阻止** `oninput`事件 :当输入值更改后，就会触发 input 事件。我们无法使用 event.preventDefault() —— 已经太迟了，不会起任何作用了。

- 剪切/拷贝/粘贴 : cut，copy，paste
- > 它们属于 ClipboardEvent 类，并提供了对拷贝/粘贴的数据的访问方法。我们也可以使用 event.preventDefault() 来中止行为，然后什么都不会被复制/粘贴


##### 表单：事件和方法提交

- 提交表单时，会触发 submit 事件，它通常用于在将表单发送到服务器之前对表单进行校验，或者中止提交，并使用 JavaScript 来处理表单。

- > 如果要手动将表单提交到服务器，我们可以调用 form.submit()。这样就不会产生 submit 事件。我们可以动态地创建表单，使用该方法将其发送到服务器。
  
  
  
- 提交表单主要有两种方式：

- >- 第一种 —— 点击 < input type="submit"> 或 < input type="image">。
  >
  >- 第二种 —— 在 input 字段中按下 Enter 键。
  >
  >  
  >
  >- 这两个行为都会触发表单的 submit 事件。处理程序可以检查数据，如果有错误，就显示出来，并调用 event.preventDefault()，这样表单就不会被发送到服务器了。

  

  > 在输入框中使用 Enter 发送表单时，会在 < input type="submit"> 上触发一次 click 事件

  

#### 加载文档 和资源

##### 页面生命周期

- HTML 页面的生命周期包含三个重要事件：

        DOMContentLoaded —— 浏览器已完全加载 HTML，并构建了 DOM 树，但像 <img> 和样式表之类的外部资源可能尚未加载完成。
        load —— 浏览器不仅加载完成了 HTML，还加载完成了所有外部资源：图片，样式等。
        beforeunload/unload —— 当用户正在离开页面时。

- 每个事件都是有用的：

- > DOMContentLoaded 事件 —— DOM 已经就绪，因此处理程序可以查找 DOM 节点，并初始化接口。
  >  load 事件 —— 外部资源已加载完成，样式已被应用，图片大小也已知了。
  >  beforeunload 事件 —— 用户正在离开：我们可以检查用户是否保存了更改，并询问他是否真的要离开。
  >  unload 事件 —— 用户几乎已经离开了，但是我们仍然可以启动一些操作，例如发送统计数据。

- **DOMContentLoaded **

- DOMContentLoaded 事件发生在 document 对象上。我们必须使用 addEventListener 来捕获它,不能使用on<event>


- 当浏览器处理一个 HTML 文档，并在文档中遇到 <script> 标签时，就会在继续构建 DOM 之前运行它。这是一种防范措施，因为脚本可能想要修改 DOM，**所以 DOMContentLoaded 必须等待脚本执行结束。**


- `DOMContentLoaded` 不会等待样式表, 但是如果在样式后面有一个脚本，那么该脚本必须等待样式表加载完成, 

- > 原因: 脚本可能想要获取元素的坐标和其他与样式相关的属性, 脚本等待样式表,`DOMContentLoaded` 等待脚本

  

  例外 :

  > 具有 async 特性（attribute）的脚本不会阻塞 DOMContentLoaded，稍后 我们会讲到。
  >
  > 使用 document.createElement('script') 动态生成并添加到网页的脚本也不会阻塞 DOMContentLoaded。



- Firefox，Chrome 和 Opera 都会在 `DOMContentLoaded` 中自动填充表单。




- **load /unload/beforeunload事件**:

- 当整个页面，包括样式、图片和其他资源被加载完成时，会触发 window 对象上的 load 事件.

- 当访问者离开页面时，window 对象上的 unload 事件就会被触发。我们可以在那里做一些不涉及延迟的操作，例如关闭相关的弹出窗口。 发送分析数据。

- 如果访问者触发了离开页面的导航（navigation）或试图关闭窗口，beforeunload 处理程序将要求进行更多确认。 如果我们要取消事件，浏览器会询问用户是否确定。

- >window.onbeforeunload = function() {  return false; };




- **document.readyState 属性**: 提供当前加载状态的信息。

        loading —— 文档正在被加载。
        interactive —— 文档被全部读取。
        complete —— 文档被全部读取，并且所有资源（例如图片等）都已加载完成。

- > 还有一个 readystatechange 事件，会在状态发生改变时触发



##### 脚本：async，defer

- 当浏览器加载 HTML 时遇到 <script>...</script> 标签，浏览器就不能继续构建 DOM。对于外部脚本 <script src="..."></script> 也是一样的.

- > 这会导致两个重要的问题：
  >
  > 脚本不能访问到位于它们下面的 DOM 元素，因此，脚本无法给它们添加处理程序等。
  >
  > 如果页面顶部有一个笨重的脚本，它会“阻塞页面”。在该脚本下载并执行结束前，用户都不能看到页面内容：

  解决方法: 

- > 例如，我们可以把脚本放在页面底部, 但是这种解决方案远非完美。例如，浏览器只有在下载了完整的 HTML 文档之后才会注意到该脚本（并且可以开始下载它）。
  >
  > 对于长的 HTML 文档来说，这样可能会造成明显的延迟。
  >
  > 使用 defer async解决

  

- **defer 和 async**

- defer 特性告诉浏览器它应该继续处理页面，并“在后台”下载脚本，然后等页面加载完成后，再执行此脚本。

- >具有 defer 特性的脚本不会阻塞页面, 等到 DOM 解析完毕，但在 DOMContentLoaded 事件之前执行。

- 具有 defer 特性的脚本保持其相对顺序，就像常规脚本一样。 多个脚本下载时,并行下载,按顺序执行,如果我们有一个长脚本在前，一个短脚本在后，那么后者就会等待前者执行

- **defer 特性仅适用于外部脚本**, 如果 `<script>` 脚本没有 `src`，则会忽略 `defer` 特性。


- **async 特性意味着脚本是完全独立的**：
  
- > 页面不会等待异步脚本，它会继续处理并显示页面内容。
  >
     > DOMContentLoaded 和async异步脚本不会彼此等待：DOMContentLoaded可能会发生在异步脚本之前也可能在后面发生
     >
     > 其他脚本不会等待 async 脚本加载完成，同样，async 脚本也不会等待其他脚本, 如果我们有几个 async 脚本，它们可能按任意次序执行。总之是先加载完成的就先执行
  
- 当我们将独立的第三方脚本集成到页面时，此时采用异步加载方式是非常棒的：计数器，广告等，因为它们不依赖于我们的脚本，我们的脚本也不应该等待它们：

- **动态脚本** : 

- > 使用 JavaScript 动态地添加脚本：默认情况下，动态脚本的行为是“异步”的。
     >
     > 它们不会等待任何东西，也没有什么东西会等它们。  先加载完成的脚本先执行（“加载优先”顺序）。

- > 我们可以通过将 async 特性显式地修改为 false，以将脚本的加载顺序更改为文档顺序（就像常规脚本一样）

##### 资源加载：onload，onerror
- 浏览器通过load 和 error 事件,允许我们跟踪外部资源的加载  —— 脚本，iframe，图片等。基本上（basically）适用于具有外部 src 的任何资源。

- > 唯一的例外是 <iframe>：出于历史原因，不管加载成功还是失败，即使页面没有被找到，它都会触发 load 事件。

-  onload —— 成功加载，     onerror —— 出现 error。

- > 我们需要加载第三方脚本时,使用 onload事件,在成功加载时执行,失败时执行 onerror

- > onload/onerror 事件仅跟踪加载本身。也就是说：如果脚本成功加载，则即使脚本中有编程 error，也会触发 onload 事件。如果要跟踪脚本 error，可以使用 window.onerror 全局处理程序。

  

- **跨源策略** : 这里有一条规则：来自一个网站的脚本无法访问其他网站的内容。更确切地说，一个源（域/端口/协议三者）无法获取另一个源（origin）的内容。因此，即使我们有一个子域，或者仅仅是另一个端口，这都是不同的源，彼此无法相互访问。

- 如果我们使用的是来自其他域的脚本，并且该脚本中存在 error，那么我们无法获取 error 的详细信息。(如果想要获取到就必须允许跨源访问)

- > 对其他类型的资源也执行类似的跨源策略（CORS）。
  


- 要允许跨源访问，<script> 标签需要具有 crossorigin 特性（attribute），并且远程服务器必须提供特殊的 header。

    ```javascript
    无 crossorigin 特性 —— 禁止访问。
    
    crossorigin="anonymous" —— 如果服务器的响应带有包含 * 或我们的源（origin）的 header Access-Control-Allow-Origin，则允许访问。浏览器不会将授权信息和 cookie 发送到远程服务器。
    
    crossorigin="use-credentials" —— 如果服务器发送回带有我们的源的 header   
    Access-Control-Allow-Origin 和 Access-Control-Allow-Credentials: true，则允许访问浏览器会将授权信息和 cookie 发送到远程服务器。
    ```


#### 杂项

- ##### DOM 变动观察器（Mutation observer）:

- >  MutationObserver 是一个内建对象，它观察 DOM 元素，在其发生更改时触发回调。
  >
  > `MutationObserver` 可以对 DOM 的变化作出反应：特性（attribute），添加/删除的元素，文本内容。
  >
  > 我们可以用它来跟踪代码其他部分引入的更改，以及与第三方脚本集成

  语法: 

- > let observer = new MutationObserver(callback);  创建一个带有回调函数的观察器
  >
  > observer.observe(node, config);   将其附加到一个 DOM 节点

- 用于集成 : 你需要添加一个第三方脚本，该脚本不仅包含有用的功能，还会执行一些我们不想要的操作，例如显示广告 <div class="ads">Unwanted ads</div>。当然，第三方脚本没有提供删除它的机制。使用 MutationObserver，我们可以监测到我们不需要的元素何时出现在我们的 DOM 中，并将其删除

- 我们可以使用 `MutationObserver` 来自动检测何时在页面中插入了代码段，并高亮显示之它们

  

##### 选择（Selection）和范围（Range）

  

- 双击鼠标会有副作用，在某些界面中可能会出现干扰：它会选择文本,如果按下鼠标左键，并在不松开的情况下移动鼠标，这也常常会造成不必要的选择。有多种防止选择的方法.


- Range 是用于管理选择范围的通用对象。我们可能会创建此类对象，并传递它们 —— 它们在视觉上不会自行选择任何内容。

- > let range = new Range();

- range 对象具有以下属性：

        startContainer，startOffset —— 起始节点和偏移量，
        
        endContainer，endOffset —— 结束节点和偏移量，
        
        collapsed —— 布尔值，如果范围在同一点上开始和结束（所以范围内没有内容）则为 true，
        
        commonAncestorContainer —— 在范围内的所有节点中最近的共同祖先节点，
    


- 设置范围的起点：

        setStart(node, offset) 将起点设置在：node 中的位置 offset
        setStartBefore(node) 将起点设置在：node 前面
        setStartAfter(node) 将起点设置在：node 后面
        设置范围的终点（类似的方法）：
        
        setEnd(node, offset) 将终点设置为：node 中的位置 offset
        setEndBefore(node) 将终点设置为：node 前面
        setEndAfter(node) 将终点设置为：node 后面
        如前所述，node 既可以是文本节点，也可以是元素节点：对于文本节点，offset 偏移的是字符数，而对于元素节点则是子节点数。
        
        其他：
        
        selectNode(node) 设置范围以选择整个 node
        selectNodeContents(node) 设置范围以选择整个 node 的内容
                collapse(toStart) 如果 toStart=true 则设置 end=start，否则设置 start=end，从而折叠范围
        cloneRange() 创建一个具有相同起点/终点的新范围
        如要操纵范围内的内容：
        
        deleteContents() —— 从文档中删除范围内容
        extractContents() —— 从文档中删除范围内容，并将删除的内容作为 DocumentFragment 返回
        cloneContents() —— 复制范围内容，并将复制的内容作为 DocumentFragment 返回
        insertNode(node) —— 在范围的起始处将 node 插入文档
        surroundContents(node) —— 使用 node 将所选范围内容包裹起来。要使此操作有效，则该范围必须包含其中所有元素的开始和结束标签：不能像 <i>abc 这样的部分范围。




- 文档选择是由 Selection 对象表示的，可通过 window.getSelection() 或 document.getSelection() 来获取。
- 选择属性 :

        anchorNode —— 选择的起始节点，
        anchorOffset —— 选择开始的 anchorNode 中的偏移量，
        focusNode —— 选择的结束节点，
        focusOffset —— 选择开始处 focusNode 的偏移量，
        isCollapsed —— 如果未选择任何内容（空范围）或不存在，则为 true 。
        rangeCount —— 选择中的范围数，除 Firefox 外，其他浏览器最多为 1。

- 在文档中，选择的终点可能在起点之前

- 选择获取 : 
  
        作为文本：只需调用 document.getSelection().toString()。
        作为 DOM 节点：获取底层的（underlying）范围，并调用它们的 cloneContents() 方法（如果我们不支持  Firefox 多选的话，则仅取第一个范围）。

- 选择事件 : 有一些事件可以跟踪选择：
  
        elem.onselectstart —— 当选择从 elem 上开始时，例如，用户按下鼠标键并开始移动鼠标。
        阻止默认行为会使选择无法开始。
        
        document.onselectionchange —— 当选择变动时。
        请注意：此处理程序只能在 document 上设置。

- 选择方法 :

        getRangeAt(i) —— 获取从 0 开始的第 i 个范围。在除 Firefox 之外的所有浏览器中，仅使用 0。
        addRange(range) —— 将 range 添加到选择中。如果选择已有关联的范围，则除 Firefox 外的所有浏览器都将忽略该调用。
        removeRange(range) —— 从选择中删除 range。
        removeAllRanges() —— 删除所有范围。
        empty() —— removeAllRanges 的别名。
        
        外，还有一些方便的方法可以直接操作选择范围，而无需使用 Range：
        
        collapse(node, offset) —— 用一个新的范围替换选定的范围，该新范围从给定的 node 处开始，到偏移    offset 处结束。
        setPosition(node, offset) —— collapse 的别名。
        collapseToStart() —— 折叠（替换为空范围）到选择起点，
        collapseToEnd() —— 折叠到选择终点，
        extend(node, offset) —— 将选择的焦点（focus）移到给定的 node，位置偏移 oofset，
        setBaseAndExtent(anchorNode, anchorOffset, focusNode, focusOffset) —— 用给定的起点      anchorNode/anchorOffset 和终点 focusNode/focusOffset 来替换选择范围。选中它们之间的所有内容。
        selectAllChildren(node) —— 选择 node 的所有子节点。
        deleteFromDocument() —— 从文档中删除所选择的内容。
        containsNode(node, allowPartialContainment = false) —— 检查选择中是否包含 node（特别是如果第二个参数是 true 的话）


- https://developer.mozilla.org/zh-CN/docs/Web/API/Selection

- 对于许多任务，我们可以调用 Selection 方法，而无需访问底层的（underlying）Range 对象。

- 如要选择，请先移除现有的选择
如果选择已存在，则首先使用 removeAllRanges() 将其清空。然后添加范围。否则，除 Firefox 外的所有浏览器都将忽略新范围。

- 表单控件中的选择 :

- 诸如 input 和 textarea 等表单元素提供了 专用的选择 API，没有 Selection 或 Range 对象。由于输入值是纯文本而不是 HTML，因此不需要此类对象，一切都变得更加简单。

        input.selectionStart —— 选择的起始位置（可写），
        input.selectionEnd —— 选择的结束位置（可写），
        input.selectionDirection —— 选择方向，其中之一：“forward”，“backward” 或 “none”（例如使用鼠标双击进行的选择），
        事件：
        
        input.onselect —— 当某个东西被选择时触发。
        方法：
        
        input.select() —— 选择文本控件中的所有内容（可以是 textarea 而不是 input），
        
        input.setSelectionRange(start, end, [direction]) —— 在给定方向上（可选），从 start 一直选择到 end。
        
        input.setRangeText(replacement, [start], [end], [selectionMode]) —— 用新文本替换范围中的文本。
        
        可选参数 start 和 end，如果提供的话，则设置范围的起点和终点，否则使用用户的选择。
        
        最后一个参数 selectionMode 决定替换文本后如何设置选择。可能的值为：
        
        "select" —— 将选择新插入的文本。
        "start" —— 选择范围将在插入的文本之前折叠（光标将在其之前）。
        "end" —— 选择范围将在插入的文本之后折叠（光标将紧随其后）。
        "preserve" —— 尝试保留选择。这是默认值。

- > onselect 是在某项被选择时触发，而在选择被删除时不触发。根据 规范，发表单控件内的选择不应该触发 document.onselectionchange 事件，因为它与 document 选择和范围不相关。一些浏览器会生成它，但我们不应该依赖它。

    

- 要使某些内容不可选，有三种方式：

- > 使用 CSS 属性 user-select: none。
    >
    > 防止 onselectstart 或 mousedown 事件中的默认行为。 这样可以防止在 elem 上开始选择，但是访问者可以在另一个元素上开始选择，然后扩展到 elem。
    >
    > 我们还可以使用 document.getSelection().empty() 来在选择发生后清除选择。很少使用这种方法，因为这会在选择项消失时导致不必要的闪烁。


- 关于光标。在诸如 <textarea> 之类的可编辑元素中，光标的位置始终位于选择的起点或终点。我们可以通过设置 elem.selectionStart 和 elem.selectionEnd 来获取光标位置或移动光标。

##### 事件循环  微任务 宏任务

- 浏览器中 JavaScript 的执行流程和 Node.js 中的流程都是基于 事件循环 的。

- 事件循环的概念:  

- > 它是一个在 JavaScript 引擎等待任务，**执行任务** 和 **进入休眠状态**  **等待更多任务**  这几个状态之间转换的无限循环。

- 多个任务组成了一个队列，即所谓的“宏任务队列”

- > 队列中的任务基于“先进先出”的原则执行
  >
  > 引擎执行任务时永远不会进行渲染（render）,仅在任务完成后才会绘制对 DOM 的更改。
  >
  > 
  >
  > 如果一项任务执行花费的时间过长，浏览器将无法执行其他任务，无法处理用户事件，因此，在一定时间后浏览器会在整个页面抛出一个如“页面未响应”之类的警报，建议你终止这个任务。这种情况常发生在有大量复杂的计算或导致死循环的程序错误时。

  **微任务:**

- 微任务仅来自于我们的代码。它们通常是由 promise 创建的：对 .then/catch/finally 处理程序的执行会成为微任务。微任务也被用于 await 的“幕后”，因为它是 promise 处理的另一种形式。

- 还有一个特殊的函数 queueMicrotask(func)，它对 func 进行排队，以在微任务队列中执行。

- > 每个宏任务之后，引擎会立即执行微任务队列中的所有任务，然后再执行其他的宏任务，或渲染，或进行其他任何操作。

  


- 安排（schedule）一个新的 宏任务：用零延迟的 setTimeout(f)。

- 安排一个新的 微任务：使用 queueMicrotask(f)。 promise 处理程序也会通过微任务队列。

- > 在微任务之间没有 UI 或网络事件的处理：微任务之间它们一个立即接一个地执行。


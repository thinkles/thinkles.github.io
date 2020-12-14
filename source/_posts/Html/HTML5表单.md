---
title: html表单
date: 
tags: html表单
categories: HTML
---


### html5  表单

##### 原生表单部件
- 表单标签 

    < form name method action >   
    < input type ="" 多个type>   
    < label> 
    < select：选择列表>  
    < input type="image"：创建图形按钮 > 
    < button type  > 
    < textarea> 

- 表单组件通用属性： 
    name		元素的名称;这是跟表单数据一起提交的。
    value		元素的初始值。
    autofocus	(false)	这个布尔属性允许您指定当页面加载时元素应该自动具有输入焦点,文档中只有一个与表单相关的元素可以指定这个属性。
    disabled	这个布尔属性表示用户不能与元素交互

- 所有输入用的文本框都有一些通用规范:
    readonly:用户不能修改输入值
    disabled：输入值永远不会与表单数据的其余部分一起发送  
    placeholder:提供输入提示信息
    formaction:制定后台处理软件 
    autofocus ：自动获得焦点 
    pattern：验证输入元素，验证输入格式。。 
    requid:非空检测    multiple属性添加到< select>元素，可以允许选择几个值
    它们可以被限制在size (框的物理尺寸) 和 长度 (可以输入的最大字符数)。

##### 各个表单部件之间注意点

###### input  button textarea 注意点
- 要定义< input>的默认值，你必须使用value 属性,如果您想定义< textarea>的默认值，您只需在< textarea>元素的开始和结束标记之间放置默认值
 - 注意，< textarea>元素与< input>元素的编写略有不同。< input>元素是一个空元素，这意味着它不能包含任何子元素。< textarea>元素可以包含文本内容的子元素。< textarea>元素只接受文本内容；这意味着将任何HTML内容放入< textarea>中都呈现为纯文本内容。



- < button>元素也接受一个 type属性，它接受submit, reset或者 button 三个值中的任一个
- 使用< button>元素或< input>元素定义的按钮几乎没有区别。唯一值得注意的区别是按钮本身的标签。在< input>元素中，标签只能是字符数据，而在< button>元素中，标签可以是HTML(br strong)，因此可以相应地进行样式化。
  
-对于checkbox  通过name属性识别此复选框,value属性是被后台获取的值,不指定value结果为默认"on" ,复选框单选框都需要拥有value属性

> HTML表单文本字段是简单的纯文本输入控件(input)。 这意味着您不能使用它们执行富文本编辑(粗体、斜体等)。你遇到的所有富文本编辑器（rich text editors）都是使用HTML、CSS和JavaScript所创建的自定义小部件
> 我们还需要为我们的数据提供一个名称, 要将数据命名为表单，您需要在每个表单小部件上使用 name 属性来收集特定的数据块。
> 严格禁止在一个表单内嵌套另一个表单 

- 表单结构的常用做法
- >用< div>元素包装标签和它的小部件是很常见的做法。< p>元素也经常被使用，HTML列表 ul也是如此（后者在构造多个复选框或单选按钮时最为常见） 除了< fieldset>元素之外，使用HTML标题（例如，< h1>、 < h2>）和分段（如< section>）来构造一个复杂的表单也是一种常见的做法。

- 通过包括multiple属性，它还可以让用户将多个电子邮件地址输入相同的输入(以逗号分隔)。
- > < input type="email" id="email" name="email" multiple>

###### 选择表单 注意点
- 如果一个< option>元素设置了value属性，那么当提交表单时该属性的值就会被发送。如果忽略了value属性，则使用< option>元素的内容作为选择框的值。
- select框中 在< optgroup >元素中，label属性(和label元素不是一个)显示在值之前，但即使它看起来有点像一个选项，它也不是可选的。
作系统提供的默认机制来选择几个值。 (如， 同时按下 Cmd/Ctrl 并点击多个值).


- 您可以使用< datalist>元素来为表单小部件提供建议的、自动完成的值(自动补全框)，并使用一些< option>子元素来指定要显示的值。 然后使用list属性将数据列表绑定到一个文本框(通常是一个 < input> 元素)。 
```
<input type="text" name="myFruit" id="myFruit" list="mySuggestion">
<datalist id="mySuggestion">
```


- 几个单选按钮可以连接在一起。如果它们的name属性共享相同的值，那么它们将被认为属于同一组的按钮。同一组中只有一个按钮可以同时被选；这意味着当其中一个被选中时，所有其他的都将自动未选中。如果没有选中任何一个，那么整个单选按钮池就被认为处于未知状态，并且没有以表单的形式发送任何值。
> 单选项  复选项 复选框和单选按钮。两者都使用checked属性，以指示该部件的默认状态: "选中"或"未选中"。,值得注意的是，这些小部件与其他表单小部件不一样。对于大多数表单部件，一旦表单提交，所有具有name属性的小部件都会被发送，即使没有任何值被填。对于可选中项，只有在勾选时才发送它们的值。如果他们没有被勾选，就不会发送任何东西，甚至连他们的名字也没有。

  

>默认情况下，选择框只允许用户选择一个值。通过将multiple属性添加到< select>元素，您可以允许用户通过操


##### 高级表单部件

1. 数字 type=number
>通过设置min和max属性来约束该值。
>通过设置step属性来指定增加和减少按钮更改小部件的步进值大小。

2. 滑块 type=range 正确配置滑块是很重要的；为了达到这个目的，我们强烈建议您设置min、max和step属性。

3. 拾色器  < input type="color" name="color" id="color">

4. 文件选择器小部件，您可以使用< input>元素，将它的type属性设置为file。被接受的文件类型可以使用accept属性来约束。此外，如果您想让用户选择多个文件，那么可以通过添加multiple属性来实现。

>有时候，由于为了方便技术原因，有些数据是用表单发送的，但不显示给用户。要做到这一点，您可以在表单中添加一个不可见的元素。要做到这一点，需要使用< input>将它的type属性设置为hidden值。如果您创建了这样一个元素，就需要设置它的name和value属性：

- < input type="file" id="input"> 上传组件, 你不能使用 input.value 来获取该文件路径,由于浏览器安全问题,路径会被修改, 使用window.URL.createObjectURL() 来获取路径, 如果是使用文件,通过files属性返回选择的一个/多个文件, 返回值是包含file对象的类数组.


1. 图像按钮是使用type属性值设置为image< input>的元素创建的。这个元素支持与< img>元素相同的属性，和其他表单按钮支持的所有属性。如果使用图像按钮来提交表单，这个小部件不会提交它的值；相反，提交的是在图像上单击处的X和Y坐标(坐标是相对于图像的，这意味着图像的左上角表示坐标0，0)，坐标被发送为两个键/值对：

6.进度条 使用< progress>元素创建的。
- >  < progress max="100" value="75">75/100</ progress>
7.仪表
- > < meter min="0" max="100" value="75" low="33" high="66" optimum="50">75</ meter>


>警告——日期和时间窗口小部件仍然很不受支持。目前，Chrome、Edge和Opera都支持它们，但IE浏览器没有支持，Firefox和Safari对这些都没有太大的支持。

#### 表单数据效验
- 分为客户端效验 服务器效验两种
##### 效验方式
1. html5内置的效验属性

     1. required 
     2. pattern 属性 (属性内书写正则表达式)
     3. 所有文本框 (< input> 或 < textarea>) 都可以使用minlength 和 maxlength 属性来限制长度 在数字条目中 (< input type="number">), 该 min 和 max 属性同样提供校验约束
   
- >该元素将可以通过 CSS 伪类 :valid 进行特殊的样式化；如果一个元素未校验通过：该元素将可以通过 CSS 伪类 :invalid 进行特殊的样式化；


2. 使用Js 效验表单
- 越来越多的浏览器支持限制校验API，并且这逐渐变得可靠。这些 API 由成组的方法和属性构成，可在特定的表单元素接口上调用：
- > HTMLButtonElement  HTMLFieldSetElement  HTMLInputElement...
- 约束校验的 API 及属性  validity

> 也可以不使用js的API 属性,通过原生js实现建立自己的校验系统并不难。 困难的部分是使其足够通用，以跨平台和任何形式使用它可以创建。 有许多库可用于执行表单校验; 你应该毫不犹豫地使用它们。 这里有一些例子：独立的库（原生 Javascript 实现）：Validate.js    jQuery 插件:Validation  Valid8
  
##### 自定义错误信息  远程效验
- 每次我们提交无效的表单数据时, 浏览器总会显示错误信息. 但是显示的信息取决于你所使用的浏览器.
- HTML5 提供 constraint validation API (指的就是js 调用的验证API 属性 方法)来检测和自定义表单元素的状态. 除此之外,他可以改变错误信息的文本.
>关于自定义问题观看 表单数据效验--MDN


- 远程校验 : 一个应用实例就是注册表单，在这里你需要一个用户名。 为了避免重复，执行一个 AJAX 请求来检查用户名的可用性，要比让先用户发送数据，然后因为表单重复了返回错误信息要好得多。
> 两个条件:需要确保它不是敏感数据。网络滞后需要执行异步校验。确保如校验没有适当的执行，用户还能继续操作


#### Css样式化表单
- 有些元素根本不能用应用CSS样式。 这些包括：所有高级用户界面小部件，如范围，颜色或日期控件; 和所有下拉小部件，包括< select>, < option>, < optgroup>和< datalist> 元素。 文件选择器小部件也被称为不可样式化。 新的< progress>和< meter> 元素也属于这个类别

- 搜索框是唯一一种应用CSS样式有点棘手的文本字段。 在基于WebKit的浏览器（Chrome，Safari等）上，您必须使用-webkit-appearance专有属性来调整它

- 如果你想保持组件的原生观感，又想给它们一致的大小，你会面临一些困难)。这是因为每个小部件都有自己的边框，填充和边距的规则。 所以如果你想给几个不同的小部件相同的大小，你必须使用box-sizing 属性：

- 默认情况下，所有浏览器都认为< textarea> 元素是inline block，与文本底线对齐, 这很少是我们真正想看到的。 要将内联(inline-block)块更改为块(block)，使用display属性非常简单。 但是如果你想以inline方式使用它，通常改变垂直对齐方式：vertical-top

- < textarea> 元素默认地被渲染成一个块级元素。这里有重要地两点是 resize 和 overflow 属性(css属性而不是html属性)。因为我们的设计是一个固定大小的设计，所以我们会使用 resize 属性来防止用户调整我们的多行文本域的大小。overflow 属性是用来让域在不同的浏览器上渲染得更一致。一些浏览器默认值为 auto，而一些将默认值设为 scroll。在我们得例子中，最好确定每个浏览器都使用 auto： 

- < button> 元素上使用 CSS 非常方便；你可以做你任何想做得事情，甚至包括使用 伪元素：

- 注意添加outline 属性非常重要，这样可以移除由某些浏览器添加的默认高亮效果：

CSS Basic UI Level 3 也增加了几个伪类用于描述小部件的状态：
      
      :default
      :valid
      :invalid      
      :in-range      
      :out-of-range     
      :required      
      :optional      
      :read-only      
      :read-write

> 要实现对表单小部件的完全控制，你别无选择，只能选择依靠JavaScript。使用JS库
---------------

##### 表单 样式各个浏览器的兼容问题
##### 自定义表单部件 通过js发送表单数据,而不是采用默认
##### 表单提交时安全问题  格式问题
- 跨站脚本(XSS)和跨站点请求伪造(CSRF)是常见的攻击类型


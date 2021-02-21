---

title: HTML表单 
date: {{ date }}
updated: {{date}}
tags: HTML表单
categories: HTML

---
## HTML原生表单

#### 原生表单部件

- 表单标签 :

- > < form  name method action >  
  >
  > < input  type  id name>  
  >
  > < label for>      <label>标签与 <input> 通过他们各自的`for` 属性和 `id` 属性正确相关联,  这样可访问性会好
  >
  > < textarea cols rows wrap>  wrap 预设值 ，表示提交的文字不会换行，但浏览器呈现的文字会换行
  >
  > < select  multiple> < optgroup>< option>
  >
  > < datalist id> 建议列表 ,使用list属性将数据列表绑定到一个文本框
  >
  > < button type  > 
  
  

- 严格禁止在一个表单内嵌套另一个表单。嵌套会使表单的行为不可预知

- form name 属性不推荐使用, 现在使用id

- <fieldset> 和 <legend> 元素  可以用来给表单分出结构(语义化标签) , legend 元素会有边框的 css样式

  
#### 表单组件通用属性

- **通用属性:** 

  > `name`		元素的名称;这是跟表单数据一起提交的。
  > `value	`	元素的初始值。
  > `autofocus`	(false)	这个布尔属性允许您指定当页面加载时元素应该自动具有输入焦点,文档中只有一个与表单相关的元素可以指定这个属性。
  > `disabled`	这个布尔属性表示用户不能与元素交互

- **所有输入用的文本框(input textarea)都有一些通用规范:**

  > readonly: 用户不能修改输入值
  > disabled：输入值永远不会与表单数据的其余部分一起发送  
  >
  > size (框的物理尺寸) 和 maxlength (可以输入的最大字符数)。
  >
  > placeholder:提供输入提示信息

  


###### 输入框.按钮注意点
- **< input>的默认值**，必须使用value 属性,**< textarea>的默认值**，在 textarea 元素的开始和结束标记之间放置默认值

  

 - < textarea>元素可以包含文本内容的子元素。将任何HTML内容放入< textarea>中都呈现为纯文本内容。



-  对于<button> 元素, 省略 type 属性 (或是一个无效的 type 值) 的结果就是一个提交(type=submit)按钮.

- > type 属性 : submit  reset   button
  >
  > 
  >
  > 使用<button>元素或<input>元素定义的按钮几乎没有区别。
  >
  > 唯一的区别是在<button>元素中，标签可以是HTML，因此可以相应地进行样式化。在<input>元素中，标签只能是字符数据，

  

- HTML表单文本字段是简单的纯文本输入控件(input)。 这意味着您不能使用它们执行富文本编辑(粗体、斜体等)。

  

 

###### 选择小部件注意点
- 如果一个< option>元素设置了value属性，那么当提交表单时该属性的值就会被发送。如果忽略了value属性，则使用< option>元素的内容作为选择框的值。



- 可选中项是可以通过单击它们来更改状态的小部件。有两种可选中项：复选框和单选按钮

  > 对于可选中项(radio  checkbox)，只有在勾选时才发送它们的值。如果他们没有被勾选，就不会发送任何东西，甚至连他们的名字也没有
  >
  > 对于大多数表单部件，一旦表单提交，所有具有`name`属性的小部件都会被发送，即使没有任何值被填。

- 几个单选按钮(radio)可以连接在一起。如果它们的name属性共享相同的值，那么它们将被认为属于同一组的按钮。同一组中只有一个按钮可以被选；

  

- 对于可选项,value属性是被后台获取的值,

  > 不指定value默认"on" ,反之是"off", 复选框,单选框都需要拥有value属性


#### 高级表单部件

1. 数字 type='number'

   > 通过设置min和max属性来约束该值。
   > 通过设置step属性来指定增加和减少按钮更改小部件的步进值大小。

2. 滑块 type='range'  设置min、max和step属性。

3. 拾色器  < input type="color" name="color" id="color">

4. 日期时间选择器 : type ='datetime-local'  'month '  ' time'  'week'

5. 文件选择器 :  input  type=file  ,  accept属性来约束接受的文件类型 , multiple属性让用户选择多个文件

   > 有些数据是用表单发送的，但不显示给用户。使用< input>将它的type属性设置为hidden值。然后设置它的name和value属性

   -  **上传文件:**

   - 获得上传文件的地址:

     > < input type="file" id="input"> 上传组件, 你不能使用 input.value 来获取该文件路径,由于浏览器安全问题,路径会被修改, 会出现 fakepath.
     >
     > 后端node   可以通过.upload.path 获得 绝对路径
     >
     > 前端的话,只能获得表示该文件的字符串 

   - 获得上传文件:

     > 使用`HTMLInputElement.files`返回一个包含一列 File 对象的 FileList 对象(类数组)。 length 属性来获得已选择文件的数量。
     >
     > File 对象常用属性 : name size type lastModified....       通过[File API](https://developer.mozilla.org/zh-CN/docs/Web/API/File/Using_files_from_web_applications)  操作

   

   - 上传文件是图片时获得缩略图:

   - >   `window.URL.createObjectURL() `和`window.URL.revokeObjectURL() `方法的支持。这使得你可以创建用于引用任何数据的简单URL字符串，也可以引用一个包括用户电脑上的本地文件的DOM File对象。 
     >
     >  利用此可以获得选中文件的字符串,可以直接付给src
     >
     > FileReader() 可以获得base64图片 赋值给src

6. 图像按钮:  type=image 

   > 这个元素支持与< img>元素相同的属性，和其他表单按钮支持的所有属性。
   >
   > 如果使用图像按钮来提交表单，它提交的是在图像上单击处的X和Y坐标(坐标是相对于图像的，图像的左上角表示原点，坐标被发送为两个键/值：

7. 进度条 使用< progress>元素创建的。

   > < progress max="100" value="75">75/100</ progress>

8. 仪表

   > < meter min="0" max="100" value="75" low="33" high="66" optimum="50">



#### 发送表单数据

- **表单发送数据:**

- > form  action属性: 定义了发送数据要去的位置。它的值必须是一个有效的URL,如果没有提供此属性，则数据将被发送到包含这个表单的页面的URL。
  >
  > method 属性 : 该属性定义了如何发送数据 Get  post

- 发送敏感信息/ 发送大量数据 post方法是首选,因为一些浏览器限制了URL的大小。



- **表单发送文件:**

- > 表单发送文件,需要更改Content-Type, 通过在form的enctype="multipart/form-data"
  >
  > 文件是二进制数据,被分成多个部分传送,所以需要单独设置类型
  >
  > 
  >
  > 默认情况下，它的值是application/x-www-form-urlencoded。它的意思是：“这是已编码为URL参数的表单数据。”

------



- **表单发送数据 和 ajax 的区别**
  
- > 标准的 HTML 表单提交会加载数据要发送到的URL，这意味着浏览器窗口以整页加载进行导航。 可以通过隐藏闪烁和网络滞后来避免整页加载以提供更平滑的体验
  >
  > 
  >
  > AJAX 技术主要依靠 XMLHttpRequest 它可以构造 HTTP 请求、发送它们，并获取请求结果。
  
  
  
  - **AJAX 发送表单数据的主要方法:**
  
  - > get/post 简单的使用编码为URL参数的表单数据。  类型 :application/x-www-form-urlencoded
    >
    > 使用FormData 对象构建数据,post来发送数据  类型: multipart/form-data
  
  
  
  - 请注意，[FormData](https://developer.mozilla.org/zh-CN/docs/Web/API/FormData/Using_FormData_Objects) 对象是“只写”的，这意味着您可以更改它们，但不能检索其内容,你也可以把一个 `FormData` 对象绑定到一个表单上, new FormData(form);
  
  - > 如果表单含有type=file组件, FormData对象会自动处理
    >
    > 后端拿到formdata数据,也需要单独处理
  
  ------
  
  - **Get 和 Post 请求的区别:** ...
  
  - > 使用post方法, 数据追加到HTTP请求的主体中
    >
    > ....
    >
    > HTTP请求由两个部分组成：一个包含关于浏览器功能的全局元数据集的头部，和一个包含服务器处理特定请求所需信息的主体。(简单说头 和 主体)
  
  

#### 表单数据效验

- **客户端效验:** 发生在浏览器端，表单数据被提交到服务器之前

  > JavaScript  校验，这是可以完全自定义的实现方式, 会触发invalid事件,可以自定义样式
  >
  > HTML5 **内置校验**，这不需要 JavaScript ，而且性能更好，但是不能像JavaScript那样可自定义。

  - 内置效验:

  - >  `required`(必填)  `pattern`正则表达式, 
    >
    > 所有文本框 (<input> 或 <textarea>) 都可以使用minlength 和 maxlength 属性,数字类型 type =number `min` 和 `max` 属性同样提供校验约束

  - **JS自定义验证:** `novalidate` 属性关闭浏览器的自动校验；这允许我们使用脚本控制表单校验

  - > 使用 JavaScript css伪类 自定义错误提示信息的外观  文本




- 要自定义错误提示消息的外观和文本, 你必须使用 JavaScript自定义; 不能使用 HTML 和 CSS 来改变.     [validation API](https://developer.mozilla.org/zh-CN/docs/Learn/HTML/Forms/Data_form_validation) 只可以用来改变文本或者监测验证情况

  > 通过效验时, 通过 CSS 伪类 [`:valid`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:valid) 进行特殊的样式化,
  >
  > 没有通过效验时, 通过 CSS 伪类 [`:invalid`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:invalid) 进行特殊的样式化；

  ------

  

- **服务器端校验**:  发生在数据被服务器端程序接收之后, 将数据写入数据库之前

  > 服务器端校验不像客户端校验那样有好的用户体验，因为它直到整个表单都提交后才能返回错误信息

  

- 远程效验:  一个应用实例就是注册表单，在这里你需要一个用户名。 为了避免重复，执行一个 AJAX 请求来检查用户名的可用性，




#### Css样式化表单
- 有些元素根本不能用应用CSS样式。 这些包括：所有高级表单小部件，如范围，颜色或日期控件....  , 所有下拉小部件，包括< select>, < option>, < optgroup>和< datalist> 元素。



- 因为每个小部件都有自己的边框，padding和边距的规则。如果你想给不同的小部件相同的大小，你必须使用box-sizing 属性



- 默认情况下，所有浏览器都认为< textarea> 元素是inline block，与文本底线对齐, 如果你想以inline方式使用它，通常改变垂直对齐方式：vertical-align

  

- 注意添加outline 属性非常重要，这样可以移除由某些浏览器添加的默认高亮效果

  

- CSS 3 也增加了几个伪类用于描述小部件的状态：

- >    :default :valid :invalid  :in-range :out-of-range 
  >
  >   :required    :optional    :read-only   :read-write

- 要实现对表单小部件的完全控制，你别无选择，只能选择依靠JavaScript。使用JS库



#### 表单提交时安全问题  
- 跨站脚本(XSS)和跨站点请求伪造(CSRF)是常见的攻击类型


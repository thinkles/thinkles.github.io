---
title: Vue
date: 
tags: Vue
categories: Vue
index_img: /images/bag.jpg
sticky: 1
updated:
---

## VUE一遍
 #### vue的核心思想是什么
 - 构建用户界面的渐进式框架  设计为可以**自底向上逐层应用**(----串联线)  只关注视图层 提供MVVM数据双向绑定的库 其核心思想包括数据驱动，组件化思想
 - > 渐进式  :  vue每个功能是独立的,你可以只用其中一种  
 - > Vue的体系从内到外依次是声明式渲染(Declarative Rendering)、组件系统(Component System)、客户端路由(Client-side Routing)、大规模状态管理(Large Scale State Management)、构建系统(Build System)。 
 - > MVVM  数据(Model)和视图(View)是不能直接通讯的，而是需要通过ViewModel来实现双方的通讯 (Viewmodel:就是连接视图与数据的中间件)
- > 组件化思想:  把复杂逻辑分为多个组件处理, 所以本质上,我们是要写各个组件, 各个组件之间的通讯,渲染..




#### 生命周期钩子
- 每个 Vue 实例在被创建时都要经过一系列的初始化过程——例如，需要设置数据监听、编译模板、将实例挂载到 DOM 并在数据变化时更新 DOM 等。同时在这个过程中也会运行一些叫做生命周期钩子的函数，这给了用户在不同阶段添加自己的代码的机会。
- 

### 第一层:声明式渲染

- ####  声明式地将数据渲染进 DOM 的系统:

#### 模板语法

- Vue.js 使用了基于 HTML 的模板语法，允许开发者声明式地将 DOM 绑定至底层 Vue 实例的数据。所有 Vue.js 的模板都是合法的 HTML，所以能被遵循规范的浏览器和 HTML 解析器解析。

- 在底层的实现上，Vue 将模板编译成虚拟 DOM 渲染函数。结合响应系统，Vue 能够智能地计算出最少需要重新渲染多少组件，并把 DOM 操作次数减到最少。 可以不用模板，[直接写渲染 render函数](https://cn.vuejs.org/v2/guide/render-function.html)，使用可选的 JSX 语法也是可以的

**渲染数据**

- **文本**   
- > 通过双括号插值
- >v-once 进行一次性插值, 后续改变不在变化

- **原始 HTML**   双大括号会将数据解释为普通文本，而非 HTML 代码。为了输出真正的 HTML，你需要使用 v-html

- > 会直接渲染被标签处理过的文本内容,而不是连同标签一起输出

- > 你的站点上动态渲染的任意 HTML 可能会非常危险，因为它很容易导致 XSS 攻击。请只对可信内容使用 HTML 插值，绝不要对用户提供的内容使用插值。

-  **attribute**  作用在 HTML attribute 上，遇到这种情况应该使用 v-bind

- > 对于布尔 attribute 如果是null`、`undefined` 或 `false,那个该特性就不会渲染出来

- vue 提供了 JavaScript 表达式支持,可以在大括号中写表达式, 有个限制是只能包含**单个表达式**

- > 模板表达式都被放在沙盒中，只能访问[全局变量的一个白名单](https://github.com/vuejs/vue/blob/v2.6.10/src/core/instance/proxy.js#L9)，如 `Math` 和 `Date` 。
  >
  > 你不应该在模板表达式中试图访问用户定义的全局变量

**指令**

- 指令 (Directives) 是带有 `v-` 前缀的特殊 attribute, 指令的职责是，当表达式的值改变时，将响应式地作用于 DOM

- `v-if` 指令将根据表达式 的值的真假来插入/移除 DOM元素

- `v-bind` 指令可以用于响应式地更新 HTML attribute, 

- `v-on` 指令，它用于监听 DOM 事件,绑定事件参数

- > 一些指令能够接收一个“参数”，在指令名称之后以冒号表示。例如:
  >
  > <a v-bind:href="url"> 
  
- 当使用v-bind 绑定 attribute 的时,将不再是静态字符串,而是一个JavaScript表达式

- ```
  < p v-bind:value="a"> </p> 
  //这时 a 是一个 JavaScript 表达式而不是一个字符串.
  如果是字母 将会被解析成表达式,必须在vue定义
  数字      表达式进行计算,返回一个数字作为绑定的值
  ```

- 动态参数: 从 2.6.0 开始，可以用方括号括起来的 JavaScript 表达式作为一个指令的参数, 表达式将进行动态求值
  
- 动态参数事项:
  
- > 动态参数预期会求出一个字符串，异常情况下值为 `null`。这个特殊的 `null` 值可以被显性地用于移除绑定。任何其它非字符串类型的值都将会触发一个警告。
  >
  > 
  >
  > 动态参数表达式有一些语法约束:
  >
  > 因为某些字符，如空格和引号,放在 HTML attribute 名里是无效  
  >
  > 大写字符来命名键名，因为浏览器会把 attribute 名全部强制转为小写
  

 **修饰符** : 用于指出一个指令应该以特殊方式绑定

- ```html
  <form v-on:submit.prevent="onSubmit">...</form> 
  //.prevent 修饰符告诉 v-on 指令对于触发的事件调用 event.preventDefault()：
  ```

 **缩写:**

- > v-bind 缩写   => ":"  
  >
  >   v-on 缩写 => "@"

##### v-bind 绑定class style

  - 在将 `v-bind` 用于 `class` 和 `style` 时，Vue.js 做了专门的增强。表达式结果的类型除了字符串之外，还可以是对象或数组。

**绑定class基本语法:**

​		**对象语法:** 可以动态地切换 class

  - ```html
    1.把class名字内联写在模板里,通过更改isActive的bool值决定,是否渲染active类
    <div v-bind:class="{ active: isActive }"></div>  
    
    2.把class名字外联写在data中的classObject对象里,通过在data中改变 bool值渲染类名
    <div v-bind:class="classObject"></div>
    data: {
      classObject: {
        active: true,
        'text-danger': false
      }
    }
    ```
    
    **数组语法:** 应用一个 class 列表
    
- ```html
  <div v-bind:class="[activeClass, errorClass]"></div>
  data: {
    activeClass: 'active',
    errorClass: 'text-danger'
  }
  ```

- **在数组语法中也可以使用对象语法**:

- ```html
  <div v-bind:class="[{ active: isActive }, errorClass]"></div>
  ```


**绑定在自定义组件上**:

- > 当在一个自定义组件上使用 `class` property 时，这些 class 将被添加到该组件的**根元素**上面。
  >
  > 这个元素上已经存在的 class 不会被覆盖。

  ------

  

**绑定style基本语法:**

**对象语法:**

​      内联式:

- `v-bind:style` 的对象语法十分直观——看着非常像 CSS，但其实是一个 JavaScript 对象。

- ```html
  
  <div v-bind:style="{ color: activeColor, fontSize: fontSize + 'px' }"></div>
  CSS property 名可以用驼峰式 (camelCase) 或短横线分隔 (kebab-case，记得用引号括起来) 来命名
  ```
  
  外联式:
  
- 直接绑定到一个样式对象通常更好，这会让模板更清晰：

- ```html
  <div v-bind:style="styleObject"></div>
  data: {
    styleObject: {
      color: 'red',
      fontSize: '13px'
    }
  }
  ```

**数组语法:**数组语法可以将多个样式对象应用到同一个元素上

- ```html
  <div v-bind:style="[baseStyles, overridingStyles]"></div>
  ```

**自动添加前缀:**

- > 当 `v-bind:style` 使用需要添加[浏览器引擎前缀](https://developer.mozilla.org/zh-CN/docs/Glossary/Vendor_Prefix)的 CSS property 时，如 `transform`，Vue.js 会自动侦测并添加相应的前缀

**多重值:**

- 从 2.3.0 起你可以为 `style` 绑定中的 property 提供一个包含多个值的数组，常用于提供多个带前缀的值,这样写只会渲染数组中最后一个被浏览器支持的值

- ```html
  <div :style="{ display: ['-webkit-box', '-ms-flexbox', 'flex'] }"></div>
  ```



##### v-if /v-show条件渲染

**v-if条件渲染:**

- 条件渲染一块内容:

- ```html
  <h1 v-if="awesome">Vue is awesome!</h1>
  <h1 v-else>Oh no 😢</h1>
  ```
  
- > 可以添加`v-else` 添加一个“else 块” : 还可以添加 v-else-if块
  >
  > v-else 元素必须紧跟在带 v-if 或者 v-else-if 的元素的后面，否则它将不会被识别。

- 条件渲染多个元素:   

- ```html
  在 <template> 元素上使用 v-if 条件渲染分组:
      
  <template v-if="ok">
    <h1>Title</h1>
    <p>Paragraph 1</p>
    <p>Paragraph 2</p>
</template>
  ```

**用 key 管理可复用的元素:**

  - Vue 会尽可能高效地渲染元素，通常会复用已有元素而不是从头开始渲染. 

  - > 即两个相同的标签,不会渲染第二个,而是复用第一个,
    >
    > 把标签内的值进行更换, 但是特殊情况 input 输入内容,会被复用而不是清空
    >
    > 因此,我们使用key 方式,表达“这两个元素是完全独立的，不要复用它们”

-  只需添加一个具有唯一值的 `key` attribute 即可

- >  <input placeholder="Enter your email address" key="email-input">

**v-show条件渲染:**

- `v-show` 的元素始终会被渲染并保留在 DOM 中。`v-show` 只是简单地切换元素的 CSS property `display`。

- > 注意，`v-show` 不支持 `<template>` 元素，也不支持 `v-else`。

- **v-show v-if的区别:** 

- > v-if  是“真正”的条件渲染，因为它会确保在切换过程中条件块内的事件监听器和子组件适当地被销毁和重建。
  >
  > 也是**惰性的**：直到条件第一次变为真时，才会开始渲染条件块。
  >
  > `v-show` 就简单得多——不管初始条件是什么，元素总是会被渲染，并且只是简单地基于 CSS 进行切换
- > 一般来说，`v-if` 有更高的切换开销，而 `v-show` 有更高的初始渲染开销。
  >
  > 因此，如果需要非常频繁地切换，则使用 `v-show` 较好；如果在运行时条件很少改变，则使用 `v-if` 较好。

  

##### v-for列表渲染

- 用 `v-for` 指令基于一个数组来渲染一个列表:

- ```html
  <li v-for="item in items" >   items 是源数据数组，而 item 则是被迭代的数组元素的别名
  <li v-for="(item, index) in items">      支持一个可选的第二个参数，即当前项的索引。0开始   of可以代替in
  ```

- 用 `v-for` 来遍历一个对象的 property

- ```html
  <li v-for="value in object">   以此遍历对象的属性值
  <li v-for="(value,name) in Object"></li>  遍历对象属性值 value 属性名 name
  <div v-for="(value, name, index) in object"> 用第三个参数作为索引：
  ```

- > 在遍历对象时，会按 `Object.keys()` 的结果遍历，但是**不能**保证它的结果在不同的 JavaScript 引擎下都一致。

**列表渲染状态**

- 当 Vue 正在更新使用 `v-for` 渲染的元素列表时，它默认使用“就地更新”的策略。

- > **如果数据项的顺序被改变**，Vue 将**不会移动 DOM 元素**来匹配数据项的顺序，而是就地更新每个元素.只会渲染数据,不会更改DOM元素

- 这个默认的模式是高效的，但是**只适用于不依赖子组件状态或临时 DOM 状态 (例如：表单输入值) 的列表渲染输出**。
  
- > 但是并不总是高效的,为了更好的重用现有元素提高效率,要利用`key` 让其能跟踪每个节点的身份.
  
- **使用key 值的原因:**

  > vue Dom 更新原理,通过Virtual DOM diff算法 ==> 通过对比DOM中的值,不一致进行更新值,直到没有数据.
  >
  > 例子 : 当把一条数据插入到很多数据的中间,那么它将会比对DOM中的数据是否一致,不一致,新的数据项将替换旧的数据项,一直渲染到没有数据,导致在中间插入一条数据,需要更新多个DOM的值,很没有效率.
  >
  > 
  >
  > 通过添加 key 值, 我们以相同的key值为基准(相同的key之间进行对比),进行新旧DOM值比对,不一致进行值更新,没有的key值渲染新的DOM添加进来,这样大大提高了效率
  >
  > 
  >
  > 所以一句话，key的作用主要是在新旧 nodes 对比时辨识 VNodes,更高效的更新虚拟DOM。
  >
  > 另外vue中在使用相同标签名元素的过渡切换时，也会使用到key属性，其目的也是为了让vue可以区分它们，否则vue只会替换其内部属性而不会触发过渡效果。

- key 值最好不要使用数组中的 index, 因为这会导致key变化,更新很多的dom数据,降低效率

  

- > 不要使用对象或数组之类的非基本类型值作为 `v-for` 的 `key`。请用字符串或数值类型的值。



**列表源数据(数组)的方法:**

- Vue 将被侦听的数组的变更方法进行了包裹，所以它们也将会触发视图更新。这些被包裹过的方法包括：

- 更改数组(变更方法)

- > push()      pop()
  >
  > shift()    unshift()
  >
  > splice()      sort()        reverse()  

- 替换数组:(非变更方法)

- > `filter()`、`concat()` 和 `slice()`。它们不会变更原始数组，而**总是返回一个新数组** ,
  >
  > 这不会使Vue 丢弃现有 DOM 并重新渲染整个列表,Vue 为了使得 DOM 元素得到最大范围的重用使用了优化方法
  >
  > 所以用一个含有相同元素的数组去替换原来的数组是非常高效的操作。

- 由于 JavaScript 的限制，Vue **不能检测**数组和对象的变化



- **直接使用过滤/排序后的结果:**

- 我们想要显示一个数组经过过滤或排序后的版本，而不实际变更或重置原始数据。

- > 解释: 不再进行把原数据替换/重置后,更新视图. 而是直接使用处理过的数组显示视图. 这样省去了更新dom的麻烦

- 在这种情况下，可以创建一个计算属性，来返回过滤或排序后的数组。使用这个数组进行渲染

- ```html
  <li v-for="n in evenNumbers">{{ n }}</li>
  computed: {
    evenNumbers: function () {返回处理后的数组}
    }
  ```

- 在计算属性不适用的情况下 (例如，在嵌套 `v-for` 循环中) 你可以使用一个方法

- > 待拓展



- **在 v-for 里使用值** : 

- ```html
   `v-for` 也可以接受整数。在这种情况下，它会把模板重复对应次数。
  <span v-for="n in 10">{{ n }} </span> //输出 1 2 3..10
  ```


- **v-for重复渲染多个元素:**

- > 可以利用带有 `v-for` 的 `<template>` 来循环渲染一段包含多个元素的内容

- **使用 `v-if` 和 `v-for`:**

- > 当它们处于**同一节点**，`v-for` 的优先级比 `v-if` 更高，这意味着 `v-if` 将分别重复运行于每个 `v-for` 循环中
  >
  > 如果你的目的是有条件地跳过循环的执行，那么可以将 `v-if` 置于外层元素 
  
  **在组件上使用 v-for:**
  
- > 在自定义组件上，你可以像在任何普通元素上一样使用 `v-for`。
   >
   > 2.2.0+ 的版本里，当在组件上使用 `v-for` 时，`key` 现在是必须的。




##### v-on处理事件

**监听事件:** 

- 内联式:

- ```html
  1.直接传入js代码段
  <button v-on:click="counter += 1">Add 1</button>
2.直接调用一个方法
   <button v-on:click="say('hi')">Say hi</button>
     用特殊变量 `$event` 把它传入方法,可以访问原始的DOM事件
  ```


- 外联式

- ```html
   1.传递一个方法 methods
   <button v-on:click="greet">Greet</button>
   ```


  **事件修饰符:**

- 方法只有纯粹的数据逻辑，而不是去处理 DOM 事件细节。为了解决这个问题，Vue.js 为 `v-on` 提供了**事件修饰符**

- > .stop 停止冒泡
  >
  > .prevent  阻止默认事件
  >
  > .capture  捕获模式
  >
  > .self  只当在 event.target 是当前元素自身时触发处理函数
  >
  > .once  只会触发一次
  >
  > > 不像其它只能对原生的 DOM 事件起作用的修饰符，`.once` 修饰符还能被用到自定义的**组件事件**上
  >
  > .passive  会告诉浏览器你**不**想阻止事件的默认行为。
  >
  > > 不要把 `.passive` 和 `.prevent` 一起使用，因为 `.prevent` 将会被忽略，同时浏览器可能会向你展示一个警告

  

  **按键/系统修饰符:**
  
- Vue 允许为 `v-on` 在监听键盘事件时添加按键修饰符：只在满足修饰符的情况下执行事件

- ```html
  <input v-on:keyup.enter="submit">
  <input v-on:keyup.page-down="onPageDown">
  ```


- 按键码:  `keyCode` 的事件用法[已经被废弃了](https://developer.mozilla.org/en-US/docs/Web/API/KeyboardEvent/keyCode)并可能不会被最新的浏览器支持。但使用 `keyCode` attribute 也是允许的：

- ```
  <input v-on:keyup.13="submit">
  ```


- > 为了在必要的情况下支持旧浏览器，Vue 提供了绝大多数常用的按键码的别名：
  >
  > .enter  .tab  .delete  (捕获“删除”和“退格”键)   .esc    .space  .up  .down    .left   .right
  >
  > 你还可以通过全局 `config.keyCodes` 对象[自定义按键修饰符别名](https://cn.vuejs.org/v2/api/#keyCodes)：
>
  > Vue.config.keyCodes.f1 = 112

  

- 系统修饰符 : `.ctrl .alt .shift .meta`   仅在按下相应按键时才触发鼠标或键盘事件的监听器

- ```html
  <div v-on:click.ctrl="doSomething">Do something</div> <!-- Ctrl + Click -->
  ```


- > 请注意**修饰键与常规按键不同**，在和 `keyup` 事件一起用时，事件触发时修饰键必须处于按下状态然后释放另一个按键。

  

- **`.exact` 修饰符: **允许你控制由**精确的**系统修饰符组合触发的事件。

- ```html
  <button v-on:click.ctrl.exact="onCtrlClick">A</button>
  <!-- 有且只有 Ctrl 被按下的时候才触发 -->
  ```

-  **鼠标按钮修饰符: ** `.left` `.right` `.middle` 仅响应特定的鼠标按钮



##### v-model处理表单输入

- `v-model` 指令在表单 `<input>`、`<textarea>` 及 `<select>` 元素上创建双向数据绑定。

- > `v-model` 会忽略所有表单元素的 `value`、`checked`、`selected` attribute 的初始值而总是将 Vue 实例的数据作为数据来源

- **各个类型的表单基本用法:**

- > 1.文本/多行文本  input  textarea   数据使用字符串 例如:
  >
  > ```html
  > <input v-model="message" placeholder="edit me">
  > <p>Message is: {{ message }}</p>
  >  vue 绑定数据 message:""
  > ```
  >
  > 2.复选框 type="checkbox"    单个复选框 : 数据使用字符串   多个复选框: 数据使用数组
  >
  > ```html
  > 单个复选框:
  > <input type="checkbox" id="checkbox" v-model="checked">
  > <label for="checkbox">{{ checked }}</label>  // checked:"",数据是字符串, 默认会显示ture false
  > 多个单选框:
  > <input type="checkbox" id="jack" value="Jack" v-model="checkedNames">
  > <label for="jack">Jack</label>
  > <input type="checkbox" id="john" value="John" v-model="checkedNames">
  > <label for="john">John</label>   
  > <label for="john">{{checkedNames}}</label> 
  > 
  > // checkedNames:[],数据是数组, 使用value 属性进行显示
  > ```
  >
  > 数据使用数组,复选框没有value 值 ,那么数组中显示null,  有value值显示value
  >
  > 数据使用字符串, 复选框没有value 值, 显示ture/false 并且会造成全选.
  >
  > 
  >
  > 3.单选框  type="radio"  
  >
  > ```html
  > <input type="radio" id="one" value="One" v-model="picked">
  > <label for="one">One</label>
  > <span>Picked: {{ picked }}</span>
  > ```
  >
  > 数据使用字符串,必须有value值才会显示,没有value值会造成全选,还不会显示值
  >
  > 数据使用数组, 有value值的话也可以,也只会显示单个value值,没有value值会造成全选,不显示值
  >
  > 想让radio 单选框默认被选中,必须是数据和 value值相同, radio不存在 true/false值, 它只使用value 
  >
  > 
  >
  > 4.选择框   <select>  数据可以是数组/字符串,有value显示value值,没有value值显示option中的值,但是都只显示一个值
  >
  > 
  
  **表单的值(value)绑定:**
  
- 对于单选按钮，复选框及选择框的选项，`v-model` 绑定的值通常是静态字符串,这里指的是表单的value属性值,我们可以使用`v-bind` 绑定value 属性到一个动态值上.

- ```html
  <select v-model="selected">
    <option v-bind:value="number">123</option>
  </select>
  ```

  **修饰符:**

- **.lazy** :  在默认情况下，`v-model` 在每次 `input` 事件触发后将输入框的值与数据进行同步, 使用 `lazy` 修饰符，从而转为在 `change` 事件_之后_进行同步, 

- ```html
    <input type="text" v-model.lazy="msg">
    ```

- **.number** : 如果想自动将用户的输入值转为数值类型，可以给 `v-model` 添加 `number` 修饰符

- > 因为即使在 `type="number"` 时，HTML 输入元素的值也总会返回字符串。如果这个值无法被 `parseFloat()` 解析，则会返回原始的值。

- **.trim** :  如果要自动过滤用户输入的首尾空白字符，可以给 `v-model` 添加 `trim` 修饰符

- > 对于需要使用输入法的语言，你会发现 `v-model` 不会在输入法组合文字过程中得到更新。
  >
  > 
  >
  > 在文本区域插值 (`<textarea>{{text}}</textarea>`) 并不会生效，应用 `v-model` 来代替。



**v-model实现原理:**

- `v-model` 在内部为不同的输入元素使用不同的 property 并抛出不同的事件：

- > text 和 textarea 元素使用 `value` property 和 `input` 事件；
  >
  > checkbox 和 radio 使用 `checked` property 和 `change` 事件；
  >
  > select 字段将 `value` 作为 prop 并将 `change` 作为事件。

  
  
- v-model 只不过是一个语法糖而已 

- ```html
  <input v-model="searchText">  等价于=>
  
  <input
    v-bind:value="searchText"
    v-on:input="searchText = $event.target.value"
  >
  ```

  

- **自定义组件上使用v-model**

- > 1.需要定义 绑定value属性一个动态值
  >
  > 2.需要在组件模板内$emit 触发input事件,并抛出一个值给父组件

- ```vue
  Vue.component('custom-input', {
    props: ['value'],
    template: `
      <input
        v-bind:value="value"             
        v-on:input="$emit('input', $event.target.value)"
      >
    `
  })
  这样父组件就可以使用 v-model 了
  
  <custom-input v-model="searchText"></custom-input>
  相当于=>
  <custom-input
    v-bind:value="searchText"
    v-on:input="searchText = $event"
  ></custom-input>
  ```

  
  
- 一个组件上的 `v-model` 默认会利用名为 `value` 的 prop 和名为 `input` 的事件，但是像单选框、复选框等类型的输入控件可能会将 `value` attribute 用于[不同的目的](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input/checkbox#Value)。`model` 选项可以用来避免这样的冲突

- > 首先解释下默认利用 value input的问题 : 在html正常使用 v-model,它会根据type,选择不同的属性 和事件进行处理,这点可以参考上面的原理部分,   
  >
  > 在 自定义组件内,就算使用 type="checkbox", 它也会默认使用value  input事件(绑定别的事件/属性没有用), 会影响选框类组件的 value属性,导致提交时的问题.
  >
  > 所以我们使用 `model` 解决这样的问题,这样我们可以自定义value值进行其他目的

- ```js
   Vue.component('base-checkbox', {
    model: {
      prop: 'checked',
      event: 'change'
    },
    ...
    props;{
    checked:Boolen,
    }
    }
    //注意使用model 选项,仍然需要在props 里声明这个 checked
  ```

  

  



##### 计算属性和监听器

- 模板内的表达式非常便利，但是设计它们的初衷是用于简单运算的。在模板中放入太多的逻辑会让模板过重且难以维护,所以，对于任何复杂逻辑，你都应当使用**计算属性**.    computed: { reverMessage:function(){}}

- 计算属性 vs方法

- > 我们可以将同一函数定义为一个方法而不是一个计算属性,两种方式的最终结果确实是完全相同的。然而，不同的是**计算属性是基于它们的响应式依赖进行缓存的**。
  >
  > 也就是说 计算属性中函数不发生更新, 就**只返回缓存**不在进行执行函数

- 如果你不希望有缓存，请用方法来替代。(双大括号中也可以放置方法的)

- 计算属性 v s侦听属性

- > Vue 提供了一种更通用的方式来观察和响应 Vue 实例上的数据变动：**侦听属性**: watch, 
  >
  > 通常更好的做法是使用计算属性而不是命令式的 `watch` 回调

- 计算属性的setter

- > 计算属性默认只有 getter(通常返回一个值)，不过在需要时你也可以提供一个 setter

**侦听器 watch:**

- 虽然计算属性在大多数情况下更合适，但有时也需要一个自定义的侦听器,

- > 当需要在**数据变化时**执行**异步或开销较大**的操作时，这个方式是最有用的。除了 `watch` 选项之外，您还可以使用命令式的 [vm.$watch API](https://cn.vuejs.org/v2/api/#vm-watch)。  (开销较大的操作不应该是 使用计算属性吗??)

- watch 中的选项含义 `deep` `immediate`

- > deep : bool  告诉Vue使用递归方式侦听**嵌套对象内部值**的变化  (如果没有deep选项,想要侦听内部值必须手动设置),
  >
  > immediate :bool  侦听开始就调用函数,不在等到属性变化时再调用

### 第二层:组件系统

- #### Vue使用组件化进行开发

  

#### 组件基础

- **什么是组件 :** 组件是可复用的 Vue 实例，且带有一个名字

- > 因为组件是可复用的 Vue 实例，所以它们与 `new Vue` 接收相同的选项，例如 `data`、`computed`、`watch`、`methods` 以及生命周期钩子等。
  >
  > 仅有的例外是像 `el` 这样根实例特有的选项。

  

- **组件的复用:** 每个组件都会各自独立维护它的数据, 可以进行任意次复用, 原因:

- > **一个组件的 `data` 选项必须是一个函数**，因此每个实例可以维护一份被返回对象的独立的拷贝, 如果 Vue 没有这条规则,那么data属性将被共享, 无法进行复用

- > `data` 选项返回的数据,作用域只在**组件内 模板中**, 而不能作用在自定义组件上(指自定义标签)

  

- **组件名:**

- >两种命名方法:   推荐使用连字符的写法,因为直接在 DOM (即非字符串的模板) 中使用时只有 kebab-case 是有效的。
  >
  >**字母全小写且必须包含一个连字符**, ( kebab-case)
  >
  >使用 PascalCase (**首字母大写命名**)  驼峰命名
  
  

- **每个组件模板内必须只有一个根元素**, 



- **组件注册**: 

- *全局注册*:

- ```js
  Vue.component('my-component-name', {
    // ... options ...
  })
  ```

- > 全局注册的组件可以用在其被注册之后的任何 新创建的 Vue 根实例(通过 new Vue)模板中，也可以用在子组件中
  >
  > 例如: 全局注册三个组件, 三个组件可以在各自内部中相互使用

- 全局注册往往是不够理想的。如果使用webpack构建系统,全局注册所有的组件意味着即便你已经不再使用一个组件了，它仍然会被包含在你最终的构建结果中

- *局部注册:*

- ```js
  //1.通过普通对象 来定义组件:
  var ComponentA = { /* ... */ }
  //2.然后在 components 选项中定义你想要使用的组件：
  new Vue({
    el: '#app',
    components: {
      'component-a': ComponentA,  //属性名是自定义便签名
    }
  })
  
  //如果需要子组件之间使用:
  var ComponentA = { /* ... */ }
  
  var ComponentB = {
    components: {
      'component-a': ComponentA
    },
    // ...
  }
  ```

- **模块系统下的组件注册注意事项:**

- 如果使用 es2015+ 模块系统,子组件局部注册

- ```javascript
  import ComponentA from './ComponentA'
  import ComponentC from './ComponentC'
  
  export default {
    components: {
      ComponentA,   //同时代表 自定义标签名
      ComponentC
    },
    // ...
  }
  ```

- 对于一些频繁用到的**基础组件**,我们往往会进行全局注册, 如果你使用webpack/vue cli 构建系统,那么就可以使用 `require.context` **一次性,自动化全局注册**这些基础组件

- > - [基础组件的自动化全局注册](https://cn.vuejs.org/v2/guide/components-registration.html#基础组件的自动化全局注册)  代码参考

  

#### 组件间的数据/内容传递

##### **父子组件之间传递数据**

- *父组件通过 `prop `向子组件传递数据:*

- **什么是Prop:  **Prop 是你可以在组件上注册的一些*自定义 attribute*。(渲染时并不会被渲染成 html attribute)

- > 当一个值传递给一个 prop attribute 的时候，它就变成了那个组件实例的一个 **property**。变成了一个 **property**说明传递的数据不会被渲染成 html attribute ,而是在实例中使用的属性.
  >
  > 这个值可以是静态字符串,也可以是v-bind 绑定的动态值
  >
  > 
  >
  > 一个组件默认可以拥有任意数量的 prop，任何值都可以传递给任何 prop。

- > 问题 : 改变这个 传递给prop的值 会引起外部数据的变化吗     不推荐 警告

  

- **prop命名:**HTML 中的 attribute 名是大小写不敏感的，所以浏览器会把所有大写字符解释为小写字符。这意味着当你使用 DOM 中的模板时，camelCase (驼峰命名法) 的 prop 名需要使用其等价的 kebab-case (短横线分隔命名) 命名
- **prop类型:** 字符串形式/ 对象形式



-  **prop注册: ** 给 prop 传入一个静态的值, 或者也可以使用 `v-bind` 来动态传递 prop。

- ```html
  //这里 prop 注册的是  props: ['title']
  <blog-post title="My journey with Vue"></blog-post>  //静态值
  <blog-post title="Blogging with Vue"></blog-post>
  
  <blog-post
    v-for="post in posts"
    v-bind:key="post.id"
    v-bind:title="post.title"
  ></blog-post>   								//动态值
  
  //通过 v-bind 的动态绑定,就算是静态字符串也会被解析成js表达式
  ```
  
- 如果你想要将一个对象的所有 property 都作为 prop 传入,可以使用`v-bind="post" `, 把整个post对象传入prop

- **prop单向数据流:** 

- > 所有的 prop 都使得其父子 prop 之间形成了一个**单向下行绑定** :父级 prop 的更新会向下流动到子组件中,但是反过来则不行
  >
  > 每次父级组件发生变更时，子组件中所有的 prop 都将会刷新为最新的值。这意味着你**不**应该在一个子组件内部改变 prop。

- 两种试图改变 prop的情形 :

  > 1. 子组件希望prop当成本地的数据进行使用:  定义一个本地的 data 属性, 并将这个 prop 用作其初始值：
  > 2. prop 传入后需要进行转换 :  定义一个计算属性,返回一个转换后的副本
  >
  > 
  >
  > 注意js中数组/对象传递的是引用,所以在子组件中改变这个,会影响到父组件的状态



- **Prop 验证:**

- 我们可以为组件的 prop 指定验证要求, 为了定制 prop 的验证方式，你可以为 `props` 中的值提供一个带有验证需求的对象，

- ```js
  Vue.component('my-component', {
    props: {
  
      // 多个可能的类型
      propB: [String, Number],
      
      // 必填的字符串
      propC: {
        type: String,
        required: true
      },
      
      // 带有默认值的数字
      propD: {
        type: Number,
        default: 100
      },
      
      // 带有默认值的对象
      propE: {
        type: Object,
        // 对象或数组默认值必须从一个工厂函数获取
        default: function () {
          return { message: 'hello' }
        }
      },
      
      // 自定义验证函数
      propF: {
        validator: function (value) {
          // 这个值必须匹配下列字符串中的一个
          return ['success', 'warning', 'danger'].indexOf(value) !== -1
        }
      }
    }
  })
  ```

- > 注意那些 prop 会在一个**组件实例创建之前进行验证**，所以实例的 property (如 `data`、`computed` 等) 在 `default` 或 `validator` 函数中是不可用的。



- **非 prop 的 attribute:**  

- >  attribute 传向一个组件，但是该组件并没有相应 prop 定义.
  >
  >  这些 attribute 会被添加到这个组件的根元素上。对于绝大多数 attribute 来说，从外部提供给组件的值会替换掉组件内部设置好的值。
  >
  > 但是`class` 和 `style` attribute 会稍微智能一些，即两边的值会被合并起来

- ```html
  <blog-post
  v-bind:post="post" 
  title="this is se"
  ></blog-post>
  
  // 声明组件中props: ['post'],只有一个post属性, 那么title attribute就会被传递给模板的根元素 
  //  title 就是非prop的 attribute 
  ```

- 如果你**不**希望组件的根元素继承 非prop的attribute，你可以在组件的选项中设置 `inheritAttrs: false`

- > `inheritAttrs: false` 选项**不会**影响 `style` 和 `class` 的绑定。

- 通过`inheritAttrs: false` 和 `$attrs`, 决定attribute 会被赋予哪个元素,而不总是根元素

- ```html
   <label>
        {{ label }}
        <input
          v-bind="$attrs"
          >        //通过 v-bind="$attrs" 传入内部组件
   </label>
  ```

  ------
  
  

**子组件通过`事件`向父组件传递数据:**

- > 1. 子组件模板内可以通过`v-on:click =$emit('enlarge-text', 0.1)  ` 触发事件并传递数据
  > 2. 父级组件监听这个自定义事件,内联式直接处理,或者传递一个函数,在methods中处理

- ```HTML
  <button v-on:click="$emit('enlarge-text', 0.1)">  //子组件模板内
  
  <blog-post
    v-on:enlarge-text="postFontSize += $event"  //父级组件,通过内联式处理     $event:代表数据
  ></blog-post>
  
  <blog-post
    v-on:enlarge-text="onEnlargeText"       //父级组件,通过函数处理 , 函数第一个参数接受子组件的数据
  ></blog-post>
  ```
  

	

**自定义事件**:

- **事件名**: 不同于组件和 prop，事件名不存在任何自动化的大小写转换。而是触发的事件名需要完全匹配监听这个事件所用的名称。(触发一个 camelCase 名字的事件, 监听这个名字的 kebab-case 版本是不会有任何效果)

- > `v-on` 事件监听器在 **DOM 模板中**会被自动转换为全小写 (因为 HTML 是大小写不敏感的)，所以 `v-on:myEvent` 将会变成 `v-on:myevent`——导致 `myEvent` 不可能被监听到。


- **将原生事件绑定到子组件:**

- 想要在一个组件的根元素(含有模板的父组件)上直接监听一个**原生事件**。这时，你可以使用 `v-on` 的 `.native` 修饰符：  如果不使用 native 将无法监听事件

- > 针对组件 Vue 有自己的事件系统,在父组件山 v-on希望监听一个自定义事件, 而不是原生事件, 所以需要加上native告诉他.
  >
  > $on $emit 并不是addeventlistener dispatcheventlistener 的别名,这和浏览器事件是不同的两个系统

- > 为什么在父组件 监听原生组件而不是直接在根元素上 ??
  >
  > 原因是 :  
  >
  > 因为我们在使用时,经常使用父组件, 不会去特意去修改组件里的 template 模板内容, 使用native 父组件绑定的事件传递给子组件
  >
  > 1.作用域的原因, 事件将使用父作用域,
  >
  > 2.而且不加native,父组件绑定的事件无法传给 子组件, 不像 非prop 的attribute



- > <base-input v-on:focus.native="onFocus"></base-input>  
  >
  > //这会传递到组件上的根元素上, 但是当根元素是个特殊元素,没有该事件,父级的 `.native` 监听器将静默失败。它不会产生任何报错

- 为了解决这个问题，Vue 提供了一个 `$listeners` property，它是一个对象，里面包含了作用在这个组件上 (不含 `.native` 修饰器的) `v-on` 事件监听器的所有监听器,你就可以配合 `v-on="$listeners"` 将所有的事件监听器指向这个组件的某个特定的子元素

  

  **.sync修饰符**:*(子组件通过事件的形式,改变prop绑定的值, 即像父组件传递了数据)*

- 在有些情况下，我们可能需要对一个 prop 进行“双向绑定”。不幸的是，真正的双向绑定会带来维护上的问题，因为子组件可以变更父组件，且在父组件和子组件都没有明显的变更来源。

- 这也是为什么我们推荐以 `update:myPropName` 的模式触发事件取而代之:

- > 在一个包含 `title` prop 的假设的组件中，我们可以用以下方法表达对其赋新值的意图
  >
  > ```html
  > this.$emit('update:title', newTitle)
  > 
  > 然后父组件可以监听那个事件并根据需要更新一个本地的数据 property
  > 
  > <text-document 
  > 
  > v-bind:title="doc.title" 
  > 
  > v-on:update:title="doc.title = $event" >
  > 
  > </text-document>
  > 
  > ```
  >
  > 
  >
  > 我们为这种模式提供一个缩写，即 `.sync` 修饰符, 类似于v-model 也是一个语法糖
  >
  > ```html
  > <text-document v-bind:title.sync="doc.title"></text-document>
  > // 子组件设置 this.$emit('update:title', newTitle)
  > // 父组件将会自动绑定prop 并且添加用于更新的 v-on 监听器。
  > ```
  >
  > 

- 当我们用一个对象同时设置多个 prop 的时候，也可以将这个 `.sync` 修饰符和 `v-bind` 配合使用：

- ```html
  <text-document v-bind.sync="doc"></text-document>
  //用在一个字面量的对象上，例如 v-bind.sync=”{ title: doc.title }”，也是无法正常工作的
  ```

  

- 注意带有 `.sync` 修饰符的 `v-bind` **不能**和表达式一起使用, 你只能提供你想要绑定的 property 名

  

  

##### **通过插槽传递内容**

- 和 HTML 元素一样，我们经常需要向一个组件传递内容，(注意不是数据 而是内容)

*插槽的基本用法*::

- **插槽内容:**

- ```html
  父组件: 
  <navigation-link url="/profile">
    Your Profile
  </navigation-link>
  
  子组件:
  <a
    v-bind:href="url"
    class="nav-link"
  >
    <slot>当组件渲染的时候，将会被替换为“Your Profile”。</slot>
  </a>
  
  ```

- > 如果子组件中没有<slot>标签 , 父组件的内容将会被抛弃
  >
  > **slot 插槽可以包含任何模板代码**, 可以是html标签...

- **后备内容:**(插槽的默认内容)

- ```html
  例如在一个 <submit-button> 组件template中:
  <button type="submit">
    <slot>Submit</slot>
  </button>
  ```



- **插槽中使用数据:**

- ```html
  <navigation-link url="/profile">
    Logged in as {{ user.name }}
  </navigation-link>
  ```

- > 当你想在一个插槽中使用数据时, 这里的作用域只能使用和父组件作用域一样的,(使用vue实例中的数据)
  >
  > 而子组件只能使用, Vue组件定义内数据, 两个作用域**相互独立**  =>
  >
  > **父级模板里的所有内容都是在父级作用域中编译的；子模板里的所有内容都是在子作用域中编译的。**

- **作用域插槽:**

- > 让插槽内容能够访问子组件作用域的数据*, 插槽内容作用域和子组件的作用域是独立的, 使用作用域插槽,可以访问到子组件的作用域

- ```html
  <current-user>
    <template v-slot:default="slotProps">  //将包含所有插槽 prop 的对象命名为 slotProps
      {{ slotProps.user.firstName }}
    </template>
  </current-user>     
  
  子组件 :
    <span>
      <slot v-bind:user="user">   // 绑定在 <slot> 元素上的 attribute 被称为插槽 prop,通过该prop访问子组件的数据
        {{ user.lastName }}
      </slot>
    </span>
  ```
- 在上述情况下，当被提供的内容*只有*默认插槽时，组件的标签才可以被当作插槽的模板来使用。不再使用template

- ```html
  <current-user v-slot:default="slotProps">
    {{ slotProps.user.firstName }}
  </current-user>
  //default 也可以省略
  ```

- 注意默认插槽的缩写语法**不能**和具名插槽混用，因为它会导致作用域不明确,只要出现多个插槽，请始终为*所有的*插槽使用完整的基于 `<template>` 的语法



- **解构插槽prop**:

- > 作用域插槽的内部工作原理是将你的插槽内容包裹在一个拥有单个参数的函数里:  
  >
  > - function (slotProps) {  // 插槽内容 }
  >- 这意味着 `v-slot` 的值实际上可以是任何能够作为函数定义中的参数的 JavaScript 表达式。可以使用**解构语法**来**传入具体的插槽 prop**:    在该插槽提供了多个 prop 的时候很有用
  > 


- ```html
  <current-user v-slot="{ user }">
    {{ user.firstName }}
  </current-user>
  prop 重命名:
  <current-user v-slot="{ user: person }">
    {{ person.firstName }}
  </current-user>
  定义后备内容，用于插槽 prop 是 undefined 的情形:
  <current-user v-slot="{ user = { firstName: 'Guest' } }">
  {{ user.firstName }}
  </current-user>
  ```




- **具名插槽:**(*多个插槽时需要起名字*)

- ```html
  //<base-layout>组件的template中:
  <div>
  <header>
      <slot name="header"></slot>
    </header>
  <main>
      <slot></slot>
  </main>
  </div>
  
  <base-layout>组件中:
      
  <base-layout>
  <template v-slot:header>
      <h1>Here might be a page title</h1>
    </template>
  
    <p>A paragraph for the main content.</p>
    <p>And another one.</p>
  </base-layout> ```
  
 
  ```
  
- > 现在 `<template>` 元素中的所有内容都将会被传入相应的插槽。任何没有被包裹在带有 `v-slot` 的 `<template>` 中的内容都会被视为默认插槽的内容。

- 默认插槽内容也可以使用<template v-slot:default> </template> 包裹起来

- > 默认插槽内容没有被包裹在`<template>`中 和包裹在<template v-slot:default> </template> 中的元素,后者优先级更高

-  *v-slot 只能添加在 <template> 上* 




- **动态插槽名 :**

- ```html
  <base-layout>
    <template v-slot:[dynamicSlotName]>
    </template>
  </base-layout>
  ```


- **具名插槽的缩写**:   `v-slot`: 替换为字符 `#`


- **废弃语法:**

- > 带有 slot attribute 的具名插槽 <template slot="header">
  >
  > 带有 slot-scope attribute 的作用域插槽<template slot="default" slot-scope="slotProps">



#### 组件切换(动态组件) / 异步组件:

- `is attribute`

- 有的时候，在**不同组件之间进行动态切换**是非常有用的,可以通过 Vue 的 `<component>` 元素加一个特殊的 `is` attribute 来实现:

- ```html
  <component v-bind:is="currentTabComponent"></component>
  `currentTabComponent `  可以是 已注册组件的名字，或 一个组件的选项对象(带有component选项的对象)
  
  注意和注册组件  components 选项的区别
  ```



- > `is  attribute `可以用于常规 HTML 元素，但这些元素将被视为组件，这意味着所有的 attribute **都会作为 DOM attribute 被绑定**。
  >
  >  意味着像 `value` 这样的 property,只能表示用户输入的值,而不是预设的值  参考js attr property区别    value 只能从特性 同步到 属性

- **解析 DOM 模板时的注意事项:**

- 有些 HTML 元素，诸如 `<ul>`、`<ol>`、`<table>` 和 `<select>`，对于哪些元素可以出现在其内部是有严格限制的。而有些元素，诸如 `<li>`、`<tr>` 和 `<option>`，只能出现在其它某些特定的元素内部。

- ```
  <table>
    <blog-post-row></blog-post-row>
  </table>   //无法解析该自定义组件
  
  <table
    <tr is="blog-post-row"></tr>   // 使用 is  attribute 解决该状况
  </table>
  ```

- 但是从以下来源使用模板,这条限制不存在 :

- > - 字符串 (例如：`template: '...'`)
  > - [单文件组件 (`.vue`)](https://cn.vuejs.org/v2/guide/single-file-components.html)
  > - < script type="text/x-template">
  
- **keep-alive:**

- 使用 `is` attribute 来切换不同的组件,当在这些组件之间切换的时候，会重新渲染这些组件, 你有时会想保持这些组件的状态，以避免反复重渲染导致的性能问题。  :可以用一个 `<keep-alive>` 元素将其动态组件包裹起来。

- ```html
  <keep-alive>  
  <component v-bind:is="currentTabComponent"></component> 
  </keep-alive>
  ```
  
  

- **异步组件:** 

- 在大型应用中，我们可能需要将应用分割成小一些的代码块，并且只在需要的时候才从服务器加载一个模块。为了简化，Vue 允许你以一个工厂函数的方式定义你的组件, 这个工厂函数会异步解析你的组件定义。


- > ...待拓展

- ```vue
  Vue.component('async-example', function (resolve, reject) {
    setTimeout(function () {
      // 向 `resolve` 回调传递组件定义
      resolve({
        template: '<div>I am async!</div>'
      })
    }, 1000)
  })
  ```

  



#### 处理边界情况

- > 这里记录的都是和处理边界情况有关的功能，即一些需要对 Vue 的规则做一些小调整的特殊情况。

  - **访问元素&组件**

  - 每个 `new Vue` 实例的子组件中, 通过 `$root` property 进行访问根实例

  - > this.$root.foo  //获取根实例的数据

    

  - 和 `$root` 类似, `$parent` property 可以用来从一个子组件访问父组件的实例。它提供了一种机会，可以在后期随时触达父级组件，以替代将数据以 prop 的方式传入子组件的方式。

  - 但是对于嵌套过深的父子元素推荐**依赖注入**

  - > 依赖注入 : 实例选项：`provide` 和 `inject`
    >
    > `provide` 选项允许我们指定我们想要**提供**给后代组件的数据/方法。
    >
    > 然后在任何后代组件里，我们都可以使用 `inject` 选项来接收指定的我们想要添加在这个实例上的 property：

  - 依赖注入还是有负面影响的。它将你应用程序中的组件与它们当前的组织方式耦合起来，使重构变得更加困难。同时所提供的 property 是非响应式的。

    

  - 尽管存在 prop 和事件，有的时候你仍可能需要在 JavaScript 里**直接访问一个子组件**。通过 `$ref` , 这个 attribute 为子组件赋予一个 ID 引用, 调用$ref , 访问子组件实例(即定义组件的整个作用域), 还可以在子组件实例上再次调用ref 获得子元素

  - ```html
    <base-input ref="usernameInput"></base-input>
    ```

  - > `$refs` 只会在组件渲染完成之后生效，并且它们不是响应式的。这仅作为一个用于直接操作子组件的“逃生舱”——你应该避免在模板或计算属性中访问 `$refs`。

- **程序化的事件侦听器:** 

- > Vue 实例同时在其事件接口中提供了其它的方法。
  >
  > 你通常不会用到这些，但是当你需要在一个**组件实例上手动侦听事件**时，它们是派得上用场的

- >- 通过 `$on(eventName, eventHandler)` 侦听一个事件
  >- 通过 `$once(eventName, eventHandler)` 一次性侦听一个事件
  >- 通过 `$off(eventName, eventHandler)` 停止侦听一个事件

- **循环引用:**

- **组件本身的循环引用:**

- 组件是可以在它们自己的模板中调用自身的。不过它们只能通过 `name` 选项来做这件事：

- > 使用 `Vue.component` 全局注册一个组件时，这个全局的 ID 会自动设置为该组件的 `name` 选项。

- ```html
  template: '<div><stack-overflow></stack-overflow></div>'
  ```

- 请确保递归调用是条件性的(使用v-if) , 否则组件将会导致“max stack size exceeded”错误(组件无限循环)

- **组件间的循环引用:**

- > 待拓展: ...

- **模板定义的替代品:**

- 内联模板 : `inline-template` 这个特殊的 attribute 出现在一个子组件上时，这个组件将会使用其里面的内容作为模板

- ```html
  <my-component inline-template>
    <div>
      <p>These are compiled as the component's own template.</p>
      <p>Not parent's transclusion content.</p>
    </div>
  </my-component>
  ```

- > 不过，`inline-template` 会让模板的作用域变得更加难以理解。请在组件内优先选择 `template` 选项或 `.vue` 文件里的一个 `<template>` 元素来定义模板。

- X-Template:  在一个 `<script>` 元素中，带上 `text/x-template` 的类型，然后通过一个 id 将模板引用过去。

- ```html
  <script type="text/x-template" id="hello-world-template">
    <p>Hello hello hello</p>
  </script>
  
  Vue.component('hello-world', {
    template: '#hello-world-template'
  })
  ```

- **控制更新:**

- 强制更新 : [`$forceUpdate`](https://cn.vuejs.org/v2/api/#vm-forceUpdate) 



- 通过 `v-once` 创建低开销的静态组件:

- > 组件包含了**大量**静态内容。在这种情况下，你可以在根元素上添加 `v-once` attribute 以确保这些内容只计算一次然后缓存起来, (这导致的副作用是, 该内容是无法进行更新的)

- ```html
  Vue.component('terms-of-service', {
    template: `
      <div v-once>
        <h1>Terms of Service</h1>
        ... a lot of static content ...
      </div>
    `
  })
  ```

### 番外: vue 提供的额外内容

#### 过渡&动画

- Vue 在插入、更新或者移除 DOM 时，提供多种不同方式的应用过渡效果

- > - 在 CSS 过渡和动画中自动应用 class(可以使用第三方css动画库 Animate.css自定义)
  > - 在过渡钩子函数中使用 JavaScript 直接操作 DOM ,(可以使用第三方 JavaScript 动画库 Velocity.js)

- **单元素/组件的过渡:**   使用`transition` 的封装组件

- > 在下列情形中,可以给任何元素和组件添加进入/离开过渡
  >
  > - 条件渲染 (使用 `v-if`)
  > - 条件展示 (使用 `v-show`)
  > - 动态组件
  > - 组件根节点

- 当插入或删除包含在 `transition` 组件中的元素时，Vue 将会做以下处理：

- > 1.检查元素是否应用了css过渡/动画, 如果是在恰当的时机添加/删除类名
  >
  > 2.过渡组件是否提供了JavaScript钩子函数, 这些钩子函数在恰当时机调用
  >
  > 3.如果以上都没有找到, 立即执行DOM操作

  
  
  
  - **过渡的类名:**
  
- > 1. `v-enter`：元素进入过渡的开始状态, 元素插入dom之前,添加该类到元素中,元素插入的**下一帧**移除
    > 2. `v-enter-active`：进入过渡状态激活应用该类。在元素插入DOM之前添加该类到元素 ,在过渡/动画完成之后移除
    > 3. `v-enter-to`：**2.1.8 版及以上**定义进入过渡的结束状态。在元素被插入DOM之后下一帧生效 (与此同时 `v-enter` 被移除)，在过渡/动画完成之后移除。
    > 4. `v-leave`：定义离开过渡的开始状态。在离开过渡被触发时立刻生效，**下一帧**被移除。
    > 5. `v-leave-active`：定义离开过渡生效时的状态。在整个离开过渡的阶段中应用，在离开过渡被触发时立刻生效，在过渡/动画完成之后移除。这个类可以被用来定义离开过渡的过程时间，延迟和曲线函数。
    > 6. `v-leave-to`：**2.1.8 版及以上**定义离开过渡的结束状态。在离开过渡被触发之后下一帧生效 (与此同时 `v-leave` 被删除)，在过渡/动画完成之后移除。
  >
    > 没有名字的 `<transition>`，则 `v-` 是这些类名的默认前缀。如果你使用了 `<transition name="my-transition">`  则 `v-enter` 会替换为 `my-transition-enter`。

  - > 一般不设置 `v-enter-to`   `v-leave` 类
    >
    > 因为 `v-enter-to`在结束动画时该类会被移除,移除后样式恢复默认(会发生突然变化无法保存最后的样式效果), 不添加时,直接应用v-enter 类往默认的样式过渡
    >
    > `v-leave`类 在离开过渡时立即生效,下一帧移除,导致看不到初始效果(看起来还是默认效果)
  
  - **css过渡&动画:**
  
  - ```html
     插入或删除包含在 `transition` 组件中的元素,自动应用类完成过渡
      <transition name="slide-fade">
      
          <p v-if="show">hello</p>
      
        </transition>
      .slide-fade-enter-active {
        transition: all .3s ease;
    }...省略
    ```
  
  - CSS 动画用法同 CSS 过渡，区别是在动画中 `v-enter` 类名在节点插入 DOM 后不会立即删除，而是在 `animationend` 事件触发时删除。
  
    
  
  - **自定义过渡的类名(使用第三方css库):**
  
- > 我们可以通过以下 attribute 来自定义过渡类名：
    >
    > - `enter-class`
    > - `enter-active-class`
    > - `enter-to-class` (2.1.8+)
    > - `leave-class`
    > - `leave-active-class`
    > - `leave-to-class` (2.1.8+)
  
  - ```html
    <link href="https://cdn.jsdelivr.net/npm/animate.css@3.5.1" rel="stylesheet" type="text/css"> 
  <transition
        name="custom-classes-transition"
      enter-active-class="animated tada"
        leave-active-class="animated bounceOutRight"
     >
        <p v-if="show">hello</p>
    </transition>  
    ```

  
  
    

  - **同时使用过渡和动画:**  使用过渡 动画其中一种, Vue 能自动识别类型并设置监听,事件是否完成。在一些场景中，你需要给同一个元素同时设置两种过渡动效，

  - > 在这种情况中，你就需要使用 `type` attribute 并设置 `animation` 或 `transition` 来明确声明你需要 Vue 监听的类型。
  
  - **显性的过渡持续时间:** 默认情况下，Vue 会等待其在过渡效果的根元素的第一个 `transitionend` 或 `animationend` 事件。
  
  - > 你可以用 `<transition>` 组件上的 `duration` prop 定制一个显性的过渡持续时间 ,在过渡/动画上的事件失效
    >
    > <transition :duration="{ enter: 500, leave: 800 }">...</transition>
  
  - **JavaScript钩子:**
  
  - ```html
    <transition
      v-on:before-enter="beforeEnter"
      v-on:enter="enter"
      v-on:after-enter="afterEnter"
      v-on:enter-cancelled="enterCancelled"
    
      v-on:before-leave="beforeLeave"
    v-on:leave="leave"
      v-on:after-leave="afterLeave"
    v-on:leave-cancelled="leaveCancelled"
    >
    <!-- ... -->
    </transition>
    这些钩子函数可以结合 CSS transitions/animations 使用，也可以单独使用。
    ```
  
  - 当只用 JavaScript 过渡(**只在钩子函数中定义样式变化**,一般使用第三方js库在钩子函数中实现)的时候，**在 `enter` 和 `leave` 中必须使用 `done` 进行回调**。否则，它们将被同步调用，过渡会立即完成。
  
  - > 推荐对于仅使用 JavaScript 过渡的元素添加 `v-bind:css="false"`，Vue 会跳过 CSS 的检测。
  
- **初始渲染的过渡:** 初始渲染过渡就是界面初始化时的过渡，在界面刚打开或刚刷新的时候就进入过渡渲染。

- 可以通过 `appear` attribute 或者`v-on:appear `设置节点在初始渲染的过渡, 设置类(appear-class...)样式

- 这里默认和进入/离开过渡一样，同样也可以自定义 CSS 类名。自定义 JavaScript 钩子：

- ```html
  <transition
    appear
    appear-class="custom-appear-class"
    appear-to-class="custom-appear-to-class" (2.1.8+)
    appear-active-class="custom-appear-active-class"
  >
    <!-- ... -->
  </transition>
  ```

  
  
  
  
- **多个元素的过渡:** 

- > 对于原生标签可以使用 `v-if`/`v-else`
  >
  > 当有**相同标签名**的元素切换时，需要通过 `key` attribute 设置唯一的值来标记以让 Vue 区分它们，否则 Vue 为了效率只会替换相同标签内部的内容。

- 也可以通过给同一个元素的 `key` attribute 设置不同的状态来代替 `v-if` 和 `v-else`

- **过渡模式:**   

- `<transition>` 的默认行为 - 进入和离开过渡同时发生。

  - > - `in-out`：新元素先进行过渡，完成之后当前元素过渡离开。
    > - `out-in`：当前元素先进行过渡，完成之后新元素过渡进入。

- **多个组件间的过渡:** 

- > 我们只需要使用绑定 is , 使用动态组件



- **列表过渡:**

- 以上都是单个元素/组件的过渡,或者同一时间多个节点中的一个, 如何同时渲染一个列表?

- > 使用 <transition-group> 组件, 不同于<transition>
  >
  > 1. 它默认是一个<span>作为根节点,  可以通过tag="" 更换其他的元素
  > 2. 过渡模式不可用,因为不再切换特定元素,
  > 3. 内部元素**总是需要**一个key, 因为vue高度复用dom节点的问题, 不加key造成指定的DOM节点并没有动态变化
  > 4. css过渡/动画都是应用在内部的元素中, 而不是这个容器中

- **列表的进入/离开过渡:**

- ```html
  <div id="list-demo" class="demo">
    <button v-on:click="add">Add</button>
    <button v-on:click="remove">Remove</button>
    <transition-group name="list" tag="p">
      <span v-for="item in items" v-bind:key="item" class="list-item">
        {{ item }}
      </span>
    </transition-group>
  </div>
  ```

  

- **列表的排序过渡:**(即被动移动元素的平滑过渡)

- 在操作元素的位置变化时，由于DOM文档流的变化，会同时引起其它（邻近）节点元素的位置变化, 对于这些“被动”移动的元素来说，也可以实现过渡，这就用到了`v-move` 特性。

- >      1.**`v-move` class**，它会在元素的**改变定位的过程中**(交换数据位置也算改变定位)应用。
  >
  >    可以通过 `name` attribute 来自定义前缀，也可以通过 `move-class` attribute 手动设置 
  >
  >    2. 也可以给列表的所有元素都添加一个类，直接给这个类设置CSS transition属性，元素移动的时候自动获得v-move。
  >    3.  用splice删除数组的元素，由于删除的元素经历过渡时，始终占据文档流的这个位置，导致下一个元素要等待其过渡结束,DOM移除时才移动过来，造成一个生硬的效果。要达到平滑过渡，就要在删除元素leave-active阶段用position:absolute将其移出文档流，后面的元素才能同时平滑过渡过来
>
  >    
  >
  >    内部的实现，Vue 使用了一个叫 [FLIP](https://aerotwist.com/blog/flip-your-animations/) 简单的动画队列,使用 transforms 将元素从之前的位置平滑过渡新的位置。
  >
  >    需要注意的是使用 FLIP 过渡的元素不能设置为 `display: inline` 。作为替代方案，可以设置为 `display: inline-block` 或者放置于 flex 中

  

- **列表的交错过渡:**  通过 data attribute 数据 和 js钩子函数, 直接在函数中根据data操作过渡时间, 形成交错过渡的效果

- **可复用的过渡:** 要创建一个可复用过渡组件，你需要做的就是将 `<transition>` 或者 `<transition-group>` 作为根组件

- **动态过渡:** 在 Vue 中即使是过渡也是数据驱动的！动态过渡最基本的例子是通过 `name` attribute 来绑定动态值。

- > 不仅仅只有 attribute 可以利用，还可以通过事件钩子获取上下文中的所有数据，因为事件钩子都是方法。根据数据  使用js过渡会有不同的表现

#### 状态过渡

- 数据元素本身的动效, 这些数据要么本身就以数值形式存储，要么可以转换为数值。

- 比如：

  - 数字和运算
  - 颜色的显示
  - SVG 节点的位置
  - 元素的大小和其他的 property

- 通过侦听器我们能监听到任何数值 property 的数值更新, 再使用第三方动画库, 实现数据的动效

- **动态状态过渡:** 类似于组件的动态过渡

- > 管理太多的状态过渡会很快的增加 Vue 实例或者组件的复杂性，可以提取到子组件中。



#### 可复用性&&组合

- **混入(mixin)** : 提供了一种非常灵活的方式，来分发 Vue 组件中的可复用功能。

- 一个混入对象可以包含**任意组件选项**。当组件使用混入对象时，所有混入对象的选项将被“混合”进入该组件本身的选项

- 混合语法: 

- ```javascript
  var myMixin = {
    created: function () {
      this.hello()
    },
    methods: {
      hello: function () {
        console.log('hello from mixin!')
      }
    }
  }
  var Component = Vue.extend({         //使用基础 Vue 构造器，创建一个“子类”。参数是一个包含组件选项的对象。
    mixins: [myMixin]
  })
  
  var component = new Component() // => "hello6 from mixin!"
  
  ```

- 混合规则: 

- > 数据对象(data)在内部会进行递归合并，并在发生冲突时以组件数据优先。
  >
  > 同名钩子函数将合并为一个数组，因此都将被调用。另外，混入对象的钩子将在组件自身钩子**之前**调用。
>
  > 值为对象的选项，例如 `methods`、`components` 和 `directives`，将被合并为同一个对象。两个对象键名冲突时，取组件对象的键值对。

- 全局混入:一旦使用全局混入，它将影响**每一个**之后创建的 Vue 实例。

- ```js
  new Vue({
    mixins: [mixin],
    created: function () {
      console.log('组件钩子被调用')
    }
  })
  ```

  

- 自定义选项混合规则:

- > `Vue.config.optionMergeStrategies` 添加一个函数

  

**自定义指令:**

- Vue2.0 中，代码复用和抽象的主要形式是组件。然而，有的情况下，你仍然需要对普通 DOM 元素进行底层操作，这时候就会用到自定义指令。

- 自定义指令:

- > 定义全局指令:
  >
  > Vue.directive('指令name', {});
  >
  > 定义局部指令: 
  >
  > 组件中也接受一个 `directives` 的选项

- 一个指令定义对象可以提供如下几个**钩子函数**:

- >bind  只调用一次，指令第一次绑定到元素时调用。
  >
  >inserted 被绑定元素插入父节点时调用 (仅保证父节点存在，但不一定已被插入文档中)。
  >
  >update  所在组件的 VNode 更新时调用，**但是可能发生在其子 VNode 更新之前**。
  >
  >componentUpdated  指令所在组件的 VNode **及其子 VNode** 全部更新后调用。
  >
  >unbind 只调用一次，指令与元素解绑时调用。

- 上述钩子函数的参数:

- > - `el`：指令所绑定的元素，可以用来直接操作 DOM。
  >
  > - ```
  >   binding：一个对象，包含以下 property：
  >   ```
  >
  >   - `name`：指令名，不包括 `v-` 前缀。
  >   - `value`：指令的绑定值，例如：`v-my-directive="1 + 1"` 中，绑定值为 `2`。
  >   - `oldValue`：指令绑定的前一个值，仅在 `update` 和 `componentUpdated` 钩子中可用。无论值是否改变都可用。
  >   - `expression`：字符串形式的指令表达式。例如 `v-my-directive="1 + 1"` 中，表达式为 `"1 + 1"`。
  >   - `arg`：传给指令的参数，可选。例如 `v-my-directive:foo` 中，参数为 `"foo"`。
  >   - `modifiers`：一个包含修饰符的对象。例如：`v-my-directive.foo.bar` 中，修饰符对象为 `{ foo: true, bar: true }`。
  >
  > - `vnode`：Vue 编译生成的虚拟节点。移步 [VNode API](https://cn.vuejs.org/v2/api/#VNode-接口) 来了解更多详情。
  >
  > - `oldVnode`：上一个虚拟节点，仅在 `update` 和 `componentUpdated` 钩子中可用。

- **指令的参数可以是动态的**。例如，在 `v-mydirective:[argument]="value"` 中，`argument` 参数可以根据组件实例数据进行更新！这使得自定义指令可以在应用中被灵活使用。通过调用binding.arg 来获取该动态值 

- **函数简写**:你可能想在 `bind` 和 `update` 时触发相同行为，而不关心其它的钩子

- ```javascript
  Vue.directive('color-swatch', function (el, binding) {
    el.style.backgroundColor = binding.value
  })
  ```

- **对象字面量 :**  如果指令需要多个值，可以传入一个 JavaScript 对象字面量

- ```
  <div v-demo="{ color: 'white', text: 'hello!' }"></div>
  //通过binding.value.color , binding.value.text调用
  ```

**渲染函数&&JSX**

- Vue 推荐在绝大多数情况下使用模板来创建你的 HTML。但是有时候你需要**动态编辑模板**中内容,需要js的完全编程控制,这是需要 渲染函数

- > 模板 也是一种限制, 渲染函数可以对渲染的模板进行完全的编辑, 利用函数控制渲染出的DOM

- **vue更新DOM方法:**

- > 1.在vue里使用模板 <h1>{{ blogTitle }}</h1>
  >
  > 2.或者使用渲染函数  :
  >
  > render: function (createElement) { 
  >
  >  return createElement('h1', this.blogTitle) 
  >
  > }
  >
  > 在这两种情况下，Vue 都会自动保持页面的更新

- **vue更新DOM原理:**

- **虚拟 DOM** : Vue 通过建立一个**虚拟 DOM** 来追踪自己要如何改变真实 DOM

- > return createElement('h1', this.blogTitle)
  >
  > createElement 返回的不是一个实际的DOM元素, 而是createNodeDescription, 它所包含的信息会告诉 Vue 页面上需要渲染什么样的节点包括及其子节点的描述信息。
  >
  > 我们把这样的节点描述为“虚拟节点 (virtual node)”，也常简写它为“**VNode**”。

- **渲染函数:**

- **createElement 参数**

- ```javascript
  createElement(
    // {String | Object | Function}
    // 一个 HTML 标签名、组件选项对象，或者
    'div',
  
    // {Object}
    // 一个与模板中 attribute 对应的数据对象。可选。
    {
    // 允许你绑定普通的 HTML attribute
        'class': {
      foo: true,
      bar: false
    },
         domProps: {
      innerHTML: 'baz'
    },
    },
    // {String | Array}
    // 子级虚拟节点 (VNodes)，由 `createElement()` 构建而成，
    // 也可以使用字符串来生成“文本虚拟节点”。可选。
    [
      '先写一些文字',
      createElement('h1', '一则头条'),
      createElement(MyComponent, {
        props: {
          someProp: 'foobar'
        }
      })
    ]
  )
  ```

- 待拓展`createElement` 函数...

- **使用 JavaScript 代替模板功能:**

- > 只要在原生的 JavaScript 中可以轻松完成的操作，Vue 的渲染函数就不会提供专有的替代方法。
  >
  > 比如 : 模板中的 v-if v-for  , 在渲染函数中使用 js 的 if/else/map重写即可

- 渲染函数中没有与 `v-model` 的直接对应——你必须自己实现相应的逻辑

- 对于 `.passive`、`.capture` 和 `.once` 这些事件修饰符, Vue 提供了相应的前缀可以用于 `on`(createlement 函数中)

- 你可以通过 [`this.$slots`](https://cn.vuejs.org/v2/api/#vm-slots) 访问静态插槽的内容，每个插槽都是一个 VNode 数组, 也可以通过 [`this.$scopedSlots`](https://cn.vuejs.org/v2/api/#vm-scopedSlots) 访问作用域插槽，每个作用域插槽都是一个返回若干 VNode 的函数

- JSX : 在 Vue 中使用 JSX 语法，它可以让我们回到更接近于模板的语法上。

**模板编译:**

- Vue 的模板实际上被编译成了渲染函数

#### 插件

- 使用插件: 通过全局方法 `Vue.use()` 使用插件。它需要在你调用 `new Vue()` 启动应用之前完成：

- > `Vue.use` 会自动阻止多次注册相同插件，届时即使多次调用也只会注册一次该插件。
  >
  > Vue.js 官方提供的一些插件 (例如 `vue-router`) 在检测到 `Vue` 是可访问的全局变量时会自动调用 `Vue.use()`。然而在像 CommonJS 这样的模块环境中，你应该始终显式地调用 `Vue.use()`：

- 开发插件 :  Vue.js 的插件应该暴露一个 `install` 方法。这个方法的第一个参数是 `Vue` 构造器，第二个参数是一个可选的选项对象

#### 过滤器

- Vue.js 允许你自定义过滤器，可被用于一些常见的文本格式化。

- > 过滤器可以用在两个地方：**双花括号插值和 `v-bind` 表达式** (后者从 2.1.0+ 开始支持)。

- ```js
  //过滤器应该被添加在 JavaScript 表达式的尾部，由“管道”符号指示：
  
  <!-- 在双花括号中 -->
  {{ message | capitalize }}
  
  <!-- 在 `v-bind` 中 -->
  <div v-bind:id="rawId | formatId"></div>
  
  局部过滤器:  在一个组件的选项中定义
  filters: {
    capitalize: function (value) {
      if (!value) return ''
      value = value.toString()
      return value.charAt(0).toUpperCase() + value.slice(1)
    }
  }
  
  
  全局过滤器： 在创建 Vue 实例之前使用函数
  Vue.filter('capitalize', function (value) {
    if (!value) return ''
  value = value.toString()
    return value.charAt(0).toUpperCase() + value.slice(1)
  })
  ```
  
- 当全局过滤器和局部过滤器重名时，会采用局部过滤器。

  
  



### 响应式原理



- 由于js 原因无法检测对象 和数组

- > Vue 无法检测 property 的添加或移除。

- > Vue 不能检测以下数组的变动：
>
  > 1. 当你利用索引直接设置一个数组项时
  > 2. 当你修改数组的长度时



- 这样规则对 watch 一样使用 , 对于对象内部的变化需要使用deep选项

- > 对象 : watch 无法检测到 对象某个属性的值 /嵌套对象 改变, 使用deep
>
  > 数组同上

  

  




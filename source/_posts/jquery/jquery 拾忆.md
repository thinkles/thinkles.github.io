---
title: JqueryApi 进阶
date: 
tags: Jquery
categories: Jquery
---



#### Ajax (等待)  高级ajax操作  高级DOM操作(对服务器传来的数排序)
- 理解扩展事件 : 相当于给jquery新建事件,封装起来复用
#### 延迟对象(不理解)
  -  promise  都用于ajax 这中耗时操作中, 延迟对象一般不需要手动建立,一般建立好的知道使用方法即可
  -  回调对象
  -  > \$.Callbacks()模块开发目的是：给内部\$.ajax()和$.Deferred()模块提供统一的基本功能组件。它可以用来作为类似基础定义的新组件的功能
-涉及到jquery的原理层面
##### 自定义遍历符(开发插件的一部分) 参考高级遍历
- 选择符选择优化问题(原理)
-  利用 $.expr[':']  开发自定义选择器
#### 工具类
---------------------------------



### 根据API进行分类

#### 选择器过滤
- .last()  获取匹配元素集合中最后一个元素 和:last 相同。  :last-child  判断匹配到的元素是否是其父元素的最后一个子元素  

- :selected 选取select 中被选取的元素

- :even 从0开始算
#### 遍历
- add() 添加元素到匹配的元素集合。  addback([ selector]) 添加**当前堆栈中**元素**集合**到当前集合 ,(选择器可以筛选出之前堆栈内容 之后添加到当前集合)
- > 在end addback中的堆栈操作解释 : 每次遍历方法都会找到新的元素,这些新的元素构成一个jq对象压入堆栈

- end() addback()都是通过堆栈来实现的,end终止在当前链的过滤操作,返回匹配的元素的以前状态,本质使用了出栈操作,返回原来的一个状态

- closest() 从元素本身开始，逐级向上找到符合条件的第一个元素，该元素可能是当前元素自身，也可能是最靠近当前元素的一个祖先元素。
  

- .children()  获得元素的子元素(可以进行选择筛选)   .contents()获得元素的子元素包括文字注释节点

- nextAll() prevAll() 返回的结果都不包括自身

- next([selector ] )   返回元素紧邻的后面的同辈元素,如果提供一个选择器，那么只有紧跟着的兄弟元素满足选择器时，才会返回此元素。

- has() 筛选相匹配的元素,如果符合条件就留下来,最后返回一个集合    not() 通过条件筛选,符合条件的去除 和has 相反
-  is()判断当前元素,返回布尔值

- map(callback)  用于处理当前jQuery对象匹配的所有元素，并将处理结果封装为新的jq数组 
- > 特别适用于获取或设置元素集合中的值

- .slice(start [, end )  根据指定的下标范围，过滤匹配的元素集合 (从零开始计数,所以范围是到end-1) 

-find() 是往匹配元素的后代元素里寻找符合条件的

#### 偏移 尺寸
- offsetparent() 获得被定位后的祖先元素的top left值,没有就返回undefined
  
- position()  获得当前元素针对被定位祖先元素的偏移量 没有就返回针对document的偏移量
- >被定位 指的是 position不为static的元素

- 指的注意的是 偏移的top left值的计算是不包括margin值,到maigrin的边界的值才是偏移量
  
- height() 获取高度值(只包括内容)  innerheight() 获取高度值(包含padding不包含border) outerheight(包括 padding border 参数为true 则包含margin)

#### DOM操作 (css属性 html属性 DOM增删改查)
- 删除操作
 
 - empty() 这个方法不仅移除子元素（和其他后代元素),同样移除元素里的文本
  
 - remove() 和.empty()相似 , 当我们想将元素自身移除时我们用 .remove()
  
 - detach() 要删除的元素不删除数据和事件的情况下，使用.detach()来代替。 当需要移走一个元素，不久又将该元素插入DOM时，这种方法很有用。

- prop()  val() 对表单进行操作 
- val() 主要用于获取表单元素的值, val()获得value属性内容,对于多选下拉列表则返回一个数组包含每个选择项     通过.val([ "表单value值"])可以设置被选中的状态 (对于select 没有value值的直接写选项中的值也可以被选中)

- .prop( propertyName, value ) 获取/设置元素的property值(一般我们用在表单属性上,具体参考API)


-  html() 获取设置匹配元素内部的html内容(设置时会把之前的内容替换掉) 
-  text() 得到匹配元素集合中每个元素的文本内容结合，包括他们的后代.     可以用来给指定元素设置文本内容(文本内容会替换指定元素里的所有内容)


- DOM 操作分为几类 : 内部/外部添加到前面/后面   调用顺序不同
#### 事件

- .triggerHandler()   .triggerHandler() 方法的行为与 .trigger() 相似,不会触发默认行为 事件冒泡, 只会对匹配到的第一个元素生效   trigger()与之相反
  
- trigger() 除了一次模拟事件还可以给事件提供额外参数: .trigger( eventType [,extraParameters ])  额外参数是数组类型
- > $("p").click( function (event, a, b) {}).triggr("click", ["foo", "bar"]);
  
- .one() 要绑定一个事件，并且只运行一次，然后删除自己

- on()函数  .on( events [, selector ] [, data ], handler(eventObject) ) 
- > .on( events [, selector ] [, data ] ) 这里的events是一个对象键值是事件名 值是处理函数,可以利用此一次处理多个事件

- keydown  keypress 事件区别 : keypress表示被输入哪个字符(字符代码) ,若想捕获敲击了哪个特殊键的话，例如，方向键(keycode)，  使用 .keydown()或.keyup() 更好。想要得到输入的文本内容只能是keyup,其他的只能获取输入前的文本
  

- change 事件被< input>, < select>, 和< textarea> 触发,对元素值**更改后**触


#### 特效

- .animate() 所有用于动画的属性(设置变化的键值对)必须是数字的，除非另有说明,不要使用css简写属性
除了定义数值，每个属性能使用'show', 'hide', 和 'toggle'。这些快捷方式允许定制隐藏和显示动画用来控制元素的显示或隐藏。动画属性可以使用 +=

- 精细控制动画  animate 函数 step progress函数使用  : step函数被每个动画元素的每个动画属性调用 ,它接受两个参数（now 和 fx） this是当前正在执行动画的DOM元素集合
- >now: 每一步动画属性的数字值 fx: jQuery.fx 原型对象的一个引用 
- > 比如elem 表示前正在执行动画的元素，start和end分别为动画属性的第一个和最后一个的值，prop为进行中的动画属性。

- queue 属性,决定要不要进队列中一个动画接下一个,flase 则是会和下一个动画方法同时发生(前提 : 这些都是连缀书写动画方法)

- progress函数 每一步动画(step大约13毫秒调用一次,progress相似的方式)完成后调用的一个函数，无论动画属性有多少，每个动画元素都执行单独的函数


- .delay()  设置一个延时来推迟执行队列中后续的项  它无法取消延时,不是JavaScript的原生 setTimeout函数的替代品

- .queue( [queueName ] 默认为fx),显示在匹配元素上已经执行的函数队列 (这里的执行不一定是已经完成了,有时动画可能在队列中排队,完成函数调用就算是执行) ,
  
- .queue( [queueName ], newQueue ) newQueue是array类型代表函数数组,用来替换当前队列中的内容,newQueue可以是空的数组 [],可以用来取消队列中不想进行的函数操作  类似 clearQueue()操作
  
- .queue( [queueName ], callback( next ) ) 它让我们把新函数置入到队列的末端。为jQuery集合中的每个元素执行一次回调函数。类似在动画方法提供回调函数
> 当使用.queue()添加一个函数的时候，我们应该保证在函数最后调用了 jQuery.dequeue()，这样就能让队列中的其它函数按顺序执行。 从jQuery 1.4开始，向队列中追加函数时，可以向该函数中传入另一个函数next()

> 队列允许一个元素来异步的访问一连串的动作，而不终止程序执行,在执行上一个同时还能继续下个操作,队列存储信息  :$('#foo').slideUp().fadeIn(); ,这个元素开始立即做滑动动画，但渐入动画放置在 fx列队,  所以jq可以执行完slideup还能继续执行渐变, 


- .dequeue()  移除队列中的首个函数, 然后执行这个函数, 这个执行的函数中也应当直接或间接的包含.dequeue()语句，这样才能继续执行队列中的其它函数

- .clearQueue( [queueName ] )方法被访问的时候，所有在这个列队中未执行的函数将被移除 。当不使用参数的时候，.clearQueue()会从标准的动画队列fx中移除剩下的函数
- >这个方法类似.stop(true)。然而.stop()方法只适用在动画中。.clearQueue()还可以用来移除用.queue()方法添加到普通jQuery列表的任何函数。 



- stop()  , 只会停止当前动画,对于队列中的其他动画可以选择停止,stop停止的动画函数,无法执行动画里的回调函数
- .stop( [clearQueue ] [, jumpToEnd ] ) 默认都为false
- .stop( [queue ] [, clearQueue ] [, jumpToEnd ] ),还可以选择队列名称

- finish() 在一个元素上被调用，立即停止当前正在运行的动画和所有排队的动画（如果有的话），并且他们的CSS属性设置为它们的目标值 和.stop(true, true)很相似,但是stop只会把当前的动画跳转到目标值

>关于动画队列,每个元素维护自己的一个动画队列,不会不同的动画元素放在一起

#### 杂项
- data() 在匹配元素上存储任意相关数据. .removeData()移除存储数据 通过data()函数存取的数据都是临时数据，一旦页面刷新，之前存放的数据都将不复存在

- toArry() 返回包含jquery对象的DOM元素数组  get()返回一个对应的DOM元素
  
#### 高级事件处理
- 理解早委托 : 把事件绑定到document,不必等到文档全部加载好才能够处理事件,当委托加载好就可以
- 理解自定义事件 -自定义事件参数 : 用on函数定义 自定义事件处理 trigger 进行调用
- >.on("myCustomEvent", function(e, myName, myValue){}) 自定义事件 还可以可以给与参数,在trigger调用时添加参数即可

#### 使用插件
- 使用插件原理是一样的,默认的只需要调用一个(插件自定义函数),如果想要自定义内容就要在函数中提供参数,来修改一些属性(这些属性只能通过插件API 文档获得)

#### 插件开发
- 添加jquery对象的全局函数    $.func    $.extend({})  
- 扩展jquery 对象的方法    $.fn.func  $.fn.extend({})
- >jQuery.fn对象是jQuery.prototype的别名
- > 在任何插件方法内部，关键字this引用的都是当前的jQuery对象
  

- 使用命名空间隔离函数 : 其他jQuery插件也可能定义相同的函数名。为了避免和自己定义插件的函数冲突，最好的办法是 把属于一个插件的全局函数都封装到一个对象中 (实际上创建一个对象,在对象里定义方法,这样就起到隔离的效果)
- >  $.mathUtils = { 函数 ) 
##### 开发插件注意的问题

-考虑方法的隐式迭代问题 :  jQuery的选择符表达式可能会匹配零、一或多个元素, 调用.each()方法；这样就会执行隐式迭代 (在调用的.each()方法内部，this依次引用每个DOM元素)
- > this.each(function(){})  这里this代表一个有很多元素集合的 jquery对象
  
- 考虑方法的连缀问题:  函数的返回值要是一个jquery 对象

- 参数默认值 默认参数的可修改 自定义参数 问题
 - 提供自定义参数,做到和animate()方法一样,能够给与参数修改属性值的操作, 提供默认值,有时候不用给参数的属性值也可以调用 或者给一部分, 
  
 - 方法参数也可能不是一个简单的数字值，可能会更复杂。在各种jQuery API中经常可
 以看到另一种参数类型，即回调函数

 - 多次调用一个插件,给与同一个参数改变默认值,很麻烦,可以直接修改默认值,例如 jquery.fx.interval 可以直接修改默认的动画频率

```
(function($) { 
  $.fn.shadow = function(opts) { 
    var defaults = { 
      copies: 5, 
      opacity: 0.1 
      copyOffset: function(index) {    //默认方法参数是一个函数
        return {x: index, y: index};
    }; 
    var options = $.extend(defaults, opts); 
    var offset = options.copyOffset(i);   //自定义参数 (函数形式)
    // ...  自定义参数格式: options.参数名
  }; 
})(jQuery);
```
- extend函数,会使opt对象对defaults 进行覆盖.
- 上式只能改变自定义参数, 但是不能直接改变默认值,想直接改变默认值,默认值参数对象必须放在函数代码外,能够被外部调用

```
 (function($) { 
  $.fn.shadow = function(opts) {  
    var options = $.extend({},$.fn.shadow.defaults, opts); //只能修改空对象,如果修改defaults, 在自定义参数中就会更改了默认值.
 
    // ...  自定义参数值 :  options.参数名
  }; 
  var $.fn.shadow.defaults = {
       copies: 5, 
       opacity: 0.1,
  };
})(jQuery);

```
- 修改默认值 :   $.shadow.defaults.参数名=...


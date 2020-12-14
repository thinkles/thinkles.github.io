---
title: JS指导 下
date: 
tags: js基础
categories: Javascript
---


#### 函数进阶

##### Rest参数  Spread语法

- 在 JavaScript 中，很多内建函数都支持传入任意数量的参数

- > 如何实现 : 使用Rest 参数 `...`
  >
  > Rest 参数`...` : 在 JavaScript 中，无论函数是如何定义的，你都可以使用任意数量的参数调用函数。不会因为传入“过多”的参数而报错,只会取得前几个参数匹配             

- **Rest 参数语法**:

- ```javascript
  function showName(firstName, lastName, ...titles) {
      
  }
  //Rest 参数必须放到参数列表的末尾, 收集 参数放到titles数组中
  ```



- **arguments 变量**:

- >  有一个名为 arguments 的特殊的**类数组对象**，该对象包含所有参数,使用索引访问参数
  >
  > ```javascript
  > function showName() {
  >   alert( arguments.length );
  >   alert( arguments[0] );
  >   alert( arguments[1] );
  >   alert(arguments) //代表所有参数
  >   // 它是可遍历的
  >   // for(let arg of arguments) alert(arg);
  > }
  > 
  > // 依次显示：2，Julius，Caesar
  > showName("Julius", "Caesar");
  > ```

- > 在过去，JavaScript 中没有 rest 参数，而使用 arguments 是获取函数所有参数的唯一方法,两者区别:arguments 是一个类数组，也是可迭代对象，但它终究不是数组,不支持数组方法

- > 箭头函数是没有 "arguments", 箭头函数内访问到的是属于外部'普通函数'的

  

- **Spread** :

- > 把可迭代对象 spread 开, 分给每个函数参数, 和Rest参数相反
  >
  > ```javascript
  > let arr = [3, 5, 1];
  > 
  > alert( Math.max(1,2,3,...arr) ); // 5（spread 语法把数组转换为参数列表）
  > 
  > spread 参数是在调用时 使用...     Rest参数是在函数声明时 使用...
  > ```

- >  对于可迭代对象都可以展开, 甚至可以用它来合并数组, 转化数组,复制数组
  >
  > ```javascript
  > let merged = [0, ...arr, 2, ...arr2];  //返回一个数组
  > 
  > alert( [...str] ); // [..str] 把字符串转化为数组    
  > 
  > let arr = [1, 2, 3]; let arrCopy = [...arr] //复制一个数组, arr是对象也成立
  > 
  > ```
  
- > 和` Array.from(obj)`的结果相同,
  >
  > 区别在于`Array.from `**适用于类数组对象也适用于可迭代对象**, Spread 语法**只适用于可迭代对象**。

- 若 ... 出现在函数参数列表的最后，那么它就是 rest 参数，它会把参数列表中剩余的参数收集到一个数组中。

- 若 ... 出现在函数调用或类似的表达式中，那它就是 spread 语法，它会把一个数组展开为列表。


##### 闭包
- 词法环境 : 
  1. **环境记录（Environment Record）** —— 一个存储所有局部变量作为其属性（包括一些其他信息，例如 `this` 的值）的对象。
  2. 对 **外部词法环境** 的引用，与外部代码相关联

- 一个“变量”只是 **环境记录** 这个特殊的内部对象的一个属性。“获取或修改变量”意味着“获取或修改词法环境的一个属性”。

> “词法环境”是一个规范对象（specification object）：它仅仅是存在于 [编程语言规范](https://tc39.es/ecma262/#sec-lexical-environments) 中的“理论上”存在的，用于描述事物如何运作的对象。我们无法在代码中获取该对象并直接对其进行操作。

- [闭包](https://en.wikipedia.org/wiki/Closure_(computer_programming)) 是指内部函数总是可以访问其所在的外部函数中声明的变量和参数，即使在其外部函数被返回（寿命终结）了之后。在某些编程语言中，这是不可能的，或者应该以特殊的方式编写函数来实现。

- > 在 JavaScript 中，所有函数都是天生闭包的（只有一个例外，将在 ["new Function" 语法](https://zh.javascript.info/new-function) 中讲到） 
  >
  > 原因 : 所有的函数在“诞生”时都会记住创建它们的词法环境。所有函数都有名为 `[[Environment]]` 的隐藏属性，该属性保存了对创建该函数的词法环境的引用。

- 垃圾收集 : 通常，函数调用完成后，会将词法环境和其中的所有变量从内存中删除。因为现在没有任何对它们的引用了。与 JavaScript 中的任何其他对象一样，词法环境仅在可达时才会被保留在内存中。

-  嵌套函数 =  null时, 才能完成真正的垃圾收集

- 理论上当函数可达时，它外部的所有变量也都将存在。

  但在实际中，JavaScript 引擎会试图优化它。它们会分析变量的使用情况，如果从代码中可以明显看出有未使用的外部变量，那么就会将其删除。

  > 在 V8（Chrome，Opera）中的一个重要的副作用是，此类变量在调试中将不可用。

  

##### let 和 var 区别: 

- 变量提升和函数提升

- > 函数表达式不存在提升 , let有提升但是有暂时锁区

- let 在创建时变量提升, 初始化时正常初始化, 有暂时锁区
- var 的变量提升,导致在声明之前都可以使用

> ```javascript
> 案例一: 
> let x="fef";
>  {
>    console.log(x); //x 显示错误
>    let x = "eew";
>    //在代码块中, x创建时变量提升, 所以log() 使用的是代码块中的x值.但是初始化并没有被提升, 所以初始化之前不能使用的x 报错
>  }
> 案例二:
> 
> for(var i =0;i<5;i++)
> {
>  element.onclick = function(){
>  console.log(i)  // 结果都会是 5 ,因为var不存在块作用域,循环中的i值 一直只有一个, 所以结果都是I的最终值
>  
>  }
>  //var 改为 let  可以解决,他会在每次循环重新声明初始化,保证每次循环只用当前的一个值
> }
>  
> ```
>
> 

- “var” 没有块级作用域,不是函数作用域就是全局作用域,例如if块中定义的变量,在外面还是能访问到
-   使用 var，我们可以重复声明一个变量，不管多少次都行
- “var” 声明的变量，可以在其声明语句前被使用,当函数开始的时候，就会处理 var 声明



- IIFE 函数书写   存在意义: 创建一个块级作用域进行使用,不会污染全局变量
- > jq 中开发插件也使用了IIFE ,为了继续使用$ 和 创建一个作用域

##### 全局对象(不是全局变量搞清楚)

- 全局对象提供可在任何地方使用的变量和函数,默认情况下，这些全局变量内置于语言或环境中,全局对象的所有属性都可以被直接访问
- >在浏览器中，它的名字是 “window”，对 Node.js 而言，它的名字是 “global”
- 在浏览器中，使用 var（而不是 let/const！）声明的全局函数和变量会成为全局对象的**属性**。使用 let，就不会发生这种情况.
- >如果一个值非常重要，以至于你想使它在全局范围内可用，那么可以直接将其作为属性写入：
  >
  >window.currentUser = {
  >  name: "John"
  >};
- >  我们使用全局对象来测试对现代语言功能的支持。
  >
  >  if (!window.Promise){...}   测试是否存在promise对象

#####  函数对象 
> 在 JavaScript 中，函数就是值。类型是一个对象, 我们可以把它当成一个对象使用, 
>
> - 函数名字通过属性name 访问获得  length返回函数需要参数的个数(rest 参数不参与计数)。
> -  我们也可以在函数内添加自定义属性,在函数外可以调用,改变它. 通过函数名.属性名添加, 创建只属于函数的属性


- 命名函数表达式 (NFE): 指带有函数名字的函数表达式 (函数声明没有这个东西)
- >let sayHi = function func(who) ,通过func可以在函数内调用自己 而且在函数外不可见你, 
- >为什么不使用嵌套调用的原因:为了避免在函数内嵌套调用时, 函数表达式在外部已被修改(引用其他函数..)
  >
  >比如: let sayhi =null 那么使用sayhi的嵌套调用就无效了


##### new Function 语法
- 创建函数体, 参数内接受函数参数 和函数体 ,应用在从服务器获取代码 ..复杂场景才会使用 , 创建的函数只能访问全局变量

##### 调度：setTimeout 和 setInterval

- 有时我们并不想立即执行一个函数，而是等待特定一段时间之后再执行。这就是所谓的“计划调用（scheduling a call）”
- 两种方法事件 : setTimeout 和 setInterval

- ```javascript
  let timerId = setTimeout(func|code, [delay], [arg1], [arg2], ...)
  func|code : 想要执行的函数或代码字符串。 一般传入的都是函数。
   由于某些历史原因，支持传入代码字符串，但是不建议这样做。尽量使用箭头函数代替它们,(JavaScript 会自动为字符代码块其创建一个函数) 
  arg1 ..要传入被执行函数（或代码字符串）的参数列表
  
  // 实例 : setTimeout(sayHi, 1000, "Hello", "John");
  ```
- setInterval 方法 setTimeout 的语法相同
- 使用clearTimeout() , clearInterval()  取消调度,在浏览器中，定时器标识符是一个数字。在其他环境中，可能是其他的东西。

> alert 弹窗显示的时候计时器依然在进行计时, 在大多数浏览器中，包括 Chrome 和 Firefox，在显示 `alert/confirm/prompt` 弹窗时，内部的定时器仍旧会继续“嘀嗒”。



- 周期性调度有两种方式 ： 一种是使用 setInterval，另外一种就是嵌套的 setTimeout 

- > 嵌套的 `setTimeout` 要比 `setInterval` 灵活得多,采用这种方式可以根据当前执行结果来调整下次调用的时间间隔

- > 使用 setInterval 时，func 函数的实际调用间隔要比代码中设定的时间间隔要短！ 因为其中设置的间隔时间包括函数执行的时间, 所以实际间隔要小
  >
  > 嵌套的 setTimeout 就能确保延时的固定（这里是 100 毫秒）。这是因为下一次调用是在前一次调用完成时再调度的。  嵌套调用时 函数名字不能再是 匿名函数..

- 垃圾回收 和 计时函数 : 对于 setInterval，传入的函数也是一直存在于内存中，直到 clearInterval 被调用。 如果函数还引用了一些外部变量,那么函数存在,变量也会随之存在(闭包), 会占用内存

  

- 这儿有一种特殊的用法：`setTimeout(func, 0)`，或者仅仅是 `setTimeout(func)`。这样调度可以让 `func` 尽快执行。但是首先执行的仍然是脚本中的其他表达式. 因为是异步操作

  > 在浏览器环境下，嵌套定时器的运行频率是受限制的。必须经过 4 毫秒以上的强制延时
##### 装饰者模式和转发，call/apply
- 装饰者模式 : 一个特殊的函数 :  它接受另一个函数并改变它的行为(参数是一个函数)。返回一个函数引用,用来覆盖被改变的函数

- > 用函数来装饰另一个函数
  >
  > 装饰者不适用于对象方法, 原因在于传递函数时,因为是对象方法,和对象分离之后,丢失this , 使用call() 解决,设定上下文


- ```javascript
  func.call(context, arg1, arg2, ...)
  提供的第一个参数作为 this，后面的作为参数.
  例如: 
   
  func(1, 2, 3);
  func.call(obj, 1, 2, 3)
  
  call 返回调用该函数的返回值  , bind 返回this绑定的函数
  call() 方法接受的是一个参数列表，而 apply() 方法接受的是一个包含多个参数的数组。
  
  ```

- ```javascript
  func.apply(context, args)
  context: this指向, args: 参数数组
  
  
  func.call(context, ...args); // 使用 spread 语法将数组作为列表传递
  func.apply(context, args);   // 与使用 call 相同
  ```

- 将所有参数连同上下文一起传递给另一个函数被称为“呼叫转移（call forwarding）”。

  

- 方法借用 :  [].join.call(arguments)  借用原始方法,给与上下文,它为什么有效？那是因为原生方法 `arr.join(glue)` 的内部算法非常简单。类数组就可以


> 通常，用装饰的函数替换一个函数或一个方法是安全的，除了一件小东西。如果原始函数有属性，例如 func.calledCount 或其他，则装饰后的函数将不再提供这些属性.
>
> 函数本质上是一个对象, 可以添加属于函数的属性

##### 函数绑定 

- 丢失"this" : 一旦方法被传递到与对象分开的某个地方 —— `this` 就丢失。

- > 当对象方法不是直接调用,就会丢失this
  >
  > 例如:  setTimeout(user.func,1000); 直接调用user,func() 就不会发生丢失; 
  >
  > 本质上第一种的调用方法是和对象分开的,当传递对象方法时,会直接传递该函数地址, 不会连同对象一起传输,this又是运行时才被计算出来,所以就会丢失
  >
  > 第一种写法相当于    let f  = user.func;  setTimeout(f,1000);

- ##### 解决方法 :

- 包装器 ,使用匿名函数/箭头函数, 在内部写调用对象方法

- > 传递时传递包装器,调用时对象方法会直接调用,就不会丢失

- > 包装器的缺点:
  >
  > 当对象方法在**包装器之前**更新,包装器中的对象方法就会更新,  
>
  > 如果利用bind进行绑定, 只要**在bind之后**再更改对象方法,那么都不会影响(已经绑定不会影响) ,调用的还是原来的对象方法.
  >
  > 本质区别在于 :  bind绑定后可以和调用分开. 包装器不能可以和调用分开

- bind 方法,用来绑定this

- > func.bind(对象),  返回一个类似于函数的“外来对象（exotic object）”，它可以像函数一样被调用
  >
  > ```javascript
  > let user = {
  >   firstName: "John",
  >   sayHi() {
  >     alert(`Hello, ${this.firstName}!`);
  >   }
  > };
  > let sayhi = user.sayhi.bind(user); //绑定user对象到this
  > sayhi() // 可以直接调用,不需要对象也可以
  > 
  > ```


- > func.bind()方法 返回的"类函数" ,参数列表和func一样,一样传递参数调用,只是this被绑定了
  >
  > bindAll : 对象有很多方法都需要绑定, 可以在一个循环中完成所有方法绑定

  

- 偏函数 :  不仅可以绑定this ,还可以绑定参数

- > ```javascript
  > let bound = func.bind(context, [arg1], [arg2], ...);
  > 当this 不需要绑定时 传递 null给conttext
  > ```

- > 通过绑定一个函数的一些参数来创建一个新函数
  >
  > 当我们有一个非常通用的函数，并希望有一个通用型更低的该函数的变体时，偏函数会非常有用

- 想直接绑定参数, 忽略this 绑定参数, 原生bind方法不支持, 使用js 编写partial(偏函数)

- > ```
  > partial(func[, arg1, arg2...])` 调用的结果是一个包装器 
  > 
  > ```

  

#### 对象属性配置

##### 属性标志和属性描述符

- 对象可以存储属性。到目前为止，属性对我们来说只是一个简单的“键值”对。但对象属性实际上是更灵活且更强大的东西。


- 对象属性（properties），除 value 外还有特殊的特性 

- > 属性标志 : 
  >
  > writable  修改权限   如果为 `true`，则值可以被修改
  >
  >  enumerable 循环是否列出  如果为 `true`，则会被在循环中列出
  >
  >  configurable   如果为 `true`,此特性可以被删除，这些属性也可以被修改  否则不行

- > 它们通常不会出现。当我们用“常用的方式”创建一个属性时，它们都为 true。但我们也可以随时更改它们。

  ##### 设置/访问标志

  ```
  Object.getOwnPropertyDescriptor(obj, "propertyName");  
  方法允许查询有关属性的完整信息。
  obj 查询的对象   properName 属性名称
  返回值是一个所谓的“属性描述符”对象：它包含值和所有的标志
  
  Object.defineProperty(obj, propertyName, descriptor)
   descriptor : 要应用的属性描述符对象。
   
  如果该属性(propertyName)存在，defineProperty 会更新其标志。其余标志不变
  属性不存在时,创建该属性使用提供标志更新, 未提供的标志默认为false
  
  方法都可以在类/ 构造函数中使用, 对象名改为this
  ```

- > writable : false;  值不可改变, 在非严格模式下不会出现Error
  >
  > 如果还想要修改值, 只能使用defineproperty() 设置value属性覆盖,进行更改
- >  enumberable   :false  使其不可枚举
  >
  >  在console.log(obj) , 不会显示该属性, for..in 中不会枚举出来;
  >
  >  例如: 对象的toString()函数, 不会显示出来  


- > configurable:false  不可配置,
  >
  >  我们无法使用 `defineProperty` 把它改回去
  >
  > 定义过其他属性值后, 就无法更改属性标志或删除属性标志
  
  

- [Object.defineProperties(obj, descriptors)](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperties)，允许一次定义多个属性


- [Object.getOwnPropertyDescriptors(obj)](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Object/getOwnPropertyDescriptors)  方法。一次获取所有属性描述符，


- > for..in 会忽略 symbol 类型的属性，但是 Object.getOwnPropertyDescriptors 返回包含 symbol 类型的属性在内的所有属性描述符。

- > 复制属性标志:  let clone = Object.defineProperties({},Object.getOwnPropertyDescriptors(obj)); 
  >
  > 拷贝对象 一般我们使用赋值的方式复制属性,但是我们不能复制标志,而上式解决了这个问题

  - 还有一些限制访问 **整个** 对象的方法

##### 属性的 getter 和 setter
- 有两种类型的对象属性 : 1. 数据属性 2.  访问器属性
  
- > 访问器属性 : 本质上是用于获取和设置值的函数
  
- ```
  
  
  第一种设置方式 (对象内使用, 类中constructor 外使用): 
  let user = {
    name: "John",
    surname: "Smith",
  
    get fullName() {
      return `${this.name} ${this.surname}`;
    }
  };
  
  第二种设置方式(对象外, 构造函数内, 类中constructor使用):   
  在构造函数 / 类中constructor使用,对象名改为this,
  
  Object.defineProperty(user, 'fullName', {
  get() {
  return `${this.name} ${this.surname}`;
  },
  set(value) {
  [this.name, this.surname] = value.split(" ");
  }
  });
  
  
  一个属性要么是访问器（具有 get/set 方法），要么是数据属性（具有 value），但不能两者都是
  Object.defineProperty({}, 'prop', {
    get() {
      return 1
    },
  
    value: 2
  });
  ```
  
- >访问器属性的设计思想 : 我们不以函数的方式调用 user.fullName，我们正常读取它： getter 在幕后运行
  >
  >相当于我们有了一个虚拟的属性
  
- > 访问器属性名字 不能和对象数据属性名字一样,否则会被覆盖

  

- 访问器属性的描述符与数据属性的不同,   对于访问器属性,没有value 和 writable，**enumerable  configurable** 与数据属性的相同 

  

- Getter/setter用途 : 

- 1. 可以用作“真实”属性值的包装器，以便对它们进行更多的控制。例如限制属性值的长度 

-    2. 它们允许随时通过使用 getter 和 setter 替换“正常的”数据属性

        > 通过设置getter和 "旧的"数据属性相同的名字 , 来替换旧的属性

#### 原型,继承

##### 原型继承

- 原型继承解决,在原来的对象基础上**扩展构建**新的对象

- > 因为构造函数只能是重复创建类似功能的对象,无法通过其进行扩展

- 在 JavaScript 中，对象有一个特殊的隐藏属性 `[[Prototype]]`，它要么为 `null`，要么就是对另一个对象的引用。被引用的对象被称为“原型”

- 属性 `[[Prototype]]` 是内部的而且是隐藏的,设置它的方式 :

- > 使用特殊的名字 `__proto__` .   可以获取和设置 ,每边是两个下划线
  >
  > `__proto__` 的存在是历史的原因。在现代编程语言中，将其替换为函数 `Object.getPrototypeOf/Object.setPrototypeOf` 也能 get/set 原型

  

  

- 原型链中引用不能形成闭环, --proto-- 的值可以是对象，也可以是 null,其他类型被忽略, 只能有一个 `[[Prototype]]`。一个对象不能从其他两个对象获得继承。( 单继承)

- >  --proto-- 是 [[Prototype]] 的因历史原因而留下来的 getter/setter

  

- this 不受原型的影响,在一个对象还是在原型中。在一个方法调用中，this 始终是点符号 . 前面的对象


- for..in 循环也会迭代继承的属性。想要排除继承的属性  obj.hasOwnProperty(key) 如果本身存在(不是继承的属性)key则返回true  如果是继承来的 返回false

- > hasOwnProperty()  很严格 , 对于对象的构造函数prototype中的属性,都算是继承属性
  >
  > 还有 ....OwnProperty() 等函数 ,返回非继承的属性

- > `for..in` 只会列出可枚举的属性, `enumerable:false` 标志导致不可枚举

  > 几乎所有其他键/值获取方法，例如 `Object.keys` 和 `Object.values` 等，都会忽略继承的属性。
  >
  > 它们只会对对象自身进行操作。**不考虑** 继承自原型的属性。

- 

##### Func.prototype 

- JavaScript 从一开始就有了原型继承,但是在过去，没有直接对其进行访问的方式。唯一可靠的方法是构造函数的 `"prototype"` 属性。目前仍有许多脚本仍在使用它。

- >  `F.prototype` 指的是 `F` 的一个名为 `"prototype"` 的常规属性
  >
  > 不要和原型的 prototype 混淆, 原型的是引用

> F.prototype 属性**仅在** new F 被调用时使用，它为新对象的 [[Prototype]] 值。
>
> 如果在创建之后，F.prototype 属性赋了其他值, 那么通过 new F 创建的新对象也将改变 [[Prototype]]，但已经存在的对象将保持旧有的值。



- 每个函数都有 "prototype" 属性，即使我们没有提供它。 默认的 "prototype" 是一个**只有属性 constructor 的对象**，属性 constructor 指向函数自身。

- ```javascript
  function Rabbit() {}
  
  /* default prototype
  Rabbit.prototype = { constructor: Rabbit };
  */
  如果我们什么都不做 , constructor 属性可以通过 [[Prototype]] 给所有 rabbits 使用 , rabbits的[[Prototype]] 引用默认的对象
  ```

> 有一点需要注意,对象不存在prototype属性,只有构造函数 类 存在prototype属性,  对象只有[[prototype]];

- > 我们可以使用 `constructor` 属性来创建一个新对象，该对象使用与现有对象相同的构造器:
  >
  > new rabbit.constructor("Black Rabbit");    // 两者相等
  >
  >  new Rabbit("White Rabbit");
  >
  > 
  >
  > 当我们有一个对象，但不知道它使用了哪个构造器（例如它来自第三方库），并且我们需要创建另一个类似的对象时，用这种方法就很方便。

  

- JavaScript 自身并不能确保正确的 "constructor" 函数值。 它存在于函数的默认 `"prototype"` 中,它存在于函数的默认 `"prototype"` 中

- 为了确保正确的 "constructor"，我们可以选择添加/删除属性到默认 "prototype"，而不是将其整个覆盖. 也可以手动重新创建 `constructor` 属性

- > ```javascript
  > function Rabbit() {}
  > Rabbit.prototype.jumps = true
  > ```

- > F.prototype =的值要么是一个对象，要么就是 null：其他值都不起作用。 

  > 注意一点:  更改prototype中的属性,和对prototype赋值的区别
  >
  > ```
  > Rabbit.prototype.jumps = true  这种是改变prototype属性指向对象中的属性
  > Rabbit.prototype = {} 或者对象名, 这是改变构造函数创建对象后[[prototype]]指向的对象
  > 
  > 两者要分清楚区别, 一个是改变[[prototype]]  指向,一个仅仅改变其中的属性
  > 
  > 
  > 
  > 还要清楚一点: Rabbit.prototype = animal;   是指向animal对象
  >  Rabbit.prototype = Animal .prototype; 是指向  animal构造函数的prototype
  >  (类似于原生的原型 ,内建对象都是指向自己构造函数的prototype, 而大部分的构造函数 prototype都指向Object()的prototype)
  > ```
  >
  > 

##### 原生的原型

-  `"prototype"` 属性在 JavaScript 中所有的内置构造函数都用到了它.

- > 通过对内建对象的构造函数 的prototype属性, 进行添加功能

-  通过构造函数new对象时, 对象的[[prototype]]默认指向构造函数的Prototype

- 内建对象，像 Array、Date、Function 及其他，都在 prototype 上挂载了方法

- 数据类型对象 也有相应的prototype,但是null undefined没有对象包装器所以不存在 prototype

- > 这里的 prototype  都是指Array() ...构造函数的属性, 属性指向一个大对象,对象内有很多方法   
  >
  > Array.prototype = { ...//方法};

- >按照规范，所有的内建原型顶端都是 `Object.prototype`。这就是为什么有人说“一切都从对象继承而来”。
  >
  >即使是函数 —— 它们是内建构造器 `Function` 的对象，并且它们的方法（`call`/`apply` 及其他）都取自 `Function.prototype`。函数也有自己的 `toString` 方法。

> 原生的原型是可以被修改的,通常是一个很不好的想法。原型是全局的,如果两个库都添加了相同的方法, 所以很容易造成冲突。
>
> 在现代编程中，只有一种情况下允许修改原生原型。那就是 polyfilling:

- > **polyfilling**: 表示某个方法在 JavaScript 规范中已存在，但是特定的 JavaScript 引擎尚不支持该方法，那么我们可以通过手动实现它，并用以填充内建原型。

- ##### 原型方法借用:

- > 从原型中借用方法 ,例如创建类数组,我们向其复制Array的原型方法,
  >
  > ```javascript
  > 1. obj.join = Array.prototype.join;   通过赋值进行借用
  > 
  > 2. obj.__proto__  = Array.prototype ; 通过设置__proto__ , 来进行借用方法, 但是js不支持多继承,如果obj已经继承了其他对象, 该方法不可用
  > 
  > ```

  > call /apply 方法借用指的是 从一个对象获取一个方法，并将其复制到另一个对象

> 所有的内建对象都遵循相同的模式（pattern）：
>
> - 方法都存储在 prototype 中（`Array.prototype`、`Object.prototype`、`Date.prototype` 等）。
> - 对象本身只存储数据（数组元素、对象属性、日期）。

##### 原型方法
-  __proto__ 被认为是过时且不推荐使用的，这里的不推荐使用是指 JavaScript 规范中规定，proto 必须仅在浏览器环境下才能得到支持。(实际上服务器端也支持)

>-  现代的方法 : Object.getPrototypeOf(obj)  返回对象 obj 的 [[Prototype]]。
>-  Object.create(proto, [descriptors]) ,创建一个空的新对象, 新对象的[[Prototype]指向proto对象  ,[descriptors] : 可选的属性描述  ,可以使用属性描述给新对象提供额外的属性,描述器的格式与 [属性标志和属性描述符](https://zh.javascript.info/property-descriptors) 一章中所讲的一样
>-  Object.setPrototypeOf(obj, proto)  将对象 obj 的 [[Prototype]] 设置为 proto

> 我们可以使用 `Object.create` 来实现比复制 `for..in` 循环中的属性更强大的对象克隆方式: 包括所有的属性：可枚举和不可枚举的，数据属性和 setters/getters

```
let clone = Object.create(Object.getPrototypeOf(obj), Object.getOwnPropertyDescriptors(obj));
实现一个对象的完全复制 , 完成深度拷贝
```



##### 原型简史

> 如果速度很重要，就请不要修改已存在的对象的 [[Prototype]] , 通常我们只在创建对象的时候设置它一次，自那之后不再修改, JavaScript 引擎对此进行了高度优化。用 Object.setPrototypeOf 或 obj.__proto__= “即时”更改原型是一个非常缓慢的操作，因为它破坏了对象属性访问操作的内部优化。

- 对象可以用作关联数组（associative arrays）来存储键/值对。,当用户存储---prototype-- ,不会存储值,他只能是null  对象

- `__proto__` 不是一个对象的属性，只是 `Object.prototype` 的访问器属性,

- > `__proto__` 是一种访问 `[[Prototype]]` 的方式，而不是 `[[prototype]]` 本身。
  >
  > 如果 `obj.__proto__` 被读取或者赋值，那么对应的 getter/setter 会被从它的原型中调用，它会 set/get `[[Prototype]]`。

- 当一个对象不继承 Object 对象时, --Prototype-- 就不会当成访问器了,我们可以把这样的对象称为 “very plain” 或 “pure dictionary” 对象,缺点是没有内建对象的函数

  

#### 类

##### class基本语法

```
class MyClass {
  // class 方法
  constructor() {...}  //new来新建对象时会自动调用该函数,初始化对象
  method1() { ... }
  method2() { ... }
  ...
}
```

> 类的方法之间没有逗号

- 在 JavaScript 中，类是一种函数。`class User {...}` 构造实际上做了如下的事儿：

- ```javascript
  class User {
    constructor(name) { this.name = name; }
    sayHi() { alert(this.name); }
  }
  ```

- > 创建一个名为 `User` 的函数，该函数成为类声明的结果。该函数的代码来自于 `constructor` 方法
  >
  > 存储类中的方法给 `User.prototype` ,  `constructor` 中的方法不会存储在prototype中,而是在每个对象中




- 使用构造函数,构造函数本身只存储数据,方法则放在函数的prototype 中, class则省略了这个过程
  
  > 人们常说 `class` 是一个语法糖（旨在使内容更易阅读，但不引入任何新内容的语法）,就是来源此
  >
  > 但是class 不仅仅是语法糖 :
  >
  > 1.使用class 创建的函数具有特殊的内部属性标记,[[FunctionKind]]:"classConstructor",  会检查该属性,与普通函数不同，必须使用 `new` 来调用它：
  >
  > 2.类方法不可枚举  ,类定义将 `"prototype"` 中的所有方法的 `enumerable` 标志设置为 `false`
  >
  > 3.类总是使用 `use strict`。 在类构造中的所有代码都将自动进入严格模式。

  

- 类表达式 : 就像函数一样，类可以在另外一个表达式中被定义，被传递，被返回，被赋值等。

- >let User = class{}   类似于命名函数表达式（NamedFunctionExpressions）
  >
  >类表达式可能也应该有一个名字。
  >
  >let User = class MyClass {}  这个类名只在类内可见

  

- 类中可以也存在 getter setter  计算属性等

- > 也可以通过在 F.prototype中创建getter和setter来起作用。

- 类字段 : 是一种允许直接在类中添加任何属性的语法,不再通过constructor()

- > 效果和在constructor()中声明一样
  >
  > class字段 :旧的浏览器可能需要 polyfill , 类字段（field）是最近才添加到语言中的。之前，我们的类仅具有方法

  

- 传递对象方法时,会出现丢失this的现象, 类字段提供了另一种非常优雅的语法：

- > 普通字段解决方法: 
  >
  > 1.在类中 constructor 中可以使用包装器
  >
  > 2.在类中 constructor 中绑定 bind
  >
  > ```javascript
  > class Logger {
  > constructor() {
  > this.printName = this.printName.bind(this);
  > this.getThis = () => { 内容};
  > }
  > //..定义printName(); 
  > ```
> }
>
> ```
> 
> ```

  > 类字段解决方法 :

  ```javascript
  
  class Button {
    constructor(value) {
      this.value = value;
    }
    click = () => {
      alert(this.value);
    }
  }
  //每一个对象都有独立的click方法,内部都有指向该对象的this,  传递到哪里都不会丢失this
  
  ```

##### 类继承
- 类继承是一个类扩展另一个类的一种方式。因此，我们可以在现有功能之上创建新功能。

- 关键字 extends 使用了很好的旧的原型机制进行工作,通过设置[[prototype]] 来进行继承操作 :

- > `Rabbit extends Animal` 创建了两个 `[[Prototype]]` 引用：
  >
  > 1. `Rabbit` 函数原型继承自 `Animal` 函数。 解释了静态属性为什么会被继承
  >
  > 2. `Rabbit.prototype` 原型继承自 `Animal.prototype`。
  >
  >    -----来源于 (静态属性章节)

  

  >类中继承时, constructor()也会被继承,其中定义的变量 ,派生类和基类都会各自维护一份
>
  >对于类字段 来说 ,相当于在constructor( ) 中定义变量一样,会被复制
  >

  

  > prototype属性里面一般只维护函数
  >
  > 类语法不仅允许指定一个类，在 extends 后可以指定任意表达式。

  

  ##### 重写方法

- 不希望完全替换父类方法，希望在父类方法的基础上进行调整或扩展其功能,Class 为此提供了 "super" 关键字。 

- > 1. 执行super.method(...) 来调用一个父类方法 
  > 2. 执行 super(...) 来调用一个父类 constructor（只能在我们的 constructor 中）

- > 箭头函数没有 super,在类中如果被访问，它会从外部函数获取,但是如果是普通函数调用, 将会抛出错误

  ##### 重写constructor

- 未提供constructor 时,默认生成:

  ```
  constructor(...args) 
  { super(...args); }
  ```

- 继承类提供constructor()必须调用 super(...),**并且 (!) 一定要在使用 `this` 之前调用。**

- > 继承类的构造函数:派生构造器具有特殊的内部属性 `[[ConstructorKind]]:"derived"`。该标签会影响它的 `new` 行为:
  >
  > 当通过 `new` 执行一个常规函数时，它将创建一个空对象，并将这个空对象赋值给 `this`。
  >
  > 当继承的 constructor 执行时，**它不会执行新建空对象并把this传给他的操作**。它期望父类的 constructor 来完成这项工作。

- 当我们在构造器(constructor)中访问一个被重写的class字段时, **父类构造器总是会使用它自己字段的值，而不是被重写的那一个。** 

- > 如果是函数,则是重写这个

  > 原因 : 在于类字段初始化的顺序: 
  >
  > 1.对于基类（还未继承任何东西的那种），在构造函数调用前初始化。
  >
  > 2.对于派生类，在 `super()` 后立刻初始化
  >
  > 如果出问题了，我们可以通过使用方法或者 getter/setter 替代类字段，来修复这个问题

##### 静态属性和静态方法

- 我们可以把一个方法赋值给类的函数本身，而不是赋给它的 "prototype"。这样的方法被称为 静态的（static） 以 static 关键字开头

- ```
  class User {
    static staticMethod() {
      alert(this === User); //this是类构造器 User 自身
    }
  }
  
  User.staticMethod = function() {
    alert(this === User);
  };   
  //在外面作为类的属性赋值作用相同
  ```

- > 从技术上讲，静态声明与直接给类本身赋值相同
  >
  > 静态属性可能不被支持,最新语法

- 静态属性和方法是可被继承的。详见继承过程

- > 类的构造函数 也会通过[[prototype]] 链接起来,所以调用静态属性也会通过链进行查找     静态方法也可以

##### 私有 保护属性

- 在 JavaScript 中, 有两种类型的对象字段（属性和方法） : 公共 私有 (**js语言实际实现的**)

- 受保护的字段不是在语言级别的 Javascript 中实现的，但实际上它们非常方便，因为它们是在 Javascript 中**模拟的类定义语法**。

- >受保护的属性通常以下划线 _ 作为前缀。这不是在语言级别强制实施的，但是程序员之间有一个众所周知的约定，即不应该从外部访问此类型的属性和方法。
  >
  >受保护的字段是可以被继承的

- 只读属性 : 只需要设置 getter，而不设置 setter：

- 私有属性和方法应该以 # 开头。它们只在类的内部可被访问。我们无法从外部或从继承的类中访问它。

- > getter/setter 访问器 可以让外部访问到私有属性

- >  JavaScript引擎不支持或部分支持，需要进行填充。

- 私有字段与公共字段不会发生冲突。我们可以同时拥有私有的 #waterAmount 和公共的 waterAmount 字段。

- >私有字段不能通过 this['#name'] 访问

##### 扩展内建类
- 内建的类，例如 Array，Map 等也都是可以扩展的(**可被继承**)

- > 继承自原生 `Array` 的类 `PowerArray`
  >
  > ```
  > class PowerArray extends Array {
  > isEmpty() {
  >  return this.length === 0;
  > }
  > ```
> }
>
>
> ```
> 
> 内建的方法例如 `filter`，`map` 等 — 返回的正是子类 `PowerArray` 的新对象,原因: 内部使用了对象的 `constructor` 属性来实现这一功能
> 
> 
> 
> 优点: 返回结果可以继续调用`PowerArray` 的方法
> 
> 我们也可以使用特殊的静态 getter `Symbol.species`使结果放回一个Array
> ```

- 通常，当一个类扩展另一个类时，静态方法和非静态方法都会被继承,但内建类却是一个例外。它们相互间不继承静态方法。

- > 原因 :  构造函数不会[[prototype]]指向另一个构造函数

##### 类检查

- **instanceof 操作符** 用于检查一个对象是否属于某个特定的 class。同时，它还考虑了继承。

- >  还可以检查是否对象是或属于特定的构造函数
  >
  >  算法的执行过程大致如下 :
  >
  >  1. 如果这儿有静态方法 `Symbol.hasInstance`，那就直接调用这个方法
  >  2. 大多数 class 没有 `Symbol.hasInstance`,使用 `obj instanceOf Class` 检查 `Class.prototype` 是否等于 `obj` 的原型链中的原型之一
  >
  >  > ```javascript
  >  > obj.__proto__.__proto__.__proto__ === Class.prototype
  >  > ```

  

  > **prototypeObj.isPrototypeOf(object)** 方法 , 在该对象object的原型链上搜寻,有的话返回true
  >
  > > 例子 : Foo.prototype.isPrototypeOf(baz);

  

- 使用 Object.prototype.toString 方法来揭示类型 ,内建的 toString 方法可以被从对象中提取出来，并在任何其他值的上下文中执行

- 可以使用特殊的对象属性 Symbol.toStringTag 自定义对象的 toString 方法的行为。

- 如果我们想要获取内建对象的类型，并希望把该信息以字符串的形式返回，而不只是检查类型的话，我们可以用 {}.toString.call 替代 instanceof

##### Mixin 模式
- 在 JavaScript 中，我们只能继承单个对象。每个对象只能有一个 [[Prototype]]。并且每个类只可以扩展另外一个类。但是有些时候这种设定（译注：单继承）会让人感到受限制。

- mixin 是一个包含其他类的方法的类,不需要继承

- > 方法一 : 构造一个 mixin 最简单的方式就是构造一个拥有实用方法的对象,通过拷贝对象完成

  

#### 错误处理

##### try..catch

- try..catch，它使我们可以“捕获（catch）”错误，因此脚本可以执行更合理的操作，而不是死掉

- > 要使得 try..catch 能工作，代码必须是可执行的。对于代码中的语法错误等导致无法正常工作,
  >
  > try..catch 只能处理有效代码中出现的错误。这类错误被称为“运行时的错误（runtime errors）”，有时被称为“异常（exceptions）”。

- 为了捕获到计划的（scheduled）函数中的异常，那么 try..catch 必须在这个函数内 例如:setTimeout()

- >原因在于计划要执行的函数，该函数本身要稍后才执行，这时引擎已经离开了 try..catch 结构

- 发生错误时，JavaScript 生成一个包含有关其详细信息的error 对象。然后将该对象作为参数传递给 catch, 

- > error 对象具有两个主要属性： 
  >
  > name :  Error名称    
  >
  > message   : 关于 error 的详细文字描述。 人类可读的 error 信息  
  >
  >  stack : 当前的调用栈

- 如果我们不需要 error 的详细信息，catch 也可以忽略它：(新属性 可能有兼容性)  catch不加括号即可



- `throw` 操作符 :

- > 为了统一进行 error 处理，我们将使用 `throw` 操作符。
  >
  > 通过 throw 关键字 , 可以抛出自定义错误 error对象

  


- **自定义 error**   : 技术上讲，我们可以将任何东西用作 error 对象。  但最好使用对象，最好使用具有 name 和 message 属性的对象, 

- > JavaScript 中有很多内建的标准 error 的构造器：Error，SyntaxError，ReferenceError，TypeError 等
  >
  > 对于内建的 error,`name` 属性刚好就是构造器的名字。`message` 则来自于参数（argument）。
  >
  > ```javascript
  > let error = new SyntaxError(message);
  > let error = new ReferenceError(message); // 通过内建构造器自定义error对象
  > 
  > let error = new Error("Things happen o_O");
  > 
  > alert(error.name); // Error
  > alert(error.message); // Things happen o_O
  > ```






- 为什么**重新抛出 error**

- > 当我们使用`try..catch` 旨在捕获“数据不正确”的 error, 出现了编程错误意外,这时依然会被catch捕捉, 并显示数据不正确的错误
  >
  > 这是不正确的, 为了避免这类问题, 我们采用重新"抛出", **`catch` 应该只处理它知道的 error，并“抛出”所有其他 error。**留给其他catch处理,  
  >
  > 具体解释:
  >
  > 1. Catch 捕获所有 error。
  > 2. 在 `catch(err) {...}` 块中，我们对 error 对象 `err` 进行分析。我们可以使用 `instanceof` 操作符判断错误类型,还可以使用err.name 获取错误的类名
  > 3. 如果我们不知道如何处理它，那我们就 `throw err`。
>
  > 使用err.name类名 还是 instanceof 分析error对象, 尽量使用instanceof,`instanceof` 检查对于新的继承类也适用。所以这是面向未来的做法。

  

- **try…catch…finally 语句**:

- >  finally 子句（clause）通常用在：当我们开始做某事的时候，希望无论出现什么情况都要完成完成某个任务。  try存在显式的return   throw 也还是会执行finally 

  **全局错误处理**

  >设想一下，在 `try..catch` 结构外有一个致命的 error，然后脚本死亡了。这个 error 就像编程错误或其他可怕的事儿那样。我们可能想要记录这个 error，并向用户显示某些内容
  >
  >这时需要全局错误处理程序,
  >
  >全局错误处理程序 window.onerror 的作用通常不是恢复脚本的执行 — 如果发生编程错误，那这几乎是不可能的，它的作用是将错误信息发送给开发者。也有针对这种情况提供错误日志的 Web 服务

  

##### 自定义 Error，扩展 Error

- 当我们在开发某些东西时，经常会需要我们自己的 error 类来反映在我们的任务中可能出错的特定任务.,这些类需要支持基本的error基本属性 : name,message ,还可能有属于它们自己的属性, 所以我们需要扩展ERROR

- >JavaScript 允许将 `throw` 与任何参数一起使用，所以从技术上讲，我们自定义的 error 不需要从 `Error` 中继承。但是，如果我们继承，那么就可以使用 `obj instanceof Error` 来识别 error 对象。因此，最好继承它。

- 包装异常 : 多种error 会发生,但是没必要检查所有的error类型,新建一个抽象的erroer类型,用来表示一类的error , 如果需要更多 error 细节，那么可以检查 抽象异常类的 cause 属性


#### Promise

##### 回调
- 所以回调与同步、异步并没有直接的联系，回调只是一种实现方式，既可以有同步回调，也可以有异步回调，还可以有事件处理回调和延迟函数回调，这些在我们工作中有很多的使用场景
- > 异步行为: 我们现在开始执行的行为,  但它们会在稍后完成。

- “基于回调”的异步编程风格。异步执行某项功能的函数应该提供一个 callback 参数用于在相应事件完成时调用。
- 厄运金字塔 :“基于回调”的异步编程如果有过度的嵌套和回调使得结构混乱, 所以是用 **promisde** 解决.

##### promise操作

- promise 构造语法 :

  ```
  let promise = new Promise(function(resolve, reject)
  {
  //executor
  }
  ```

  

- > 当 new Promise 被创建，executor 会自动运行(我们的代码仅在 executor 的内部),它的参数 resolve 和 reject 是由 JavaScript 自身提供的回调
  >
  > 运行结束后，如果成功则调用 resolve(value)，如果出现 error 则调用 reject。 reject(error);
  >
  > 
  >
  > executor 中只能调用一个 resolve **或**一个 reject。任何状态的更改都是最终的。所有其他的再对 resolve 和 reject 的调用都会被忽略
  >
  > 并且，`resolve/reject` 只需要一个参数（或不包含任何参数），并且将忽略额外的参数

- > executor 通常是异步执行某些操作  ,但是Resolve/reject 是可以立即进行的

  
  
- 由 new Promise 构造器返回的 promise 对象具有以下内部属性：

      state — 最初是 "pending"，然后在 resolve 被调用时变为 "fulfilled"，或者在 reject 被调用时变为 "rejected"。
      result —最初是 undefined，然后在 resolve(value) 被调用时变为 value，或者在 reject(error) 被调用时变为 error。(待考证..)
      一个 resolved 或 rejected 的 promise 都会被称为 “settled”。

- Promise 对象的 state 和 result 属性都是内部的。我们无法直接访问它们。但我们可以对它们使用 .then/.catch/.finally 方法

  


- then 语法 :  
``` 
  promise.then(
  function(result) { /* handle a successful result */ },
  function(error) { /* handle an error */ }
);
如果我们只对成功完成的情况感兴趣，那么我们可以只为 .then 提供一个函数参数,如果我们只对 error 感兴趣，那么我们可以使用 null 作为第一个参数
```
- > .catch(f)  语法  :    是.then(null, f) 的完全的模拟，它只是一个简写形式。 只捕获错误情况,

- > .finally(f)  ，f 总是在 promise 被 settled 时运行：即 promise 被 resolve 或 reject。
  >
  > 在 finally 中，我们不知道 promise 是否成功,finally 处理程序将结果和 error 传递给下一个处理程序。 f:处理程序没有参数

- 如果 promise 为 pending 状态，**.then/catch/finally 处理程序（handler）将等待它**。否则，如果 promise 已经是 settled 状态，它们就会立即执行：

  


##### promise链 :用来连续的处理回调函数, 

- **将多个 `.then` 添加到一个 promise 上。但这并不是 promise 链**,这是promise  的几个处理程序

- .then中所使用的处理程序创建并返回一个 promise。在这种情况下，其他的处理程序（handler）将等待它 settled 后再获得其结果,这样可以构建一条promise链

- > 当then() 函数中**不创建并返回**一个promise时, 而是return 一个值,会直接进行下一轮then()的调用,不会等待其中的异步调用
  >
  > then() 函数处理完后,默认返回一个成功处理的promise,

  ```
  let p1 =new  Promise((resolve,reject)=> {
         reject("rejiec");
     });
     p1.then(
         value =>{console.log(value); },
         reason => {console.log(reason)}
          
      
     ).then(
         a => console.log("asdf"), // 第一个then 不管进行那个处理,都会进行这一行操作
         b => console.log(b)
     );
  
  ```

  



- > 确切地说，处理程序（handler）返回的不完全是一个 promise，而是返回的被称为 “thenable” 对象 — 一个具有方法 .then 的任意对象。它会被当做一个 promise 来对待。这个特性允许我们将自定义的对象与 promise 链集成在一起，而不必继承自 Promise。
  
- promise链是向下增长 , 嵌套式的promise调用是向右增长

- then()  函数的功能在于,等promise处理完后完成回调函数的功能


##### promise 处理错误  
- >  捕获所有 error 的最简单的方法是，将 .catch 附加到链的末尾

- Promise 的执行者（executor）和 promise 的处理程序（handler）周围有一个“隐式的 try..catch”。如果发生异常，它就会被捕获，并被视为 rejection 进行处理。使用catch就可以捕获上述的异常

  ```
  reject(new Error("Whoops!"));  
  throw new Error("Whoops!"); 效果是一样的
  ```

  

- 在常规的 try..catch 中，如果我们无法处理它，可以将其再次抛出。对于 promise 来说，这也是可以的。如果我们在 .catch 中 throw error，那么它会忽略其他的处理程序跳转到下一个catch。如果我们处理该 error 并正常完成，那么它将继续到最近的成功的 .then 处理程序（handler）。

- 当一个 error 没有被处理会发生什么？例如，我们忘了在链的尾端附加 .catch, 如果出现 error，promise 的状态将变为 “rejected”,   然后执行应该跳转至最近的 rejection 处理程序, 如果没有, JavaScript 引擎会跟踪此类 rejection，在这种情况下会生成一个全局的 error 

- > 在浏览器中，我们可以使用 unhandledrejection 事件来捕获这类 error, 这个事件对象有两个特殊的属性
  >
  > 在任何情况下我们都应该有 `unhandledrejection` 事件处理程序（用于浏览器，以及其他环境的模拟），以跟踪未处理的 error 并告知用户（可能还有我们的服务器）有关信息，以使我们的应用程序永远不会“死掉”。

  ```
  window.addEventListener('unhandledrejection', function(event) {
    alert(event.promise); // [object Promise] - 生成该全局 error 的 promise
    alert(event.reason); // Error: Whoops! - 未处理的 error 对象
  
  }
  ```

  


##### promise API
- 在 Promise 类中，有 5 种静态方法。

- >  all allSettled  race  resolve/reject 

  ```javascript
  1. Promise.all([
    new Promise(resolve => setTimeout(() => resolve(1), 3000)), // 1
    new Promise(resolve => setTimeout(() => resolve(2), 2000)), // 2
    new Promise(resolve => setTimeout(() => resolve(3), 1000))  // 3
  ]).then(alert); // 1,2,3 当上面这些 promise 准备好时：每个 promise 都贡献了数组中的一个元素
  
  如果任意一个 promise 被 reject，由 Promise.all 返回的 promise 就会立即 reject，并且带有的就是这个 error。
  完全忽略列表中其他的 promise。它们的结果也被忽略。
  
  
  2.我们想要获取（fetch）多个用户的信息。即使其中一个请求失败，我们仍然对其他的感兴趣。
  让我们使用 Promise.allSettled：
  
  3. 与 Promise.all 类似，但只等待第一个 settled 的 promise 并获取其结果（或 error）
  第一个 settled 的 promise “赢得了比赛”之后，所有进一步的 result/error 都会被忽略。
  
  4.Promise.resolve(value) 用结果 value 创建一个 resolved 的 promise。 Promise.reject(error) 用 error 创建一个 rejected 的 promise。 当一个函数被期望返回一个 promise 时，这个方法用于兼容性 ,async/await 语法已经替代
  
  
  ```

  ##### Promisification

- “Promisification”   它指将一个接受回调的函数转换为一个返回 promise 的函数。

- (有待扩充)
##### 微任务
- Promise 的处理程序（handlers）.then、.catch 和 .finally 都是异步的。

- > 即便一个 promise 立即被 resolve，
  >
  > .then、.catch 和 .finally  函数,还是会在别的语句执行完之后 执行 ,原因在于微任务队列的规定
  >
  > 如果想要改变这种顺序,把语句加入then() 函数中

- > 异步任务需要适当的管理。为此，ECMA 标准规定了一个内部队列 PromiseJobs，通常被称为“微任务队列（microtask queue）”（ES8 术语）。
  >
  > 队列规范:
  >
  > 队列（queue）是先进先出的：首先进入队列的任务会首先运行。
  > 只有在 JavaScript 引擎中没有其它任务在运行时，才开始执行任务队列中的任务

- 未处理的 rejection : 如果一个 promise 的 error 未被在微任务队列的末尾进行处理，则会出现“未处理的 rejection”。如果我们忘记添加 .catch，那么，微任务队列清空后，JavaScript 引擎会触发unhandledrejection 事件, 

##### Async/await
- Async/await 是以更舒适的方式使用 promise 的一种特殊语法
  
- async function()  这个函数总是返回一个 promise。其他值将自动被包装在一个 resolved 的 promise 中。
- await 它只在 async 函数内工作, 字面的意思就是让 JavaScript 引擎等待直到 promise settle，(async 函数会在暂停在哪一行, 直到得到promise settle)然后以 promise 的结果继续执行。这个行为不会耗费任何 CPU 资源，因为引擎可以同时处理其他任务：执行其他脚本，处理事件等。
- > 相比于 promise.then，它只是获取 promise 的结果的一个更优雅的语法，用它来代替.then获得promise 的结果

- await 接受 “thenables”, await 允许我们使用 thenable 对象（那些具有可调用的 then 方法的对象)

- 如果一个 promise 正常 resolve，await promise 返回的就是其结果。但是如果 promise 被 reject，它将 throw 这个 error，就像在这一行有一个 throw 语句那样。
- > try ..catch 语法去捕获这个错误 ,或者使用 async函数().catch() ,一般我们使用await就不再使用then等方法. 
- > async/await 可以和 Promise.all 一起使用 等待多个promise的结果


#### Generator
> Generator 可以按需一个接一个地返回（“yield”）多个值。它们可与 iterable 完美配合使用，从而可以轻松地创建数据流。
- 创建一个 generator，我们需要一个特殊的语法结构：function* ,在此类函数被调用时，它不会运行其代码。而是返回一个被称为 “generator object” 的特殊对象，来管理执行流程。

- 一个 generator 的主要方法就是 next(),next() 的结果始终是一个具有两个属性的对象： 
-  value: 产出的（yielded）的值。done: 如果 generator 函数已执行完成则为 true，否则为 false。

-  generator 是 可迭代（iterable）的,因为当 done: true 时，for..of 循环会忽略最后一个 value(return返回的值)

- 我们可以通过提供一个 generator 函数作为 Symbol.iterator，来使用 generator 进行迭代：

- Generator 组合 我们可以使用 yield* 这个特殊的语法来将一个 generator “嵌入”（组合）到另一个 generator 中：

#### 模块 (moudule)

- 模块的核心功能:

- > 模块始终默认使用 use strict。
  >
  > 每个模块都有自己的块作用域
  >
  > 模块代码仅在第一次导入时被执行,然后将导出的内容提供给所有的导入。当改变其中的对象时其他模块随之改变

- > `import.meta` 对象包含关于当前模块的信息,   它包含当前脚本的 URL，或者如果它是在 HTML 中的话，则包含当前页面的 URL。

- 在浏览器中使用模块 , 必须有 type="module"

- > 下载外部模块脚本 `<script type="module" src="...">`

  

- 模块脚本总是被延迟的 :

- > 下载外部模块脚本不会阻塞 HTML 的处理，它们会与其他资源并行加载。
  >
  > 模块脚本会等到 HTML 文档完全准备就绪,然后执行。而常规脚本则会立即运行，

  

- 对于非模块脚本，async 特性（attribute）仅适用于外部脚本。异步脚本会在准备好后立即运行，独立于其他脚本或 HTML 文档。 对于模块脚本，它也适用于内联脚本

- 使用打包工具的一个好处是 —— 它们可以更好地控制模块的解析方式，允许我们使用裸模块和更多的功能，例如 CSS/HTML 模块等。

  > import export 写在开头结尾都可以

#### 导入和导出
- >导出
  >
  >1. 在声明前导出
  >
  >2. 导出与声明分开    **export {sayHi, sayBye};** // 导出变量列表*
  >
  >导入
  >
  >3. 使用 `import * as <obj>` 将所有内容导入为一个对象    **import \* as say from './say.js';**  导出say对象,或者使用import { } from 'say.js'  导入所有
  >4. **import {sayHi} from './say.js';**  明确导入那个



> 大部分 JavaScript 样式指南都不建议在函数和类声明后使用分号。

- >  使用 `as` 让导入具有不同的名字 , 
  >
  > ***import {sayHi as hi, sayBye as bye} from './say.js';***
  >
  > **export {sayHi as hi, sayBye as bye};**

  

- 主要有两种模块: 

- > 1.包含库或函数包的模块  
  >
  > 2.声明单个实体的模块(更倾向 以便每个“东西”都存在于它自己的模块中)
  >
  > export default 语法，以使“一个模块只做一件事”的方式看起来更好,在单个实体模块中添加 export default
  >
  > **import *User* from './user.js';**  将其导入而不需要花括号：  每个文件最多只能有一个默认的导出

  

  

- 重新导出（Re-export）”

- > 语法:   **export {login, logout} from './helpers.js';**  相当于 = > 
  >
  > import {login, logout} from './helpers.js'; 
  >
  > export {login, logout};

- >  重新导出只导出了命名的导出，但是忽略了默认的导出，
  >
  >  默认导出需要单独处理 , 要重新导出默认导出，我们必须明确写出 export {default as User} 
  >
  >  >  重新导出默认的导出的这种奇怪现象是某些开发者不喜欢它们的原因之一。

- **请注意在 `{...}` 中的 import/export 语句无效。** 


- 动态导入 : 根据条件或者在运行时导入

- > import(module) 表达式加载模块并返回一个 promise，
  >
  > 该 promise resolve 为一个包含其所有导出的模块对象。我们可以在代码中的任意位置调用这个表达式。
  >
  > **let {hi, bye} = await import('./say.js');**

- > 动态导入在常规脚本中工作时，它们不需要 script type="module". 
  >
  > 尽管 import() 看起来像一个函数调用，但它只是一种特殊语法,不是真正的函数

- 

#### Generator，高级 iteration  杂项有待扩展....  



- 常规函数只会返回一个单一值（或者不返回任何值）。

  而 Generator 可以按需一个接一个地返回（“yield”）多个值。它们可与 [iterable](https://zh.javascript.info/iterable) 完美配合使用，从而可以轻松地创建数据流。

- 异步迭代器（iterator）允许我们对按需通过异步请求而得到的数据进行迭代。例如，我们通过网络分段（chunk-by-chunk）下载数据时。异步生成器（generator）使这一步骤更加方便。
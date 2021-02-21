---

title:  JS基础下
date: {{ date }}
updated: {{date}}
tags: js
categories: Javascript

---
#### 函数进阶

##### Rest参数  Spread语法

- Rest 参数`...` : 在 JavaScript 中，无论函数是如何定义的，你都可以使用任意数量的参数调用函数。不会因为传入“过多”的参数而报错,只会取得前几个参数匹配  
  
  - >  在 JavaScript 中，很多内建函数都支持传入任意数量的参数          

- **Rest 参数语法**:

- `function showName(firstName, lastName, ...titles)` 
  
  >    Rest  参数必须放到参数列表的末尾, 收集参数放到titles`数组`中



- **arguments 变量**:

- 有一个名为 arguments 的特殊的**类数组对象**，该对象包含所有参数,使用索引访问参数,我们可以在一些老代码里找到它。
  
  >  ```javascript
  >function showName() {
  > alert( arguments[0] );
  > // 它是可遍历的
  >   // for(let arg of arguments) alert(arg);
  >   }
  >   
  >   showName("Julius", "Caesar");
  >   ```
  
- rest  arguments 两者区别:

  > arguments 是一个类数组，也是可迭代对象，但它终究不是数组,不支持数组方法
  >
  > 它始终包含所有参数，我们不能像使用 rest 参数那样可以还可以给与别的参数值

- 箭头函数是没有 "arguments", 如果我们在箭头函数中访问 `arguments`，访问到的 `arguments` 并不属于箭头函数，而是属于箭头函数外部的“普通”函数。

  

- **Spread** 语法 :

- > 把可迭代对象 spread 开, 分给每个函数参数, 和Rest参数相反
  >
  > ```javascript
  > let arr = [3, 5, 1];
  > alert( Math.max(1,2,3,...arr) ); // 5
  > ```
  
- Rest Spread 语法区别:

  > Spread 在函数调用时使用`...`    Rest 是在函数声明时使用`... `  
  >
  > Spread 语法 把数组转换成参数列表,  Rest 把参数列表转换为数组



- 对于可迭代对象都可以展开, 甚至可以用它来合并数组, 转化数组,复制数组
  
  >  ```javascript
  >let merged = [0, ...arr, 2, ...arr2];  //返回一个数组
  > alert( [...str] ); // [..str] 把字符串转化为数组    
  > let arr = [1, 2, 3]; let arrCopy = [...arr] //复制一个数组, arr是对象也成立
  > ```
  
- > ` Array.from(obj)`  和 `Spread` 区别:
  >
  > 区别在于`Array.from` **适用于类数组对象也适用于可迭代对象**, 
>
  > Spread 语法**只适用于可迭代对象**。



- **简单区分 Rest  Spread:**

- 若 ... 出现在函数参数列表的最后，那么它就是 rest 参数，它会把参数列表中剩余的参数收集到一个数组中。
- 若 ... 出现在函数调用或类似的表达式中，那它就是 spread 语法，它会把一个数组展开为列表。


##### 闭包
- 词法环境 : 
  1. **环境记录（Environment Record）** —— 一个存储所有局部变量作为其属性（包括一些其他信息，例如 `this` 的值）的对象。
  2. 对 **外部词法环境** 的引用，与外部代码相关联

- 一个“变量”只是 **环境记录** 这个特殊的内部对象的一个属性。“获取或修改变量”意味着“获取或修改词法环境的一个属性”。

  > “词法环境”是一个规范对象（specification object）：它仅仅是存在于 [编程语言规范](https://tc39.es/ecma262/#sec-lexical-environments) 中的“理论上”存在的，用于描述事物如何运作的对象。我们无法在代码中获取该对象并直接对其进行操作。

- `闭包`是指内部函数总是可以访问其所在的外部函数中声明的变量和参数，即使在其外部函数被返回（寿命终结）了之后。在某些编程语言中，这是不可能的，或者应该以特殊的方式编写函数来实现。

- > 在 JavaScript 中，所有函数都是天生闭包的（只有一个例外，将在 ["new Function" 语法](https://zh.javascript.info/new-function) 中讲到） 
  >
  > 原因 : 所有的函数在“诞生”时都会记住创建它们的词法环境。所有函数都有名为 `[[Environment]]` 的隐藏属性，该属性保存了对创建该函数的词法环境的引用。

- 垃圾收集 : 通常，函数调用完成后，会将词法环境和其中的所有变量从内存中删除。因为现在没有任何对它们的引用了。与 JavaScript 中的任何其他对象一样，词法环境仅在可达时才会被保留在内存中。

- 理论上当函数可达时，它外部的所有变量也都将存在。实际上，JavaScript 引擎会试图优化它。它们会分析变量的使用情况，如果从代码中可以明显看出有未使用的外部变量，那么就会将其删除。

  > 在 V8（Chrome，Opera）中的一个重要的副作用是，此类变量在调试中将不可用。

  

##### let 和 var 区别: 

- 变量提升和函数提升

- > 函数表达式不存在提升 , let有提升但是有暂时锁区

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
>  }
>  }
>  
> ```
>  
> 

- `var` 没有块级作用域,不是函数作用域就是全局作用域

  > 例如if块中定义的变量,或者是for循环中定义的变量,在外面还是能访问到

- `var`我们可以重复声明一个变量，不管多少次都行,

  > 新的声明语句被忽略,但是仍然被赋值

- `var` 声明的变量可以在其声明语句前被使用,变量提升

- IIFE 函数书写   存在意义: 创建一个块级作用域进行使用,不会污染全局变量
- > jq 中开发插件也使用了IIFE ,为了继续使用$ 和 创建一个作用域

##### 全局对象(不是全局变量搞清楚)

- 全局对象提供可在任何地方使用的变量和函数,默认情况下，这些全局变量内置于语言或环境中,全局对象的所有属性都可以被直接访问

- >在浏览器中，它的名字是 “window”，对 Node.js 而言，它的名字是 “global”

- 在浏览器中，使用 var（而不是 let/const！）声明的全局函数和变量会成为全局对象的**属性**。使用 let，就不会发生这种情况.



- 如果一个值非常重要，以至于你想使它在全局范围内可用，那么可以直接将其作为属性写入：
  
  >window.currentUser = {
  >name: "John"
  >};
  
- 我们使用全局对象来测试对现代语言功能的支持。
  
  >  `if (!window.Promise){...} `  测试是否存在promise对象

#####  函数对象 
- 在 JavaScript 中，函数的值类型是一个对象, 我们可以把它当成一个对象使用, 

  > 函数名字通过属性name 访问获得  length返回函数需要参数的个数(rest 参数不参与计数)。
  >
  > 我们也可以在函数内添加自定义属性,在函数外可以调用,改变它. 通过函数名.属性名添加, 创建只属于函数的属性




- 命名函数表达式 (NFE): 指带有函数名字的函数表达式 
- >`let sayHi = function func(who)` ,
  >
  >通过func可以在函数内调用自己 而且在函数外不可见
- >为什么不使用嵌套调用的原因:为了避免在函数内嵌套调用时, 函数表达式在外部已被修改(引用其他函数..)
  >
  >比如: let sayhi =null 那么使用sayhi的嵌套调用就无效了

##### new Function

- `new Function` 允许我们将任意字符串变为函数

- 创建函数体, 参数内接受函数参数 和函数体 ,应用在从服务器获取代码 ..复杂场景才会使用 , 创建的函数只能访问全局变量

##### 调度：setTimeout 和 setInterval

- 有时我们并不想立即执行一个函数，而是等待特定一段时间之后再执行。这就是所谓的“计划调用（scheduling a call）”

- 两种方法事件 : setTimeout 和 setInterval

- ```javascript
  let timerId = setTimeout(func|code, [delay], [arg1], [arg2], ...)
  // 实例 : setTimeout(sayHi, 1000, "Hello", "John");                       
  ```
  
- > func|code : 想要执行的函数或代码字符串。 一般传入的都是函数。
  > 由于某些历史原因，支持传入代码字符串，但是不建议这样做。(JavaScript 会自动为字符代码块其创建一个函数) 
  > arg1 ..要传入被执行函数（或代码字符串）的参数列表,一般都使用函数了

- setInterval 方法 setTimeout 的语法相同

  

- 使用clearTimeout() , clearInterval()  取消调度,在浏览器中，定时器标识符是一个数字。在其他环境中，可能是其他的东西。

- 在大多数浏览器中，包括 Chrome 和 Firefox，在显示 `alert/confirm/prompt` 弹窗时，内部的定时器仍旧会继续“嘀嗒”。



- `周期性调度`有两种方式 ： 一种是使用 setInterval，另外一种就是嵌套的 setTimeout 

- > 嵌套的 `setTimeout` 要比 `setInterval` 灵活得多,采用这种方式可以根据当前执行结果来调整下次调用的时间间隔

- > 使用 setInterval 时，func 函数的实际调用间隔要比代码中设定的时间间隔要短！ 因为其中设置的间隔时间包括函数执行的时间, 所以实际间隔要小
  >
  > 嵌套的 setTimeout 就能确保延时的固定（这里是 100 毫秒）。这是因为下一次调用是在前一次调用完成时再调度的。  嵌套调用时 函数名字不能再是 匿名函数..
>
  > ```
  > let timerId = setTimeout(function tick()
  > {  alert('tick');  *timerId = setTimeout(tick, 2000);}, 2000);
  > ```
  >
  > 

- 垃圾回收 和 计时函数 : 对于 setInterval，传入的函数也是一直存在于内存中，直到 clearInterval 被调用。 如果函数还引用了一些外部变量,那么函数存在,变量也会随之存在(闭包), 会占用内存

  > 在浏览器环境下，嵌套定时器的运行频率是受限制的。必须经过 4 毫秒以上的强制延时

##### 装饰者模式和转发，call/apply
- 装饰者模式 : 一个特殊的函数 :  它接受另一个函数并改变它的行为(参数是一个函数)。返回一个新函数

  


- **call/apply:**


- ```javascript
  func.call(context, arg1, arg2, ...)
  // 提供的第一个参数作为 this
  func.apply(context, args)   
  // 第一个参数作为this , args: 参数数组/类数组对象
  ```
  
- > 两者区别: 
  >
  > `call()` 方法接受的是一个参数列表，而` apply() `方法接受的是一个包含多个参数的数组。
  >
  > `apply` 可能会更快，因为大多数 JavaScript 引擎在内部对其进行了优化。
  >
  > 这两个函数 和 bind函数的区别:
  >
  > 前者更像是调用函数自身,只是给与了this 指针, 后者bind 则是返回了一个新函数

  

- 将所有参数连同上下文一起传递给另一个函数被称为“呼叫转移（call forwarding）”。

- 方法借用 : 

  > `[].join.call(arguments)`  借用原始方法,给与上下文,它为什么有效？
  >
  > 因为原生方法内部,需要`this` 来操作

- 通常，用装饰的函数替换一个函数或一个方法是安全的，除了一件小东西。如果原始函数有属性，装饰后的函数将不再提供这些属性.



##### 函数绑定 

- 丢失"this" : 一旦方法被传递到与对象分开的某个地方 —— `this` 就丢失。例如对象方法作为回调函数进行传递

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
  >  常见情况: 如果传递的包装器是作为异步函数的参数,那么只要主线程中更改对象方法, 包装器中的方法就会受到影响和 顺序不一致

  
  
- 如果利用bind进行绑定, 只要**在bind之后**再更改对象方法,那么都不会影响之前的对象方法, 这样就算是异步函数, 和书写顺序也保持一致

  > let funcUser = func.bind(user);  
  >
  > func.bind(user),  
  >
  > 返回一个类似于函数的“外来对象（exotic object）”，它可以像函数一样被调用   funcUser() ,参数列表和func一样,一样传递参数调用,只是this被绑定了
  
  


- bindAll : 对象有很多方法都需要绑定, 可以在一个循环中完成所有方法绑定
  
- ```
  for (let key in user) {
  if (typeof user[key] == 'function') {
  user[key] = user[key].bind(user);
  }
  }
  ```

  

  

- 偏函数 :  bind 不仅可以绑定this ,还可以绑定参数

- > ```javascript
  > let bound = func.bind(context, [arg1], [arg2], ...);
  > 当this 不需要绑定时 传递 null给conttext
  > ```

- > 通过绑定一个函数的一些参数来创建一个新函数
  >
  > 当我们有一个非常通用的函数，并希望有一个通用型更低的该函数的变体时，偏函数会非常有用



- 如果不想要绑定 this 参数, 我们可以自己实现 '偏函数'

- > ```
  > function partial(func, ...argsBound) {
  > return function(...args) { // (*)
  > return func.call(this, ...argsBound, ...args);}}
  > ```



  

#### 对象属性配置

##### 属性标志和属性描述符

- 对象可以存储属性。 除了简单的“键值”对。对象属性实际上是更灵活且更强大的东西。


- 对象属性（properties），除 value 外还有特殊的特性 

- > 属性标志 : 
  >
  > writable  修改权限   如果为 `true`，则值可以被修改
  >
  >  enumerable 循环是否列出  如果为 `true`，则会被在循环中列出
  >
  >  configurable   如果为 `true`,此特性可以被删除，以上这些属性也可以被修改  否则不行

- > 它们通常不会出现。当我们用“常用的方式”创建一个属性时，它们都为 true。但我们也可以随时更改它们。

  ##### 设置/访问标志

  - `Object.getOwnPropertyDescriptor(obj, "propertyName"); ` 返回值是一个所谓的“属性描述符”对象：它包含值和所有的标志
  - `Object.defineProperty(obj, propertyName, descriptor)`, 返回对象  , descriptor : 要应用的属性描述符对象。

  - > 如果对象存在该属性, 那么标志的默认值为true, 
    >
    > 如果对象没有该属性, 而是直接使用defineProperty() 定义,那么标志默认都为 false


- **"configurable: false" 的用途是防止更改和删除属性标志，但是允许更改对象的值。**




- `Object.defineProperties(obj, descriptors)`，允许一次定义多个属性


- `Object.getOwnPropertyDescriptors(obj)`方法。一次获取所有属性描述符，(返回对象)

- 组合使用拷贝对象:

  > `let clone = Object.defineProperties({}, Object.getOwnPropertyDescriptors(obj));
  >
  > 和普通拷贝对象的区别: 
  >
  > for..in 会忽略 symbol 类型的属性，但是 Object.getOwnPropertyDescriptors 返回包含 symbol 类型的属性在内的所有属性描述符。  使用defineProperties可以拷贝不可枚举的属性

  

- 还有一些限制访问 **整个** 对象的方法

##### 属性的 getter 和 setter
- 有两种类型的对象属性 : 1. 数据属性 2.  访问器属性
  
- > 访问器属性 : 本质上是用于获取和设置值的函数
  
- 语法:
  
  > 在对象内定义get/set  funcname() 函数
  >
  > 使用Object.defineProperty() 函数在其中定义get/set函数
  >
  > ```javascript
  > Object.defineProperty(user, 'fullName', {
  > get() {
  > return `${this.name} ${this.surname}`;
  > },
  > set(value) {
  > [this.name, this.surname] = value.split(" ");
  > }
  > });
  > ```
  
  
  
- 访问器属性的设计思想 : 我们不以函数的方式调用 user.fullName，我们正常读取它,相当于我们有了一个虚拟的属性
  
- 访问器属性名字 不能和对象数据属性名字一样,否则会被覆盖

- 一个属性要么是访问器（具有 `get/set` 方法），要么是数据属性（具有 `value`），但不能两者都是。



- 访问器属性的描述符与数据属性的不同,   

  > 对于访问器属性,没有value 和 writable，
  >
  > **enumerable  configurable** 与数据属性的相同 

  

- Getter/setter用途 : 

- 1. 可以用作“真实”属性值的包装器，以便对它们进行更多的控制。例如限制属性值的长度 

-    2. 它们允许随时通过使用 getter 和 setter 替换“正常的”数据属性

        > 一些老的属性,我们不想删除他们, 我们可以改变操作
        >
        > 通过设置getter和 "旧的"数据属性相同的名字 , 来替换旧的属性

#### 原型,继承



##### 原型继承

- 原型继承解决,在原来的对象基础上**扩展构建**新的对象

- > 因为构造函数只能是重复创建类似功能的对象,无法通过其进行扩展

- 在 JavaScript 中，对象有一个特殊的隐藏属性 `[[Prototype]]`，它要么为 `null`，要么就是对另一个对象的引用。被引用的对象被称为“原型”

- 属性 `[[Prototype]]` 是内部的而且是隐藏的,设置它的方式 :

- > 使用特殊的名字 `__proto__` .   可以获取和设置 ,每边是两个下划线
  >
  > `__proto__` 的存在是历史的原因。在现代编程语言中，将其替换为函数 `Object.getPrototypeOf/Object.setPrototypeOf` 也能 get/set 获得原型

  

- 原型链中引用不能形成闭环, `__proto__` 的值可以是对象，也可以是 null,其他类型被忽略, 只能有一个 `[[Prototype]]`。一个对象不能从其他两个对象获得继承。( 单继承)

- **原型仅用于读取属性**。对于写入/删除操作可以直接在对象上进行

  > 例如原型上有name 属性 ,在子类中是无法修改的, 尝试修改会在该对象上生成name属性
  >
  > 特殊的像访问器属性,可以调用原型的,如果调用set()结果还是会在自身创建属性



- `Object.keys `只返回自己的 key,   `for..in` 循环会迭代继承的属性。

  > 如果排除继承的属性  `obj.hasOwnProperty(key)`   返回true/false
  >
  > ` enumerable:false` 标志导致不可枚举,  for ..in..就不会列举了

- 几乎所有其他键/值获取方法，例如 `Object.keys` 和 `Object.values` 等，都会忽略继承的属性。 它们只会对对象自身进行操作。**不考虑** 继承自原型的属性。

##### Func.prototype 

- 如果 `F.prototype` 是一个对象，那么 `new` 操作符会使用它为新对象设置 `[[Prototype]]`。

- JavaScript 从一开始就有了原型继承,但是在过去，没有直接对其进行访问的方式。唯一可靠的方法是构造函数的 `"prototype"` 属性。目前仍有许多脚本仍在使用它。

- >  `F.prototype` 指的是 `F` 的一个名为 `"prototype"` 的常规属性听起来与“原型”这个术语很类似，但这里我们实际上指的是具有该名字的常规属性。

- 如果在创建之后，F.prototype 属性赋了其他值, 那么通过 new F 创建的新对象也将改变 [[Prototype]]，但已经存在的对象将保持旧有的值。



- 每个函数都有 "prototype" 属性，即使我们没有提供它。 默认的 "prototype" 是一个**只有属性 constructor 的对象**，属性 constructor 指向函数自身。

- ```javascript
  function Rabbit() {}
  /* default prototype
  Rabbit.prototype = { constructor: Rabbit };
  */
  
  ```
  
- 如果我们什么都不做 , constructor 属性可以通过 [[Prototype]] 给所有 rabbits 使用(继承),我们还可以利用此属性, 创建新对象

  > new rabbit.constructor("Black Rabbit");    // 两者相等
  >
  > new Rabbit("White Rabbit");
  >
  > 当我们有一个对象，但不知道它使用了哪个构造器（例如它来自第三方库），并且我们需要创建另一个类似的对象时，用这种方法就很方便。



- JavaScript 自身并不能确保正确的 "constructor" 函数值。 它存在于函数的默认 `"prototype"` 中, 如果我们把默认的prototype 完全替换掉,就不存在constructor

  > 为了确保正确的 "constructor"，我们可以选择添加/删除属性到默认 "prototype"，而不是将其整个覆盖. 
  >
  > 也可以手动重新创建 `constructor` 属性
  >
  > function Rabbit() {}
  > Rabbit.prototype.jumps = true

- F.prototype =的值要么是一个对象，要么就是 null：其他值都不起作用。



-  注意一点:  更改prototype中的属性,和对prototype赋值的区别

  

  > ```
  >Rabbit.prototype.jumps = true  这种是改变prototype属性指向对象中的属性
  > Rabbit.prototype = {} 或者对象名, 这是改变构造函数创建对象后[[prototype]]指向的对象
  > 
  > 两者要分清楚区别, 一个是改变[[prototype]]  指向,一个仅仅改变其中的属性
  > 
  > 
  > 
  > 还要清楚一点: Rabbit.prototype = animal;   是指向animal对象
  > Rabbit.prototype = Animal .prototype; 是指向  animal构造函数的prototype
  > (类似于原生的原型 ,内建对象都是指向自己构造函数的prototype, 而大部分的构造函数 prototype都指向Object()的prototype)
  >  ```
  >  

- 有一点需要注意,对象不存在prototype属性,只有构造函数 类 存在prototype属性,  对象只有[[prototype]];

##### 原生的原型

-  `"prototype"` 属性在 JavaScript 自身的核心部分中被广泛地应用。所有的内置构造函数都用到了它。

- 如何使用它为内建对象添加新功能???

  > 通过构造函数new对象时, 对象的[[prototype]]默认指向构造函数的Prototype
  >
  > String.prototype.show = function() {  alert(this); };  
  >
  > //这样 所有的字符串就可以使用该方法

-  **原生原型修改:**,通常是一个很不好的想法。原型是全局的,如果两个库都添加了相同的方法, 所以很容易造成冲突。

   > **在现代编程中，只有一种情况下允许修改原生原型。那就是 polyfilling。**

- **原型借用:**  通过*Array.prototype.join;* 获得该方法 赋值给创建的对象,  另一种方式是通过将 obj.__proto__设置为Array.prototyp，这样 Array 中的所有方法都自动地可以在 obj中使用了。

  > ```
  > 1. obj.join = Array.prototype.join;   
  > 2. obj.__proto__  = Array.prototype ;
  > ```



-  内建对象，像 Array、Date、Function 及其他，都在 prototype 上挂载了方法, 数据类型对象 也有相应的prototype,但是null undefined没有对象包装器所以不存在 prototype

- 按照规范，所有的内建原型顶端都是 `Object.prototype`。这就是为什么有人说“一切都从对象继承而来”。
  
  >即使是函数 —— 它们是内建构造器Function的对象
  >
  >并且它们的方法(call/apply 及其他)都取自Function.prototyp函数也有自己的 toString方法。



- 所有的内建对象都遵循相同的模式（pattern）：

> 方法都存储在 prototype 中（`Array.prototype`、`Object.prototype`、`Date.prototype` 等）。
>
> 对象本身只存储数据（数组元素、对象属性、日期）。

##### 原型方法
- `__proto__` 被认为是过时且不推荐使用的，这里的不推荐使用是指 JavaScript 规范中规定，proto 必须仅在浏览器环境下才能得到支持。(实际上服务器端也支持)

- 现代方法 :

  > `Object.getPrototypeOf(obj)`  返回对象 obj 的 [[Prototype]]。
  >
  > `Object.create(proto, [descriptors]) `,创建一个空的新对象, 新对象的[[Prototype]指向proto对象  ,  [descriptors] : 可选的属性描述  
  >
  > `Object.setPrototypeOf(obj, proto)`  将对象 obj 的 [[Prototype]] 设置为 proto



- 我们可以使用 `Object.create` 来实现比复制 `for..in` 循环中的属性更强大的对象克隆方式: 包括所有的属性：可枚举和不可枚举的，数据属性和 setters/getters

```javascript
//Object.create 提供了一种简单的方式来浅拷贝一个对象的所有描述符：
let clone = Object.create(Object.getPrototypeOf(obj), Object.getOwnPropertyDescriptors(obj));

```



##### 原型简史

> 如果速度很重要，就请不要修改已存在的对象的 [[Prototype]] , 通常我们只在创建对象的时候设置它一次，自那之后不再修改, JavaScript 引擎对此进行了高度优化。
>
> 用 Object.setPrototypeOf 或 obj.__proto__= “即时”更改原型是一个非常缓慢的操作，因为它破坏了对象属性访问操作的内部优化。

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
  method2() { ... }  // 类的方法之间没有逗号
  ...
}
```

- **类的运行原理:**

- 在 JavaScript 中，类是一种函数。`class User {...}` 构造实际上做了如下的事儿：

- ```javascript
  class User {
    constructor(name) { this.name = name; }
    sayHi() { alert(this.name); }
  }
  ```

- > 创建一个名为 `User` 的函数，该函数成为类声明的结果。该函数的代码来自于 `constructor` 方法
  >
  > 存储类中的方法给 `User.prototype` , 包括 constructor方法



- 人们常说 `class` 是一个语法糖（旨在使内容更易阅读，但不引入任何新内容的语法, 但是class 不仅仅是语法糖 :

  > 1. 使用class 创建的函数具有特殊的内部属性标记,[[FunctionKind]]:"classConstructor",  会检查该属性,与普通函数不同，必须使用 `new` 来调用它：
  >
  > 2. 类方法不可枚举  ,类定义将 `"prototype"` 中的所有方法的 `enumerable` 标志设置为 `false`
  >
  > 3. 类总是使用 `use strict`。 在类构造中的所有代码都将自动进入严格模式。




- 类表达式 : 就像函数一样，类可以在另外一个表达式中被定义，**被传递，被返回，被赋值等。**

  > let User = class{}  

- 类似于命名函数表达式（NamedFunctionExpressions）
  
  > let User = class MyClass {}  这个类名只在类内可见
  
- 类中可以也存在 getter/setter 在class类中声明

- > 也可以通过在 F.prototype中创建getter和setter来起作用。
  >
  > 不能在构造函数中声明

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
  > click = () => {
  >       alert(this.value);
  >     }
  > }
  > //..定义printName(); 
  > ```
  
- 
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

- 对于类字段 来说 ,相当于在constructor( ) 中定义变量一样,会被复制, 类中继承时, constructor()也会被继承,其中定义的变量 ,派生类和基类都会各自维护一份

- 类语法不仅允许指定一个类，在 extends 后可以指定任意表达式。

  

- **重写方法:**

- 不希望完全替换父类方法，希望在父类方法的基础上进行调整或扩展其功能,Class 为此提供了 "super" 关键字。 

- > 1. 执行super.method(...) 来调用一个父类方法 
  > 2. 执行 super(...) 来调用一个父类 constructor（只能在我们的 constructor 中）

- > 箭头函数没有 super,在类中如果被访问，它会从外部函数获取,但是如果是普通函数调用, 将会抛出错误




- **重写constructor**:

- 未提供constructor 时,默认生成:

  ```
  constructor(...args) 
  { super(...args); }
  ```

- 继承类提供constructor()**必须调用** super(...),**并且 (!) 一定要在使用 `this` 之前调用。**

- > 继承类的构造函数:派生构造器具有特殊的内部属性 `[[ConstructorKind]]:"derived"`。该标签会影响它的 `new` 行为:
  >
  > 普通构造函数 new行为 : 当通过 `new` 执行一个常规函数时，它将创建一个空对象，并将这个空对象赋值给 `this`。
  >
  > 当继承的 constructor 执行时，**它不会执行新建空对象并把this传给他的操作**。它期望父类的 constructor 来完成这项工作。

- 当我们在构造器中访问一个被重写的class字段时, **父类构造器总是会使用它自己字段的值，而不是被重写的那一个。** (例如使用this.name 访问的都是父类构造器的name)

  > 原因 : 在于类字段初始化的顺序: 
  >
  > 1.对于基类，在构造函数调用前初始化。
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
- 内建的方法例如 `filter`，`map` 等 , 内部使用对象的  `constructor`  来实现这功能, 返回的正是子类 `PowerArray` 的新对象,

  > 原因: 内部使用了对象的 `constructor` 属性来实现这一功能



- 通常，当一个类扩展另一个类时，静态方法和非静态方法都会被继承,但内建类却是一个例外。它们相互间不继承静态方法。



##### 类检查

- **instanceof 操作符** 用于检查一个对象是否属于某个特定的 class。同时，它还考虑了继承。 还可以检查对象是否属于特定的构造函数

  >  算法的执行过程大致如下 :
  >
  >  1. 如果这儿有静态方法 `Symbol.hasInstance`，那就直接调用这个方法
  >2. 大多数 class 没有 `Symbol.hasInstance`,使用 `obj instanceOf Class` 检查 `Class.prototype` 是否等于 `obj` 的原型链中的原型之一
  >  

  

  > **prototypeObj.isPrototypeOf(object)** 方法 , 在该对象object的原型链上搜寻,有的话返回true
  >
  > > 例子 : Foo.prototype.isPrototypeOf(baz);

  

- 可以使用特殊的对象属性 Symbol.toStringTag 自定义对象的 toString 方法的行为。

- > 如果我们想要获取内建对象的类型，并希望把该信息以字符串的形式返回，而不只是检查类型的话，我们可以用 {}.toString.call 替代 instanceof

##### Mixin 模式
- 在 JavaScript 中，我们只能继承单个对象。每个对象只能有一个 [[Prototype]]。并且每个类只可以扩展另外一个类。但是有些时候这种设定（译注：单继承）会让人感到受限制。

- mixin 是一个包含其他类的方法的类,不需要继承

- > 方法一 : 构造一个 mixin 最简单的方式就是构造一个拥有实用方法的对象,通过拷贝对象完成  Object.assign(User.prototype, sayHiMixin);

  

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



- **抛出自定义的error:**

- `throw` 操作符 : 通过 throw 关键字 , 可以抛出自定义错误 error对象

  



- **自定义 error**   : 技术上讲，我们可以将任何东西用作 error 对象。  但最好使用对象，最好使用具有 name 和 message 属性的对象, 

   > JavaScript 中有很多内建的标准 error 的构造器：Error，SyntaxError，ReferenceError，TypeError 等

- 对于内建的 error,`name` 属性刚好就是构造器的名字。`message` 则来自于参数（argument）。

  > ```javascript
  >let error = new Error("Things happen o_O");
  > alert(error.name); // Error
  >alert(error.message); // Things happen o_O
  > ```




- **重新抛出 error**

- catch 会捕获到 **所有** 来自于 `try` 的 error, 但是我们希望一个catch单独处理一个类型的error, 所以进行重新抛出

  - 具体操作:

    > Catch 捕获所有 error。
    >
    > 在 `catch(err) {...}` 块中，我们对 error 对象 `err` 进行分析。我们可以使用 `instanceof` 操作符判断错误类型,还可以使用err.name 获取错误的类名
    >
    > 如果我们不知道如何处理它，那我们就 `throw err`。

- 使用err.name类名 还是 instanceof 分析error对象, 尽量使用instanceof,`instanceof` 检查对于新的继承类也适用。所以这是面向未来的做法。

  

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
- 回调与同步、异步并没有直接的联系，回调只是一种实现方式，既可以有同步回调，也可以有异步回调，还可以有事件处理回调和延迟函数回调，这些在我们工作中有很多的使用场景

- “基于回调”的异步编程风格: 异步执行某项功能的函数提供一个 callback 参数用于在相应事件完成时调用。

  > 这种风格有以下几种问题:
  >
  > 1. 异步函数之间的执行顺序,是谁先被处理完谁先调用,这使得我们无法控制调用顺序, 而当多个异步函数之间有依赖关系,必须控制回调函数的执行顺序, 我们只能通过嵌套的方式来实现控制顺序,但是会造成混乱的结构,所谓的'厄运金字塔'
  >
  > 
  >
  > 2. 我们提供的callback 参数中,必须要有两种方案,成功/失败的情况,这种情况只能传递二个callback ,或者一个函数多个参数,写起来也很繁琐
  >
  > 

- 基于以上问题 es6 引入Promise,解决异步编程的回调问题

  

##### promise操作

- promise 构造语法 :

  ```
  let promise = new Promise(function(resolve, reject)
  {
        ....
        resolve()/reject()
  }
  ```

-  `new Promise()` 函数是同步函数, resolve/reject()函数调用后才会产生微任务,之后调用的then..是异步函数,也是微任务

- 特殊的我们可以在类中 使用then()函数创建一个promise对象

  > class{  then(resolve,reject){ resolve()...}}

  

- 由 new Promise 构造器返回的 promise 对象具有以下内部属性：

  > 1. state — 初始值是 "pending"，然后在 resolve 被调用时变为 "fulfilled"，或者在 reject 被调用时变为 "rejected"。
  >
  > 2. result —初始值是 'undefined'，然后在 resolve(value) 被调用时变为 value，或者在 reject(error) 被调用时变为 error。
  >
  >    
  >
  > 一个 'resolved '或 'rejected' 的 promise 都会被称为 “settled”。promise的状态改变之后不可逆转

  

- `resolve/reject` 只需要一个参数（或不包含任何参数），并且将忽略额外的参数

- Promise 对象的 state 和 result 属性都是内部的。我们无法直接访问它们。但我们可以对它们使用 .then/.catch/.finally 方法




- **then 、catch、finally函数:**  
``` 
  promise.then(
  function(result) { /* handle a successful result */ },
  function(error) { /* handle an error */ }
);
如果我们只对成功完成的情况感兴趣，那么我们可以只为 .then 提供一个函数参数,如果我们只对 error 感兴趣，那么我们可以使用 null 作为第一个参数
```
- > `.catch(func) `, 是.then(null, f) 的完全的模拟，它只是一个简写形式。只捕获错误情况,    当catch捕获错误之后, promise对象又会回到 resolve的状态,这也和then()有关

- > `finally(func)` 是执行清理的很好的处理程序,  func函数中**并没有参数**,它的目的就是清理程序
  >
  > finally 总是在 promise 被 settled 时运行：即 promise 被 resolve 或 reject。
  >
  > 它会将 result 或者 error 传递给下一个处理程序(如果存在)




- 如果 promise 为 pending 状态，**.then/catch/finally 处理程序（handler）将等待它。**

  

##### promise链 

- Promise 链解决了控制异步函数顺序的问题

- 对 `promise.then` 的调用仍旧返回一个 promise对象,所以它才能链式调用

  > 当然我们也可以在then函数中,return一个值 或者return 新的 promise对象,
  >
  > 1. .then中如果创建并返回一个新的promise。其他的处理程序(例如then..)将等待它 settled 后再获得其结果,  使我们能够构建异步行为链。
  > 2. 如果只是return 一个值,不在进行等待直接进行调用链的下一个处理函数

  

- **默认情况下,  then() /catch()函数调用之后,总是返回一个 resolve 状态的Promise对象**,换句话说就算传递给then()函数的Promise对象是reject的,下一个链式调用的then 收到的还是一个resolve 状态的Promise对象,  这种情况只限于默认情况,如果return一个Promise就需要看这个promise的状态
  
- 例子: 
  
  ```
new  Promise((resolve,reject)=> {
         reject("rejiec");}).then(
         value =>{console.log(value); },
         reason => {console.log(reason)}
     ).then(
         a => console.log("asdf"), // 第一个then 不管进行那个处理,都会进行这一行操作
         b => console.log(b)
     );
  // 如果then中处理了error状况, then后面如果有catch就不再执行
  ```
  
  

- > 确切地说，处理程序返回的不完全是一个 promise，而是返回的被称为 “thenable” 对象 — 一个具有方法 .then 的任意对象。它会被当做一个 promise 来对待。

- promise链是向下增长 , 嵌套式的promise调用是向右增长

  

  

##### promise 处理错误  

- 捕获所有 error 的最简单的方法是，将 .catch 附加到链的末尾,如果then把错误捕获,就不在调用catch

  > 在实际书写中,我们只在then里写成功的情况,用catch进行捕获

- 抛出自定义异常: 

  ```
  reject(new httpError("Whoops!")); 
  throw new ParamsError("Whoops!"); //在then处理程序中定义
  ```

- 异常再次抛出:

  > 在常规的 try..catch 中，我们可以将其再次抛出。对于 promise 来说，如果我们在 .catch 中 throw error，那么它会忽略其他的处理程序跳转到下一个catch。如果我们处理该 error 并正常完成，那么它将继续到最近的成功的 .then 处理程序

- 当一个 error 没有被处理会发生什么？

  > 如果出现 error，promise 的状态将变为 “rejected”,   然后执行应该跳转至最近的 rejection 处理程序, 如果没有, JavaScript 引擎会跟踪此类 rejection，在这种情况下会生成一个全局的 error 

- 全局的error捕获:

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
- `Promise.all()`:  如果任意一个 promise 被 reject，由 Promise.all 返回的 promise 就会立即 reject，并且带有的就是这个 error。都成功才算成功

- `Promise.allSettled()`：等待所有的 promise 都被 settle，无论结果如何。他自身始终返回一个 resolve的promise对象
- `Promise.race`: 只等待第一个 settled 的 promise 并获取其结果（或 error）

- `Promise.resolve(value)` 用结果 value 创建一个 resolved 的 promise。可以用来缓存数据,不再进行 promise() 中的运算 

- `Promise.reject(error)` 用 error 创建一个 rejected 的 promise。 

  
  
  
  
##### Promisification

- “Promisification”   它指将一个接受回调的函数转换为一个返回 promise 的函数。




##### Async/await
- Async/await 是以更舒适的方式使用 promise 的一种特殊语法, 本质上是一种语法糖
  
- `async关键字`: async function()  这个函数总是返回一个 promise。return 其他值, 将自动被包装在一个 resolved 的 promise 中。

- `await关键字` :它只在 async 函数内工作, 等待直到 promise settle，接受结果/error,  还可以使用await newPromise() 返回一个新的promise对象

  > 使用await时 , 如果 promise 被 reject，它将 throw 这个 error，使用try ..catch 语法去捕获这个错误 ,或者使用 async函数().catch() 

- async/await 可以和 Promise.all 一起使用 等待多个promise的结果

- await 本质上就是then()函数

- await 接受 “thenables”, await 允许我们使用 thenable 对象（那些具有可调用的 then 方法的对象)


#### Generator  异步迭代
- 一般的函数只能返回一个单一值, Generator 函数可以按需一个接一个地返回（“yield”）多个值。它们可与 iterable 完美配合使用，从而可以轻松地创建数据流。

- 创建generator

  > function* generateSequence(){ yield 1;  yield 2;  return 3}
  >
  > 它不会运行其代码。而是返回一个被称为 “generator object” 的特殊对象，来管理执行流程。

- 一个 generator 的主要方法就是 next(),`generator object`每次调用函数,就返回一个值,

- next() 的结果始终是一个具有两个属性的对象：

  >  value: 产出的（yielded）的值。
  >
  > done: 如果 generator 函数已执行完成则为 true，否则为 false。

- generator 是 可迭代（iterable）的,我们可以使用 `for..of` 循环遍历它所有的值

  > 如果我们想要通过 `for..of` 循环显示所有的结果，我们必须使用 `yield` 返回它们,不可以使用return, 否则循环就不再显示最后一个

- 我们可以通过提供一个 generator 函数作为 Symbol.iterator，来使用 generator 进行迭代

  >  *[Symbol.iterator] (){ }    // [Symbol.iterator]: function*() 的简写形式

- Generator 嵌套  ,可以迭代更复杂的输出

  > 我们可以使用 yield* 这个特殊的语法来将一个 generator “嵌入”（组合）到另一个 generator 中：

**异步迭代：**

- 要使对象异步迭代：
  1. 使用 `Symbol.asyncIterator` 取代 `Symbol.iterator`。
  2. `next()` 方法应该返回一个 `promise`
- Generator 异步迭代 。。。。

#### 模块 (moudule)

- 模块的核心功能:

- > 模块始终默认使用 use strict。
  >
  > 每个模块都有自己的块作用域
  >
  > **模块代码仅在第一次导入时被执行**,然后将导出的内容提供给所有的导入。当改变其中的对象时其他模块随之改变

- > `import.meta` 对象包含关于当前模块的信息,   它包含当前脚本的 URL

- **浏览器模块:**

- 在浏览器中使用模块 , 必须有 type="module"

- > 下载外部模块脚本 `<script type="module" src="...">`

- 在本地 外联引入模块化的脚本会出现跨域问题,解决方法开启本地服务器即可

- 模块脚本总是被延迟的 :

- > 下载外部模块脚本不会阻塞 HTML 的处理，它们会与其他资源并行加载。
  >
  > 模块脚本会等到 HTML 文档完全准备就绪,然后执行。而常规脚本则会立即运行，

- 具有 `type="module"` 的外部脚本（external script）在两个方面有所不同：

  > 具有相同 `src` 的外部脚本仅运行一次
  >
  > 从另一个源（例如另一个网站）获取的外部脚本需要 [CORS](https://developer.mozilla.org/zh/docs/Web/HTTP/CORS) header，

- 旧时的浏览器不理解 `type="module"`。未知类型的脚本会被忽略。对此，我们可以使用 `nomodule` 特性来提供一个后备

- 对于非模块脚本，async 仅适用于外部脚本。异步脚本会在准备好后立即运行，独立于其他脚本或 HTML 文档。 对于模块脚本，它也适用于内联脚本

  

- 浏览器模块很少单独使用，通常使用构建工具， 

- [构建工具的必要性](https://zh.javascript.info/modules-intro#gou-jian-gong-ju)

  > 如果我们使用打包工具，那么脚本会被打包进一个单一文件（或者几个文件），在这些脚本中的 `import/export` 语句会被替换成特殊的打包函数（bundler function）。因此，最终打包好的脚本中不包含任何 `import/export`，它也不需要 `type="module"`，我们可以将其放入常规的 `<script>`：

- 还有的打包工具利用原生的 import 打包，不再使用打包函数进行替换了。。。

##### 导入和导出
- 导出

  > 在声明前导出  : *export* let months={}
  >
  > 导出与声明分开 :   export {sayHi, sayBye};    // 导出变量列表

- 导入
  

  >import {sayHi} from './say.js'; 明确导入
  >
  >import \* as say from './say.js';   所有内容导入为一个对象



- 使用 `as` 让导入具有不同的名字 , 

  >  **import {sayHi as hi, sayBye as bye} from './say.js';**
  >
  > **export {sayHi as hi, sayBye as bye};**

  

- 主要有两种模块: 

- > 1.包含库或函数包的模块  
  >
  > 2.声明单个实体的模块,例如模块 `user.js` 仅导出 `class User`
  
- export default 语法，以使“一个模块只做一件事”的方式看起来更好

  > 将 `export default` 放在要导出的实体前 或者分开`export {sayHi as default}`;
  >
  > **import *User* from './user.js';**  将其导入而不需要花括号
-  每个文件最多只能有一个默认的导出

 

- **重新导出:**

- > 语法:   **export {login, logout} from './helpers.js';**  相当于
  >
  > import {login, logout} from './helpers.js'; 
  >
  > export {login, logout};

- 重新导出中的 默认导出需要单独处理 , 
  
  >  export User from './user.js'  无效  我们必须明确写出 export {default as User}`
  >
  >  `export * from './user.js  重新导出只导出了命名的导出，但是忽略了默认的导出。

  
  

##### 动态导入 


- > import(module) 表达式加载模块并返回一个 promise，该 promise resolve 为一个包含其所有导出的模块对象。
  >
  > **let {hi, bye} = await import('./say.js');**
  
- 动态导入在常规脚本中工作时，它们不需要 script type="module". 
  
- 尽管 import() 看起来像一个函数调用，但它只是一种特殊语法,不是真正的函数




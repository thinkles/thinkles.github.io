---
title: JS指导 上
date: 
tags: js基础
categories: Javascript
---

##  JS 入门精通

#### JS简介 
- JS在任意搭载了 JavaScript 引擎 的设备中都可以执行
> V8 —— Chrome 和 Opera 中的 JavaScript 引擎。
> SpiderMonkey —— Firefox 中的 JavaScript 引擎。
- 现代的 JavaScript 是一种“安全”语言。它不提供对内存或 CPU 的底层访问，因为它最初是为浏览器创建的，不需要这些功能
- “use strict” 严格模式 原因:最初的js代码语法太过松散,但是为了兼容旧版本所以需要手动开启严格模式


#### 变量常量
- let var   变量
- 常量 : const   初始赋值之后就不会改变。
- >大写命名的常量仅用作“硬编码（hard-coded）”值的别名。常规命名用作执行中被计算出的值
>变量代码规范:  变量的名要尽量具体,远离a b这种,或者data value这种,相当于没说明这变量的用处,变量是重用还是新建: 额外声明一个变量绝对是利大于弊的
#### 数据类型
- 在 JavaScript 中有 8 种基本的数据类型(7 种原始类型和 1 种引用类型)
> 在 JavaScript 中做数学运算是安全的。我们可以做任何事：除以 0(得到infinity)，将非数字字符串视为数字，等等。脚本永远不会因为一个致命的错误（“死亡”）而停止。最坏的情况下，我们会得到 NaN 的结果。

1. number 类型代表整数和浮点数。
  
2. BigInt 类型，用于表示任意长度的整数(有些大数据超出了number表示范围)。可以通过将n附加到整数字段的末尾来创建 BigInt 值。

3. String 类型, JavaScript 中的字符串必须被括在引号里。有三种包含字符串的方式:单/双引号,反引号
>反引号是功能扩展引号。它们允许我们通过将变量和表达式包装在 ${…}中
>可以在 ${…} 内放置任何东西：诸如名为 name 的变量，或者诸如 1 + 2 的算数表达式，或者其他一些更复杂的。仅仅在反引号内有效
4. boolean 类型仅包含两个值：true 和 false。
   
5. undefined 的含义是未被赋值。如果一个变量已被声明，但未被赋值，那么它的值就是 undefined.

6. symbol 类型,(在ECMAScript 6 中新添加的类型)。一种实例是唯一且不可改变的数据类型。
  
7. null null类型值只有一个(null), (JavaScript 是大小写敏感的，因此 null 与 Null、NULL或变体完全不同。) 例如 : let a = null;

8. Object类型(引用类型)

- typeof 运算符返回参数的类型 支持两种方式  作为运算符：typeof x。 函数形式：typeof(x)。
> NaN 即 Not a Number 它是用来表示是否属于number类型的一种状态,不属于类型/值
#### 交互函数
- alert、prompt 和 confirm
- 这些方法都是模态的：它们暂停脚本的执行，并且不允许用户与该页面的其余部分进行交互，直到窗口被解除。
#### 类型转换
- 隐式类型转换
  - 大多数情况下，运算符和函数会自动将赋予他们的值转换为正确的类型。
  - > 例如: alert 会自动将任何值都转换为字符串以进行显示。算术运算符会将值转换为数字
- 显示类型转换
  - 我们也可以显式地调用 String(value) 来将 value 转换为字符串类型
  - 使用 Number(value) 显式地将这个 value 转换为 number 类型,如果该字符串不是一个有效的数字，转换的结果会是 NaN .
  - > 例如:alert( Number("123z") );// NaN （从字符串“读取”数字，读到 "z" 时出现错误）

- number 类型转换规则： 
  
       undefined 	  NaN  
       null 	       0   
       true和false  1 and 0
- 布尔型转换 转换规则如下：

        直观上为“空”的值（如 0、空字符串、null、undefined 和 NaN）将变为 false。
        其他值变成 true。
        字符串 "0" 是 true 例如;alert(Boolean("0"));// true

#### 运算符
- " ** " 运算符  =>   2**3 等于 2^3

- "+" 运算符中(二元运算符), 只要任意一个运算元是字符串，那么另一个运算元也将被转化为字符串。
- > alert( '1' + 2 ); // "12"
  
- "+" 运算符中(一元运算符),会将其转化为数字。
- > 一元运算符+ ,可以转换成数字  alert( +true ); // 1
  
- 对于其他算术运算符,仅使用数字，并且始终将其操作数转换为数字。
> alert( 6 - '2' ); // 4

- 逗号运算符能让我们处理多个语句，使用 , 将它们分开。每个语句都运行了，但是只有最后的语句的结果会被返回。
> 例子 : for (a = 1, b = 3, c = a * b; a < 10; a++)

#### 逻辑运算符

-  "||" 运算的链，将返回第一个真值，如果不存在真值，就返回该链的最后一个值。

-  "&&" 运算的链中,返回第一个假值，如果没有假值就返回最后一个值。

- ! 表示布尔非运算 , 首先转换为布尔值,再给出相反值 
- >!! 有时候用来将某个值转化为布尔类型：

#### 值的比较
- 当对不同类型的值进行比较时，JavaScript 会首先将其转化为数字（number）再判定大小。

- 普通的相等性检查 == 存在一个问题，它不能区分出 0 和 false,因为两侧的值会先被转化为数字,如果想在进行比较时不会做任何的类型转换。使用 === 符号

- null 和 undefined 进行比较,当使用严格相等 === 比较二者时,它们不相等，因为它们属于不同的类型。
- 当使用非严格相等 == 比较二者时,JavaScript 存在一个特殊的规则，会判定它们相等。
- >undefined 和 null 在相等性检查 == 中不会进行任何的类型转换，它们有自己独立的比较规则，所以除了它们之间互等外，不会等于任何其他的值
  
- "!="  "!==" 之间的差别 : "!=" 会进行数据转换比较," !== "不会,数据类型不同就不存在相等
#### 语句
- if判断 , 循环语句,  : ?语句

- 跳出嵌套循环  标签 + break/continue 
> outer: for (let i = 0; i < 3; i++) 标签后加循环语句
> 只有在循环内部才能调用 break/continue，并且标签必须位于指令上方的某个位置。

- switch case中不会进行类型转换,如果类型不相等就不匹配



#### 函数

#### 函数参数return值
- 函数参数
- >函数修改的是复制的变量值副本,对原来的值并没有影响(浅拷贝)

- 函数参数--默认值 
- >如果调用带参数的函数,未提供参数，也没有给默认值, 这时参数默认 为undefined
- > function showMessage(from, text = "no text given")  在等号后给与默认参数

- 空值的 return 或没有 return 的函数返回值为 undefined

#### 函数表达式 函数声明
- 函数声明和函数表达式的区别
  - 函数表达式是在代码执行到达时被创建，并且仅从那一刻起可用。

  - 函数声明,调用时不在乎函数声明的位置(这是内部算法的原故,当 JavaScript 准备 运行脚本时，首先会在脚本中寻找全局函数声明)

> 函数表达式结尾有一个分号 ;，而函数声明没有,因为它不是代码块而是一个赋值语句

- 在其他编程语言中，只要提到函数的名称都会导致函数的调用执行，但 JavaScript 可不是这样。在 JavaScript 中，函数是一个值，所以我们可以把它当成值对待 
- > 例如 alert(sayHi) 会输出函数的源码而不会被调用,想要调用需要有括号

#### 回调函数 

- 回调函数 : 函数参数是另一个函数名字(不带括号), 

- 函数参数可以是匿名函数,但是函数外无法调用,

- >   例如:   ask( function() { alert("You agreed."); },)

  > 函数命名 :一种普遍的做法是用动词前缀来开始一个函数，这个前缀模糊地描述了这个行为 例如 show cacl create

  

#### 箭头函数 + 深入理解箭头函数式

``` 
let sum = (a, b) => {  // 花括号表示开始一个多行函数
  let result = a + b;
  return result;       // 如果我们使用了花括号，那么我们需要一个显式的“return”
};
```

> 箭头后是返回值  如果没有参数，括号将是空的（但括号应该保留）
>
> 参数只有一个,不需要括号

- 箭头函数可以像函数表达式一样使用

- 箭头函数是针对那些没有自己的“上下文”，但在当前上下文中起作用的短代码的

  ----------

- 箭头函数没有 “this” , 如果访问 this，则会从外部获取。

- > 不具有 this 自然也就意味着另一个限制：箭头函数不能用作构造器（constructor）。不能用 new 调用它们。

- 箭头函数没有 “arguments”

- 箭头函数和 bind函数的区别 :

  ```
  bind(this)创建了一个该函数的“绑定版本”。
  箭头函数=> 没有创建任何绑定。箭头函数只是没有this。
  this 的查找与常规变量的搜索方式完全相同：在外部词法环境中查找。 
  ```



--------------------------

### 

### 对象
#### 构建对象
  > let user = new Object(); // “构造函数” 的语法   创建空对象
  > let user = {};  // “字面量” 的语法

#### 定义/操作对象属性
  - 我们可以随时添加、删除和读取文件。通过 .运算符    delete关键字 
  - 对于多词属性，点操作就不能用了
  - > user.likes birds = true  // 报错 用方括号 user["likes birds"] = true;
    >
    > 方括号同样提供了一种可以通过任意表达式来获取属性名的方法
    >
    > 当属性名是已知且简单的时候，就是用点符号。如果我们需要一些更复杂的内容，那么就用方括号。



> 带有cosnt的对象内容仍然可以被更改, 但是以对象为整体无法改变 
> 例如: constn user = { ame: "John"};  user=...  会出现报错,但是更改name的值是允许的


- 计算属性  : 当创建一个对象时，我们可以在对象字面量中使用方括号
- >let fruit = prompt("Which fruit to buy?", "apple");
  >
  >let bag = {
  >
  >[ fruit]: 5, // 属性名是 fruit 计算得到的
  >
  >};

  

属性的其他操作

-  属性值缩写 :
```
function makeUser(name, age) {  
  return {
    name, // 与 name: name 相同
    age,  // 与 age: age 相同
  };
}
```

- 属性名称限制 :

- >不能是保留字,其他类型会自动转换为字符串类型   , 如果是复杂的标识名,加上引号声明
  >
  >例如: {0:test  "ee er":22}  0转换为字符串  

- 检查属性是否存在 :

- > 使用操作符 "in"  例如: "key" in objectname
  >
  > 为了遍历一个对象的所有键（key），可以使用一个特殊形式的循环：for..in

- > js可以访问任何对象的属性,就算该属性不存在也不会发生错误,返回一个undefined类型

- 对象属性顺序 : 整数属性会被进行排序，其他属性则按照创建的顺序显示(保证在遍历对象时,它的顺序可预知)

- > 这里的“整数属性”指的是一个可以在不作任何更改的情况下转换为整数的字符串
  >
  > “49” 是一个整数属性名，因为我们把它转换成整数，再转换回来，它还是一样。但是 “+49” 和 “1.2” 就不行了

#### 对象拷贝 引用



> 对象与原始类型其中一个基本的区别是：对象“通过引用的形式”被存储和拷贝,所以对象只有一份
 - 变量存储的不是对象自身，而是该对象的“内存地址”，换句话说就是一个对该对象的“引用”
 - >对于对象来说，普通相等 == 和严格相等 === 是两个作用结果完全一样的运算符


- 对象拷贝  :

 - 通过 for...in进行对象属性的复制, 如果对象中嵌套对象,在循环中进行判断,通过复制嵌套对象的结构完成拷贝(深拷贝)
  
 - > 也可以使用其他js库中的方法,  这些拷贝方法都不能拷贝到不能列举的属性
  
 - 通过 Object.assign() 进行浅拷贝 => 对象中有另一个对象时,Object.assign()只会拷贝引用 (浅拷贝)

 - > Object.assign(dest, [src1, src2, src3..])
   >
   > src : 对象, 可以是多个对象合并到dest

#### 对象方法

>作为对象属性的函数被称为 方法。

    通过属性名添加函数:   user.sayHi = function() {alert("Hello!");};
    在对象中简写方法      user = {  sayHi: function() { alert("Hello");}}; 
    更简单的书写          user = {  sayHi(){alert("fog);} }
    通过属性名添加函数名  function sayHi() { alert("Hello!");};  user.sayHi = sayHi;

#### 垃圾回收
 - 可达性
 - >对于不可达的对象 数据,进行垃圾处理
- 垃圾回收算法: 垃圾收集器找到所有的根，并“标记”（记住）它们。然后它遍历并“标记”来自它们的所有引用用,没有被标记的就被删除。 算法还有很多优化这里不再提及





#### 方法中的"this"

- 作为对象属性的函数被称为 **方法**。

- > 为了访问该对象，方法中可以使用 `this` 关键字。`this` 的值就是在点之前的这个对象，即调用该方法的对象。

- 为什么在方法中使用this?

- > 使用变量名访问数据不太可靠,当我们换了一个对象访问该方法,以前的数据还是旧数据

- 在没有对象的情况下(全局函数)调用：

```
this == undefined  严格模式下的 this 值为 undefined 
在非严格模式的情况下，this 将会是全局对象

```

- 在 JavaScript 中，`this` 关键字与其他大多数编程语言中的不同。JavaScript 中的 `this` 可以用于任何函数。`this` 的值是在代码运行时计算出来的，它取决于代码上下文。

- > 当函数分配给对象时,运行方法,this的值才会计算出来
  >
  > 它的值并不取决于方法声明的位置，而是取决于在“点符号前”的是什么对象。

  - this 功能主要使用在 方法上, 用在函数中this表示

#### 构造函数  new

- 常规的 `{...}` 语法允许创建一个对象。但是我们经常需要**创建许多类似**的对象,例如多个用户. 这时需要构造函数, 构造器的主要目的 —— 实现可重用的对象创建代码
- 构造函数在技术上是常规函数。不过有两个约定：
 1.它们的命名以大写字母开头。
 2.它们只能由 "new" 操作符来执行。


- new通过构造函数创建对象的过程
```
let user= new User("john");
function User(name) {
  // this = {};（隐式创建）

  // 添加属性到 this
  this.name = name;
  this.isAdmin = false;

  // return this;（隐式返回）
}
构造函数只能使用 `new` 来调用。这样的调用意味着在开始时创建了空的 `this`，并在最后返回填充了值的 `this`。
```


- > 从技术上讲，任何函数都可以用作构造器。即：任何函数都可以通过 `new` 来运行，它会执行上面的算法。“首字母大写”是一个共同的约定，以明确表示一个函数将被使用 `new` 来运行

  > 在一个函数内部，我们可以使用 new.target 属性来检查它是否被使用 new 进行调用了。true 被new调用

- 如果我们有许多行用于创建**单个复杂对象**的代码，我们可以将它们封装在构造函数中: 
  
- ```javascript
  let user = new function() {
    this.name = "John";
    this.isAdmin = false;
    // ……用于用户创建的其他代码
  };
  ```
  
- 通常，构造器没有 return 语句。它们的任务是将所有必要的东西写入 this，并自动转换为结果

- > 带有对象的 return 返回该对象，在所有其他情况下返回 this。





#### 可选链运算

- 可选链 `?.` 是一种访问**嵌套对象属性**的防错误方法。即使中间的属性不存在，也不会出现错误。

      可选链 ?. 语法有三种形式：
      obj?.prop —— 如果 obj 存在则返回 obj.prop，否则返回 undefined。
      obj?.[ prop] —— 如果 obj 存在则返回 obj[prop]，否则返回 undefined。
      obj?.method() —— 如果 obj 存在则调用 obj.method()，否则返回 undefined。
      ?. 检查左边部分是否为 null/undefined，如果不是则继续运算。?. 链使我们能够安全地访问嵌套属性。

> 但是，我们应该谨慎地使用 ?.，仅在当左边部分不存在也没问题的情况下使用为宜。以保证在代码中有编程上的 error 出现时，也不会对我们隐藏

#### Symbol 类型
- “Symbol” 值表示**唯一的标识符**,即使我们创建了许多具有相同描述的 Symbol，它们的值也是不同。

- > let id = Symbol();  
  >
  > 添加描述 let id = Symbol("id");

  

- > Symbol 不会被自动转换为字符串,为了防止字符串和Symbol类型混淆
  >
  > 可以通过函数 Tostring()转换为字符串显示 
  >
  > 通过symbol.description 属性，只显示描述.

  

- Symbol 允许我们创建对象的“隐藏”属性，代码的任何其他部分都不能**意外访问**或重写这些属性。

- > "隐藏" 指的是Symbol 类型的属性,不会被`for..in `列举出来 `Object.keys(user)` 也会忽略它们。
  >
  > 使用enumberable属性标志也不会被枚举, 但是console.log还是会显示
  >
  > `[Object.assign]`会同时复制字符串和 symbol 属性, 但不会复制enumberable : false的属性
> 如果我们使用的是属于第三方代码的 user 对象，我们想要给它们添加一些属性。
>
> 如果使用 字符串添加, 很容易引起冲突, 但是使用Symbol 创建,就算描述一样,返回的也是不同的值,不会引起冲突

- 创建Symbol返回值名字 和 属性名一样, 也不会有问题, 但是调用Symbol属性必须加上[], 如果有点符号调用则是普通的属性名

> ```javascript
> let vip ={
>  id:3,
>  name:"vip",
> }
> let id = Symbol("id");
> vip[id] = "er";
> 
> console.log(vip[id]) //   er   用点符号进行调用显示3
> ```
>
> 


- 全局 symbol使用  : 

- > 有时我们想要名字相同的 Symbol 具有相同的实体。
  >
  > 例如，应用程序的不同部分想要访问的 Symbol "id" 指的是完全相同的属性。 注册表内的 Symbol 被称为 全局 Symbol。
  >
  > Symbol.for(key)。  key:描述   通过描述返回symbol  (在注册表中查找,不存在则创建) 
  >
  > Symbol.keyFor(sym) 通过全局 Symbol返回一个描述。 sym: 一个symbol

- > Symbol.keyFor 内部使用全局 Symbol 注册表来查找 Symbol 的键, 如果 Symbol 不是全局的，它将无法找到它并返回 undefined。

> symbol("")  和 Symbol.for(key)  使用相同的描述 创建的symbol 依然不相等
>
> 局部的Symbol  和全局的Symbol仍然不相等
- JavaScript 使用了许多系统 Symbol. 可以用来改变一些内置行为

> Symbol 主要运用 : 
>
> 1.“隐藏” 对象属性 : 向对象添加 属性
>
> 2.系统Symbol :改变内置行为


#### 对象 原始值转换
- 所有的对象在布尔上下文（context）中均为 true。所以对于对象，不存在 to-boolean 转换，只有字符串和数值转换。

- 数值转换发生在对象相减或应用数学函数时。至于字符串转换 —— 通常发生在我们像 alert(obj) 这样输出一个对象和类似的上下文中。

- > 三个类型转换的变体，被称为 “hint” :  "String" "number" "default"
  >
  > 根据对象的使用情况向这三种类型转换

- 对象 在上下文 需要转换时,会根据转换类型尝试调用内置函数进行转换

- > 1. 调用 `obj[Symbol.toPrimitive](hint)` 内置函数 (如果存在的话) ,我们可以进行自定义该属性
  >
  >    > [Symbol.toPrimitive] = function(hint){.....}
  >
  > 2. 根据 hint 类型 , 调用 tostring / valueOf() ...

### 数据类型

#### 原始类型的方法

- 人们想操作原始数据类型 string number...,像操作方法一样,这时候需要对象支持,但是对象有需要额外的资源支持,因为需要原始数据尽量小,所以提出解决方法:

- 创建了提供额外功能的特殊“对象包装器”，使用后即被销毁。

- > 特殊的原始类型 `null` 和 `undefined` 是例外。它们没有对应的“对象包装器”，也没有提供任何方法。

- “对象包装器”对于每种原始类型都是不同的，它们被称为 String、Number、Boolean 和 Symbol。因此，它们提供了不同的方法。

- > ```javascript
  > let str = "Hello";
  > 
  > alert( str.toUpperCase() ); // HELLO
  > ```
  >
  > 访问其属性时，会创建一个包含字符串字面值的特殊对象, 进行调用函数, 调用结束后销毁对象,留下字符串原始值


> new Boolean(false) 等语法，明确地为原始类型创建“对象包装器”。但是极其不推荐,因为他返回一个对象,而不是返回转换过的值, 对象在判断语句中一直 true
> 另一方面，调用不带 new（关键字）的 String/Number/Boolean 函数是完全理智和有用的,这进行显式转换数据类型  let num = Number("123"); // 将字符串转成数字

#### 数字类型

- 在 JavaScript 中，我们通过在数字后附加字母 “e”，并指定零的数量来缩短数字：

- > let billion = 1e9;  //数字 1后面9个0  (相当于1乘10的9次方)  1e-6; // 1 的左边有 6个0

- toString(base) 方法  base: 给定base 进制 返回一个字符串形式的base进制数,默认值为10

- >数字也可以直接调用该方法 ,使用两个点   1234..tostring(base)

- Infinity（和 -Infinity）是一个特殊的数值，比任何数值都大（小）。

- > 值 “NaN” 是独一无二的，它不等于任何东西，包括它自身：不能使用===

- 它们属于 number 类型，但不是“普通”数字，因此，这里有用于检查它们的特殊函数：

- isNaN(value) 将其参数转换为数字，然后测试它是否为 NaN：

- isFinite(value) 将其参数转换为数字，如果是常规数字，则返回 true

  ##### 舍入（rounding）   计算精度缺失: 

- Math.floor  向下(小)舍入  Math.ceil 向上(大)舍入 Math.round 向最近的整数舍入  Math.trunc（IE 浏览器不支持这个方法）移除小数点后的所有内容而没有舍入  都是向整数舍入

- toFixed(n) 将数字舍入到小数点后 n 位，**返回字符串**。如果小数部分比所需要的短，则在结尾添加零：舍入时根据数字判断向上 向下进行舍入

``` 
let num = 12.34;
alert( num.toFixed(5) ); // "12.34000"，在结尾添加了 0，以达到小数点后5位
```

- >alert( 0.1 + 0.2 == 0.3 ); // false

- 原因 : 一个数字以其二进制的形式存储在内存中，一个 1 和 0 的序列。但是在十进制数字系统中看起来很简单的 0.1，0.2 这样的小数，实际上在二进制形式中是无限循环小数。

- > 最可靠的方法是借助方法 toFixed(n) 对结果进行舍入：使用一元加号将其强制转换为一个数字 
  >
  > let sum = 0.1 + 0.2; alert( +sum.toFixed(2) ); // 0.30

- alert( 6.35.toFixed(1) ); // 6.3 而不是6.4 

- >原因: 在内部，6.35 的小数部分是一个无限的二进制
  >
  >如果我们希望以正确的方式进行舍入，在进行舍入前，我们应该使其更接近整数

- >alert( (6.35 * 10).toFixed(20) ); // 63.50000000000000000000  63.5 完全没有精度损失。这是因为小数部分 0.5 实际上是 1/2。以 2 的整数次幂为分母的小数在二进制数字系统中可以被精确地表示，

- 在处理小数时避免相等性检查 精度缺失导致小数计算时发生意外

  ######  praseInt/praseFloat

- 使用加号 + 或 Number() 的数字转换是严格的。如果一个值不完全是一个数字，就会失败

- >alert( +"100px" ); // NaN

- 这就是 parseInt 和 parseFloat 的作用 :
  它们可以**从字符串中“读取”数字**，直到无法读取为止。如果发生 error，则返回收集到的数字。函数 parseInt 返回一个整数，而 parseFloat 返回一个浮点数：

- > parseInt/parseFloat 当没有数字可读时会返回  NaN。
  >
  > 例如 :  alert( parseInt('a123') ); // NaN，第一个符号停止了读取

  


> parseInt(str, radix) 的第二个参数 它指定了数字系统的基数，因此 parseInt 还可以解析十六进制数字、二进制数字等的字符串
>
> 如果 parseInt() 的参数是数字的话,它的效果类似Math.trunc() ,移除小数点之后的数只取整数 

- 其他数学函数 ----参考Math对象


#### 字符串

- 反引号它们允许字符串跨行(单/双引号不行),在输入时也会是换行效果,反引号允许我们通过 `${…}` 将任何表达式嵌入

  >  反引号可以用来指定一个模板函数 


- **字符串基本操作**:

- 访问字符 :使用方括号[ pos] 或者调用 `str.charAt(pos)` 方法。pos从零开始 

- >它们之间的唯一区别是，如果没有找到字符，[] 返回 undefined，而 charAt 返回一个空字符串



- length 属性表示字符串长度, 特殊字符,如\n 则算一个单独的字符

- 使用` for..of `遍历字符串和`for..in`区别在于,for in得到的是索引,根据索引得到内容, for of 直接得到内容

- 在 JavaScript中，**字符串不可更改**,通常解决创建一个新字符串.

  ```javascript
  let a="jsonp";
  a[2] = "h";
  console.log(a); // jsonp
  
  // 就算改变字符, 也不会有什么影响
  ```

  

- **字符串方法:**

      toLowerCase() 和 toUpperCase() 方法可以改变大小写：
      
      查找子字符串 :  substr => 字符串
      str.indexOf(substr, [pos])。没有找到返回 -1，否则返回匹配成功的位置。
      str.lastIndexOf(substr, [position])，默认它从字符串的末尾开始搜索到开头。或者从postition往前
      str.includes(substr, [pos]) 返回 true/false。如果我们只需要检测匹配时使用
      
      获取子字符串 : 
      str.slice(start [, end] 返回字符串从 start（但不包括）end 的部分。如果没有第二个参数，slice 会一直运行到字符串末尾, start/end 是负值。起始位置从字符串结尾计算
      str.substring(start [, end])返回字符串在 start 和 end 之间 的部分。它允许start大于end。
      str.substr(start [, length])返回字符串从 start 开始的给定 length 的部分.
      
      匹配字符串 :  startsWith() endsWith() 

- 所有常用的字符都是一个 2 字节(16byte)的代码,所以稀有的符号被称为“代理对”的一对 2 字节的符号编码。

- > `String.fromCodePoint` 和 `str.codePointAt` 是几种处理代理对的少数方法, 处理特殊符号

#### 数组 数组方法

- ##### 数组声明 :

  ```
  let arr = [ ]
  let arr =new Array() 用单个参数可以指定一个长度固定,没有任何项的数组
  ```
  
> 数组可以存储任何类型的元素,以逗号结尾.
  >
  > length属性的值是数组中元素的总个数, 
  >
  > 可以用 alert 来显示整个数组



- ##### 数组使用 : 

- 队列(queue)是最常见的使用数组的方法之一。

  > push  :末端添加
  >
  >  shift : 取出首端  

- 数组作为栈使用:

  > push 末端添加
  >
  > pop 末端取出

- 数组可以同时当做队列 栈使用, 允许在首段末段/添加删除元素, 我们叫这样的数据结构成为双端队列

- > unshift : 首端添加元素
  >
  > `push` 和 `unshift` 方法都可以一次添加多个元素：
>
  > `shift` 和`pop`方法,都没有参数

- ##### 数组内部:

- **数组是一种特殊的对象**。使用方括号来访问属性 `arr[0]` 实际上是来自于对象的语法. 它们扩展了对象，提供了特殊的方法来处理有序的数据集合以及 length 属性。但从本质上讲，它仍然是一个对象.

- > 数组的行为很像一个对象,  比如可以直接使用  "=" , 进行引用复制,两个变量引用一个数组
  
  > 我们可以像常规对象一样使用数组, 添加属性等等, 但是js针对数组有优化,如果像常规对象使用, 这种优化就不再使用

- ##### 数组 length   toString

- > 当我们修改数组的时候，length属性会自动更新.
  >
  > length可写,如果我们减少它，数组就会被截断.清空数组 :  arr.length = 0;
  >
  > 数组有自己的 `toString` 方法的实现，会返回以逗号隔开的元素列表。

  

- ##### 数组循环 : for..in  for..of  for()

- > 建议使用for ..of  ,   for()
  >
  >  for..in 有很多潜在问题 : 会遍历所有属性(对于类数组不好) ,遍历数组比遍历普通对象要慢

- ##### 多维数组

- >```javascript
  >let matrix = [
  >[1, 2, 3],
  >[4, 5, 6],
  >[7, 8, 9]
  >];
  >```
- ##### 数组方法

- ##### 从数组中添加,删除,插入元素:

- >  arr.splice(index[, deleteCount, elem1, ..., elemN])   //既可以单独删除 也可以删除插入
  >
  > 返回已删除的元素数组, 插入是插入到索引之前
  >
  > 使用delete 可以删除数组中的值,但不会自动释放占据的空间 

- ##### 数组部分拷贝: 

- arr.slice([ start], [ end]) 返回一个新数组，新数组的索引是 start 到 end（不包括 end）   

- > arr.slice()  不带参数,会创建一个arr数组副本

- arr.concat(arg1, arg2...) 创建一个新数组，其中包含来自于其他数组和其他项的值。arg 是一个数组，那么其中的所有元素都会被复制。否则，将复制参数本身, 

- >concat() 方法返回的数组结果, 不会进行排序
  >
  >如果类似数组的对象具有 `Symbol.isConcatSpreadable` =true 属性，那么它就会被 `concat` 当作一个数组来处理


> slice  concat 方法的拷贝注意事项: 
>
> 数组里是 对象引用 , `concat`将对象引用复制到新数组中, 原始数组和新数组都引用相同的对象
>
> 数组里是 数据类型如字符串，数字和布尔（不是String，Number 和 Boolean 对象）,会复制一份给新数组
>
> > 当用来拷贝二维数组时注意,不要直接进行拷贝,因为多维数组里的数组,被当成了对象引用

- ##### 数组遍历

- arr.forEach 方法允许为数组的每个元素都运行一个函数。不会改变数组的值,也没有返回值

> ```javascript
> arr.forEach(callback(currentValue [, index [, array]])[, thisArg])
> {
> //  index : 正在处理元素的索引, array 正在处理的数组
> }
> //该函数的结果（如果它有返回）会被抛弃和忽略。
> ```

  

- ##### 数组搜索 

  > arr.indexOf、arr.lastIndexOf 和 arr.includes 方法 检查内容返回索引 ,没有返回-1
  >
  > 与字符串操作具有相同的语法，并且作用基本上也与字符串的方法相同

- arr.find 方法, 找到特定条件的对象: find 方法搜索的是使函数返回 true 的第一个（单个）元素。

  ```javascript
  let result = arr.find(function(item, index, array) {
  
  item 是元素。index 是它的索引。 array 是数组本身。
  
  }
  ```

- arr.findIndex 方法（与 arr.find 方法）基本上是一样的，但它返回找到元素的索引，而不是元素本身。并且在未找到任何内容时返回 -1。

- arr.filter(fn) 方法   : 返回匹配元素组成的数组

- ```javascript
   arr.filter(function(item, index, array){}
   ```



- ##### 数组转换(分割 联结 倒置) 排序

>  `arr.map` 方法,它对数组的每个元素都调用函数，callback 每次执行后的返回值（包括 undefined）组合起来形成一个新数组, 并返回这个新数组。
>
>  ```javascript
>  let result = arr.map(function(item, index, array)
>   {
>      // 返回新值而不是当前元素, arr也不会被改变
>  }
>    //如果函数没有返回值,则新数组默认都是undefined
>  ```
>
>  `arr.sort(fn)` 方法对数组进行排序，更改元素的顺序,不会生成一个新数组.它还返回排序后的数组
>  排序方法默认按照**字符串**进行排序。需要使用自定义排序顺序,提供一个fn函数作为参数
>
>  `arr.reverse( )`方法  返回arr颠倒过的数组

- 字符串转换为数组: 

>  str.split(delim) 方法。它通过给定的分隔符 delim 将**字符串**分割成一个数组。
>
>  有一个可选的第二个数字参数 —— 对数组长度的限制

   > 调用带有空格的 `split(' ')`，方法, 会将字符串拆分为字母数组

- 数组转化为字符串:

   > arr.join(glue) 与 split 相反。它会在它们之间创建一串由 glue 粘合的 arr 项。

- [arr.reduce](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Array/reduce) 方法和 [arr.reduceRight](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Array/reduceRight) 方法

- > 遍历数组, 然后计算单个值,并把结果传递给下一次计算
  >
  > arr.reduce(function(accumulator, item, index, array) {  // ... }, [initial]);
  >
  > `accumulator` —— 是上一个函数调用的结果, 本质上是一个累加器




- **判断是否为数组** : 数组是基于对象的，不构成单独的语言类型。所以 typeof 不能帮助从数组中区分出普通对象：

- > Array.isArray(value)。如果 value 是一个数组，则返回 true；否则返回 false。




> 几乎所有调用函数的数组方法(需要函数作为参数) ,都接受一个可选的附加参数 thisArg, 
>
> 传递对象方法,往往会丢失this,  这个参数在func 中时对象名, 绑定了this指针
>
> 往往这个附加参数,可以被箭头函数/ 匿名函数替代



#### 可迭代对象

- 任何对象都可以被定制为可迭代的,换句话说,都可以被定制为在for..of循环中使用

  > 内置的可迭代对象例如字符串和数组，都实现了 Symbol.iterator。

- 通过设置**obj[ Symbol.iterator]** 属性, 来进行定制对象可迭代的迭代过程

- > obj[Symbol.iterator] =func方法 , 会被 for..of 自动调用, 它必须返回一个 **迭代器（iterator）** —— 一个有 `next` 方法的对象。
  >
  > 当 `for..of` 循环希望取得下一个数值，它就调用这个对象的 `next()` 方法。
  >
  > next()` 方法返回的结果的格式必须是 `{done: Boolean, value: any}`，当 `done=true` 时，表示迭代结束，否则 `value` 是下一个值。

> 在对象内部写 [Symbol.iterator](), next() 函数时, 当同一个对象运行两个for..of 会共享迭代状态
>
> 我们还可以显式调用迭代器, 对象名[Symbol.iterator] ()会返回一个迭代器对象



- ##### 可迭代（iterable）和类数组（array-like）

- > - **Iterable** 如上所述，是实现了 `Symbol.iterator` 方法的对象。
  > - **Array-like** 是有索引和 `length` 属性的对象，所以它们看起来很像数组
  >
  > > 一个可迭代对象也许不是类数组对象。反之亦然，类数组对象可能不可迭代

```javascript
类数组 : 有索引和 length 属性
let arrayLike = {
  0: "Hello",
  1: "World",
  length: 2
};
```

- **转化可迭代对象, 类数组 为数组:**

- > 全局方法 Array.from 可以接受一个可迭代或类数组的值，并从中获取一个“真正的”数组
  >
  > **Array.from(obj[, mapFn, thisArg])**可选的第二个参数 mapFn 可以是一个函数，该函数会在对象中的元素被添加到数组前，被应用于每个元素

  

#### Map Set (映射和集合)

- Map 是一个带键的数据项的集合，就像一个 Object 一样。 但是它们最大的差别是 Map **允许任何类型**的键（key）

- > Objct 的键只允许 是字符串 或者 Symbol 类型, 如果是其他的也会转化为字符串类型

- **定义 使用map** : 

  ```javascript
  let map = new Map();
  map.set(key, value);  —— 根据键存储值
  map.get(key) ;       —— 根据键来返回值如果 map 中不存在对应的 key，则返回 undefined。
  map.has(key) —— 如果 key 存在则返回 true，否则返回 false。
  map.delete(key) —— 删除指定键的值。
  map.clear() —— 清空 map。
  map.size —— 返回当前元素个数。
  ```

- > map[ key] 不是使用 Map 的正确方式 , 因为map的键值可以存储任何类型,包括对象, 
  >
  > 所以当用[ object] 调用时会发生错误, 不存在对象键.
  >
  > 每一次 map.set 调用都会返回 map 本身，所以我们可以进行**“链式”调用**



- **Map 迭代** :(使用for..of 结合以下函数进行遍历)

  ```javascript
  map.keys() —— 返回含有所有的键 的 map iterable  
  map.values() —— 返回含有所有的值 的 map iterable  
  map.entries() —— 遍历并返回所有的实体 [key, value], for..of 在默认情况下使用的就是这个。
  
  map.forEach( (value, key, map){}  //与 Array 类似
  ```

- >迭代的顺序与插入值的顺序相同。与普通的 Object 不同，Map 保留了此顺序。

  

- **通过可迭代对象 数组创建map**

- ```javascript
  传入一个带有键值对的数组初始化
  let map = 
  new Map([ 
  ['1',  'str1'], 
  [1,    'num1'], 
  [true, 'bool1'] 
  ]); 
  ```

- > 使用内建方法 Object.entries(obj)，将对象转换为键/值对数组，该数组格式完全按照 Map 所需的格式。
  >
  > *let map = new Map(Object.entries(obj));*

- **从Map 创建对象**:

- `Object.fromEntries()` ：给定一个具有 [key, value] 键值对的数组，它会根据给定数组创建一个对象

- > 从 Map 创建对象 : let obj = Object.fromEntries(map.entries()); // 创建一个普通对象;
  >
  > 简化为:  let obj = Object.fromEntries(map);   
  >
  > 原因 :  Object.fromEntries 期望得到一个可迭代对象作为参数，而不一定是数组

  ##### set


- set : Set 是一个特殊的类型集合 —— “值的集合”（没有键），它的每一个值只能出现一次。

- **set方法** : 

  - `new Set(iterable)` —— 创建一个 `set`，如果提供了一个 `iterable` 对象（通常是数组），将会从数组里面复制值到 `set` 中。
  - `set.add(value)` —— 添加一个值，返回 set 本身
  - `set.delete(value)` —— 删除值，如果 `value` 在这个方法调用的时候存在则返回 `true` ，否则返回 `false`。
  - `set.has(value)` —— 如果 `value` 在 set 中，返回 `true`，否则返回 `false`。
  - `set.clear()` —— 清空 set。
  - `set.size` —— 返回元素个数。
  
  


- > 重复使用同一个值调用 set.add(value) 并不会发生什么改变,set中每个值还是一个


```javascript
我们可以使用 for..of 或 forEach 来遍历 Set：
set.forEach((value, valueAgain, set) => {
  alert(value);
});

set.keys(); 遍历并返回所有的值
set.values(); 与 set.keys() 作用相同，这是为了兼容 Map
set.entries();  遍历并返回所有的实体[value, value]，它的存在也是为了兼容 Map。
```

> forEach 的回调函数有三个参数：一个 value，然后是 同一个值 valueAgain，最后是目标对象。没错，同一个值在参数里出现了两次。  是为了与 Map 兼容。



#### WeakMap and WeakSet

- 如果把一个对象放入到数组中，那么只要这个数组存在，那么这个对象也就存在，即使没有其他对该对象的引用。类似的，如果我们使用对象作为常规 Map 的键，那么当 Map 存在时，该对象也将存在.

- WeakMap 在这方面有着根本上的不同。它不会阻止垃圾回收机制对作为键的对象（key object）的回收。

- WeakMap 和 Map 的第一个不同点就是**，WeakMap 的键必须是对象**，不能是原始值

- WeakMap **不支持迭代** 以及 keys()，values() 和 entries() 方法。所以没有办法获取 WeakMap 的所有键或值。

- > - `weakMap.get(key)`
  > - `weakMap.set(key, value)`
  > - `weakMap.delete(key)`
  > - `weakMap.has(key)`
  >
  > `WeakMap` 只有以上的方法  支持size() ???

- > 当需要对象 和它附加的数据共存亡时,可以用到weakmap

- WeakSet **只能存储对象**,**不支持迭代** 无法使用size 和迭代用函数 keys()

- > `WeakSet` 支持 `add`，`has` 和 `delete` 方法


- >WeakMap 和 WeakSet 最明显的局限性就是不能迭代，并且无法获取所有当前内容。那样可能会造成不便，但是并不会阻止 WeakMap/WeakSet 完成其主要工作 — 成为在其它地方管理/存储“额外”的对象数据。
  >
  >`WeakMap` 和 `WeakSet` 被用作“主要”对象存储之外的“辅助”数据结构。一旦将对象从主存储器中删除，如果该对象仅被用作 `WeakMap` 或 `WeakSet` 的键，那么它将被自动清除。


#### Object.keys，values，entries

-  这些方法是通用的，有一个共同的约定来将它们用于各种数据结构。如果我们创建一个我们自己的数据结构，我们也应该实现这些方法。

>  它们支持： Map  Set Array  
>
>  普通对象也支持类似的方法，但是语法上有一些不同  :map.keys()    ==>	Object.keys(obj)， 
>
>  为了自己创建的对象方法不会被覆盖,使用Object.keys(obj)更灵活

- 对于普通对象，下列这些方法是可用的 :

- > - [Object.keys(obj)](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Object/keys) —— 返回一个包含该对象所有的键的数组。
  > - [Object.values(obj)](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Object/values) —— 返回一个包含该对象所有的值的数组。
  > - [Object.entries(obj)](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Object/entries) —— 返回一个包含该对象所有 [key, value] 键值对的数组。
  >
  > **Object.keys/values/entries 会忽略 symbol 属性**, 如果我们也想要 Symbol 类型的键，那么这儿有一个单独的方法 [Object.getOwnPropertySymbols](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Object/getOwnPropertySymbols)，它会返回一个只包含 Symbol 类型的键的数组

- 转换对象,调用数组方法 :

- >对象缺少数组存在的许多方法,我们可以转换对象来调用
  >
  >1.使用 Object.entries(obj) 从 obj 获取由键/值对组成的数组。
  >2.对该数组使用数组方法
  >
  >3.对结果数组使用 Object.fromEntries(array) 方法，将结果转回成对象。

#### 解构赋值

> 基本语法 : 
>
> 1. let {prop : varName = default, ...rest} = object
> 2. let [item1 = default, item2, ...rest] = array

- 对象让我们能够创建通过键来存储数据项的单个实体，数组则让我们能够将数据收集到一个有序的集合中。
  但是，当我们把它们传递给函数时，它可能不需要一个整体的对象/数组，而是需要单个块。

- 解构赋值 是一种特殊的语法，它使我们可以将数组或对象“拆包”为到一系列变量中，因为有时候使用变量更加方便

- “解构”并不意味着“破坏” 因为它通过将结构中的各元素复制到变量中来达到“解构”的目的。但数组本身是没有被修改的。

```javascript
1. 数组解构
let arr = ["Ilya", "Kantor"]
let [firstName, surname] = arr;   => 相当于 let firstName = arr[0]; let surname = arr[1];

let [firstName, , title] = ["Julius", "Caesar", "Consul", "of the Roman Republic"];///数组中不想要的元素也可以通过添加额外的逗号来把它丢弃
alert( title ); // Consul

2. 可迭代对象解构      //等号右侧可以是任何可迭代对象,而不仅限于数组,  等号左侧使用任何“可以被赋值的”东西

let [one, two, three] = new Set([1, 2, 3]);
let user = {};
[user.name, user.surname] = "Ilya Kantor".split(' ');

```

- > 使用 .entries() 转换对象为键值对数组,可以进行遍历操作
  >
  > ```java
  > for (let [key, value] of Object.entries(user)) {
  > 
  >  alert(`${key}:${value}`); // name:John, then age:30
  > 
  >  }
  > ```

- 交换变量

- > ```javascript
  > let guest = "Jane"; let admin = "Pete";   // 交换值：让 guest=Pete, admin=Jane 
  > [guest, admin] = [admin, guest];
  > ```

- 把后面的元素收集成数组,通过 '...'

- > ```javascript
  > let [name1, name2, ...rest] = ["Julius", "Caesar", "Consul", "of the Roman Republic"]; 
  > alert(rest[0]); // Consul   rest 的值会被组成一个数组
  > 
  > ```

- 如果我们想要一个“默认”值给未赋值的变量，我们可以使用 = 来提供,如果赋值语句中，变量的数量多于数组中实际元素的数量，赋值不会报错。未赋值的变量被认为是 undefined

- >let [name = "Guest", surname = "Anonymous"] = [ "Julius"];




- ##### 对象解构

- ```javascript
  let {var1, var2} = {var1:"er", var2: "fef"};
  
  let {title, width, height} = options; // options 对象名
  ```

- 变量的顺序并不重要,会根据两侧相同的名称赋值, 

- > 如果我们想把一个属性赋值给不同名字的变量:
  >
  >  let {width: w, height: h, title} = options;   // 把 options.width 属性赋值给变量 w

- 对象解构 也可以提供默认值 , 可以把剩余元素通过'...' 收集成一个对象

- 不使用let 进行 对象解构时 会出现错误, 原因:  因为{}会被解释成普通代码块 而不是解析语法

```javascript
// 这一行发生了错误
{title, width, height} = {title: "Menu", width: 200, height: 100};
```

- > 解决方法 : 要把整行用括号括住

- ##### 嵌套解构

- > 如果一个对象或数组嵌套了其他的对象和数组,我们可以在等号左侧来提取数据
  >
  > ```javascript
  > let {
  >   size: { // 把 size 赋值到这里
  >     width,
  >     height
  >   },
  >   items: [item1, item2], // 把 items 赋值到这里
  >   title = "Menu" // 在对象中不存在（使用默认值）
  > } = options;
  > 
  > alert(title);  // Menu
  > alert(width);  // 100
  > alert(height); // 200
  > ```

- 智能函数参数: 

- > 对于拥有很多参数的函数,在调用时我们会传递一个对象,而不是所有参数,
  >
  > 对象会进行解构,赋值给函数中的参数,  调用函数时,参数直接传一个对象即可


#### 日期 时间 

- 创建Date对象:

> new Date() 
>
> new Date(datestring)   
>
> new Date(year, month, date, hours, minutes, seconds, ms) 

- Date常用方法:

> getFullYear()  获取年份（4 位数）
>
> [getMonth()](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Date/getMonth) 获取月份，**从 0 到 11**。
>
> [getDate() ](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Date/getDate)获取当月的具体日期，从 1 到 31，这个方法名称可能看起来有些令人疑惑。
>
> [getHours()](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Date/getHours)，[getMinutes()](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Date/getMinutes)，[getSeconds()](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Date/getSeconds)，[getMilliseconds()](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Date/getMilliseconds)获取相应的时间组件。
>
> [getDay()](https://developer.mozilla.org/zh/docs/Web/JavaScript/Reference/Global_Objects/Date/getDay)获取一周中的第几天，从 `0`（星期日）到 `6`（星期六）。第一天始终是星期日，在某些国家可能不是这样的习惯，但是这不能被改变

- **以上的所有方法返回的组件都是基于当地时区的。** 得到与当地时区的 UTC 对应项只需要在 `"get"` 之后插入 `"UTC"` 即可。
- set..方法和get基本一致,不在赘述

- 自动校准 ,是 Date 对象的一个非常方便的特性,在设置超过范围的值,他会自动调整,比如闰年

- 日期转换为数字 ,当 Date 对象被转化为数字时，得到的是对应的时间戳(从1970到现在的毫秒数), 日期对象前面有+号完成转换

- >日期可以相减，相减的结果是以毫秒为单位时间差。

- 只想要得到时间间隔使用,  Date.now() 会返回当前的时间戳。两个相减得到时间间隔

- Date.parse(str) 方法可以从一个字符串中读取日期。返回时间戳(毫秒数);

- > let date = new Date( Date.parse('2012-01-26T13:51:50.417-07:00') );


####  JSON(之后在研究)

- > JSON.stringify(obj) 将对象转换为 JSON。 
  >
  >  
  >
  > ```javascript
  > let json = JSON.stringify(value[, replacer, space])
  > 
  > /*replacer :如果该参数是一个函数，则在序列化过程中，被序列化的值的每个属性都会经过该函数的转换和处理；如果该参数是一个数组，则只有包含在这个数组中的属性名才会被序列化到最终的 JSON 字符串中；如果该参数为 null 或者未提供，则对象所有的属性都会被序列化
  > space :指定缩进用的空白字符串，用于美化输出（pretty-print）*/
  > ```
  >
  > JSON.parse 将 JSON 转换回对象。 返回一个js对象
  >
  > ```javascript
  > let value = JSON.parse(str, [reviver]);
  > //reviver : 转换器, 如果传入该参数(函数)，可以用来修改解析生成的原始值
  > ```

  

- JSON 是语言无关的纯数据规范，因此一些特定于 JavaScript 的对象属性会被 JSON.stringify 跳过,例如:

  函数属性（方法）。 Symbol 类型的属性。存储 undefined 的属性。

- JSON 支持嵌套对象转换, 但是不能有循环引用

- 像 `toString` 进行字符串转换，对象也可以提供 `toJSON` 方法来进行 JSON 转换。如果可用，`JSON.stringify` 会自动调用它。

- > JSON 支持以下数据类型：
  >
  > - Objects `{ ... }`
  > - Arrays `[ ... ]`
  > - Primitives：
  >   - strings，
  >   - numbers，
  >   - boolean values `true/false`，
  >   - `null`。

  


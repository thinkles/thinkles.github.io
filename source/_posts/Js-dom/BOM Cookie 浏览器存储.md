---
tags: [BOM,Cookie,浏览器存储]
categories: Javascript
title: BOM-cookie-存储
---




### BOM ---window对象
- 一个浏览器窗口就是一个window对象

- window对象有多个子对象

- > - document 操作页面元素
  > - location 地址对象,
  > - navigator 用于获取浏览器版本信息 
  > - history 历史对象操作浏览记录
  > - screen 操作屏幕宽度高度  

- > ​    以上都是window 的属性, 返回一个对象 , 可以不用window前缀

- 由于windwo对象下面的子对象都是操作浏览器窗口,所以我们称为BOM

  

- location对象 属性: 

- >  href 当前页面地址  
  >
  > search 当前地址?后内容
  >
  >  hash 当前页面地址#后面的内容  
  >
  > pathname
>
  > location.assign() 方法加载新的文档。

- history 属性:

- > history.forward() - 与在浏览器中点击向前按钮相同
  >
  > history.back() - 与在浏览器点击后退按钮相同



- > screen.availWidth - 可用的屏幕宽度
    >
    > screen.availHeight - 可用的屏幕高度



- **弹窗**

- > window.open('https://javascript.info/') : 打开一个具有给定 URL 的新窗口。大多数现代浏览器都配置为打开新选项卡
  >
  > 现在大多数浏览器都会通过阻止弹窗来保护用户。所以使用open() 会被拦截弹窗

- > 弹窗是一个独立的窗口，具有自己的独立 JavaScript 环境。因此，使用弹窗打开一个不信任的第三方网站是安全的。

- > `window.open(url, name, params) `   调用会返回对新窗口的引用。它可以用来操纵弹窗的属性，更改位置，甚至更多操作
  >
  > name : 新窗口的名称, 每个窗口都有一个 `window.name`
  >
  > params : 新窗口的配置字符串。它包括设置，用逗号分隔。参数之间不能有空格，例如：`width:200,height=100`。
  >
  > `params` 的设置项：
  >
  > - 位置:
  >   - `left/top`（数字）—— 屏幕上窗口的左上角的坐标。这有一个限制：不能将新窗口置于屏幕外（offscreen）。
  >   - `width/height`（数字）—— 新窗口的宽度和高度。宽度/高度的最小值是有限制的，因此不可能创建一个不可见的窗口。
  > - 窗口功能：
  >   - `menubar`（yes/no）—— 显示或隐藏新窗口的浏览器菜单。
  >   - `toolbar`（yes/no）—— 显示或隐藏新窗口的浏览器导航栏（后退，前进，重新加载等）。
  >   - `location`（yes/no）—— 显示或隐藏新窗口的 URL 字段。Firefox 和 IE 浏览器不允许默认隐藏它。
  >   - `status`（yes/no）—— 显示或隐藏状态栏。同样，大多数浏览器都强制显示它。
  >   - `resizable`（yes/no）—— 允许禁用新窗口大小调整。不建议使用。
  >   - `scrollbars`（yes/no）—— 允许禁用新窗口的滚动条。不建议使用。

- 从窗口访问弹窗 , 也可以从弹窗访问窗口

- 关闭窗口: `win.close()`。检查一个窗口是否被关闭：`win.closed`

- > 如果 `window` 不是通过 `window.open()` 创建的，那么大多数浏览器都会忽略 `window.close()`。因此，`close()` 只对弹窗起作用。





##### Cookie

- Cookie 是直接存储在浏览器中的一小串数据。它们是 HTTP 协议的一部分

- > 当服务器收到 HTTP 请求时，服务器可以在响应头里面添加一个 [`Set-Cookie`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Set-Cookie) 选项。浏览器收到响应后通常会保存下 Cookie，之后对该服务器每一次请求中都通过 [`Cookie`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Cookie) 请求头部将 Cookie 信息发送给服务器。

  

- 使用 `document.cookie` 属性从浏览器访问 cookie

- > 写入 `document.cookie`。但这不是一个数据属性，它是一个访问器（getter/setter）
  >
  > **对 `document.cookie` 的写入操作只会更新其中提到的 cookie，而不会涉及其他 cookie。**
  >
  > document.cookie = "user=John"; // 只会更新名称为 user 的 cookie

- cookie 的名称和值可以是任何字符，为了保持有效的格式，它们应该使用内建的 `encodeURIComponent` 函数对其进行转义

- > ```javascript
  > let name = "my name"; let value = "John Smith" 
  > // 将 cookie 编码为 my%20name=John%20Smith 
  > document.cookie = encodeURIComponent(name) + '=' + encodeURIComponent(value);
  > ```

- Cookie属性:

- >- **`path=/mypath`** 我们应该将 `path` 设置为根目录：`path=/`，以使 cookie 对此网站的所有页面可见。
  >
  >- **`domain=site.com`**默认情况下，cookie 只有在设置的域下才能被访问到,
  >
  >  >  但是棘手的是，我们在子域 `forum.site.com` 下也无法获取它！如果我们想要批准像 `forum.site.com` 这样的子域访问 cookie，将 `domain` 选项显式地设置为根域：`domain=site.com`
  >  >
  >  > document.cookie = "user=John; domain=site.com"
  >
  
- >  - expires，max-age
  >
  >  - >默认情况下，如果一个 cookie 没有设置这两个参数中的任何一个，那么在关闭浏览器之后，它就会消失。此类 cookie 被称为 "session cookie”。
  >    >
  >    >为了让 cookie 在浏览器关闭后仍然存在，我们可以设置 `expires` 或 `max-age` 选项中的一个。
  >    >
  >    >- **`expires=Tue, 19 Jan 2038 03:14:07 GMT`**
  >    >- 日期必须完全采用 GMT 时区的这种格式。我们可以使用 `date.toUTCString` 来获取它
  >    >
  >
  >  - > **`max-age=3600`**
  >    >
  >    > `expires` 的替代选项，具指明 cookie 的过期时间距离当前时间的秒数。
  >    >
  >    > // cookie 会在一小时后失效 
  >    >
  >    > document.cookie = "user=John; max-age=3600";
  >
  >  - **`secure`**
  >
  >  - > Cookie 应只能被通过 HTTPS 传输。
  >    >
  >    > **默认情况下，如果我们在 `http://site.com` 上设置了 cookie，那么该 cookie 也会出现在 `https://site.com` 上，反之亦然。**
  >    >
  >    > 也就是说，cookie 是基于域的，它们不区分协议。
  >
  >  -  cookie samesite 选项
  >
  >  - > Cookie 的 `samesite` 选项提供了另一种防止此类攻击的方式，跨网站请求伪造（Cross-Site Request Forgery，简称 XSRF）攻击
  >    >
  >    > - **`samesite=strict`（和没有值的 `samesite` 一样)**
  >    >
  >    > - **`samesite=lax`**
  >    >
  >    > - > `samesite` 会被到 2017 年左右的旧版本浏览器忽略（不兼容
  
  
  
  
  
  - 有关 cookie 操作的函数，比手动修改 `document.cookie` 方便得多。有很多这种 cookie 库，
  
    



#### LocalStorage，sessionStorage
- Web 存储对象 localStorage 和 sessionStorage 允许我们在浏览器上保存键/值对。
在页面刷新后（对于 sessionStorage）甚至浏览器完全重启（对于 localStorage）后，数据仍然保留在浏览器中。


- 我们已经有了 cookie。为什么还要其他存储对象呢？
- > 与 cookie 不同，Web 存储对象不会随每个请求被发送到服务器。(节省开销)
  >
  > 还有一点和 cookie 不同，服务器无法通过 HTTP header 操纵存储对象。一切都是在 JavaScript 中完成的。
  >
  > 存储绑定到源（域/协议/端口三者）。也就是说，不同协议或子域对应不同的存储对象，它们之间无法访问彼此数据。
  
- 

- 两个存储对象都提供相同的方法和属性：

- > 
  >
  > - `setItem(key, value)` —— 存储键/值对。
  > - `getItem(key)` —— 按照键获取值。
  > - `removeItem(key)` —— 删除键及其对应的值。
  > - `clear()` —— 删除所有数据。
  > - `key(index)` —— 获取该索引下的键名。
  > - `length` —— 存储的内容的长度。

- `localStorage` 最主要的特点是：

- > - 在同源的所有标签页和窗口之间共享数据。
  > - 数据不会过期。它在浏览器重启甚至系统重启后仍然存在。

- > 我们还可以像使用一个普通对象那样，读取/设置键,这是历史原因造成的，并且大多数情况下都可行，但通常不建议这样做: 用户保存的数据可能是关键字, 读取出来之后导致错误

- > `Object.keys` 只返回属于对象的键，会忽略原型上的。可以配合他进行键值迭代



- 键和值都必须是字符串, 如果是任何其他类型，例数字或对象，它会被自动转换为字符串

- > 我们可以使用 `JSON` 来存储对象,使用JSON.srtingify,转换为字符串,使用时再换出来



**sessionStorage:**

- `sessionStorage` 对象的使用频率比 `localStorage` 对象低得多。

  属性和方法是相同的，但是它有更多的限制：

  > - 数据只存在于当前浏览器标签页。具有相同页面的另一个标签页中将会有不同的存储。
  >
  >   但是，它在同一标签页下的 iframe 之间是共享的（假如它们来自相同的源）。
  >
  > - 数据在页面刷新后仍然保留，但在关闭/重新打开浏览器标签页后不会被保留。

- 当 `localStorage` 或 `sessionStorage` 中的数据更新后，[storage](https://www.w3.org/TR/webstorage/#the-storage-event) 事件就会触发，它具有以下属性：

- > - `key` —— 发生更改的数据的 `key`（如果调用的是 `.clear()` 方法，则为 `null`）。
  >
  > - `oldValue` —— 旧值（如果是新增数据，则为 `null`）。
  >
  > - `newValue` —— 新值（如果是删除数据，则为 `null`）。
  >
  > - `url` —— 发生数据更新的文档的 url。
  >
  > - `storageArea` —— 发生数据更新的 `localStorage` 或 `sessionStorage` 对象。
  >
  > 



- >   重要的是：该事件会在所有可访问到存储对象的 `window` 对象上触发
  >
  >   如果两个窗口都在监听 `window.onstorage` 事件，那么每个窗口都会对另一个窗口中发生的更新作出反应

  

#### **indexDB:**

- IndexedDB 是一个浏览器内置的数据库，它比 `localStorage` 强大得多。

- > 通过支持多种类型的键，来存储几乎可以是任何类型的值。
  >  支撑事务的可靠性。
  >  支持键范围查询、索引。
  >  和 `localStorage` 相比，它可以存储更大的数据量。

  - > 对于传统的 客户端-服务器 应用，这些功能通常是没有必要的。IndexedDB 适用于离线应用，可与 ServiceWorkers 和其他技术相结合使用。
  
- > 基本操作就是, 根据监听事件,对数据库进行增删改查, 就像操作数据库一样, 所以传统的客户端-服务器 应用,用不到



- 语法: 

- > let openRequest = indexedDB.open(name, version);
  >
  > - `name` —— 字符串，即数据库名称。
  > - `version` —— 一个正整数版本，默认为 `1`（下面解释）。

- 调用之后会返回 `openRequest` 对象，我们需要监听该对象上的事件：

- > - `success`：数据库准备就绪，`openRequest.result` 中有了一个数据库对象“Database Object”，使用它进行进一步的调用。
  > - `error`：打开失败。
  > - `upgradeneeded`：数据库已准备就绪，但其版本已过时（见下文）。

- 

  

  

  

  

  

  

  
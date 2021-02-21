---

title:  网络请求
date: {{ date }}
updated: {{date}}
tags: ajax
categories: Javascript

---
## 网络请求

- **传统网页的网络请求:**

- 最初加载页面很简单 ---- 你为网站发送一个请求到服务器， 只要没有出错你将会获取资源并显示网页到你的电脑上。

- > 这种方法导致不管 用户是否需要全部数据,都会重新加载整个页面
  >
  > 例如 : 
  >
  > 填写表单时错误,跳转网页后重定向回注册表单,但是信息消失
  >
  > 只需要加载部分内容,(例如:导航栏侧边栏一样不在加载),但仍然会加载整个页面,导致资源浪费



- 这时出现了AJAX技术, 允许网页请求小块数据,实现局部刷新而不是重新加载



## AJAX (Asynchronous JavaScript and XML 早期称呼)

- 使用诸如 **XMLHttpRequest** 之类的API或者**Fetch API** 来实现,这些技术允许网页直接处理对服务器上可用的特定资源的 HTTP 请求，并在显示之前根据需要对结果数据进行格式化。

- **AJAX原理:**

- > 通过xml /fetch api 作为中间件, 帮客户端发送数据,接收数据,使客户端可以做其他的事,数据接收之后再进行处理,不再堵塞等待数据

#### XMLHttpRequest

- 在现代 Web 开发中，出于以下三种原因，我们还在使用 XMLHttpRequest：

- > 我们需要支持现有的使用了 XMLHttpRequest 的脚本。
  > 我们需要兼容旧浏览器，并且不想用 polyfill（例如为了使脚本更小）。
  > 我们需要做一些 fetch 目前无法做到的事情，例如跟踪上传进度。


##### 异步网络请求过程

- 1. *建立连接*:

- ```javascript
  
  let xhr = new XMLHttpRequest();
  
  xhr.open(method, URL, [async, user, password])
// open 调用与其名称相反，不会建立连接。它仅配置请求，而网络活动仅以 send 调用开启, 
  
  xhr.send([body])
  // 可选参数 body 包含了 request body。像例如post传递才需要,get没有body
  ```
  
- > method —— HTTP 方法。通常是 "GET" 或 "POST"。
  >
  > URL —— 要请求的 URL，通常是一个字符串，也可以是 URL 对象。
  > async —— 如果显式地设置为 false，那么请求将会以同步的方式处理，我们稍后会讲到它。
  > user，password —— HTTP 基本身份验证（如果需要的话）的登录名和密码。  如果填写该功能 请求地址会加上该参数   `http://user:pass@domain.com` 类似这种,但是有些浏览器可能不支持 ,
>
  > 这时我们需要在open 方法之后 设置头setRequestHeader("Authorization",..)

- > 如果在 open 方法中将第三个参数 async 设置为 false，那么请求就会以**同步的方式进行**。JavaScript 执行在 send() 处暂停，并在收到响应后恢复执行。这有点儿像 alert 或 prompt 命令



- 2. *监听事件*

- 网络请求是异步的,所以我们需要监听`xhr`事件来获取响应:

- ```js
  xhr.onload = function() {}
  xhr.onprogress = function(event){}
  ```

- >  load —— 当请求完成（即使 HTTP 状态为 400 或 500 等），并且响应已完全下载。
  >   error —— 当无法发出请求，例如网络中断或者无效的 URL。
  >   progress —— 在下载响应期间定期触发，报告已经下载了多少。progerss 事件存在两个对象属性 event.loaded 已经下载了多少字节  event.total 总字节数
  
  
  
- 3. *获得数据*


- 一旦服务器有了响应，我们可以在以下 **xhr 属性中**接收结果：

- ```js
   xml.onload=function(){
     if(xml.status ==200){
       console.log(xml.responseText);
     }
  ```

- >status  : HTTP 状态码（一个数字）：200，404，403 等，如果出现非 HTTP 错误，则为 0。
  >
  >statusText : HTTP 状态消息（一个字符串）：状态码为 200 对应于 OK，404 对应于 Not Found，403 对应于 Forbidden。
  >
  >response（旧脚本可能用的是 responseText）  : 服务器的 response body。
  
  

- **xhr 其他属性:**

- `timeout属性` 指定超时：xhr.timeout = 10000;  // timeout 单位是 ms，此处即 10 秒, 如果在给定时间内请求没有成功执行，请求就会被取消，并且触发 timeout 事件。

- ```
  xhr.timeout = 10000;   // 在建立连接时设置使用, 在监听事件时监听 timeout事件
  ```

  

- **`xhr.responseType 属性`**来设置响应格式：

- ```js
  xhr.responseType = 'json';
  ```

- >""（默认）—— 响应格式为字符串，
  >"text" —— 响应格式为字符串，
  >"arraybuffer" —— 响应格式为 ArrayBuffer（对于二进制数据，请参见 ArrayBuffer，二进制数组），
  >"blob" —— 响应格式为 Blob（对于二进制数据，请参见 Blob），
  >"document" —— 响应格式为 XML document（可以使用 XPath 和其他 XML 方法），
  >"json" —— 响应格式为 JSON（自动解析）。
  >
  >
  >
  >使用response属性, 通过设置responseType 属性,会进行自动解析, 获取响应后可以直接使用
>
  >老的脚本使用 responseTEXT  responseXML 获取响应, 响应只以文本字符/xml的形式返回 , 使用时要进行解析  

  

- **`XMLHttpRequest` 的state属性**

- XMLHttpRequest 的状态（state）会随着它的处理进度变化而变化。可以通过 **xhr.readyState** 来了解当前状态。

- > UNSENT = 0; // 初始状态
  > OPENED = 1; // open 被调用
  > HEADERS_RECEIVED = 2; // 接收到 response header
  > LOADING = 3; // 响应正在被加载（接收到一个数据包）
  > DONE = 4; // 请求完成
  >
  > XMLHttpRequest 对象以 0 → 1 → 2 → 3 → … → 3 → 4 的顺序在它们之间转变。每当通过网络接收到一个数据包，就会重复一次状态 3。我们可以使用 readystatechange 事件来跟踪它们：
- > readystatechange 事件 存在于很老的代码中, 现在被load/error/progress事件替代




- 调用 `xhr.abort()` ,  我们可以随时**终止请求**。



- XMLHttpRequest 允许发送自定义 header，并且可以从响应中读取 header。

  ```javascript
  setRequestHeader(name, value), //使用给定的 name 和 value 设置 request header。
  xhr.setRequestHeader('Content-Type', 'application/json');
  
  getResponseHeader(name) : 获取具有给定 name 的 header（Set-Cookie 和 Set-Cookie2 除外）。
  getAllResponseHeaders()  返回除 Set-Cookie 和 Set-Cookie2 外的所有 response header。
  ```

- > 一些 header 是由浏览器专门管理的，例如 Referer 和 Host 为了用户安全和请求的正确性，XMLHttpRequest 不允许更改它们。
  >
  > XMLHttpRequest 的另一个特点是不能撤销 setRequestHeader。一旦设置了 header，就无法撤销了。其他调用会向 header 中添加信息，但不会覆盖它
  >
  > xhr.setRequestHeader('X-Auth', '123');
  > xhr.setRequestHeader('X-Auth', '456');



##### **post方法 和 FormData 对象:**

- 建立一个 POST 请求，可以使用内建的 FormData 对象来传递信息

- ```javascript
  let formData = new FormData([form]); // 创建一个对象，可以选择从 <form> 中获取数据 
  formData.append(name, value); // 附加一个字段
  
  formData.append("middle", "Lee"); 
  xhr.open('POST', ...);
  xhr.send(formData); // 将表单发送到服务器。
  ```
  
- > send发送 FormData 对象,它会自动以 Content-Type: multipart/form-data 发送数据,不用在设置Content-Type
  

  
- **post方法:**

- 如果我们想用其他的数据类型发送,那就不再使用FormData 对象,使用普通的post即可,但是不要忘记`Content-Type`: 

- > 因为客户端只能发送字符串格式, 设置`Content-Type`告诉服务器你发送的数据是什么格式的，然后服务器才能以对应格式解析请求.  

- ```js
  let json = JSON.stringify({
    name: "John",
    surname: "Smith"
  });
  
  xhr.open("POST", '/submit')
  xhr.setRequestHeader('Content-type', 'application/json; charset=utf-8');
  
  xhr.send(json);
  // 如果我们更喜欢 JSON，那么可以使用 `JSON.stringify` 并以字符串形式发送( 客户端只能发送字符串)但是不要忘记设置 header `Content-Type: application/json`
  // *'Content-type','application/x-www-form-urlencoded'* 发送 'name=fef&age=23'格式数据
  ```



- **.send(body) 方法,它几乎可以发送任何 body**，包括 Blob 和 BufferSource 对象。



#####  **上传进度:**

- 也就是说：如果我们 POST 一些内容，XMLHttpRequest 首先上传我们的数据（request body），然后下载响应。如果我们要上传的东西很大，那么我们肯定会对跟踪上传进度感兴趣。但是 xhr.onprogress 在这里并不起作用。(progress 事件仅在下载阶段触发。)

- xhr.upload。它会生成事件，类似于 xhr，但是 xhr.upload 仅在上传时触发它们：

- > loadstart —— 上传开始。
  > progress —— 上传期间定期触发。
  > abort —— 上传中止。
  > error —— 非 HTTP 错误。
  > load —— 上传成功完成。
  > timeout —— 上传超时（如果设置了 timeout 属性）。
  > loadend —— 上传完成，无论成功还是 error。
  
- ```js
  xhr.upload.onprogress = function(event) {
  alert(`Uploaded ${event.loaded} of ${event.total} bytes`);
  };
  ```

  

- > xhr 自身还具有 上述事件,(不需要调用.upload属性)
  > loadstart —— 请求开始。
    > progress —— 一个响应数据包到达，此时整个 response body 都在 response 中。
    > abort —— 调用 xhr.abort() 取消了请求。
    > error —— 发生连接错误，例如，域错误。不会发生诸如 404 这类的 HTTP 错误。
    > load —— 请求成功完成。
    > timeout —— 由于请求超时而取消了该请求（仅发生在设置了 timeout 的情况下）。
    > loadend —— 在 load，error，timeout 或 abort 之后触发。
    >
    > 
    >
    > error，abort，timeout 和 load 事件是互斥的。其中只有一种可能发生。
  
    

##### **带有凭证的跨源请求:**

- XMLHttpRequest 可以使用和 fetch 相同的 CORS 策略进行跨源请求。

- > 就像 fetch 一样，默认情况下不会将 cookie 和 HTTP 授权发送到其他域。
  >
  > 要启用它们，可以将` xhr.withCredentials` 设置为 true, 服务器还应该在响应中添加 header Access-Control-Allow-Credentials: true。



#### Fetch



#####   网络连接过程

- 1. 建立连接

- 基本语法 :

- ```js
  let promise = fetch(url, [options])
  // options —— 可选参数：method，header , body等,
  //没有 options，那就是一个简单的 GET 请求，下载 url 的内容。
  
  fetch(url, {
      body: JSON.stringify(data), 
      headers: {
        'content-type': 'application/json'
      },
      method: 'POST',
      ....
      })
  
  ```

  

- 2. 获取响应 (promise 的原因不再监听事件)


- > 获取响应通常需要经过两个阶段:
  >
  > **第一阶段**，当**服务器**发送了响应头（response header），fetch 返回的 promise 就使用内建的 **Response class** 对象来对响应头进行解析。 请求成功相当于 promise resolve(Response class),失败相当于reject(Response class)
  >
  > 
  >
  > 在这个阶段，我们可以通过检查响应头，来检查 HTTP 状态以确定请求是否成功，当前还没有响应体
  >
  > 
  >
  > 我们可以在 response 的属性中看到 HTTP 状态：
  >
> - **`status`** —— HTTP 状态码，例如 200。
  > - **`ok`** —— 布尔值，如果 HTTP 状态码为 200-299，则为 `true`。

- ```js
  let response = await fetch(url);
  
  if (response.ok) { // 如果 HTTP 状态码为 200-299
    // 获取 response body（此方法会在下面解释）
    let json = await response.json();
  } else {
    alert("HTTP-Error: " + response.status);
  }
  ```

- > **第二阶段**，为了获取 **response body**，我们需要使用一个其他的方法调用。`Response` 提供了多种基于 promise 的方法，来以不同的格式访问 body：
  >
  > response.text() ——读取 response，并以文本形式返回 response，
  > response.json() —— 将 response 解析为 JSON，
  > response.formData() —— 以 FormData 对象（在 下一章 有解释）的形式返回 response，
  > response.blob() —— 以 Blob（具有类型的二进制数据）形式返回 response，
  > response.arrayBuffer() —— 以 ArrayBuffer（低级别的二进制数据）形式返回 response，
  >
  > 
  >
  > 我们只能选择一种读取 body 的方法。 多种后面的会失效

- ```js
  //使用纯promise语法演示
  
  fetch('https://api.github.com/repos/javascript-tutorial/en.javascript.info/commits')
    .then(response => response.json())
    .then(commits => alert(commits[0].author.login));
  ```

  


- 如果 fetch 无法建立一个 HTTP 请求，例如网络问题，亦或是请求的网址不存在，那么 promise 就会 reject。异常的 HTTP 状态，例如 404 或 500，不会导致出现 error。 导致try catch 无法处理

  > 当接收到一个代表错误的 HTTP 状态码时，从 fetch()返回的 Promise 不会被标记为 reject， 即使该 HTTP 响应的状态码是 404 或 500。相反，它会将 Promise 状态标记为 resolve （但是会将 resolve 的返回值的 ok 属性设置为 false ），仅当网络故障时或请求被阻止时，才会标记为 reject。



##### Response/request header:


- **Response header** :

- response.headers 返回一个类似于 Map 的 header 对象,可以使用Map类似的方法,headers相当于response  class的属性

- ```js
  let response = await fetch('https://api.github.com/repos/javascript-tutorial/en.javascript.info/commits');
  
  // 获取一个 header　
  alert(response.headers.get('Content-Type')); 
  ```

- **request header:**

- 使用fetch方法中option参数 中 headers 选项设置请求头

- ```js
  let response = fetch(protectedUrl, {
    headers: {
      Authentication: 'secret'
    }
  });
  ```

- > 但是有一些我们无法设置的headers , 它们仅由浏览器控制, 详见 [forbidden HTTP headers](https://fetch.spec.whatwg.org/#forbidden-header-name)

##### **POST请求:**

- ```js
  let response = await fetch('/article/fetch/post/user', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json;charset=utf-8'
    },
    body: JSON.stringify(user)
  });
  ```

- 请注意，如果请求的 `body` 是字符串，则 `Content-Type` 会默认设置为 `text/plain;charset=UTF-8`。 所以我们发送数据时要显式的设置  `Content-Type`.

- > **body** —— request body，其中之一：
  >
  > 字符串（例如 JSON 编码的），
  >
  > FormData` 对象，以 `form/multipart` 形式发送数据，
  >
  > Blob`/`BufferSource` 发送二进制数据，
  >
  > URLSearchParams](https://zh.javascript.info/url)，以 `x-www-form-urlencoded` 编码形式发送数据，很少使用



#### **Fetch：下载进度:**

- fetch 方法允许去跟踪下载进度,  到目前为止，fetch 方法无法跟踪 上传 进度。对于这个目的，请使用 XMLHttpRequest

- > 要跟踪下载进度，我们可以使用 response.body 属性。它返回一个 ReadableStream 特殊的对象，它可以逐块（chunk）提供 body 

- `response.body.getReader()`返回读取器,具备两个属性

- > - **`done`** —— 当读取完成时为 `true`，否则为 `false`。
  >
  > - **`value`** —— 字节的类型化数组：`Uint8Array`。
  >
  >   我们在循环中接收响应块（response chunk），直到加载完成，也就是：直到 `done` 为 `true`。
- ```
  // 获得总长度（length）
const contentLength = +response.headers.get('Content-Length');
  // 通过计算读取器的属性 value.length    两者的比例就是下载的进度
  ```
  
  
  

####   **fetch 终止:** 

- `fetch` 返回一个 promise。JavaScript 通常并没有“中止” promise 的概念。使用`AbortController`。它不仅可以中止 `fetch`，还可以中止其他异步任务

- 创建控制器 (作用于普通的异步任务):
  
- ```js
  let controller = new AbortController();
  // controller 它具有单个方法 abort()，和单个属性 signal。
  
  // 当 abort() 被调用时：abort 事件就会在 controller.signal 上触发,  controller.signal.aborted 属性变为 true。
  
  // 在promise中注册事件
   controller.signal.addEventListener('abort', ...);
  
  //调用 controller.abort() ,终止promise
  ```
  

  
- 控制器和fetch一起使用:

- ```js
  let controller = new AbortController();
  fetch(url, {
    signal: controller.signal
  });
  controller.abort();
  // 调用 controller.abort() 来中止fetch 请求
  //当一个 fetch 被中止，它的 promise 就会以一个 error AbortError reject
  ```

  

- AbortController 是可扩展的，它允许一次取消多个 fetch。使用promise.all 并行请求url,将 signal 属性传递给 fetch 选项, 调用abort()函数可以终止请求

- 如果我们有自己的与 fetch 不同的异步任务(自己新建一个promise任务)，我们可以使用单个 AbortController 中止这些任务以及 fetch。

#### Fetch API选项 

- 这些选项 (option) 大多都很少使用。即使跳过本章，你也可以很好地使用 fetch。

- 这是所有可能的 fetch 选项及其默认值（注释中标注了可选值）的完整列表：

- ```javascript
  let promise = fetch(url, {
    method: "GET", // POST，PUT，DELETE，等。
    headers: {
      // 内容类型 header 值通常是自动设置的
      // 取决于 request body
      "Content-Type": "text/plain;charset=UTF-8"
    },
    body: undefined // string，FormData，Blob，BufferSource，或 URLSearchParams
    referrer: "about:client", // 或 "" 以不发送 Referer header，
    // 或者是当前源的 url
    referrerPolicy: "no-referrer-when-downgrade", // no-referrer，origin，same-origin...
    mode: "cors", // same-origin，no-cors
    credentials: "same-origin", // omit，include
    cache: "default", // no-store，reload，no-cache，force-cache，或 only-if-cached
    redirect: "follow", // manual，error
    integrity: "", // 一个 hash，像 "sha256-abcdef1234567890"
    keepalive: false, // true
    signal: undefined, // AbortController 来中止请求
    window: window // null
  });
  ```

  

- referrer，referrerPolicy , 这些选项决定了 fetch 如何设置 HTTP 的 Referer header。

- mode 选项是一种安全措施，可以防止偶发的跨源请求：

- credentials 选项指定 fetch 是否应该随请求发送 cookie 和 HTTP-Authorization header

- 使用 cache 选项可以忽略 HTTP 缓存或者对其用法进行微调

- 通常来说，fetch 透明地遵循 HTTP 重定向，例如 301，302 等。redirect 选项允许对此进行更改

- integrity 选项允许检查响应是否与已知的预先校验和相匹配

- keepalive   当访问者离开我们的网页时 —— 我们希望能够将数据保存到我们的服务器上。keepalive 选项告诉浏览器，即使在离开页面后，也要在后台执行请求。所以，此选项对于我们的请求成功至关重要。



####  跨源请求 和 CORS(跨源资源共享)

- **跨源请求** —— 那些发送到其他域（即使是子域）、协议或端口的请求

- 跨源域资源共享（ [CORS](https://developer.mozilla.org/zh-CN/docs/Glossary/CORS) ）机制允许 Web 应用服务器进行跨源访问控制，从而使跨源数据传输得以安全进行



- **跨源请求方法 :**

- `“JSONP (JSON with padding)”`协议:

- 原理: **script可以具有任何域的 src**, 使用jsonp必须需要服务器端支持,后端接口要改变返回的数据格式才行,数据格式不能是`applicition/ json` 应该为`text/javascript	` , 格式设置错误显示`MIME 类型（“application/json”）不是有效的 JavaScript MIME 类型`

  

- 方法:

- > 1.我们先声明一个全局函数gotWeather(...)来接收数据
  >
  > 2.我们动态创建一个`<script>` 标签 ,属性`src="http://another.com/weather.json?callback=gotWeather"` 的，使用我们的函数名作为它的 `callback` URL-参数
  >
  > 3.**服务器端**处理请求,传递`callback(...)`的**字符串**，实际上传给客户端的是gotWeather字符串, 参数里写我们服务器端想要传递的数据,把字符串传递给客户端
  >
  > 4.当远程脚本加载并执行时，客户端找到`gotWeather` 函数运行，我们也获得了需要的数据。

- 现在仍然有提供这种访问的服务，因为即使是非常旧的浏览器它依然适用



- `CORS`跨源资源共享:

- **有两种类型的跨源请求**：

- > 1. 简单的请求。 
  > 2.  所有其他请求。

-  `简单的请求`:   简单的方法：GET，POST 或 HEAD,  简单的 header —— 仅允许自定义下列 header：
  
- > ​    Accept，
  > ​    Accept-Language，
  > ​    Content-Language，
  > ​    Content-Type 的值为 application/x-www-form-urlencoded，multipart/form-data 或 text/plain。

- > **本质区别在于，可以使用 `<form>` 或 `<script>` 进行“简单请求”，而无需任何其他特殊方法。**因此，即使是非常旧的服务器也能很好地接收简单请求。

  

- `所有其他请求`: 任何其他请求都被认为是“非简单请求”。例如，具有 PUT 方法或 API-Key HTTP-header 的请求就不是简单请求。

- > 当我们尝试发送一个非简单请求时，浏览器会发送一个特殊的“预检（preflight）”请求到服务器 —— 询问服务器，你接受此类跨源请求吗？并且，除非服务器明确通过 header 进行确认，否则非简单请求不会被发送。

  

- **简单请求的CORS:**

- 如果一个请求是跨源的，浏览器始终会向其添加 `Origin` header。`Origin` 包含了确切的源（domain/protocol/port）, 服务器可以检查 `Origin`，如果同意接受这样的请求，就会在响应中添加一个特殊的 header `Access-Control-Allow-Origin`。该 header 包含了允许的源,或者一个星号 `*`。然后响应成功，否则报错。

- 浏览器扮演受信任的中间人的角色:

- > 1.确保发送的跨源请求带有正确的orgin,
  >
  >  2.它检查响应中的许可 Access-Control-Allow-Origin(包含一个源,或者*)，如果存在，则允许 JavaScript 访问响应，否则将失败并报错。


- 对于跨源请求，默认情况下，JavaScript 只能访问“简单” response header：
  
- > Cache-Control
    > Content-Language
    > Content-Type
    > Expires
    > Last-Modified
    > Pragma
    > 访问任何其他 response header 都将导致 error

    
  
- 要授予 JavaScript 对任何其他 response header 的访问权限，服务器必须发送 Access-Control-Expose-Headers  header。




- **非简单的跨源请求:**

- 我们可以使用任何 HTTP 方法：不仅仅是 GET/POST，也可以是 PATCH，DELETE 及其他。可能仍然存在有些 Web 服务将非标准方法视为一个信号,为了避免误解，任何“非标准”请求 —— 浏览器不会立即发出,即在它发送这类请求前，会先发送“预检（preflight）”请求来请求许可。

- 预检请求 :使用 OPTIONS 方法，它没有 body，但是有两个 header：

- > Access-Control-Request-Method header 带有非简单请求的方法。
  > Access-Control-Request-Headers header 提供一个以逗号分隔的非简单 HTTP-header 列表。

- 预检响应 :如果服务器同意处理请求，那么它会进行响应，此响应的状态码应该为 200，没有 body，具有 header：

- > Access-Control-Allow-Methods 必须具有允许的方法。
  > Access-Control-Allow-Headers 必须具有一个允许的 header 列表。

- 预检成功后，浏览器现在发出主请求, 然后才是实际响应

- > 预检响应会缓存一段时间，该时间由 `Access-Control-Max-Age` header 指定,因此，后续请求将不会导致预检

- 实际响应中也要添加`Access-Control-Allow-Origin`。成功的预检并不能免除此要求

- > 预检请求发生在“幕后”，它对 JavaScript 不可见。

  

**带有凭据(cookie https的请求):**

- 默认情况下，由 JavaScript 代码发起的跨源请求不会带来任何凭据（cookies 或者 HTTP 认证（HTTP authentication））。

- > 要在 fetch 中发送凭据，我们需要添加` credentials: "include" `选项, 
  >
  > 如果服务器同意接受 带有凭据 的请求，则除了 Access-Control-Allow-Origin 外，服务器还应该在响应中添加 header Access-Control-Allow-Credentials: true。
- > 对于具有凭据的请求，禁止 Access-Control-Allow-Origin 使用星号 *。它必须有一个确切的源。这是另一项安全措施，以确保服务器真的知道它信任的发出此请求的是谁

  

- 拓展: 服务器方法绕过同源检测

- >  同源策略只存在于ajax,不存在与服务器端开发:
  >
  > 1号网站客户端访问1号网站服务端, 1号服务器端访问2号服务端, 1号服务端把数据响应给1号客户端,此方法可以绕过同源策略

#### FormData

- 表示 HTML 表单数据的对象 ,传递表单中的数据, 带文件或不带文件

- ```js
  let formData = new FormData([form]);
  // 如果提供了 HTML form 元素，它会自动捕获 form 元素字段。
  ```

- > FormData 的特殊之处在于网络方法（network methods），例如 fetch 可以接受一个 FormData 对象作为 body。它会被编码并发送出去，带有 Content-Type: multipart/form-data。从服务器角度来看，它就像是一个普通的表单提交。

  

- 如果内容不是表单,我们也可以new FormData() , 然后通过append()添加,  然后发送,向一个普通表单一样发送

- 使用以下方法修改 FormData 中的字段：

- >formData.append(name, value) —— 添加具有给定 name 和 value 的表单字段，
    >formData.append(name, blob, fileName) —— 添加一个字段，就像它是 <input type="file">，第三个参数 fileName 设置文件名（而不是表单字段名），因为它是用户文件系统中文件的名称，
    >formData.delete(name) —— 移除带有给定 name 的字段，
    >formData.get(name) —— 获取带有给定 name 的字段值，
    >formData.has(name) —— 如果存在带有给定 name 的字段，则返回 true，否则返回 false。
    >
    >

- > 一个表单可以包含多个具有相同 name 的字段，因此，多次调用 append 将会添加多个具有相同名称的字段。

    

-  set 方法，语法与 append 相同。不同之处在于 .set 移除所有具有给定 name 的字段，然后附加一个新字段,它确保了只有一个具有这种 `name` 的字段

- > formData.set(name, value)，
    >
    > formData.set(name, blob, fileName);

    

- 我们也可以使用 for..of 循环迭代 formData 字段：

- > for(let [name, value] of formData) 
  >
  > {  alert(`${name} = ${value}` ); // key1=value1，然后是 key2=value2 }



#### URL 对象
- 内建的 URL 类提供了用于创建和解析 URL 的便捷接口。没有任何一个网络方法一定需要使用 URL 对象，字符串就足够了。

- > new URL(url, [base])
  >
  >  base —— 可选的 base URL：如果设置了此参数，且参数 url 只有路径，则会根据这个 base 生成 URL。

- URL 对象立即允许我们访问其结构组成，因此这是一个解析 url 的好方法 :

- > 通过属性访问URL结构: 
  >
  > href 是完整的 URL，与 url.toString() 相同
  >host : hostname + port   域名加端口号
  > protocol 以冒号字符 : 结尾      协议(http...)
  > host+potocol  : origin  
  > pathname : 路径名字, 端口后面的
  > search —— 以问号 ? 开头的一串参数  (请求传递的参数)
  > hash 以哈希字符 # 开头   
> 如果存在 HTTP 身份验证，则这里可能还会有 user 和 password 属性：http://login:password@site.com

- URL 对象可以替代字符串传递给任何方法，因为大多数方法都会执行字符串转换



- **SearchParams “?…”**

- 我们想要创建一个具有给定搜索参数的 url，: new URL('https://google.com/search?query=JavaScript') , 如果参数中包含空格，非拉丁字母等，**参数就需要被编码。url.searchParams** 属性解决问题, 

- >append(name, value) —— 按照 name 添加参数，
    >delete(name) —— 按照 name 移除参数，
    >get(name) —— 按照 name 获取参数，
    >getAll(name) —— 获取相同 name 的所有参数（这是可行的，例如 ?user=John&user=Pete），
    >has(name) —— 按照 name 检查参数是否存在，
    >set(name, value) —— set/replace 参数，
    >sort() —— 按 name 对参数进行排序，很少使用，
    >……并且它是可迭代的，类似于 Map。
    
- ```javascript
  let url = new URL('https://google.com/search');
  url.searchParams.set('tbs', 'qdr:y'); // 添加带有一个冒号 : 的参数
  // 参数会被自动编码
  alert(url); // https://google.com/search?q=test+me%21&tbs=qdr%3Ay
  ```

  


- 如果我们不使用URL对象, 而是字符串就要手动对特殊字符进行编码 : 
> 下面是用于编码/解码 URL 的内建函数：

    encodeURI —— 编码整个 URL。
    decodeURI —— 解码为编码前的状态。
    encodeURIComponent —— 编码 URL 组件，例如搜索参数，或者 hash，或者 pathname。
    decodeURIComponent —— 解码为编码前的状态。




#### 长轮询 websocket  Server Sent Events
- 常规轮询 : 从服务器获取新信息的最简单的方式是定期轮询。每个周期发送请求,看看服务器有没有信息
 缺点:两个请求之间延迟是一个周期,不管有没有消息, 在一定时间服务器都会被请求轰炸

- 所谓“长轮询”是轮询服务器的一种更好的方式。它也很容易实现，并且可以无延迟地传递消息它不使用任何特定的协议，例如 WebSocket 或者 Server Sent Event。

- 其流程为：

- > 请求发送到服务器。
   > 服务器在有消息之前不会关闭连接。
   > 当消息出现时 —— 服务器将对其请求作出响应。
   > 浏览器立即发出一个新的请求。
   >
   > > 对于此方法，浏览器发出一个请求并与服务器之间建立起一个挂起的（pending）连接的情况是标准的。仅在有消息被传递时，才会重新建立连接。
   >
   > 

- > 如果连接丢失，可能是因为网络错误，浏览器会立即发送一个新请求。



**WebSocket:** 

-  WebSocket 协议提供了一种在浏览器和服务器之间建立持久连接来交换数据的方法。数据可以作为“数据包”在两个方向上传递，而不会断开连接和其他 HTTP 请求。

- > 对于需要连续数据交换的服务，例如网络游戏，实时交易系统等，WebSocket 尤其有用。

  

-  语法: 

-  > let socket = new WebSocket("*ws*://javascript.info");   // 同样也有一个加密的 `wss://` 协议。类似于 WebSocket 中的 HTTPS。 `wss://` 协议不仅是被加密的，而且更可靠
- 一旦 socket 被建立，我们就应该监听 socket 上的事件。一共有 4 个事件：

- > - **`open`** —— 连接已建立，
  > - **`message`** —— 接收到数据，
  > - **`error`** —— WebSocket 错误，
  > - **`close`** —— 连接已关闭。
  >
  > 想发送一些东西，那么可以使用 `socket.send(data)`。data:字符串或二进制格式无需设置直接发送, 

- WebSocket 对象是原生支持跨源的。没有特殊的 header 或其他限制。旧的服务器无法处理 WebSocket，因此不存在兼容性问题。

- WebSocket 通信由 “frames”（即数据片段）组成，可以从任何一方发送:

- > 有以下几种类型：
  >
  > 文本或二进制 frames......,浏览器中只是用二进制和文本类型

- **当我们收到数据时，文本总是以字符串形式呈现。而对于二进制数据，我们可以在 `Blob` 和 `ArrayBuffer` 格式之间进行选择。它是由 `socket.bufferType` 属性设置的，默认为 `"blob"`，**



- **关闭连接:**

- > 当一方想要关闭连接时（浏览器和服务器都具有相同的权限），它们会发送一个带有数字码（numeric code）和文本形式的原因的 “connection close frame”。
  >
  > 
  >
  > socket.close([code], [reason]);
  >
  > - `code` 是一个特殊的 WebSocket 关闭码（可选）
  > - `reason` 是一个描述关闭原因的字符串（可选）

- **连接状态:**

- 要获取连接状态，可以通过带有值的 `socket.readyState` 属性：

- > - **`0`** —— “CONNECTING”：连接还未建立，
  > - **`1`** —— “OPEN”：通信中，
  > - **`2`** —— “CLOSING”：连接关闭中，
  > - **`3`** —— “CLOSED”：连接已关闭。



- 在使用时,服务器端也有处理websocket的方法,要查看服务端的算法



**Server-Sent Events:**

- [Server-Sent Events](https://html.spec.whatwg.org/multipage/comms.html#the-eventsource-interface) 规范描述了一个内建的类 `EventSource`，它能保持与服务器的连接，并允许从中接收事件。与 `WebSocket` 类似，其连接是持久的。

- 和websocket区别:

- > 单向：仅服务端能发送消息
  >
  > 仅传输文本数据
  >
  > 使用常规的http协议

  

- 我们为什么要使用它？主要原因：简单。在很多应用中，`WebSocket` 有点大材小用。

- 我们需要从服务器接收一个数据流：可能是聊天消息或者市场价格等。这正是 `EventSource` 所擅长的。它还支持自动重新连接，而在 `WebSocket` 中这个功能需要我们手动实现。此外，它是一个普通的旧的 HTTP，不是一个新协议

- **链接过程**:

- > 创建 `let eventSource = new EventSource("/events/subscribe");`, 开始接收消息,浏览器将会连接到 `url` 并保持连接打开，等待事件。
  >
  > 服务器响应状态码应该为 200，header 为 `Content-Type: text/event-stream`，然后保持此连接并以一种特殊的格式写入消息，对于每个这样的消息，传递时都会生成 `message` 事件:
  >
  > eventSource.onmessage = function(event) 
  >
  > 

- `EventSource` 支持跨源请求,就像 `fetch` 任何其他网络方法,应该设置附加选项 `withCredentials`,远程服务器将会获取到 `Origin` header，并且必须以 `Access-Control-Allow-Origin` 响应

- 创建之后，`new EventSource` 连接到服务器，如果连接断开 —— 则**重新连接**。这非常方便，我们不用去关心重新连接的事情。

- > 服务器可以使用 `retry:` 来设置需要的延迟响应时间（以毫秒为单位）

- **链接断开**:

- > - 如果服务器想要浏览器停止重新连接，那么它应该使用 HTTP 状态码 204 进行响应。
  >
  > - 如果浏览器想要关闭连接，则应该调用 `eventSource.close()`：
  >
  >   
  >
  >   如果响应具有不正确的 `Content-Type` 或者其 HTTP 状态码不是 301，307，200 和 204，则不会进行重新连接。在这种情况下，将会发出 `"error"` 事件，并且浏览器不会重新连接



- 当一个连接由于网络问题而中断时，客户端和服务器都无法确定哪些消息已经收到哪些没有收到。为了正确地恢复连接，每条消息都应该有一个 `id` 字段

- > 当收到具有 `id` 的消息时，浏览器会：
  >
  > - 将属性 `eventSource.lastEventId` 设置为其值。
  > - 重新连接后，发送带有 `id` 的 header `Last-Event-ID`，以便服务器可以重新发送后面的消息。

- **连接状态：readyState**

- `EventSource` 对象有 `readyState` 属性，该属性具有下列值之一：

- > EventSource.CONNECTING = 0; // 连接中或者重连中
  >
  >  EventSource.OPEN = 1;       // 已连接
  >
  >  EventSource.CLOSED = 2;     // 连接已关闭

- 默认情况下 `EventSource` 对象生成三个事件：

- > - `message` —— 收到消息，可以用 `event.data` 访问。
  > - `open` —— 连接已打开。
  > - `error` —— 无法建立连接，例如，服务器返回 HTTP 500 状态码。

- 

#### RESTful 风格API

- 如何设计请求地址的规范格式

#### 模板引擎

- 把获得的数据拼接在html上进行展示
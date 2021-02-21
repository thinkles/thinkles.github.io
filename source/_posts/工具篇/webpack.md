---

title: webpack
date: {{ date }}
updated: {{date}}
tags: webpack
categories: [工具,webpack]

---
## webpack

- **为什么使用构建工具?**

  > 前端模块要在客户端(浏览器)中执行，所以他们需要增量加载到浏览器中。
  >
  > 模块的加载和传输，我们首先能想到两种极端的方式，一种是每个模块文件都单独请求，另一种是把所有模块打包成一个文件然后只请求一次
  >
  > 但是两种方法都不是最佳的, 所以我们采用**分块传输**，按需进行懒加载，在实际用到某些模块的时候再增量更新，才是较为合理的模块加载方案,要实现模块的按需加载，就需要一个对整个代码库中的模块进行静态分析、编译打包的过程。

- 在前端开发过程中还涉及到样式、图片、字体、HTML 模板等等众多的资源,如果他们都可以视作模块，并且都可以通过`require`的方式来加载，将带来优雅的开发体验
  
- 构建工具 还可以帮助我们处理各种项目的依赖项, 优化打包后的文件...



**Webpack的重要概念:**

- [webpack 术语表](https://webpack.docschina.org/glossary/)

- 依赖图: 当 webpack 处理应用程序时，它会根据命令行参数中或配置文件中定义的模块列表开始处理。 从 [*入口*](https://webpack.docschina.org/concepts/entry-points/) 开始，webpack 会递归的构建一个*依赖关系图*，这个依赖图包含着应用程序中所需的每个模块，然后将所有模块打包为少量的 *bundle* —— 通常只有一个 —— 可由浏览器加载。

- loader : Webpack 本身只能处理原生的 JavaScript 模块，但是 loader 转换器可以将各种类型的资源转换成 JavaScript 模块。这样，任何资源都可以成为 Webpack 可以处理的模块。

- plugin : 插件目的在于解决 loader 无法实现的**其他事**。

- runtime  : 在浏览器运行过程中，webpack 用来连接模块化应用程序所需的所有代码。

- manifest : 是保留所有模块要点的数据集合, runtime 会通过 manifest 来解析和加载模块, 通过使用 manifest 中的数据，runtime 将能够检索这些标识符，找出每个标识符背后对应的模块。

  > 通过使用内容散列(content hash)作为 bundle 文件的名称，这样在文件内容修改时，会计算出新的 hash，浏览器会使用新的名称加载文件，从而使缓存无效。一旦你开始这样做，你会立即注意到一些有趣的行为。即使某些内容明显没有修改，某些 hash 还是会改变。这是因为，注入的 runtime 和 manifest 在每次构建后都会发生变化。

- 模块热替换: 在应用程序运行过程中，替换、添加或删除模块，而无需重新加载整个页面

  > - 保留在完全重新加载页面期间丢失的应用程序状态。
  > - 只更新变更内容，以节省宝贵的开发时间。
  > - 在源代码中 CSS/JS 产生修改时，会立刻在浏览器中进行更新，这几乎相当于在浏览器 devtools 直接更改样式。

- `style-loader` 实现了 HMR 接口；当它通过 HMR 接收到更新，它会使用新的样式替换旧的样式。 类似的也可以在模块中实现 HRM接口, 用来模块更新后调用该函数

#### webpack简单使用

- 安装 `webpack-cli/init `工具 (可选), 也可以手动创建 webpack 的config文件

  > 也可以利用 webpage  vscode插件

- 自定义` packge.json` 文件中的 编译命令 ,  运行该命令

  > 注意:  `npx webpack`  默认寻找webpack.config.js 文件 也可以自定义
  >
  > `npx webpack --config webpack.config.js`

  > 也可以使用 webpack-cli 命令进行零配置的编译

- 加载 CSS/Scss.. 媒体资源(CSS中的图像 字体)

  > 下载相应的loader配置(style-loader+css-loader 是基础 还有sass-loader.. ), 然后就可以使用import 像模块一样加载CSS
  >
  > 加载CSS中的媒体资源:  webpack5 使用内置的 [Asset Modules](https://webpack.docschina.org/guides/asset-modules/)  进行加载, 5之前使用相应的loader进行加载 例如: url-loader file-loader ..
  >
  > 对于html中的 媒体资源 使用 html-loader 处理 否则打包后找不到资源

  > 以上方法打包后的图片路径都是针对本机的绝对路径,如果部署到线上是不行的,所以通过output 中的 publicpath 属性设置路径

- > 加载数据 csv 文件 tsv xml 需要相应的loader

- 输出管理

  - 打包文件输出: 

    > 单入口的情况下, 正常的进行打包就可以了
    >
    > 多入口时,打包后出现多个bundle, 使用`HtmlWebpackPlugin` 自动的添加多个 bundle

  - 输出目录的清理:

    >  每次构建时可能会遗留一些多余的代码文件,使用插件`clean-webpack-plugin`    

- 开发配置:

  - **source map:**
  
    > 将多个源文件打包为一个文件时为了追踪错误信息, 需要配置 [source maps](http://blog.teamtreehouse.com/introduction-source-maps)  
  
  - 热更新:

    > `watch`:自动重新编译更改的对象,  或者`webpack-dev-server`  :开启热更新, 配置dev server,从什么位置查找文件
  
  > 在开发设置中 `CleanWebpackPlugin` 插件会产生很多冲突, 原因在于, 该插件会在成功构建 build 后(包括编译),删除输出目录的文件.
  >
  > 导致 watch 模式下, 构建成功后,更改编译后, 静态资源 :html文件 和 图片.. 都被删除 `解决`:    cleanStaleWebpackAssets: false 更改该插件选项
  >
  > 在 dev Serve 模式下, 成功构建向内存 写文件而不是硬盘,导致该插件清空了dist目录
  >
  > `解决`: 调整 dev Serve 插件的选项 让其构建成功后使文件写入硬盘, 还有设置 cleanStaleWebpackAssets选项, 让其在热更新编译时不会删除静态文件
  
- > 建议在最后构建的时候再进行清除 使用CleanWebpackPlugin,否则会很多冲突

  ​	

  - `EslintWebpackPlugin`  插件带来eslint 功能  `Prettier Webpack Plugin` 带来 `Prettier 功能
  - babel



#### 预处理器loader

- 在引入loader时,可以通过options 提供额外的配置

- loader 相关配置

  > exclude  : 排除被正则匹配到的该模块  
  >
  > 必加项 : exclude: /node_modules/, 或者 include : /src/ 
  >
  > exclude 和 include  exclude 优先级更高
  >
  > 
  >
  > resource issuer : 可以更加精确的确定模块规则的作用范围
  >
  > resource: 被加载模块   issuer:加载者

- URL-loader  file-loader 的区别:

  > url-loader 允许你有条件地将文件转换为内联的 base-64 URL (当文件小于给定的阈值)，这会减少小文件的 HTTP 请求数。如果文件大于该阈值，会自动的交给 file-loader 处理。  url-loader内置了file-loader ,只需要安装一个即可
  >
  >  limit : true  无限制

#### 开发环境

- 手动编译文件很麻烦 , webpack 提供几种可选方式，帮助你在代码发生变化后自动编译代码：

  > webpack watch mode
  >
  > webpack-dev-server  
  >
  > webpack-dev-middleware

- webpack-dev-middleware 是一个封装器(wrapper)，它可以把 webpack 处理过的文件发送到一个 server。 webpack-dev-server 在内部使用了它，然而它也可以作为一个单独的 package 来使用，以便根据需求进行更多自定义设置。

- **webpack-dev-server:**

  > 服务启动时, `wds`进行模块打包, 当浏览器请求时,首先它先效验URL地址,
  >
  > 如果该地址是资源服务地址(指定的资源文件地址),就会从内存中寻找资源返回给服务器.  如果不是,就读取硬盘内的源文件返回给浏览器

- **webpack-dev-server 在编译打包之后不会写入到任何输出文件 ,**打包结果被放在了内存中而不是实际磁盘内。

  > 换句话说他不会生成新的文件,每次接到浏览器的请求,都只是将内存中的打包结果返回给浏览器

- 'WDS'参数说明:

- > `ContentBase`:    指定服务器资源的根目录,静态资源从该目录中找寻,默认使用和webpack-config.js 相同目录
  >
  > PublicPath:    *如果你的页面希望在其他不同路径中找到 bundle 文件，则可以通过 dev server 配置中的* [`publicPath`](https://webpack.docschina.org/configuration/dev-server/#devserverpublicpath-) 选项进行修改。 默认为'/'
  >
  > 不管 PublicPath 怎么设置, 通过路由查找文件时使用的路径都是`ContentBase` ,而且都是通过在硬盘文件中查找,而不是在内存中查找

- `publicPath` 和 output `publicPath` 保持一致, 这样静态资源文件才可以请求到, 因为设置完  publicPath, 这个路径就代表了内存中的根目录



- webpack-dev-server 的运行会进行构建,然后就调用了 `clean-webpack-plugin `来清空 dist目录下的文件, 通过设置webpack-dev-server 的 writetodisk 选项写入文件到硬盘解决

- > clean-webpack-plugin  默认情况下，此插件将在每次成功构建时(包括编译时),删除输出目录所有的文件(包括asset), 
  >
  > 如果想要在**编译**时不删除静态资源,使用  `cleanStaleWebpackAssets`  选项 设置为false, 

  
  
- **资源加载:**

- 在 webpack 5 提供 资源模块类型 asset , 默认情况下，`asset/resource` 模块以 `[hash][ext][query]` 文件名发送到输出目录。

  > 可以通过在 webpack 配置中设置 [`output.assetModuleFilename`](https://webpack.docschina.org/configuration/output/#outputassetmodulefilename) 来修改此模板字符串.

- **自动管理输出** : 

- > `HtmlWebpackPlugin`  , html-webpack-插件将自动将所有必要的CSS、JS、清单和 favicon 文件注入到标记中

- **清理/dist 输出路径的文件夹**,: [`clean-webpack-plugin`](https://www.npmjs.com/package/clean-webpack-plugin)

- 源代码映射: 追踪到 error(错误) 和 warning(警告) 在源代码中的原始位置 ,使用`source maps`功能

  

- **webpack-dashboard 插件**  :可以使控制台中打印的打包有关的信息以列表的形式提供,作为插件添加到webpack配置中,使用webpack-dashboard 模块命令替换原来的webpack启动方式即可

- >speed-measure-webpack-plugin 可以分析出构建过程的时间, 可以找出构建过程中那个步骤最慢
  >
  >size-plugin 每次打包后的体积和上次的体积变化值



- **热加载HMR** :

- 模块热替换(`Hot Module Replacement`)的技术可在不刷新整个网页的情况下做到超灵敏的实时预览。

- > 不在进行刷新网页重新发送请求, HMR保留完全加载页面状态的前提下,只更新改动,不在进行刷新
  >
  > 原理是当一个源码发生变化时，只重新编译发生变化的模块，再用新输出的模块替换掉浏览器中对应的老模块。

- **手动引入HMR:**

  > ```
  >if (module.hot) {
  > module.hot.accept('./print.js', function(){})
  >}
  > ```

  

- **模块热替换问题** :    

- 借助于 `style-loader`，使用模块热替换来加载 CSS 实际上极其简单。此 loader 在幕后使用了 `module.hot.accept`，在 CSS 依赖模块更新之后，会将其 patch(修补) 到 `<style>` 标签中。

- 社区还提供许多其他 loader 和示例，可以使 HMR 与各种框架和库平滑地进行交互…… ,比如 vue-loader

####  样式处理

- 在生产环境下,我们希望样式存在于单独的css文件中, 而不是style标签内, 因为文件更有利于客户端缓存,  这时我们使用 mini-css-extract-plugin

- >  样式的提取是以资源入口开始的整个chunk单位的封装, 如果index.js引入很多的模块,每个模块引入各自的样式, 但是最终只会生成一个css文件,因为只有一个入口
  >
  > 当有多个入口时,生成多个css文件,但是名字会重复,所以要使用[name].css 进行动态命名

- 插件中可以指定同步加载的Css文件的文件名 filename  也可以指定异步加载的Css资源 chunkfileName 文件名

  > `filename` : 是主入口的文件名，`chunkFilename` 是非主入口的文件名
  >
  > 主入口 : 指的是`entry`里面生成出来的文件名
  >
  > 非主入口  :指的是按需加载（异步）模块,在entry没有名字
  >
  > 在打包时,同步加载的会被打包成一个 称为 initial chunk    默认main
  >
  > 异步 按需加载的会被打包为另一个包 称为 :non-initial chunk,默认使用唯一 ID 来替代名称

  


- 想要在浏览器中查看CSS源码 需要给CSS-loader  SCSS-loader 单独配置source map选项

-  `module`，`chunk` 和 `bundle` 其实就是同一份逻辑代码在不同转换场景下的取了三个名字：  我们直接写出来的是 module，webpack 处理时是 chunk，最后生成浏览器可以直接运行的 bundle。

   


#### 代码分离(分片):

-  能够把代码分离到不同的 bundle 中，然后可以按需加载或并行加载这些文件。代码分离可以用于获取更小的 bundle，以及控制资源加载优先级，如果使用合理，会极大影响加载时间。

- **代码分离方法:**

- 1. 手动配置入口起点, 构建多个bundle 进行加载

  > 这种方法不够灵活,必须手动的配置,
  >
  > 如果有重复的模块还是会加载检测不出来,导致冗余代码 

  -  解决方法:

    > 方式一:  配置 `dependOn`选项,   把公共模块打包出来
    >
    > `optimization.runtimeChunk: ` 选项 : 是否把runtime 代码打包出来, 配合缓存使用
    >
    > 
    >
    > 方式二:  使用插件 [`SplitChunksPlugin`](https://webpack.docschina.org/plugins/split-chunks-plugin) 可以将公共的依赖模块提取到已有的入口 chunk 中,或者提取到一个单独的chunk, 也需要 optimization.runtimeChunk 选项把runtime 代码分离出来

- 2. 动态导入模块

  > 使用 es6 的 `import()` 动态导入代码  , 多个import导入只会引入一个,所以可以不使用代码分离插件了
  >
  > 使用webpack功能  [`require.ensure`](https://webpack.docschina.org/api/module-methods/#requireensure)

  -  import 动态引入:   

  - ```
    import('lodash')
     .then(({ default: _ })=>{})
     // 需要 default参数值来获取模块对象，因为 webpack 4 在导入 CommonJS 模块时，将不再解析为 module.exports 的值
    ```

  - 由于 `import()` 会返回一个 promise，因此它可以和 [`async` 函数]一起使用

  - import() 实现原理就是动态生成script插入文档

  - 异步资源的chunk ,资源生成名字默认为数字 id, 我们可以通过特有注释来获取异步资源 chunk的name

  - > import(/*webpackChunkName :"bar"*/  './bar.js') 
    >
    > output"{ chunkFilename: [name].js }

- **懒加载:**

  > 手动的代码分离,从技术概念上确实实现了'懒加载', 但是这个懒加载,会在每次加载页面自动加载, 而不是通过用户的交互进行加载(真正需要时加载)
  >
  > 我们添加用户交互函数,  使用 import() 进行**懒加载**  
  >
  > 当调用 ES6 模块的 import() 方法（引入模块）时 必须指向模块的 `.default` 值, 因为他实际返回的是promise 处理后返回的module 对象

  

- 3. 预获取/预加载模块

     > 在声明 import 时，使用下面这些内置指令，可以让 webpack 输出 "resource hint(资源提示)"，来告知浏览器：
     >
     > - **prefetch**(预获取)：将来某些导航下可能需要的资源
     > - **preload**(预加载)：当前导航下可能需要资源
     >
     > ```js
     > import(/* webpackPrefetch: true*/'./path/to/LoginModal.js');
     > 
     > // 这会生成 <link rel="prefetch" href="login-modal-chunk.js"> 并追加到页面头部,指示着浏览器在闲置时间预取 login-modal-chunk.js 文件。
     > ```

  

- **代码分离插件:**

- `CommonsChunkPlugin` 插件 , (webpack4 之前的插件)

  > 一个CommonsChunkPlugin 只能提取一个vendor, 
  >
  > 异步加载会出现错误, 提取runtime 的代码导致浏览器多加载资源, 
  >
  > wepback4之后被删除

- `SplitChunks`  插件

  > `optimization.splitChunks`  开箱即用, 它的默认值可以参考[官网](https://webpack.docschina.org/plugins/split-chunks-plugin/)

- optimization.splitChunks 参数解释:

  > `chunks` :   async(默认值) 针对异步资源生效,  initial: 只对入口chunk生效, all:对所有的资源生效, 这意味着即使在异步和非异步块之间也可以共享块
  >
  > `minChunks `: 拆分之前, 公共模块被共享的最低次数
  >
  > `cacahGroups` :  分离chunks时的规则,一般有vendors 和default两种  :vendors 代表在 node_module的区块, default : 代表多次被引用的区块

- **代码分离插件存在的问题:**  

  > 在使用代码分离插件时, 绕不开的问题是 hash 和长效缓存:
  >
  > 提取后的公共模块代码中, 有些chunk包含 runtime 初始化环境的代码, 导致每次打包影响chunk中 hash的变化,  而hash 的变化影响 runtime 的代码,导致某些chunk没有更新,缓存也会失效
  >
  > 因为我们使用 chunk hash 作为资源的版本号优化客户端的缓存,这导致用户频繁的更新资源
  >
  > 

  - 解决方法:

  > 把runtime 的代码提取出来 , 通过在提取完公共模块后, 再调用该插件提取一次
  >
  > 或者 **提取引导模板**
  >
  > 使用 [`optimization.runtimeChunk`](https://webpack.docschina.org/configuration/optimization/#optimizationruntimechunk) 选项将 runtime 代码拆分为一个单独的 chunk。将其设置为 `single` 来为所有 chunk 创建一个 runtime bundle

  

- `runtimeChunk` 作用就是为了线上更新版本时，充分利用浏览器缓存，使用户感知的影响到最低。

- 代码分离插件 都需要配合`optimization.runtimeChunk` 选项,把runtime 代码提取出来

  > 提取出runtime 代码后, 还可以利用插件[script-ext-html-webpack-plugin](https://links.jianshu.com/go?to=https%3A%2F%2Fgithub.com%2Fnumical%2Fscript-ext-html-webpack-plugin) 进行优化,原因每次构建上线后,runtime都要重新请求, 浪费网络资源, 把它直接内联到html中

  

- 代码分离其他的插件 , 分离CSS: [`mini-css-extract-plugin`](https://webpack.docschina.org/guides/code-splitting/plugins/mini-css-extract-plugin) 把所有的CSS文件 抽离出来,变成一个CSS文件, 然后通过link 引入

- 代码切片的意义 : 

- > 公共模块提取的收益 :  开发过程少了重复模块的打包, 提升开发速度, 减少整体体积  合理分片之后更有效利用客户端缓存

  

- bundle 分析:  [官方分析工具](https://github.com/webpack/analyse)   分析输出结果来检查模块

#### 生产环境

- *development(开发环境)* 和 *production(生产环境)* 这两个环境下的构建目标存在着巨大差异。

  > ***开发环境***:   我们需要强大的 source map 和一个有着 live reloading(实时重新加载) 或 hot module replacement(热模块替换) 能力的 localhost server
  >
  > ***生产环境*** : 目标则转移至其他方面，关注点在于压缩 bundle、更轻量的 source map、资源优化(让用户更快加载资源,最大限度利用缓存, 资源的压缩)，

- 由于要遵循逻辑分离，我们通常建议为每个环境编写**彼此独立的 webpack 配置**。也可以使用同一个配置文件,不过要在webpack.config.js文件内添加判断条件来使用那个配置

  > 使用一个名为 [`webpack-merge`](https://github.com/survivejs/webpack-merge) 的工具,此工具会引用 "common" 配置，因此我们不必再在环境特定(environment-specific)的配置中编写重复代码。需要创建文件:
  >
  > `webpack.common.js  webpack.dev.js   webpack.prod.js`  
  >
  > 通过在文件中引入  webpack-merge, 编写独立的webpack配置:  
  >
  > ```diff
  > const { merge } = require('webpack-merge');
  > const common = require('./webpack.common.js');
  > 
  >  module.exports = merge(common, {
  >    mode: 'development',
  >    devtool: 'inline-source-map',
  >    devServer: {
  >    contentBase: './dist',
  >   },
  > });
  > ```
  >
  > 更改script 命令行, 使用不同的config进行构建

  

- **指定mode(构建模式)**

  > 许多 library 通过与 `process.env.NODE_ENV` 环境变量关联，以决定 library 中应该引用哪些内容。
  >
  > 一些 library 可能针对具体用户的环境，删除或添加一些重要代码，以进行代码执行方面的优化,从 webpack v4 开始, 指定 [`mode`](https://webpack.docschina.org/configuration/mode/) 会自动地配置 [`DefinePlugin`](https://webpack.docschina.org/plugins/define-plugin)



- **source map 配置**

- 对于 JS文件添加`devtool`配置即可,对于`css less scss`来说,在loader, options选项中添加

  > 开发环境: cheap-moudle-eval-source-map 通常是一个不错的选择
  >
  > 生产环境:  只有三种可供选择, 三种在安全性上各不相同 
  >
  > nosources-source-map 推荐

  

- **资源压缩 (uglify)** ,意思是移除多余空格, 换行,和不执行的代码, 缩短变量名.. 使代码形式变得更短, 压缩之后的代码基本上不可读

  > **压缩JavaScript** : webpack4之后默认 使用了terser 的插件 terser-webpack-plugin, 可以在optimization 中的 `minisize:true`, 开启功能,   如果mode : production  则会自动设置,不用再人为设置
  >
  > 也可以尝试别的压缩功能插件[closure-webpack-plugin](https://github.com/webpack-contrib/closure-webpack-plugin)

  > **压缩 css:** 通过`mini-css-extract-plugin`  将样式提取出来到单独文件, 再使用`CssMinimizerWebpackPlugin` 进行压缩

  

  - `optimization.minimizer` 配置项中提供一个/多个压缩工具,

  - `optimization.minimize:true`  开启压缩功能, 生产模式自动开启

  

##### 缓存

- 客户端获取资源是比较耗费时间的 , 浏览器使用一种名为 [缓存](https://searchstorage.techtarget.com/definition/cache)的技术, 重复利用浏览器已经获取过的资源, 通过命中缓存，以降低网络流量 

  > 当开发者更新了bug,希望立即更新到用户的浏览器上,而不是使用客户端旧的缓存,最好的办法是改变资源的URL,一个常用的办法时改变文件的名字 : 来迫使客户端重新下载

- 技术点:

  > 1. 改变输出文件的文件名, ,使客户端只重新下载更新过的代码, 
  >
  >    > 我们可以通过替换 `output.filename` 中的 [substitutions](https://webpack.docschina.org/configuration/output/#outputfilename) 设置, 还要设置 runtimeChunk 选项,把runtime代码拆分成单独的 chunk, 否则会影响文件名  
  >    >
  >    > (但是在实际操作中 构建时不进行拆分runtime 代码, 也不会影响包含runtime的chunk的文件名  可能和文件名设置有关系  这里使用[contenthash] 并没有影响  如果使用[chunkhash]可能会发生变化 )
  >    >
  >    > 例子:    `filename: '[name].[contenthash].js',  `
  >
  > 2. 将第三方库(library)提取到单独的 vendor chunk 文件中, 因为他不会频繁改变, 可以利用 [`SplitChunksPlugin`](https://webpack.docschina.org/plugins/split-chunks-plugin/) 插件的 [`cacheGroups`](https://webpack.docschina.org/plugins/split-chunks-plugin/#splitchunkscachegroups) 选项来实现, 把`node_modules` 目录的代码提取出单独的一个chunk(所有的第三方库整合成一个chunk)
  >
  >    默认情况下 使用SplitChunksPlugin插件, 会把**每个**第三方库都声称一个单独的chunk, 
  >
  >    > 当分离出来第三方库时, 有可能(使用别的文件名时)每次构建时vendor 都会改变, 可以使optimization.moduleIds设置为 deterministic'
  >    >
  >    > 这里文件名使用了[contenthash] 构建时并没有影响vendor 的变化



#### 打包优化(构建性能)

- 让打包速度更快,资源输出体积更小 -----对应官网的构建性能

- 环境通用: 

- 缩小打包作用域 : 

  - 将 loader 应用于最少数量的必要模块

    > 通过使用 `include` 字段 规定loader应用的目录,缩小范围
    >
    > `exclude ` 字段 , 排除不需要loader 的目录
    >
    > 有些库不希望webpack 进行解析,即不应用任何的loader,但仍然会被打包进资源文件,可以使用 `moudle`中的`noParse: /loadsh/ `指明模块名字

- 每个额外的 loader/plugin 都有其启动时间。尽量少地使用工具,尽量保持 chunk 体积小,减少编译结果的整体大小，
  
- 将 `ProgressPlugin` 从 webpack 中删除，可以缩短构建时间。
  
- cache 选项: 缓存生成的 webpack 模块和 chunk，来改善构建速度。`cache` 会在[`开发` 模式](https://webpack.docschina.org/configuration/mode/#mode-development)被设置成 `type: 'memory'` 而且在 [`生产` 模式](https://webpack.docschina.org/configuration/mode/#mode-production) 中被禁用
  
- 使用ignorePlugin ,他可以完全排除一些模块, 即使被引用也不会被打包, 对于排除一些库的相关文件非常有用, 一些库产生的额外资源我们用不到,但是引用语句在库文件内,我们也无法去掉,这时使用该插件不打包
  
  
  
- 使用 DIIplugin插件,对一些不经常改变的公共(第三方)模块,进行预先打包,到工程部署时使用DiiReferencePlugin来索引打包好的文件, DIIplugin和 代码分离类似,但是前者会把整个模块拆出来,代码分离会根据规则拆分, 相应的前者需要单独一个配置文件

- 开发环境使用:

  - 在内存中编译, 使用工具`webpack-dev-server` ....
  - 需要注意的是不同的 `devtool` 设置，会导致性能差异。选择适当的`devtool`
  - webpack 只会在文件系统中输出已经更新的 chunk。最小化入口chunk,把runtime 分离出来
  - webpack 会在输出的 bundle 中生成路径信息。然而，在打包数千个模块的项目中，这会导致造成垃圾回收性能压力。在 `options.output.pathinfo` 设置中关闭：

  - 使用 **happyPack** 多线程进行打包(webpack本身是单线程的,只能一个一个的通过依赖关系查找进行转译),适用于转译任务比较重的项目效果明显,对于小项目来说并不明显

##### Tree Shaking:  (缩小chunk体积)

- 通常用于描述移除 JavaScript 上下文中的未引用代码(dead-code)。它依赖于 ES2015 模块语法的 [静态结构](http://exploringjs.com/es6/ch_modules.html#static-module-structure) 特性, 例如我们仅仅导入了文件中的部分 导出内容, 而webpack将把所有内容导入进来,这时需要 tree shaking 去除冗余

  > es6会在代码编译时确定依赖关系,可以检测出,没有引用过的模块(代码块),webpack进行标记,在开发环境下仍然可见, 在生产环境下**资源压缩时**将他们从最终的bundle去掉

- tree Shaking 只对`es6 module` 有用, 对于通过`commonJs` 引用进来的没有用处

- > 在工程中使用 `babel-loader` ,那么一定要通过配置来禁用它的模块依赖解析,因为解析过之后,webpack接受的都是commonjs形式的模块, 
  >
  > 在loader 中`options:{presets:[@babel/preset-env,{moudle:false}]}`

- 使用 Tree Shaking:

  > 配置 `optimization: { usedExports: true,} ` , 在mode: "production"时被默认添加 
  >
  > 该配置让webpack 标记标记未使用的成员,然后在压缩过程中删除代码

  > **该配置的副作用 :** 
  >
  > 在一个纯粹的 ESM 模块世界中,很容易分辨出哪些代码删除之后没有副作用,但是现实情况我们达不到这种纯度, 例如有些内容虽然没有使用,但是去除之后可能会有副作用( `polyfill ` 不需要导出,他影响全局作用域)

  - ``sideEffects` ` 的原理:

    > `usedExports` 依赖于 [terser](https://github.com/terser-js/terser) 去检测语句中的副作用,我们可以通过 `/*#__PURE__*/` 注释来帮忙 terser。
    >
    > 它将一个语句标记为没有副作用,这样进行有利于后期删除, 但是这种层面只能指定单个语句

  - 使用`sideEffects` 更加有效 ,他也是标记代码但是它作用于模块的层面, 而不是代码语句的层面

    > 通过 package.json 的 `"sideEffects"` 属性，来实现这种方式,默认为true
    >
    > > optimization.sideEffects 中的选项将辨识`package.json` 中的sideEffects 规则    在生产环境下默认添加
    >
    > 如果所有代码都没有副作用 ，我们就可以简单地将该属性标记为 false
    >
    > 或者可以给该选项提供一个数组

  - >  所有导入文件都会受到 tree shaking 的影响。这意味着，如果在项目中使用类似 `css-loader` 并 import 一个 CSS 文件，则需要将其添加到 side effect 列表中，以免在生产模式中无意中将它删除：

  - 还可以在 `module.rules` 配置选项中设置 `"sideEffects"`。

- `sideEffects`    和 `usedExports`两者的联系

  > `sideEffects`    打包时直接删除了没有副作用 被引用但没有使用的模块
  >
  > `usedExports`   打包时 标记没有使用的语句, 在生产环境下删除多余的语句,

  

- 在使用 tree shaking 时必须有 [ModuleConcatenationPlugin](https://webpack.docschina.org/plugins/module-concatenation-plugin) 的支持，您可以通过设置配置项 `mode: "production"` 以启用它。如果您没有如此做，请记得手动引入 [ModuleConcatenationPlugin](https://webpack.docschina.org/plugins/module-concatenation-plugin)。

- tree shaking 本质上只对死代码进行标记, 真正去除死代码的是在生产环境下资源压缩那一步



#### Web workers

- Web Worker为Web内容在后台线程中运行脚本提供了一种简单的方法。线程可以执行任务而不干扰用户界面。

#### 进式网络应用程序PWA

- 是一种可以提供类似于native app(原生应用程序) 体验的 web app(网络应用程序)。简单说是一种 web 应用  。PWA 可以用来做很多事。其中最重要的是，在__离线(offline)__时应用程序能够继续运行功能。这是通过使用名为 [Service Workers](https://developers.google.com/web/fundamentals/primers/service-workers/) 的 web 技术来实现的。



#### Shimming 预置依赖

- `webpack` compiler 能够识别遵循 ES2015 模块语法、CommonJS 或 AMD 规范编写的模块。然而，一些 third party(第三方库) 可能会引用一些全局依赖（例如 `jQuery` 中的 `$`）。因此这些 library 也可能会创建一些需要导出的全局变量。这些 "broken modules(不符合规范的模块)" 就是 *shimming(预置依赖)* 发挥作用的地方。

- *shim* 另外一个极其有用的使用场景就是：当你希望 [polyfill](https://en.wikipedia.org/wiki/Polyfill_(programming)) 扩展浏览器能力，来支持到更多用户时。在这种情况下，你可能只是想要将这些 polyfills 提供给需要修补(patch)的浏览器（也就是实现按需加载）

- **Shimming 预置全局变量:** 

- 使用 [`ProvidePlugin`](https://webpack.docschina.org/plugins/provide-plugin) 后，能够在 webpack 编译的每个模块中，通过访问一个变量来获取一个 package

  > 还可以使用 `ProvidePlugin` 暴露出某个模块中单个导出，通过配置一个“数组路径”
  >
  > ```diff
  > new webpack.ProvidePlugin({
  >     _: 'lodash',
  >      join: ['lodash', 'join'],
  >      }),
  > ```

- 在依赖全局变量的 第三方模块中  [`imports-loader`](https://webpack.docschina.org/loaders/imports-loader/)  很有用,  [`exports-loader`](https://webpack.docschina.org/loaders/exports-loader/)，将一个全局变量作为一个普通的模块来导出

  >  imports-loader最直接的应用场景，就是你想直接import一个开放的js文件，而不是通过npm去加载这个类库（会有一些类库不支持npm的方式）

- **加载polyfills:** 

  > 最普通的方法 : 引入 [`babel-polyfill`](https://babel.docschina.org/docs/en/babel-polyfill/) 直接  import 'babel-polyfill';使用 `import` 将其引入到我们的主 bundle 文件：
  >
  > 最佳实践仍然是，不加选择地和同步地加载所有 polyfill/shim，尽管这会导致额外的 bundle 体积成本。
  >
  > 但是你仍然可以选择性的加载polyfill, 使用`whatwg-fetch` 条件性的加载

- 优化方案:

- `babel-preset-env` package 通过 [browserslist](https://github.com/browserslist/browserslist) 来转译那些你浏览器中不支持的特性。这个 preset 使用 [`useBuiltIns`](https://babel.docschina.org/docs/en/babel-preset-env#usebuiltins) 选项，默认值是 `false`，这种方式可以将全局 `babel-polyfill` 导入，改进为更细粒度的 `import` 格式：[babel-preset-env](https://babeljs.io/docs/en/babel-preset-env)

- 
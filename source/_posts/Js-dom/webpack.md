---
title: webpack 指导
date: 
tags: 构建工具
categories: webpack
---

## webpack

- 为什么使用webpack?

- > 前端模块要在客户端中执行，所以他们需要增量加载到浏览器中。
  >
  > **分块传输**，按需进行懒加载，在实际用到某些模块的时候再增量更新，才是较为合理的模块加载方案,要实现模块的按需加载，就需要一个对整个代码库中的模块进行静态分析、编译打包的过程。
  >
  > 在前端开发过程中还涉及到样式、图片、字体、HTML 模板等等众多的资源,如果他们都可以视作模块，并且都可以通过`require`的方式来加载，将带来优雅的开发体验

- 依赖图: 当 webpack 处理应用程序时，它会根据命令行参数中或配置文件中定义的模块列表开始处理。 从 [*入口*](https://webpack.docschina.org/concepts/entry-points/) 开始，webpack 会递归的构建一个*依赖关系图*，这个依赖图包含着应用程序中所需的每个模块，然后将所有模块打包为少量的 *bundle* —— 通常只有一个 —— 可由浏览器加载。

- loader : Webpack 本身只能处理原生的 JavaScript 模块，但是 loader 转换器可以将各种类型的资源转换成 JavaScript 模块。这样，任何资源都可以成为 Webpack 可以处理的模块。

- *在安装一个 package，而此 package 要打包到生产环境 bundle 中时，你应该使用* `npm install --save`*。如果你在安装一个用于开发环境的 package 时（例如，linter, 测试库等），你应该使用* `npm install --save-dev`*。更多信息请查看* [npm 文档](https://docs.npmjs.com/cli/install)*。*



#### 输入输出

- chunk : 具有依赖关系的模块,打包时被封装为一个 chunk  可以理解为一个文件夹

- path : 打包完成后存储在硬盘上的位置 ,绝对路径

- publicPath : 指定资源的请求位置的,   (该配置项的原理: 在打包之后更改打包后文件内简介资源的路径,让打包后的资源正常获取)

- > 请求位置: 由JS或CSS所请求的间接资源路径,
  >
  > 间接资源指 : 不是首次必须加载的资源例如:
>
  > 由html页面  script请求的 js 文件  link  
  >
  > js 请求的 异步加载的js,( 首屏js 进一步加载的js) 
  >
  >  css请求的图片和字体等..
  
- publicPath的三种形式

- > "" "./js"  : 这是以html 文件 所在路径+publcPath路径 作为请求路径
  >
  > "/" "/js"  : 这是以该页面的 hostname 路径+publicPath  作为请求路径
  >
  > "http://cdn.com/"  以cdn + publicPath  作为请求路径

- publicPath 并不会对生成文件的路径造成影响，主要是对你的页面里面引入的资源的路径做对应的补全，常见的就是css文件里面引入的图片

- >为了避免开发环境 和 生产环境产生不一致 , 我们把 wds 的publcPath  和webpack output.path 保持一致
  >
  >

- filename : 可以利用filename 提供的变量 控制客户端缓存, 每次内容更改就变换名字,防止浏览器使用缓存

#### 预处理器loader

- 在引入loader时,可以通过options 提供额外的配置

- loader 相关配置

- > exclude  : 排除被正则匹配到的该模块  必加项 exclude: /node_modules/, 或者 include : /src/ 
  >
  >  include: 只对该模块进行匹配正则
  >
  > exclude 优先级更高

- > resource issuer : 可以更加精确的确定模块规则的作用范围
  >
  > resource: 被加载模块   issuer:加载者
  >
  > test exclude include 本质上是resource 的配置.我们也可以给issur 添加额外的配置
  >
  > ```
  > rules:[{
  > 	test:/\..
  > 	issuer:{
  > 		test:/\
  > 		include:src/
  > 		}}
  > ]
  > ```
  >
  > 

  
  

#### 开发环境

- 手动编译文件很麻烦 , webpack 提供几种可选方式，帮助你在代码发生变化后自动编译代码：

  1. webpack [watch mode](https://webpack.docschina.org/configuration/watch/#watch)(webpack 观察模式) 其中一个文件被更新，代码将被重新编译，所以你不必再去手动运行整个构建
  2. webpack-dev-server  
  3. webpack-dev-middleware

- > 在 watch 触发增量构建后会删除 `index.html` 文件，
  >
  > 可以在 `CleanWebpackPlugin` 中配置 [`cleanStaleWebpackAssets` 选项](https://github.com/johnagan/clean-webpack-plugin#options-and-defaults-optional) 来实现：

- > webpack-dev-middleware 是一个封装器(wrapper)，它可以把 webpack 处理过的文件发送到一个 server。 webpack-dev-server 在内部使用了它，然而它也可以作为一个单独的 package 来使用，以便根据需求进行更多自定义设置。

- **webpack-dev-server:**

- 配置中的devserve 对象,是对webpack -dev-serve 准备的, 'wds'可以看做是一个服务者,他接受浏览器的请求,返回资源浏览器,
  
- >步骤: 
  >
  > 服务启动时, 'wds'进行模块打包,便准备好资源(js文件等),当浏览器请求时,首先它先效验url地址, 如果该地址是资源服务地址(配置的publickPath) ,就会从打包结果中寻找资源返回给服务器. 如果不是,就读取硬盘内的源文件返回给浏览器

- **webpack-dev-server 在编译打包之后不会写入到任何输出文件** ,打包结果被放在了内存中而不是实际磁盘内。换句话说他不会生成新的文件,每次接到浏览器的请求,都只是将内存中的打包结果返回给浏览器

- > 删除输出目录 /dist 可以验证. 不存在输出目录也一样正常运行, 从开发角度看是符合情理的,如果每次更改都写入实际文件,会产生没有的垃圾文件

- "wds" 参数说明:

- > ContentBase:    "wds"用来指定服务器资源的根目录,静态资源从该目录中找寻,默认使用和webpack-config.js 相同目录
  >
  > PublicPath:    打包后的bundle.js,将在此路径下可用, 默认情况下，`devServer.publicPath` 为 `'/'`
  >
  > 之后ContentBase 也会以该路径为基础,加上自身的路径,来查找静态资源, 所以建议 `devServer.publicPath` 与 [`output.publicPath`](https://webpack.docschina.org/configuration/output/#outputpublicpath) 相同。 或者*ContentBase 使用绝对路径。*
  >
  > ./src/img/ba.jpg  相对于http://localhost:8080/+publicPath+
  >
  > \src\img\ba.jpg  绝对路径  ContentBase+ 该路径

  

- wds  默认情况下开启之后, 会在当前根目录中寻找静态资源(图片..), 浏览器发送请求 http://localhost:3000/, (该路径还可以使用publicPath更改,但是浏览器只默认打开localhost ), wds返回编译后在内存中的 打包文件和html文件,可以以该路径为基础,向别的文件夹请求.

- > 在不设置publicPath 路径情况下,  http://localhost:3000/  加载虚拟服务器中打包在内存中的资源, 他也表示文件夹的根目录
  >
  > 在设置publicPath 路径下(不进入publicPath设置的路径),  http://localhost:3000/ 表示文件夹的根目录,默认加载根目录的index.html文件(不属于资源服务地址,直接读取硬盘内容), 所以这时改变源文件,页面不会自动变化必须得刷新一下重新请求,因为这时使用的不是内存中的文件

-  > 浏览器进入别的文件夹之后,自动解析该文件夹的index.html 
  >
  > 本质上, wds 和打包后输出文件夹没有关系,启动wds 和打包文件没有关系, 因为启动wds会打包编译放入内存

-  **contentBase:** 代表 http://localhost:3000/   的资源请求路径, 默认请求路径是文件夹的根目录,  如果contentBase 设置为./dist,那么 http://localhost:3000/  代表根目录下的dist目录,  注意这时请求的静态资源(图片..) 相对路径 是在dist目录下解析的

- **资源加载:**

- 在 webpack 5 提供 资源模块类型 asset , 默认情况下，`asset/resource` 模块以 `[hash][ext][query]` 文件名发送到输出目录。可以通过在 webpack 配置中设置 [`output.assetModuleFilename`](https://webpack.docschina.org/configuration/output/#outputassetmodulefilename) 来修改此模板字符串. `Rule.generator.filename` 与 [`output.assetModuleFilename`](https://webpack.docschina.org/configuration/output/#outputassetmodulefilename) 相同，并且仅适用于 `asset` 和 `asset/resource` 模块类型。

- **自动管理输出** : 

- > HtmlWebpackPlugin  , html-webpack-插件将自动将所有必要的CSS、JS、清单和 favicon 文件注入到标记中

- **清理/dist 输出路径的文件夹**,: [`clean-webpack-plugin`](https://www.npmjs.com/package/clean-webpack-plugin)

- 源代码映射: 追踪到 error(错误) 和 warning(警告) 在源代码中的原始位置 ,使用 [source maps](http://blog.teamtreehouse.com/introduction-source-maps) 功能

- **webpack-dashboard 插件**可以使 控制台中打印的打包有关的信息以列表的形式提供,作为插件添加到webpack配置中,使用webpack-dashboard 模块命令替换原来的webpack启动方式即可

- >speed-measure-webpack-plugin 可以分析出构建过程的时间, 可以找出构建过程中那个步骤最慢
  >
  >size-plugin 每次打包后的体积和上次的体积变化值



- **热加载HMR** :

- 模块热替换(`Hot Module Replacement`)的技术可在不刷新整个网页的情况下做到超灵敏的实时预览。

- > 不在进行刷新网页重新发送请求, HMR保留完全加载页面状态的前提下,只更新改动,不在进行刷新
  >
  > 原理是当一个源码发生变化时，只重新编译发生变化的模块，再用新输出的模块替换掉浏览器中对应的老模块。

- **引入HMR:**

- > 自动引入HMR : 设置devserve选项 ,  本质上给每个模块绑定了module.hot对象
  >
  > 手动引入:
  >
  > ```
  > if (module.hot) {
  >    module.hot.accept('./print.js', function(){})
  >    }
  > ```

  

- **模块热替换问题** :    onclick事件处理函数可能会绑定在旧的函数上,必须手动重新渲染一遍, 有很多 loader（下面会提到一些）可以使得模块热替换变得更加容易。

- 借助于 `style-loader`，使用模块热替换来加载 CSS 实际上极其简单。此 loader 在幕后使用了 `module.hot.accept`，在 CSS 依赖模块更新之后，会将其 patch(修补) 到 `<style>` 标签中。

- 社区还提供许多其他 loader 和示例，可以使 HMR 与各种框架和库平滑地进行交互…… ,比如 vue-loader

####  样式处理

- 在生产环境下,我们希望样式存在于单独的css文件中, 而不是style标签内, 因为文件更有利于客户端缓存,  这时我们使用 mini-css-extract-plugin

- >  多样式提取:
  >
  > 样式的提取是以资源入口开始的整个chunk单位的封装, 如果index.js引入很多的模块,每个模块引入各自的样式, 但是最终只会生成一个css文件,因为只有一个入口, 当有多个入口时,生成多个css文件,但是名字会重复,所以要使用[name].css 进行动态命名

- > pligins 设置中,处理可以指定同步加载的css文件 filename  也可以指定异步加载的css资源 chunkfileName
  >
  > filename是主入口的文件名，chunkFilename是非主入口的文件名
  >
  > 主入口 : 指的是`entry`里面生成出来的文件名
  >
  > 非主入口  :指的是按需加载（异步）模块,在entry没有名字
  >
  > 在打包时,同步加载的会被打包成一个 称为 initial chunk    默认main
  >
  > 异步 按需加载的会被打包为另一个包 称为 :non-initial chunk,默认使用唯一 ID 来替代名称

  - 在打包过程中，模块会被合并成 chunk。 chunk 合并成 chunk 组, *一个 chunk 组中可能有多个 chunk。例如，*[SplitChunksPlugin](https://webpack.docschina.org/plugins/split-chunks-plugin/) *会将一个 chunk 拆分为一个或多个 chunk。*

  - > 想要在浏览器中查看css源码 需要给css-loader  scss-loader 单独配置source map选项

  
  
  
  

 

#### 代码分离(分片):

-  能够把代码分离到不同的 bundle 中，然后可以按需加载或并行加载这些文件。代码分离可以用于获取更小的 bundle，以及控制资源加载优先级，如果使用合理，会极大影响加载时间。

常用的代码分离方法有三种：

- > 通过入口起点：entry 配置 ,手动地分离代码。  通过entry 配置生成多个入口文件,每个页面只加载对应的bundle
  >
  > 使用插件：使用 [`SplitChunksPlugin`](https://webpack.docschina.org/plugins/split-chunks-plugin) 去重和分离 chunk。
  >
  > 动态导入：通过模块的内联函数调用来分离代码

- 通过入口起点手动设置的坏处: 
  
- > 如果入口 chunk 之间包含一些重复的模块(都引入了一个库)，那些重复模块都会被引入到各个 bundle 中。
  >
  > 如果将公共模块拆分出来为一个bundle,都加载这个公共模块,(如果只需要公共模块的一部分,手动拆分更麻烦) 但是并不是所有的页面都需要公共模块,还是需要手工配置
  >
  > 这变得十分复杂,不能动态地将核心应用程序逻辑中的代码拆分出来。 所以我们利用插件解决这个问题
  
- > 提取公共模块的另一种方法:  该方法可以防止在入口chunk引入重复第三方库
  >
  > 配置 [`dependOn` option](https://webpack.docschina.org/configuration/entry-context/#dependencies) 选项，这样可以在多个 chunk 之间共享模块。这个 chunk 就不会包含 `dependOn配置中` 的模块了

- 通过插件进行代码切片 : 

- > 公共模块提取的收益 :  开发过程少了重复模块的打包, 提升开发速度, 减少整体体积  合理分片之后更有效利用客户端缓存

- webpack4 之前的插件CommonsChunkPlugin ,  可以通过配置name /filename提取公共模块(第三方库 ..)

- > chunks 配置项: 规定从哪些入口提取公共模块
  >
  > 通过minChunks , 设置提取规则    minChunks:3 被引用三次才提取 ,还可以传入函数

- > 在使用该插件时, 绕不开的问题是 hash 和长效缓存
  >
  > 提取后的公共模块代码,往往包含 webapck runtime 初始化环境的代码, 导致每次打包就算内容没有改变, 也会影响chunk hash的变化,而我们使用 chunk hash 作为资源的版本号优化客户端的缓存,这导致用户频繁的更新资源
  >
  > 解决方法:
  >
  > 把runtime 的代码提取出来 , 通过在提取完公共模块后, 再调用该插件提取一次
  >
  > 或者 可使用 [`optimization.runtimeChunk`](https://webpack.docschina.org/configuration/optimization/#optimizationruntimechunk) 选项将 runtime 代码拆分为一个单独的 chunk。将其设置为 `single` 来为所有 chunk 创建一个 runtime bundle
  >
  > 这叫做**提取引导模板**

- > 一个CommonsChunkPlugin 只能提取一个vendor, 异步加载出现错误, 提取runtime 的代码导致浏览器多加载资源
  >
  > 现在使用 optimization.SplitChunks 代替

- **optimization.SplitChunks** 

- > 默认情况下的自动提取条件:
  >
  > 被多次引用的 / 在node_modules 的被提取
  >
  > 提取出来的体积chunk体积 大于 20kg
  >
  > 按需加载(动态插入script便签) 并行请求数量小于等于30
  >
  > 首次加载时请求数量小于等于30

- > 参数解释:
  >
  > chunks :   => SplitChunks 默认针对异步资源生效, async(默认值)  initial: 只对入口chunk生效, all:对所有的
  >
  > minChunks : 公共模块被共享的最底次数
  >
  > cacahGroups :  分离chunks时的规则  一般有vendors 和default两种   vendors 代表在 node_module的区块
  >
  > default : 代表多次被引用的区块

- 资源异步加载问题

- > 页面初次渲染时为了下载的资源尽可能的小. 我们可以把一些资源延迟加载,不让其堵塞
  >
  > 通过使用import() 来延迟加载, 延迟加载的属于间接资源, 通过设置out.publicPath, 来设置获取资源的路径
  >
  > import() 实现原理就是动态生成script插入文档

- 异步资源的chunk ,资源生成名字默认为数字 id, 我们可以通过特有注释来获取异步资源 chunk的name

- > import(/*webpackChunkName :"bar"*/  './bar.js') output"{ chunkFilename: [name].js }

#### 生产环境

- *development(开发环境)* 和 *production(生产环境)* 这两个环境下的构建目标存在着巨大差异。

- > 在*开发环境*中，我们需要：强大的 source map 和"wds"以及热替换。
  >
  > *生产环境*目标则转移至其他方面，关注点在于压缩 bundle、更轻量的 source map、资源优化(让用户更快加载资源,最大限度利用缓存, 资源的压缩)，
  >
  > 
  >
  > 通过这些优化方式改善加载时间。由于要遵循逻辑分离，我们通常建议为每个环境编写**彼此独立的 webpack 配置**。
  >
  > 也可以使用同一个配置文件,不过要在webpack.config.js文件内添加判断条件来使用那个配置

- 编写**彼此独立的 webpack 配置** : 使用一个名为 [`webpack-merge`](https://github.com/survivejs/webpack-merge) 的工具,此工具会引用 "common" 配置，因此我们不必再在环境特定(environment-specific)的配置中编写重复代码。



- 开发环境和生产环境通常需要添加不同的**环境变量** ,使用DefinePlugin 设置

- > 通过添加mode:"production" , 自动会进行设置, 不再人工进行添加

- **source map 配置**

- 对于 js文件添加devtool 配置即可,对于css less scss来说,需要添加额外的source map选项

- > 开发环境下 cheap-moudle-eval-source-map 通常是一个不错的选择
  >
  > 生产环境下  只有三种可供选择, 三种在安全性上各不相同 nosources-source-map 可以

- **资源压缩 (uglify)** ,意思是移除多余空格, 换行 和不执行的代码, 缩短变量名.. 使代码形式变得更短, 压缩之后的代码基本上不可读

- > **压缩JavaScript** : webpack4之后默认 使用了terser 的插件 terser-webpack-plugin, 可以在optimization 中的minisize:true, 设置开启,   minimizer选项则可以自定义 terser插件的功能
  >
  > 如果mode : production  则会自动设置,不用再人为设置
  >
  > 
  >
  > **压缩 css:** 通过mini-css-extract-plugin 将样式提取出来到单独文件, 再使用optimize-css-assets-webpack-plugin 进行压缩(本质上是使用 压缩器 cssnano)  
  >
  > optimization.minimizer 配置项中提供一个/多个压缩工具,optimize-css-assets-webpack-plugin要在其中设置
  >
  > 现在使用 CssMinimizerWebpackPlugin 插件 ,他优化了上面的插件
  >
  > 如果要在开发中也运行它，则将 选项设置为 。`optimization.minimize``true`,不加的话仅在生产环境下压缩

- **缓存:**重复利用浏览器已经获取过的资源,合理的使用缓存是提升客户端性能的一个关键因素

- > 当开发者更新了bug,希望立即更新到用户的浏览器上,而不是使用客户端旧的缓存,最好的办法是改变资源的url,一个常用的办法时改变文件的名字 :
  >
  > 在每次打包时,对资源内容计算一次hash,作为版本号放在文件名中,每当代码更新,hash就会变化,改变文件名,迫使客户端重新下载更新过的代码(使用chunkhash 作为文件版本号)
  >
  > 每次更改完名字之后,html引入资源的路径就会改变,这时使用html-webpack-plugin自动化改变, 在该插件中还可以使用模板来指定html 

- > 为了使用缓存,我们经常将不变动的代码分离出来,但是注意在webpack 3以下,使用commonsChunkPlugin时,
  >
  > 会有vendor chunk hash变化的问题,原因是新的模块插入到公共模块之前,会导致模块的id发生变化,使用插件webpack-hashed-moudle-id-plugin 解决
  >
  > 
  >
  > webpack4之后id的生成发生变了,不在有这个问题 ,如果有问题参考`optimization.moduleIds`配置项

- **bundle体积分析:** 

- > vscode 中import Cost 插件帮助我们实时的对引入模块大小监测, 或使用 [官方分析工具](https://github.com/webpack/analyse)   来保证输出资源不会超限之后被发布出去



#### 打包优化

- 让打包速度更快,资源输出体积更小 (对应官网的构建性能)

- 使用 **happyPack** 多线程进行打包(webpack本身是单线程的,只能一个一个的通过依赖关系查找进行转译),适用于转译任务比较重的项目效果明显,对于小项目来说并不明显

- 缩小打包作用域 : 

- >  使用include exclude 
  >
  > 有些库不希望webpack 进行解析,即不应用任何的loader,但仍然会被打包进资源文件,可以使用 moudle中的noParse: /loadsh/ 指明模块名字
  >
  > 使用ignorePlugin ,他可以完全排除一些模块, 即使被引用也不会被打包, 对于排除一些库的相关文件非常有用,
  >
  > 一些库产生的额外资源我们用不到,但是引用语句在库文件内,我们也无法去掉,这时使用该插件不打包

- loader 中的cache 选项.可以在转译之后保存一份缓存,当源文件不变,直接使用缓存,但是他不能检测缓存是否过期(比如更新babel-loader的配置,由于源代码没有变换,打包后将会使用缓存,这是不合理的),官方后期会优化,

- 使用 DIIplugin插件,对一些不经常改变的公共(第三方)模块,进行预先打包,到工程部署时使用DiiReferencePlugin来索引打包好的文件, DIIplugin和 代码分离类似,但是前者会把整个模块拆出来,代码分离会根据规则拆分, 相应的前者需要单独一个配置文件

- **Tree Shaking:**

- 通常用于描述移除 JavaScript 上下文中的未引用代码(dead-code)。该特性依赖于es6 Moudlede 静态编译的特性,es6会在代码编译时确定依赖关系,可以检测出,没有引用过的模块(代码块),webpack进行标记,在开发环境下仍然可见, 在生产环境下**资源压缩时**将他们从最终的bundle去掉

- tree Shaking 只对es6 module 有用, 对于通过commonjs 引用进来的没有用处

- > 在工程中使用 babel-loader ,那么一定要通过配置来禁用它的模块依赖解析,因为解析过之后,webpack接受的都是commonjs形式的模块, 在loader 中options:{presets:[@babel/preset-env,{moudle:false}]}

- tree shaking 本身只对死代码进行标记, 真正去除死代码的是在生产环境下资源压缩那一步

- tress shaking 的自身优化:

- > 配置 optimization: { usedExports: true,}  , 告知 webpack 去决定每个模块使用的导出内容。在压缩工具中的无用代码清除会受益于该选项，而且能够去除未使用的导出内容。
  >
  > 但是有些内容虽然没有使用,但是去除之后可能会有副作用,`usedExports` 依赖于 [terser](https://github.com/terser-js/terser) 去检测语句中的副作用,我们可以通过 `/*#__PURE__*/` 注释来帮忙 terser。它给一个语句标记为没有副作用
  >
  > 所以使用**`sideEffects` 更为有效** 是因为它允许跳过整个模块/文件和整个文件子树,它类似于 `/*#__PURE__*/` 但是作用于模块的层面，而不是代码语句的层面。

- **`sideEffects` 更为有效**
- > 如果所有代码都不包含 side effect，我们就可以简单地将该属性标记为 false
  > 如果你的代码确实有一些副作用，可以改为提供一个数组,
  > "sideEffects": [
  >     "./src/some-side-effectful-file.js"
  >   ]  
  >
  > 还可以在 `module.rules` 配置选项中设置 `"sideEffects"`。

- > 所有导入文件都会受到 tree shaking 的影响。这意味着，如果在项目中使用类似`css-loader` 并 import 一个 CSS 文件，则需要将其添加到 side effect 列表中，以免在生产模式中无意中将它删除

- > 在使用 tree shaking 时必须有 [ModuleConcatenationPlugin](https://webpack.docschina.org/plugins/module-concatenation-plugin) 的支持，您可以通过设置配置项 `mode: "production"` 以启用它。如果您没有如此做，请记得手动引入 [ModuleConcatenationPlugin](https://webpack.docschina.org/plugins/module-concatenation-plugin)。


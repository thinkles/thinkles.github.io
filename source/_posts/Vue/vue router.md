---

title:  Vue Router
date: {{ date }}
updated: {{date}}
tags: vue router
categories: Vue

---
## Vue Router

#### 动态路由匹配

- 通过不同参数匹配到的所有路由，全都映射到同个组件

- > 例如 :  将`/user/foo` 和 `/user/bar` 映射到相同的路由。




- 通过动态路径参数实现这个效果:

- |                               | **$route.params**                    |
  | :---------------------------- | ------------------------------------ |
  | /user/:username               | { username: 'evan' }                 |
  | /user/:username/post/:post_id | { username: 'evan', post_id: '123' } |

- > **$route.params** 获得动态路径参数
  >
  > $route.query    $route.hash  提供其他信息

监听路由变化

- 由于动态路由使用不同参数,访问的都是同一组件，组件会被复用以提升效率；

- > **这也意味着组件的生命周期钩子不会再被调用**。那怎么监听路由的变化呢？
  >
  > 可以利用Watch `$route` 对象来对路由参数的变化进行监测  , 监听$route(to, from)判断去处和来源；
  >
  > 或者使用 2.2 中引入的 `beforeRouteUpdate` 导航守卫 

- ```javascript
  组件:
  const User = {
    template: '...',
    watch: {
      $route(to, from) {
        // 对路由变化作出响应... } }
  }
  ```



- 有时路由不存在会得不到任何结果，可以设置捕获所有路由或 404

- ```javascript
  {
    // 会匹配所有路径
    path: '*'}
  {
    // 会匹配以 `/user-` 开头的任意路径
    path: '/user-*'
  }
  ```

- 当使用一个*通配符*时，`$route.params` 内会自动添加一个名为 `pathMatch` 参数。它包含了 URL 通过*通配符*被匹配的部分

- >  { path: '/user-*' }
  > this.$router.push('/user-admin')
  > this.$route.params.pathMatch // 'admin'



- 路由路径高级匹配模式

- > `vue-router` 使用 [path-to-regexp](https://github.com/pillarjs/path-to-regexp/tree/v1.7.0) 作为路径匹配引擎，所以支持很多高级的匹配模式，查看文档获取更高级的匹配形式

- 同一个路径可以匹配多个路由，此时，匹配的优先级就按照路由的定义顺序



####  嵌套路由

- 实际应用界面中, 通常由多层嵌套的组件组合而成:类似栏目分类,而路由也应该按某种结构对应这种嵌套关系,借助 `vue-router`，使用嵌套路由配置，就可以很简单地表达这种关系。

- ```js
  // 在 children 选项中定义嵌套路由设置
  new VueRouter({routes: [
      {
        name:'user'
        path: '/user/:id', component: User,
        children: [
          // 当 /user/:id 匹配成功，
          //  UserHome组件会被渲染在 User组件中的 <router-view>中
          { path: 'username', component: UserHome },
  
          // ...其他子路由
        ]
                })
  
  ```

- > 如果不在父路由组件中使用 <router-view>, 那么将不会渲染子路由组件

- > 这里组件名称和路径设置的不一样，路由采用的是路径； 
  >
  > 子路由不需要 / ，因为斜杠会被当成根路径； 

- 可以嵌套多层路由,而且子路由不需要 / 设置更方便, 不要忘记 <router-view>否则不会渲染





#### 编程式导航

- 使用 `<router-link>` 可以创建 a 标签来定义导航链接,但是缺乏编程性

- | 声明式                  | 编程式           |
  | ----------------------- | ---------------- |
  | <router-link :to="..."> | router.push(...) |

- **在 Vue 实例内部，你可以通过 `$router` 访问路由实例** (注意和$route的区别  route路由对象的属性)

- **router.push()**函数:

- > 想要导航到不同的 URL，则使用 `router.push` 方法。这个方法会向 history 栈添加一个新的记录

- ```js
  // 该方法的参数可以是一个字符串路径，或者一个描述地址的对象
  router.push('/home')  // 注意加上/就代表根目录, 
  
  // 命名的路由
  router.push({ name: 'user', params: { userId: '123' }})
  
  // 带查询参数，变成 /register?plan=private
  router.push({ path: 'register', query: { plan: 'private' }})
  
  // 如果提供了 path，params 会被忽略,path和query是配套的
  
  const userId = '123'
  router.push({ path: `/user/${userId}/test` }) // -> /user/123/test 
  //嵌套多层路由时,只能通过path解析路径
  
  ```

  

- **router.replace(...)函数:**

- > 跟 `router.push` 很像，但是它不会向 history 添加新记录， 而是替换掉当前的 history 记录。

- 在 2.2.0+，可选的在 `router.push` 或 `router.replace` 中提供 `onComplete` 和 `onAbort` 回调作为第二个和第三个参数。这些回调将会在导航成功完成 (在所有的异步钩子被解析之后) 或终止 (导航到相同的路由、或在当前导航完成之前导航到另一个不同的路由) 的时候进行相应的调用。

- **router.go(n):函数:**

- > 类似 `window.history.go(n)`,意思是在 history 记录中向前或者后退多少步
  >
  > 1 前进一步   -1 后退一步



- 如果目的地和当前路由相同，只有参数发生了改变 (比如从一个用户资料到另一个 `/users/1` -> `/users/2`)，你需要使用 [`beforeRouteUpdate`](https://router.vuejs.org/zh/guide/essentials/dynamic-matching.html#响应路由参数的变化) 来响应这个变化 (比如抓取用户信息)





#### 命名路由和视图

- 通过一个名称来标识一个路由显得更方便一些，特别是在链接一个路由，或者是执行一些跳转的时候,通过Router实例中的`name`属性实现

- ```
  routes: [
      {
        path: '/user/:userId',
        name: 'user',
        component: User
      }
  ```

- 要链接到一个命名路由，可以给 `router-link` 的 `to` 属性传一个对象：

- > <router-link :to="{ name: 'user', params: { userId: 123 }}">User</router-link>
  >
  > 注意这时候的 to是通过 v-bind绑定的, 我们通过name值 ,完成跳转



- **命名视图** : 有时候想同时 (同级) 展示多个视图，而不是嵌套展示，例如创建一个布局，有 `sidebar` (侧导航) 和 `main` (主内容) 两个视图，这个时候命名视图就派上用场了。

- > 如何理解同级展示 嵌套展示:  当通过嵌套路由设置router-view 这时会渲染成嵌套的div, 而通过命名视图设置router-view ,则是  同级的div块

- 通过设置多个带有名字的 `router-view`,没有名字默认为 `default`

- ```html
  <router-view class="view one"></router-view>
  <router-view class="view two" name="a"></router-view>
  <router-view class="view three" name="b"></router-view>
  ```

- 一个视图使用一个组件渲染，因此对于同个路由，多个视图就需要多个组件。这时在路由上需要使用`components`来规定子组件

- ```js
  const router = new VueRouter({
    routes: [
      {
       path: '/',
        components: {
          default: Foo,  //还可以是懒加载, () => import('../views/About.vue')也是可以
          a: Bar,
          b: Baz
        }
      }
    ]
  })
  ```

- > 我们也有可能使用命名视图创建嵌套视图的复杂布局, 展示嵌套视图时还存在嵌套路由

- ```js
  { 
      path:'/long/:id',
      name:'long',
      component:long,
      children:[{
        path:"test",
        components:{
          default:Test,  
          sec:button,
        },
        children : [
          {
              path : 'list',
              component :List,
          }
      ]   
      /* 展示嵌套视图里的嵌套路由 , 默认children 是第一个元素的 */
   },
   ]}, 
  ```

  

#### 重定向和别名

- “重定向”的意思是，当用户访问 `/a`时，URL 将会被替换成 `/b`，然后匹配路由为 `/b`

- 使用 redirect，可以配置路由的重定向功能，有多种方式；

- ```js
  routes: [
      { path: '/a', redirect: '/b' }   //使用路径
      { path: '/a', redirect: { name: 'foo' }}   //使用路由名字
    ]
  
  方法:
  routes: [
      { path: '/a', redirect: to => { }
        // 方法接收 目标路由 (被跳转的路由) 作为参数
        // return 重定向的 字符串路径/路径对象
      } //每次点击不同的链接跳转时 总会触发redirect函数
  
  ```

  - 注意[导航守卫](https://router.vuejs.org/zh/guide/advanced/navigation-guards.html)并没有应用在跳转路由上，而仅仅应用在其目标上。

- 别名 : **`/a` 的别名是 `/b`，意味着，当用户访问 `/b` 时，URL 会保持为 `/b`，但是路由匹配则为 `/a`，就像用户访问 `/a` 一样。**

- > ```js
  >  routes: [
  >     { path: '/a', component: A, alias: '/b' }
  >   ]
  > “别名”的功能让你可以自由地将 UI 结构映射到任意的 URL，而不是受限于配置的嵌套路由结构。
  > ```



#### 路由组件间的传参

- 在组件中使用` $route `会使之与其对应路由形成高度耦合，从而使组件只能在某些特定的 URL 上使用，限制了其灵活性。
- 使用 `props` 将组件和路由解耦:

- ```js
  const User = {
    template: '<div>User {{ $route.params.id }}</div>'
  }
  const router = new VueRouter({
    routes: [
      { path: '/user/:id', component: User }
    ]
  })
  
  解耦 :
  const User = {
    props: ['id'],
    template: '<div>User {{ id }}</div>'
  }
  
  const router = new VueRouter({
    routes: [
      { path: '/user/:id', component: User, props: true },
  
      // 对于包含命名视图的路由，你必须分别为每个命名视图添加 `props` 选项：
      {
        path: '/user/:id',
        components: { default: User, sidebar: Sidebar },
        props: { default: true, sidebar: false }
      }
    ]
  })
  ```

  

- 三种模式 : 布尔模式  对象模式 函数模式

- >布尔模式 :  `props` 被设置为 `true`，`route.params` 将会被设置为组件属性。
  >
  >这样可以直接使用 route.params 中的对象, 注意这里的 prop 的名字 需要对应route.params 中的数据

  >有对于静态路由，可以使用对象模式；
  >
  >```html
  >props : { 
  >name : '列表' 
  >},
  ><template> 
  ><h3>这里是 List 页面 : {{name}}</h3> 
  ></template>
  >
  >对于包含命名视图的路由，你必须分别为每个命名视图添加 `props` 选项：
  >props : { default : { name : '列表' } },
  >
  >```

- > 函数模式  创建一个函数返回 `props`。这样你便可以将参数转换成另一种类型，将静态值与基于路由的值结合等等。
  >
  > ```js
  >  routes: [
  >     { path: '/search', component: SearchUser, props: (route) => ({ query: route.query.q }) }
  >   ]
  > ```
  >
  > 



#### HTML5 history模式

- `vue-router` 默认 hash 模式 —— 使用 URL 的 hash 来模拟一个完整的 URL，于是当 URL 改变时，页面不会重新加载。

- 如果不想要很丑的 hash，我们可以用路由的 **history 模式**，这种模式充分利用 `history.pushState` API 来完成 URL 跳转而无须重新加载页面。

- > ```js
  > const router = new VueRouter({
  >   mode: 'history',
  >   routes: [...]
  > })
  > ```

- 需要后台配置   

- > 待拓展





#### 导航守卫

- 什么是导航 : 表示路由正在发生改变。导航守卫主要作用: 是通过跳转/取消的方式 守卫导航 ,路由跳转时会触发一些钩子函数,这些钩子函数被称为导航守卫

- > 记住**参数或查询的改变并不会触发进入/离开的导航守卫**。你可以通过[观察 `$route` 对象](https://router.vuejs.org/zh/guide/essentials/dynamic-matching.html#响应路由参数的变化)来应对这些变化，或使用 `beforeRouteUpdate` 的组件内守卫。

- 导航守卫有多种: 全局  组件  路由独享

- **全局前置守卫** :

- 使用 `router.beforeEach((to,from,next)=>{})` 注册一个全局前置守卫

- > **`to: Route`**: 即将要进入的目标 [路由对象](https://router.vuejs.org/zh/api/#路由对象)
  >
  > **`from: Route`**: 当前导航正要离开的路由
  >
  > **`next: Function`**: 一定要调用该方法来 **resolve** 这个钩子。执行效果依赖 `next` 方法的调用参数。
  >
  >  不执行 next 函数,无法继续运行
  >
  > - next()   next(false)   next('/')  `next(error)` 四类不同的调用方式

- 确保 `next` 函数在任何给定的导航守卫中都被严格调用一次。他可以出现多次但只能调用一次, 

- > ` RangeError: Maximum call stack size exceeded` 
  >
  > 注意无限递归问题 ,因为不加判断条件往往导致无限递归 

  

- **全局解析守卫** :  `router.beforeResolve` 注册一个全局守卫。

- > 和`router.beforeEach` 类似, 区别在于 调用顺序的不同   待拓展...

- **全局后置钩子** : `router.afterEach((to, from) => {}` , 这些钩子和守卫不同, 不会接受 `next`函数自然不会改变导航本身, ，一般用于路由页面加载完毕之后操作一些动作，比如取消 loading



**路由独享的守卫**:

- 在路由配置上直接定义 `beforeEnter` 守卫：

- ```js
  const router = new VueRouter({
    routes: [
      {
        path: '/foo',
        component: Foo,
        beforeEnter: (to, from, next) => {
          // ...
        }}]})
  // 这些守卫与全局前置守卫的方法参数是一样的。
  ```

- > 路由中配置的守卫, 当从该路由跳转  或者进入该路由调用守卫

**组件内的守卫:**

- ```js
  const Foo = {
    template: `...`,
    beforeRouteEnter (to, from, next) {
      // 在渲染该组件的对应路由被 confirm 前调用
      // 不！能！获取组件实例 `this`
      // 因为当守卫执行前，组件实例还没被创建
    },
    beforeRouteUpdate (to, from, next) {
      // 在当前路由改变，但是该组件被复用时调用
      // 举例来说，对于一个带有动态参数的路径 /foo/:id，在 /foo/1 和 /foo/2 之间跳转的时候，
      // 可以访问组件实例 `this`
    },
    beforeRouteLeave (to, from, next) {
      // 导航离开该组件的对应路由时调用
      // 可以访问组件实例 `this`
    }
  }
  ```



- beforeRouteEnter 守卫不能 访问 `this`, 不过，你可以通过传一个回调给 `next`来访问组件实例。在导航被确认的时候执行回调，并且把组件实例作为回调方法的参数。 注意 `beforeRouteEnter` 是支持给 `next` 传递回调的唯一守卫

- ```
  beforeRouteEnter (to, from, next) {
    next(vm => {
      // 通过 `vm` 访问组件实例
    })
  }
  ```

- 导航守卫钩子函数的执行顺序  : [#](https://router.vuejs.org/zh/guide/advanced/navigation-guards.html#完整的导航解析流程)完整的导航解析流程 

   

#### 路由元信息  过渡效果

- 定义路由的时候可以配置 `meta` 字段, 一般设置为对象

- 过渡效果:

- ```html
  <transition name="fade">
  <router-view/>
  </transition>
  //Transition 的所有功能 在这里同样适用
  ```

- 单个路由的过渡: 在路由组件上使用<transition>

- ```
  const Foo = {
    template: `
      <transition name="slide">
        <div class="foo">...</div>
      </transition>
    `
  }
  ```

- 基于当前路由与目标路由的变化关系,动态设置过渡

- ```html
  <transition :name="transitionName">
    <router-view></router-view>
  </transition>
  接着在父组件内
  // watch $route 决定使用哪种过渡
  watch: {
    '$route' (to, from) {
     const toDepth = to.path.split('/').length
  ...
  }
  
  
  ```



#### 数据获取

- 有时候，进入某个路由后，需要从服务器获取数据。例如，在渲染用户信息时，你需要从服务器获取用户的数据。我们可以通过两种方式来实现：

- > 1. **导航完成之后获取**：先完成导航，然后在接下来的组件生命周期钩子中获取数据。在数据获取期间显示“加载中”之类的指示。
  > 2. **导航完成之前获取**：导航完成前，在路由进入的守卫中获取数据，在数据获取成功后执行导航。



#### 滚动行为  路由懒加载

滚动行为:

- 使用前端路由，当切换到新路由时，想要页面滚到顶部，或者是保持原先的滚动位置，就像重新加载页面那样

- > **注意: 这个功能只在支持 `history.pushState` 的浏览器中可用。**

- ```js
  const router = new VueRouter({
    routes: [...],
    scrollBehavior (to, from, savedPosition) {
    if (savedPosition) {
      return savedPosition  //保存之前的位置  通过浏览器的 前进/后退 按钮触发
    } else {
      return { x: 0, y: 0 }
    }
      // return 期望滚动到哪个的位置
    }
  })
  //  
  ```

  

- 路由懒加载 : 当打包构建应用时，JavaScript 包会变得非常大，影响页面加载。如果我们能把不同路由对应的组件分割成不同的代码块，然后当路由被访问的时候才加载对应组件，这样就更加高效了。

- ```js
  { path: '/about', 
  name: 'About', 
  component: () => import('../views/About.vue') 
  }
  ```

  

- 


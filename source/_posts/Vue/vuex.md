---

title:  Vuex 
date: {{ date }}
updated: {{date}}
tags: vuex
categories: Vue

---
## Vuex

- Vuex 是一个专为 Vue.js 应用程序开发的**状态管理模式**。它采用**集中式存储管理**应用的**所有组件**的状态，并以相应的规则保证状态以一种可预测的方式发生变化。

- 单向数据流

- > State --> View-->Actions--->State    形成一个循环, 但是当我们多个组件共享状态时,单向数据流会被破坏
  >
  > - 多个视图依赖于同一状态。
  > - 来自不同视图的行为需要变更同一状态。
  >
  > 因此，我们把组件的共享状态抽取出来，以一个全局单例模式管理,在这种模式下，我们的组件树构成了一个巨大的“视图”，不管在树的哪个位置，任何组件都能获取状态或者触发行为

- Vuex 依赖 [Promise (opens new window)](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Using_promises)。如果你支持的浏览器并没有实现 Promise (比如 IE)，那么你可以使用一个 polyfill 的库，例如 [es6-promise (opens new window)](https://github.com/stefanpenner/es6-promise)。

#### Store 模式

- ```js
  var store = {
    debug: true,
    state: {
      message: 'Hello!'
    },
    setMessageAction (newValue) {
      if (this.debug) console.log('setMessageAction triggered with', newValue)
      this.state.message = newValue
    },
    clearMessageAction () {
      if (this.debug) console.log('clearMessageAction triggered')
      this.state.message = ''
    }
  }
  
  var vmA = new Vue({
    data: {
      privateState: {},
      sharedState: store.state  //store模式 不需要 store.state.message 这样写到具体属性值, 如果写到属 性值 调用 mutation无法进行更新数据
         // 组件和 store 需要引用同一个共享对象，变更才能够被观察到。
    }
  })
  
  var vmB = new Vue({
    data: {
      privateState: {},
      sharedState: store.state
    }
  })
  ```



- 所有 store 中 state 的变更，都放置在 store 自身的 action 中去管理。当错误出现时，我们现在也会有一个 log 记录 bug 之前发生了什么。

- > 组件不允许直接变更属于 store 实例的 state，而应执行 action 来分发 (dispatch) 事件通知 store 去改变







#### 开始

- 每一个 Vuex 应用的核心就是 store（仓库）, store 基本上是一个容器, 包含所有的state(状态).Vuex 它和单纯的全局对象不同

- > Vuex 的状态存储是响应式的   store 状态的改变,相应的组件也会发生相应的更新
  >
  > 不能直接改变 store中的状态, 只能通过显示提交 mutation 改变状态, 这样我们方便追踪每个状态的变化

- 为了在 Vue 组件中访问 `this.$store` property，你需要为 Vue 实例提供创建好的 store。Vuex 提供了一个从根组件向所有子组件，以 `store` 选项的方式“注入”该 store 的机制：

- ```js
  new Vue({
    el: '#app',
    store: store,
  })
  
  //现在可以在组件中通过提交mutation的方式, 来改变数据
  methods: {
    increment() {
      this.$store.commit('increment')
      console.log(this.$store.state.count)
    }
  }
  ```

- 再次强调，我们通过提交 mutation 的方式，而非直接改变 `store.state.count`



#### State

- Vuex 使用**单一状态树**——是的，用一个对象就包含了全部的应用层级状态, 这也意味着，每个应用将仅仅包含一个 store 实例。

- 从 store 实例中读取状态最简单的方法就是在计算属性中返回某个状态：

- ```js
  const Counter = {
    template: `<div>{{ count }}</div>`,
    computed: {
      count () {
        return store.state.count
      }
    }
  }
  // 这种模式导致组件依赖全局状态单例。在模块化的构建系统中，在每个需要使用 state 的组件中需要频繁地导入
  // 所以Vuex 通过 store 选项，提供了一种机制将状态从根组件“注入”到每一个子组件中, 这样子组件能通过 this.$store 访问到
  //  更改为       return this.$store.state.count
  ```



- 当一个组件需要获取多个状态的时候，将这些状态都声明为计算属性会有些重复和冗余,使用 `mapState` 辅助函数帮助我们生成计算属性

- 我们在需要使用这个辅组函数的地方导入 , `import { mapState } from 'vuex'`

- > 在单独构建的版本中辅助函数为 Vuex.mapState



- 当计算属性的名称与 state 的子节点名称相同时,使用数组语法

- ```js
  computed: mapState(['count','name','age'])
  ```

  

- 普通情况使用对象语法:

- ```js
  export default {
    // ...
    computed: mapState({
      // 箭头函数可使代码更简练
      count: state => state.count,
  
      // 传字符串参数 'count' 等同于 `state => state.count`
      countAlias: 'count',
  
      // 为了能够使用 `this` 获取局部状态，必须使用常规函数
      countPlusLocalState (state) {
        return state.count + this.localCount
      }
    })
  ```

  

- `mapState` 函数返回的是一个对象。我们如何将它与局部计算属性混合使用, 使用对象展开符 `...`

  ```
  computed: {
    localComputed () {},
    // 使用对象展开运算符将此对象混入到外部对象中
    ...mapState({  }),
  }
  
  ```

- > 使用 Vuex 并不意味着你需要将**所有的**状态放入 Vuex, 如果有些状态严格属于单个组件，最好还是作为组件的局部状态。



#### Getters

- 获取state状态后,我们有时会对其进行处理在computed中, 当多个组件都需要这个处理过的值, 每个组件都进行computed处理转态太麻烦, 使用getters 派生出state处理过后的状态, 直接调用getter

- > 在 store 中定义“getter”（可以认为是 store 的计算属性）。就像计算属性一样，getter 的返回值会根据它的依赖被缓存起来，且只有当它的依赖值发生了改变才会被重新计算。

- ```js
  const store = new Vuex.Store({
    state: {
      todos: [
        { id: 1, text: '...', done: true },
        { id: 2, text: '...', done: false }
      ]
    },
    getters: {
      doneTodos: state => {   // Getter 接受 state 作为其第一个参数：
        return state.todos.filter(todo => todo.done)
      }
      doneTodosCount: (state, getters)=>{}  可以接受其他 getter 作为第二个参数
                           
      getTodoById: (state) => (id) => {
      return state.todos.find(todo => todo.id === id)
    }
    }
  })
  
  通过属性访问   store.getters.doneTodos getter 在通过属性访问时是作为 Vue 的响应式系统的一部分缓存其中的
  
  通过方法访问  store.getters.getTodoById(2)  getter 在通过方法访问时，每次都会去进行调用，而不会缓存结果
  ```

  

- 在组件中使用:

- ```js
  computed: {
    doneTodosCount () {
      return this.$store.getters.doneTodosCount  属性访问
       return this.$store.getters.doneTodosCount(id)   方法访问
    }
  }
  ```

  

- `mapGetters` 辅助函数:  仅仅是将 store 中的 getter 映射到局部计算属性.

- ```js
  import { mapGetters } from 'vuex'
  
  export default {
    // ...
    computed: {
    // 使用对象展开运算符将 getter 混入 computed 对象中
      ...mapGetters([
        'doneTodosCount',
        'anotherGetter',
        // ...
      ])
    }
  }
  
  重命名: ...mapGetters({
    // 把 `this.doneCount` 映射为 `this.$store.getters.doneTodosCount`
    doneCount: 'doneTodosCount'
  })
  
  ```

  

#### mutation

- 更改 Vuex 的 store 中的状态的唯一方法是提交 mutation,Vuex 中的 mutation 非常类似于事件,每个 mutation 都有一个字符串的 **事件类型 (type)** 和 一个 **回调函数 (handler)**。

- ```js
  mutations: {
      increment (state) {   //回调函数 , 会接受 state 作为第一个参数
        // 变更状态
        state.count++
      }
    }
  // 要唤醒一个 mutation handler，你需要以相应的 type 调用 store.commit 方法：
  store.commit('increment')
  ```

- 提交载荷 : 向 `store.commit` 传入额外的参数, 在大多数情况下，载荷应该是一个对象，这样可以包含多个字段

- ```
  mutations: {
    increment (state, n) {
      state.count += n
    }
  }
  
  store.commit('increment', 10)
  
  载荷是对象:
   increment (state, payload) {
      state.count += payload.amount
    }
    
   store.commit('increment', {
    amount: 10
  }) 
    
  ```

- 对象风格方式的提交 :  当使用对象风格的提交方式，整个对象都作为载荷传给 mutation 函数

- ```
  store.commit({
    type: 'increment',
    amount: 10
  })
  //mutations 不需要变化
  mutations: {
    increment (state, payload) {
      state.count += payload.amount
    }
  }
  ```



- 一条重要的原则就是要记住 **mutation 必须是同步函数**。因为任何在回调函数中的状态改变不可追踪

- > 当你调用了两个包含异步回调的 mutation 来改变状态，你怎么知道什么时候回调和哪个先回调呢？在 Vuex 中，**mutation 都是同步事务**  而处理异步操作需要Action

- 你可以在组件中使用 `this.$store.commit('xxx')` 提交 mutation，或者使用 `mapMutations` 辅助函数将组件中的 methods 映射为 `store.commit` 调用（需要在根节点注入 `store`）。

- ```js
  import { mapMutations } from 'vuex'
  
  export default {
    // ...
    methods: {
      ...mapMutations([
        'increment', // 将 `this.increment()` 映射为 `this.$store.commit('increment')`
  
        // `mapMutations` 也支持载荷：
        'incrementBy' // 将 `this.incrementBy(amount)` 映射为 `this.$store.commit('incrementBy', amount)`
      ]),
      ...mapMutations({
        add: 'increment' // 将 `this.add()` 映射为 `this.$store.commit('increment')`
      })
    }
  }
  ```



#### Actions

- > Action 提交的是 mutation，而不是直接变更状态。
  >
  > Action 可以包含任意异步操作。

- 注册 action:

- ```js
  const store = new Vuex.Store({
    state: {
      count: 0
    },
    mutations: {
      increment (state) {
        state.count++
      }
    },
    actions: { 
      increment (context) {   // 接受一个与 store 实例具有相同方法和属性的 context 对象
        context.commit('increment')
      }
    }
  })
  //使用 es6结构语法:
  actions: {
    increment ({ commit }) {
      commit('increment')
    }
  }
  
  ```



- 触发 action:

- ```js
  store.dispatch('increment')  
  
  // action 也支持载荷, 两种带有载荷的触发方式
  
  store.dispatch('incrementAsync', {
    amount: 10
  })
  
  store.dispatch({
    type: 'incrementAsync',
    amount: 10
  })
  ```

- 你在组件中使用 `this.$store.dispatch('xxx')` 分发 action，或者使用 `mapActions` 辅助函数将组件的 methods 映射为 `store.dispatch` 调用

- > `mapActions` 和 `mapmutations `用法一样 , 



- 组合action:  处理更复杂的异步操作,  使用promise 处理

- > 待拓展...





#### Moudle

- 由于使用单一状态树，应用的所有状态会集中到一个比较大的对象。为了解决以上问题，Vuex 允许我们将 store 分割成**模块（module）**

- ```
  const moduleB = {
    state: () => ({ ... }),
    mutations: { ... },
    actions: { ... }
  }
  
  const store = new Vuex.Store({
    modules: {
      moduleA
    }
  })
  
  store.state.moudleA.数据名  //获得moudleA的状态
  ```

  

- 模块的局部状态

- > 对于模块内部的 mutation 和 getter，接收的第一个参数是**模块的局部状态对象**。
  >
  > 对于模块内部的 action，局部状态通过 `context.state` 暴露出来
  >
  > 对于模块内部的 getter，根节点状态会作为第三个参数暴露出来：



- 命名空间

- 默认情况下，模块内部的 action、mutation 和 getter 是注册在**全局命名空间**的——这样使得多个模块能够对同一 mutation 或 action 作出响应。  这使得模块 之间会产生冲突,使用命名空间隔离状态

- > 你可以通过添加 `namespaced: true` 的方式使其成为带命名空间的模块。当模块被注册后，它的所有 getter、action 及 mutation 都会自动根据模块注册的路径调整命名

- ```
  const store = new Vuex.Store({
    modules: {
      account: {
        namespaced: true,
  
        // 模块内容（module assets）
        state: () => ({ ... }), // 模块内的状态已经是嵌套的了，使用 `namespaced` 属性不会对其产生影响
        getters: {
          isAdmin () { ... } // -> getters['account/isAdmin']
        },
        actions: {
          login () { ... } // -> dispatch('account/login')
        },
        mutations: {
          login () { ... } // -> commit('account/login')
        },
        }
        }
        // 调用时需要加上模块名来调用指定的内容, 可以通过从console.log()来查看嵌套
  ```

- 当使用 `mapState`, `mapGetters`, `mapActions` 和 `mapMutations` 这些函数来绑定带命名空间的模块时，写起来可能比较繁琐：

- ```js
  computed: {
    ...mapState({
      a: state => state.some.nested.module.a,
      b: state => state.some.nested.module.b
    })
  },
  对于这种情况，你可以将模块的空间名称字符串作为第一个参数传递给上述函数，这样所有绑定都会自动将该模块作为上下文。
  computed: {
    ...mapState('some/nested/module', {
      a: state => state.a,
      b: state => state.b
    })
  },
  
  
  ```

  

- 模块动态注册:

- > 你可以使用 `store.registerModule` 方法注册模块：
  >
  > ```
  > // 注册模块 `myModule`
  > store.registerModule('myModule', {
  >   // ...
  > })
  > ```





#### Vuex 进阶

- 模块分离

  

- 严格模式  **不要在发布环境下启用严格模式**！

- > 开启严格模式，仅需在创建 store 的时候传入 `strict: true`
  >
  > 在严格模式下，无论何时发生了状态变更且不是由 mutation 函数引起的，将会抛出错误。这能保证所有的状态变更都能被调试工具跟踪到。

- 表单处理- 
- 测试
- 手动热重载
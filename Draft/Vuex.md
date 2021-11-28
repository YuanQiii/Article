# Vuex

## 前言

Vuex 是一个专为 Vue.js 应用程序开发的**状态管理模式**。它采用集中式存储管理应用的所有组件的状态，并以相应的规则保证状态以一种可预测的方式发生变化。其实它就是对vue做数据管理的，更好的存储数据、响应数据。

## 原理

![](https://vuex.vuejs.org/vuex.png)

## 核心概念

### State

- `Vuex` 使用**单一状态树**，用一个对象就包含了全部的应用层级状态

- `mapState` 辅助函数

  - 当映射的计算属性的名称与 state 的子节点名称相同时，我们也可以给 `mapState` 传一个字符串数组

  ```js
    computed: { 
      // 使用对象展开运算符将此对象混入到外部对象中
      ...mapState({
        // 箭头函数可使代码更简练
        count: state => state.count,
  
        // 传字符串参数 'count' 等同于 `state => state.count`
        countAlias: 'count',
  
        // 为了能够使用 `this` 获取局部状态，必须使用常规函数
        countPlusLocalState(state) {
          return state.count + this.localCount
        }
      }),
  
      // 映射 this.count 为 store.state.count
      ...mapState(['count'])
    }
  ```

### Getter

- 可以认为是 `store` 的计算属性

- 接受 state 作为其第一个参数，接受其他 getter 作为第二个参数

  ```js
  getters: {
    // ...
    doneTodosCount: (state, getters) => {
      return getters.doneTodos.length
    }
  }
  ```

- 通过方法访问

  ```js
  getters: {
    // ...
    getTodoById: (state) => (id) => {
      return state.todos.find(todo => todo.id === id)
    }
  }
  
  store.getters.getTodoById(2) // -> { id: 2, text: '...', done: false }
  ```

- `mapGetters` 辅助函数

  - 将一个 getter 属性另取一个名字，使用对象形式

  ```js
    computed: {
      ...mapGetters([
        'doneTodosCount',
        'anotherGetter',
        // ...
      ]),
  
      ...mapGetters({
        // 把 `this.doneCount` 映射为 `this.$store.getters.doneTodosCount`
        doneCount: 'doneTodosCount'
      })
    }
  ```

### Mutations

- 更改 `Vuex` 的 `store` 中的状态的唯一方法是提交 `mutation`
- 每个 `mutation` 都有一个字符串的 **事件类型 (type)** 和 一个 **回调函数 (handler)**

- 对象风格的提交方式

  - 把载荷和type分开提交
  - 整个对象都作为载荷传给mutation函数

  ```js
  // 把载荷和type分开提交
  store.commit('increment', {
    amount: 10
  })
  
  // 整个对象都作为载荷传给mutation函数
  store.commit({
    type: 'increment',
    amount: 10
  })
  ```

- **mutation 必须是同步函数**

  - 当 `mutation` 触发的时候，回调函数还没有被调用
  - 任何在回调函数中进行的状态的改变都是不可追踪

- `mapMutations` 辅助函数

  ```js
    methods: {
      ...mapMutations([
        'increment', 
        'incrementBy' 
      ]),
      ...mapMutations({
        add: 'increment' // 将 `this.add()` 映射为 `this.$store.commit('increment')`
      })
    }
  ```

### Actions

- `Action` 提交的是 `mutation`，而不是直接变更状态

- `Action` 可以包含任意异步操作

-  `context` 对象参数

  - `context.commit`
  - `context.state`
  - `context.getters`

- 分发 Action

  - Action 通过 `store.dispatch` 方法触发

    ```js
    // 以载荷形式分发
    store.dispatch('incrementAsync', {
      amount: 10
    })
    
    // 以对象形式分发
    store.dispatch({
      type: 'incrementAsync',
      amount: 10
    })
    ```

- `mapActions` 辅助函数

  ```js
    methods: {
      ...mapActions([
        'increment', // 将 `this.increment()` 映射为 `this.$store.dispatch('increment')`
  
        // `mapActions` 也支持载荷：
        'incrementBy' // 将 `this.incrementBy(amount)` 映射为 `this.$store.dispatch('incrementBy', amount)`
      ]),
      ...mapActions({
        add: 'increment' // 将 `this.add()` 映射为 `this.$store.dispatch('increment')`
      })
    }
  ```

- 组合 Action

  ```js
  store.dispatch('actionA').then(() => {
    // ...
  })
  
  actions: {
    // 对象解构
    actionB ({ dispatch, commit }) {
      return dispatch('actionA').then(() => {
        commit('someOtherMutation')
      })
    }
  }
  ```

### Module

- 模块的局部状态

  - 对于模块内部的 `mutation` 和 `getter`，接收的第一个参数是**模块的局部状态对象**
  - `getter`的根节点状态会作为第三个参数
  - 对于模块内部的 `action`
    - 局部状态 `context.state`
    - 根节点状态`context.rootState`

  ```js
  const moduleA = {
    state: () => ({
      count: 0
    }),
    mutations: {
      increment (state) {
        // 这里的 `state` 对象是模块的局部状态
        state.count++
      }
    },
    getters: {
      sumWithRootCount (state, getters, rootState) {
        return state.count + rootState.count
      }
    },
    actions: {
      incrementIfOddOnRootSum ({ state, commit, rootState }) {
        if ((state.count + rootState.count) % 2 === 1) {
          commit('increment')
        }
      }
    }
  }
  ```

-  命名空间

  - 默认情况下，模块内部的 `action`、`mutation` 和 `getter` 注册在**全局命名空间**
  - 添加 `namespaced: true` 使其成为带命名空间的模块

  ```js
  const store = new Vuex.Store({
    modules: {
      account: {
        namespaced: true,
  
        // 模块内容（module assets）
        state: () => ({}), // 模块内的状态已经是嵌套的了，使用 `namespaced` 属性不会对其产生影响
        getters: {
          isAdmin() { } // -> getters['account/isAdmin']
        },
        actions: {
          login() { } // -> dispatch('account/login')
        },
        mutations: {
          login() { } // -> commit('account/login')
        },
  
        // 嵌套模块
        modules: {
          // 继承父模块的命名空间
          myPage: {
            state: () => ({}),
            getters: {
              profile() { } // -> getters['account/profile']
            }
          },
  
          // 进一步嵌套命名空间
          posts: {
            namespaced: true,
  
            state: () => ({}),
            getters: {
              popular() { } // -> getters['account/posts/popular']
            }
          }
        }
      }
    }
  })
  ```

  - 在带命名空间的模块内访问全局内容

    - `rootState` 和 `rootGetters` 会作为第三和第四参数传入 `getter`
    - 使用全局的`action`或`mutation`，将 `{ root: true }` 作为第三参数

    ```js
    getters: {
      // 在这个模块的 getter 中，`getters` 被局部化了
      // 你可以使用 getter 的第四个参数来调用 `rootGetters`
      someGetter (state, getters, rootState, rootGetters) {
        getters.someOtherGetter // -> 'foo/someOtherGetter'
        rootGetters.someOtherGetter // -> 'someOtherGetter'
      }
    }
    
    actions: {
      // 在这个模块中， dispatch 和 commit 也被局部化了
      // 他们可以接受 `root` 属性以访问根 dispatch 或 commit
      someAction ({ dispatch, commit, getters, rootGetters }) {
        getters.someGetter // -> 'foo/someGetter'
        rootGetters.someGetter // -> 'someGetter'
    
        dispatch('someOtherAction') // -> 'foo/someOtherAction'
        dispatch('someOtherAction', null, { root: true }) // -> 'someOtherAction'
    
        commit('someMutation') // -> 'foo/someMutation'
        commit('someMutation', null, { root: true }) // -> 'someMutation'
      }
    }
    ```

  - 在带命名空间的模块注册全局 action

    - 添加 `root: true`

    ```js
    actions: {
      someAction: {
        root: true,
          handler(namespacedContext, payload) { } // -> 'someAction'
      }
    }
    ```

  - 带命名空间的绑定函数

    - 将模块的空间名称字符串作为第一个参数传递给函数，这样所有绑定都会自动将该模块作为上下文
    - 使用 `createNamespacedHelpers` 创建基于某个命名空间辅助函数

    ```js
    computed: {
      ...mapState('some/nested/module', {
        a: state => state.a,
        b: state => state.b
      })
    }
    
    import { createNamespacedHelpers } from 'vuex'
    const { mapState, mapActions } = createNamespacedHelpers('some/nested/module')
      computed: {
        // 在 `some/nested/module` 中查找
        ...mapState({
          a: state => state.a,
          b: state => state.b
        })
      }
    ```

- 模块动态注册

  - 注册模块`store.registerModule`
  - 卸载模块`store.unregisterModule`
    - 不能使用此方法卸载静态模块（即创建 `store` 时声明的模块）
  - 检查模块`store.hasModule(moduleName)`

  ```js
  import Vuex from 'vuex'
  
  const store = new Vuex.Store({ /* 选项 */ })
  
  // 注册模块 `myModule`
  store.registerModule('myModule', {
    // ...
  })
  // 注册嵌套模块 `nested/myModule`
  store.registerModule(['nested', 'myModule'], {
    // ...
  })
  ```

## 参考

- [Vuex (vuejs.org)](https://vuex.vuejs.org/zh/guide/)


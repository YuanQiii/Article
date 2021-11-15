# Vuex

## 前言

Vuex 是一个专为 Vue.js 应用程序开发的**状态管理模式**。它采用集中式存储管理应用的所有组件的状态，并以相应的规则保证状态以一种可预测的方式发生变化。其实它就是对vue做数据管理的，更好的存储数据、响应数据。

## 原理

![](https://vuex.vuejs.org/vuex.png)

## 核心模块

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





## 模块化

```js
const moduleA = {
  state: () => ({ ... }),
  mutations: { ... },
  actions: { ... },
  getters: { ... }
}

const moduleB = {
  state: () => ({ ... }),
  mutations: { ... },
  actions: { ... }
}

const store = new Vuex.Store({
  modules: {
    a: moduleA,
    b: moduleB
  }
})

store.state.a // -> moduleA 的状态
store.state.b // -> moduleB 的状态
```


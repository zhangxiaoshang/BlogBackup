---
title: 学习使用Vuex
categories: note
date: 2018-02-11 11:16:01
updated: 2018-02-11 11:16:01
tags: Vue
toc: true
---

## 1、如何创建一个vuex实例？

```js
import Vue from 'vue'
import Vuex from 'vuex'
Vue.use(Vuex)

const store =  new Vuex.Store({
  state: {
    count: 0
  },
  mutations: {
    increment(state) {
      state.count++
    }
  }
})

export { store as default } 
```

这样就创建了一个简单的Vuex实例`store`，这个实例有两个属性：`state` 和 `mutations`  
`state`: 用于存储各种状态，类似Vue组件中的`data`  
`mutations`: 存储方法，用于修改`state`中的各种状态(变量)，类似于Vue组件中的`methods`

## 2、如何在Vue组件中使用`state` 和 `mutations` ?

首先声明一个组件`Counter.vue`组件
```html
<template>
  <div>
    <div>counted {{ count }}</div>
    <button @click="increment">+</button>
  </div>
</template>
```
这个组件模版依赖一个`count`变量，和一个 `increment` 函数。  
对于`count`的值，会使用前面声明的`Vuex`实例的`state`属性中的`count`的值。  
对于`increment` 函数，会使用前面声明的`Vuex`实例的`mutations`属性中的`increment`函数。  

> Tip: 记得在`main.js`中导入上面声明的`store`, 并从根组件“注入”到每一个子组件中
```js
import store from '@/store'

new Vue({
  el: '#app',
  store,  // 注入store,这样在所有组件都可以使用this.$store获取实例
  components: { App },
  template: '<App/>'
})
```

`Counter.vue`  
```js
export default {
  computed: {
    count() {
      // 想要在组件中使用Vuex实例中的state，只需在computed属性中返回所需的状态即可
      return this.$store.state.count
    }
  },
  methods: {
    increment() {
      // 想要在组件中使用Vuex实例中的mutations，只需调用this.$store.commit()，参数是字符串，对应想要使用的mutations
      this.$store.commit('increment')
    }
  }
}
```

## 3、mapState 辅助函数是什么、有什么用、如何用？

1. `mapState` 是一个辅助函数  
2. 帮我们生成计算属性
3. 用法

```js
computed: mapState({
    // 箭头函数可使代码更简练
    // 第1种用法，传入state作为参数，返回了state.count,这样就获取了store中的count
    count: state => state.count,

    // 传字符串参数 'count' 等同于 `state => state.count`
    // 第2种用法，与第1种用法只是写法不同，现在可以在模版种使用变量countAlias, 值与count相同
    countAlias: 'count',

    // 为了能够使用 `this` 获取局部状态，必须使用常规函数
    // 第3种用法，使用常规函数，这样就可以使用this获取组件的局部状态
    countPlusLocalState (state) {
      return state.count + this.localCount
    }
  })
```


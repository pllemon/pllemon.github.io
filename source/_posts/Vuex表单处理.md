---
title: 涉及 Vuex 的 v-model
toc: true
date: 2019-10-22 13:33:30
tags: [VUE, VUEX]
categories: [VUE]
---

<!-- more -->
# 问题描述
```js
<input v-model="message">

computed: {
  ...mapState({
    message: state => state.message
  })
}
```
如上，v-model 绑定的是 Vuex 里的某个值，在用户输入时，v-model 会试图直接修改 message，而不是通过 mutation 函数去修改，严格模式下会导致错误。

# 解决方法
## 方法一
给 input 中绑定 value，然后侦听 input 或者 change 事件，在事件回调中调用 action
```js
<input :value="message" @input="updateMessage">
```
```js
computed: {
  ...mapState({
    message: state => state.message
  })
},
methods: {
  updateMessage (e) {
    this.$store.commit('updateMessage', e.target.value)
  }
}
```

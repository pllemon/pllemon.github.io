---
title: Vuex小结
toc: true
date: 2019-10-22 13:33:30
tags: [VUE, VUEX]
categories: [JAVASCRIPT]
---

> Vue 有一个很大的特点就是组件化，当开发一个中大型系统的时候，如何保证各个组件的数据的统一
<!-- more -->
# 前言
每个应用仅有一个 store 实例

# 核心概念
## State
> 单一状态树，唯一数据源
### mapState
```js
import { mapState } from 'vuex'

export default {
  computed: {
    ...mapState({
      count: state => state.count
    })
  }
}
```

## Getter

## Mutation

## Action

## Module
* 为了解决单一状态树过大，可以将 store 分割成模块（module）。
* 每个 module 都有自己的 state、mutation、action、getter、module。
* 在中大型系统一版都采用模块分割，并使用命名空间，使模块有更高的封装性和复用性。
---
title: vue + element-ui + vue-i18n 实现多语言切换
date: 2019-10-18 13:33:30
tags: [i18n, 插件]
categories: [VUE]
---

> 项目需求：系统需要实现多语言切换
<!-- more -->
# 安装
```
npm install vue-i18n
```

# 创建相关文件
* 新建文件目录如下
```
├—— App.vue
├—— i18n
│   ├—— i18n.js
│   └—— langs
│       ├—— cn.js
│       ├—— en.js
│       └—— index.js
├—— main.js
```

* i18n.js 基本配置
```js
import Vue from 'vue'
import locale from 'element-ui/lib/locale'
import VueI18n from 'vue-i18n'
import messages from './langs'

Vue.use(VueI18n)
const i18n = new VueI18n({
  locale: localStorage.getItem('systemLang') || 'cn',
  messages
})
locale.i18n((key, value) => i18n.t(key, value))

export default i18n
```

* langs/index.js 导出不同语言包
```js
import en from './en'
import cn from './cn'
export default {
  en,
  cn
}
```

* langs/en.js 定义English语言包
```js
import enLocale from 'element-ui/lib/locale/lang/en'
const en = {
  message: {
    'hello': 'hello, world',
  },
  ...enLocale
}

export default en
```

* langs/cn.js 定义中文语言包
```js
import zhLocale from 'element-ui/lib/locale/lang/zh-CN'
const cn = {
  message: {
    'hello': '你好，世界',
  },
  ...zhLocale
}

export default cn
```

# 引入
* main.js 记住要在vue实例里引入i18n
```js
import i18n from './i18n/i18n'

new Vue({
  store,
  i18n,
  render: h => h(App)
}).$mount('#app')
```

# 修改模板

# 切换语言
```js
methods: {
  switchLang(val)  {
    this.$i18n.locale = val 
    localStorage.setItem('systemLang', val)
  }
}
```
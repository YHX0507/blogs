---
title: Vue3全局注册问题
comments: true
date: 2020-12-12 23:40:46
tags:
- Vue
- Vue3
- Vue3全局注册
---

## vue3 如何注册全局组件

```js
import { createApp } from 'vue'
import App from './App.vue'
const app = createApp(App)

// 注册全局组件
import SaveButton from '@/globalComponents/SaveButton'
app.component('SaveButton', SaveButton)

app.mount('#app')
```

{% img https://yhx0507.github.io/wxapp_static/app/Snipaste_2020-11-27_22-50-21.png %} 

## vue3 如何注册全局自定义指令

```js
import { createApp } from 'vue'
import App from './App.vue'

const app = createApp(App)

// 注册全局自定义指令 `v-focus`
app.directive('focus', {
  inserted: function (el) {
    el.focus()
  },
})

app.mount('#app')
```

{% img https://yhx0507.github.io/wxapp_static/app/Snipaste_2020-11-27_22-50-44.png %} 

## vue3 如何全局混入

```js
import { createApp } from 'vue'
import App from './App.vue'

const app = createApp(App)

// 全局混入
app.mixin({
  beforeCreate() {
    console.log('我是全局mixin')
  },
})
app.mount('#app')
```

{% img https://yhx0507.github.io/wxapp_static/app/Snipaste_2020-11-27_22-51-07.png %} 

## vue3 如何全局挂载全局属性和方法

```js
import { createApp } from 'vue'
import App from './App.vue'
import axios from 'axios'

const app = createApp(App)

// 全局ctx(this) 上挂载 $axios
app.config.globalProperties.$axios = axios

app.mount('#app')
```

{% img https://yhx0507.github.io/wxapp_static/app/Snipaste_2020-11-27_22-51-35.png %} 

## vue3 如何注册插件

```js
import { createApp } from 'vue'
import App from './App.vue'

const app = createApp(App)

// 注册插件
class plugin {
  static install(_vue) {
    // 混入
    _vue.mixin({
      beforeCreate() {
        console.log('我是plugin')
      },
    })
  }
}
app.use(plugin)

app.mount('#app')
```

{% img https://yhx0507.github.io/wxapp_static/app/Snipaste_2020-11-27_22-52-06.png %} 

## vue3 全局注册过滤器 filter

哈哈貌似 vue 3 放弃了过滤器，暂时无法注册

## vue3 setup 函数里如何拿到 vue-router 的$router，$route

下面两种方法都是可行的（通过`getCurrentInstance`或者`useRoute` ,`useRouter` ）。

```js
import { getCurrentInstance, unref } from 'vue'
import { useRoute, useRouter } from 'vue-router'
export default {


export default {
  setup() {
    //  拿到当前组件 this(ctx)
    const { ctx } = getCurrentInstance()

    // 拿到  $router
    // const $router = useRouter()
    const $router = ctx.$router

    // 拿到  $route
    //  const $route = useRoute()
    const $route = unref(ctx.$router.currentRoute)

  }
}
```

{% img https://yhx0507.github.io/wxapp_static/app/Snipaste_2020-11-27_22-52-58.png %} 

## vue3 setup 函数里如何拿到 vuex 的$store

下面两种方法都是可行的（通过`getCurrentInstance`或者`useStore` ）。

```js
import { getCurrentInstance } from 'vue'
import { useStore } from 'vuex'

export default {
  setup() {
    //  拿到当前组件 this(ctx)
    const { ctx } = getCurrentInstance()
    // const $store = useStore()
     console.log(ctx.$store)
  },
}
```

{% img https://yhx0507.github.io/wxapp_static/app/Snipaste_2020-11-27_22-53-28.png %} 

## vue3 keep-alive组件缓存

 当用于App.vue时，所有的路由对应的页面为项目所对应的组件，使用方法如下： 

{% img https://yhx0507.github.io/wxapp_static/app/Snipaste_2020-12-12_21-01-25.png %} 

在keep-alive组件上使用`include`或`exclude`属性

如下： 使用`include`代表将缓存name为testKA的组件, 在router对应的页面中，需要设置name属性 

{% img https://yhx0507.github.io/wxapp_static/app/Snipaste_2020-12-12_21-04-50.png %} 

 此外，include用法还有如下： 

{% img https://yhx0507.github.io/wxapp_static/app/Snipaste_2020-12-12_21-05-45.png %} 

 `exclude`用法与`include`用法相同，代表不被缓存的组件。此外，keep-alive还有一个`max`属性，代表缓存组件最大数量，一旦这个数字达到了，在新实例被创建之前，已缓存组件中最久没有被访问的实例会被销毁掉 

{% img https://yhx0507.github.io/wxapp_static/app/Snipaste_2020-12-12_21-07-11.png %}

 注意：**keepalive**缓存组件时，不能跨级使用，比如在`App.vue`中使用`include`属性进行name="a"匹配时，只能匹配缓存name为a的子组件（路由页面），而不能缓存name为"a"的孙子组件（子页面引的组件）。若想缓存孙子组件，可以将整个子组件缓存，或者在子组件里再使用keepalive  


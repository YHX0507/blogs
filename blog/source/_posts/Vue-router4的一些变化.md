---
title: Vue-router4的一些变化
comments: true
date: 2020-12-12 19:54:58
tags:
- Vue
- Vue-router
- router
---

# vue-router 4.0

## createRouter 创建路由

### Vue Router不再是类，而是一组功能

- `new Router()`现在您无需调用即可调用`createRouter`，
- history替换新选项mode
- 移动了base选项，base现在作为第一个参数传递给`createWebHistory`
- `routes` 该选项是必需的 `options`

{% img https://yhx0507.github.io/wxapp_static/app/Snipaste_2020-12-12_20-36-51.png %}

```js
import { createRouter, createWebHistory } from 'vue-router'

const router = createRouter({
  history: createWebHistory(),
  routes: [],
})
```

### history属性

- `"history"`： `createWebHistory()`
- `"hash"`： `createWebHashHistory()`
- `"abstract"`： `createMemoryHistory()`

### 删除*（加注星标或捕获全部）路由

{% img https://yhx0507.github.io/wxapp_static/app/image-20201209094302843.png %}

```js
const routes = [
  { path: '/:pathMatch(.*)', name: 'bad-not-found', component: NotFound },
]
```

### scrollBehavior

 该`scrollBehavior`函数接收`to`和`from`路由对象。第三个参数，`savedPosition`仅在通过`popstate`导航（由浏览器的后退/前进按钮触发）时才可用。 

{% img https://yhx0507.github.io/wxapp_static/app/Snipaste_2020-12-12_20-38-05.png %}

由以前的`x`，`y`改为现在的`left`，`top`

{% img https://yhx0507.github.io/wxapp_static/app/Snipaste_2020-12-12_20-39-42.png %}

## 合成API

### addRoute

将新的路由记录添加到路由器。如果路线中有个name并且已经存在一个相同的路径，则会先将其删除，这里的添加路由记录有两种添加方式

| 方式           | 参数类型          | 备注                        |
| -------------- | ----------------- | --------------------------- |
| **routes**     | `RouteRecordRaw`  | **路线记录添加**            |
| **parentName** | `string`,`symbol` | **附加在父路由记录**`route` |

{% img https://yhx0507.github.io/wxapp_static/app/image-20201209110809029.png %}

- 在父路由下添加路由

  {% img https://yhx0507.github.io/wxapp_static/app/image-20201209095648742.png %}

  注意：这里第一个参数parentName就是父路由记录options里的name

- 直接添加一条路由记录

  {% img https://yhx0507.github.io/wxapp_static/app/image-20201209095507374.png %}

  {% img https://yhx0507.github.io/wxapp_static/app/image-20201209111034131.png %}

### 组件内部使用的两个路由钩子

- onBeforeRouteLeave 路由离开钩子

- onBeforeRouteLeave 路由离开钩子

  参数：

  ​	to 目标路由信息
  ​	from 当前路由信息
  ​	next 跳转函数

  注意：导航守卫可以是返回值而不是next

### 组件内部使用当前路由信息和获取路由参数

{% img https://yhx0507.github.io/wxapp_static/app/image-20201209101139573.png %}

```js
import { useRoute, useRouter, useLink } from 'vue-router'
export default {
  setup() {
    const route = useRoute()
    const router = useRouter()
    const link = useLink()
  }
}
```

**useRoute 返回当前路由**

该route对象是反应性对象，因此可以监视其任何属性，并且应避免监视整个route对象

- path
- name
- params
- query
- hash
- fullpath
- matched
- meta
- redirectedFrom

**useRouter 返回路由实例**

- currentRoute 返回当前路由 , 非ref
- addRoute 动态添加路由
- removeRoute 动态删除路由
- hasRoute 检查路由是否存在
- getRoutes 获取路由记录的数组, 替换原 3.0 routes 属性
- push 路由跳转
- replace 路由重定向
- resolve 解析目标路由
- beforeEach 全局路由守卫,  路由跳转前
- afterEach 全局路由守卫, 路由跳转后台
- onError 报错监听
- isReady 路由是否初始话， 返回Promise,  替代原3.0 onReady
- history 路由执行器
- install vue插件安装器

**useLink 自定义路由跳转函数, 接受一个路由配置，并返回路由信息及执行回调**

- route 路由对象
- href 目标地址
- isActive 是否被激活
- isExactActive
- navigate 跳转回调

---
title: Vue3新特性
comments: true
date: 2020-11-27 21:39:25
tags:
- Vue
- Vue3
- Vue3新特性
---

# setup 函数

 `setup()` 函数是 vue3 中，专门为组件提供的新属性。它为我们使用 vue3 的 `Composition API` 新特性提供了统一的入口 

##  执行时机

setup 函数会在 beforeCreate 之后、created 之前执行

 注意：由于在执行 setup 时尚未创建组件实例，因此在 setup 选项中没有 this。这意味着，除了 props 之外，你将无法访问组件中声明的任何属性——本地状态、计算属性或方法。 

##  形参

**第一个形参 props 用来接收 `props` 数据**

- 在 props 中定义当前组件允许外界传递过来的参数名称
- 通过 setup 函数的第一个形参，接收 props 数据

{% img https://yhx0507.github.io/wxapp_static/app/Snipaste_2020-11-27_21-57-30.png %} 

第二个形参 `context` 用来定义上下文** 

-  这个上下文对象中包含了一些有用的属性，这些属性在 vue 2.x 中需要通过 this 才能访问到，在 vue 3.x 中，它们的访问方式如下 

# reactive 函数

 `reactive()` 函数接收一个普通对象，返回一个响应式的数据对象 

{% img https://yhx0507.github.io/wxapp_static/app/Snipaste_2020-11-27_22-00-59.png %}

# ref 函数

  `ref()` 函数用来根据给定的值创建一个响应式的数据对象，`ref()` 函数调用的返回值是一个对象，这个对象上只包含一个 `value` 属性 

{% img https://yhx0507.github.io/wxapp_static/app/Snipaste_2020-11-27_22-02-36.png %} 

 注意：只在`setup`函数内部访问`ref`函数需要加`.value` 

{% img https://yhx0507.github.io/wxapp_static/app/Snipaste_2020-11-27_22-07-27.png %} 

{% img https://yhx0507.github.io/wxapp_static/app/Snipaste_2020-11-27_22-07-52.png %} 

# `ref()`函数与`reactive()`函数的区别

- reactive()函数一次创建的是一个对象 如 const state = reactive({ number:1,age:11 }) 一次性可以创建很多值
- ref() 函数一次性只能创建一个值 如： const number = ref(0)

# isRef

`isRef()` 用来判断某个值是否为 ref() 创建出来的对象

应用场景：当需要展开某个可能为 ref() 创建出来的值的时候

{% img https://yhx0507.github.io/wxapp_static/app/Snipaste_2020-11-27_22-08-51.png %} 

# toRefs

当setup()函数一次性要返回数据、方法等布置一个数据的时候，这时候使用`reactive()`函数创建的缺陷就体现出来了，一个方法只能有一个return语句，又没有办法写在一起return， 所以只能这时候只能将`reactive()`创建的响应式对象拆开，但是拆开这些属性就缺失了响应式的功能，所以这时候就需要`toRefs()`方法将单个数据转成响应式对象，这个也就是为什么`ref()`使用的优先级比较高了。

当有多个数据需要访问的时候 就可以使用 toRefs() 函数 将数据转换成单个的响应式数据 再统一的返回

`toRefs()` 函数可以将 `reactive()` 创建出来的响应式对象，转换为普通的对象，只不过，这个对象上的每个属性节点，都是 `ref()` 类型的响应式数据，最常见的应用场景如下

{% img https://yhx0507.github.io/wxapp_static/app/Snipaste_2020-11-27_22-09-48.png %} 

# computed

## 创建只读的计算属性

{% img https://yhx0507.github.io/wxapp_static/app/Snipaste_2020-11-27_22-11-27.png %} 

## 创建可读可写的计算属性

{% img https://yhx0507.github.io/wxapp_static/app/Snipaste_2020-11-27_22-12-21.png %} 

# watch

Vue3中的 `watch` 的功能 还是我们理解的那个watch的功能 和 Vue2x 是一样的 只是功能变得更加的强大

- watch的第一个参数存放的是监听变量的值
  如果是ref创建的直接输出创建的对象就行 如果是reactive创建的对象需要使用箭头函数导
  当一次监听多个变量的时候 第一个对象可以以数组的形式写入
- watch第二个参数是一个函数有三个形参，分别是 新值、 旧值 和 清除函数
  如果是传递多个参数的时候 新值和旧值是可以通过解构赋值的方式分别导出的
  清除函数可以 清楚掉这个监听器的上次未完成的异步任务
- 第三个参数 {lazy:true/false} 分别表示的是页面一刷新会不会就执行一次watch监听

## 基本用法

{% img https://yhx0507.github.io/wxapp_static/app/Snipaste_2020-11-27_22-14-54.png %} 

## 监视指定的数据源

- 监视 reactive 类型的数据源 

{% img https://yhx0507.github.io/wxapp_static/app/Snipaste_2020-11-27_22-15-19.png %} 

-  监视 ref 类型的数据源： 

{% img https://yhx0507.github.io/wxapp_static/app/Snipaste_2020-11-27_22-15-45.png %} 

## 监视多个数据源

{% img https://yhx0507.github.io/wxapp_static/app/Snipaste_2020-11-27_22-16-11.png %} 

## 清除监视

在 setup() 函数内创建的 watch 监视，会在当前组件被销毁的时候自动停止。如果想要明确地停止某个监视，可以调用 watch() 函数的返回值即可，语法如下 

{% img https://yhx0507.github.io/wxapp_static/app/Snipaste_2020-11-27_22-17-02.png %} 

# LifeCycle Hooks

 新版的`生命周期函数`，可以按需导入到组件中，且`只能在 setup() 函数`中使用，代码示例如下 

{% img https://yhx0507.github.io/wxapp_static/app/Snipaste_2020-11-27_22-18-49.png %} 

 vue 2.x 的生命周期函数与新版 `Composition API` 之间的映射关系 

{% img https://yhx0507.github.io/wxapp_static/app/Snipaste_2020-11-27_22-19-29.png %} 

# provide()、inject()

使用 `provide` `inject` 可以实现跨越父子之间的传值
比如父组件要给孙组件传值， 就可以在父组件用 `provide(键,值)` 的方式声明一个值
它的后代组件就可以使用 `inject(键) `的方式来获取这个共享的值
使用了这个新特性之后 我们就可以不用一层一层的 props 组件传值

{% img https://yhx0507.github.io/wxapp_static/app/Snipaste_2020-11-27_22-30-47.png %} 

{% img https://yhx0507.github.io/wxapp_static/app/Snipaste_2020-11-27_22-31-13.png %} 

# Ref引用页面DOM元素和组件

ref除了可以创建响应式的数据意外，还可以对页面的DOM元素和组件进行引用，这点和Vue2是一样，

Vue2也是有ref，Vue3只需要保证html标签结点的ref值和我们自己创建的ref是同一个值，即可绑定引用

-  绑定引用 

{% img https://yhx0507.github.io/wxapp_static/app/Snipaste_2020-11-27_22-32-42.png %} 

-  绑定组件 

{% img https://yhx0507.github.io/wxapp_static/app/Snipaste_2020-11-27_22-35-17.png %} 

# Vue 3.0 v-model用法

{% img https://yhx0507.github.io/wxapp_static/app/Snipaste_2020-11-27_22-39-40.png %} 

{% img https://yhx0507.github.io/wxapp_static/app/Snipaste_2020-11-27_22-40-02.png %} 

# 总结

Vue3的一些比较会用的到的特性都已经总结完毕了，需要注意的点如下：

- `setup()`函数很重要，操作基本都在这里面完成
- 定义响应式变量的时候尽量都使用`ref`来定义，在`setup()`里面要使用用ref的值需要`.value`才能访问到，在html部门可以直接访问。
- `watch()`事件监听现在更加强大，能够一次性的监听多个对象，（需要使用解构赋值）、和清楚上次监听的一些异步任务
- 生命周期函数函数全部都写在`setup`内部，并且周期的名字都加了on,如`onMounted()`
- `provide`,`inject`真的很好用，很方便的组件传值
- Ref引用DOM，和引用组件的细节就是要保证ref的值和我们用ref创建的值要保持一致。


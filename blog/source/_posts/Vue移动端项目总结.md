---
title: Vue移动端项目总结
comments: true
date: 2018-03-01 11:43:25
tags: 
- Vue
- 移动项目
- 总结
---
# Vue移动项目总结
## 项目初始化
### 使用Vue-cli创建项目
    1、安装脚手架
        npm install @vue/cli
    2、创建项目
        vue create 项目名称
    3、配置项目
        对项目初始化进行一些配置
        Vue CLI v4.1.2
        ? Please pick a preset:
        default (babel, eslint)
        > Manually select features      
        选择第2种：手动选择特性，支持更多自定义选项
        分别选择：
        Babel：es6 转 es5
        Router：路由
        Vuex：数据容器，存储共享数据
        CSS Pre-processors：CSS 预处理器，后面会提示你选择less、sass、stylus等Linter/Formatter：代码格式校验
    4、进入项目目录   
        cd 项目名称
    5、启动服务       
        npm run serve(dev还是serve看配置)
### 初始目录结构说明
    ├── .browserslistrc
    ├── .editorconfig
    ├── .eslintrc.js
    ├── .gitignore
    ├── README.md
    ├── babel.config.js
    ├── package-lock.json
    ├── package.json
    ├── public
    │   ├── favicon.ico
    │   └── index.html
    └── src
        ├── App.vue       根组件
        ├── assets        资源目录
        ├── components    公共组件
        ├── main.js       入口模块
        ├── router        路由
        ├── store         Vuex容器
        └── views         路由组件
### 代码的管理Git
    正常的话我们需要创建 Git 仓库并提交历史记录。
    ```sh
    git init
    git add 文件
    git commit"提交日志"
    ```
    但是Vue-cli 在生成项目的时候默认完成了Git仓库的初始化和初始提交，所以这里只需要push到线上即可。
    ```sh
    git remote add 你的远程仓库地址
    git push -u origin master
    ```
    之后如果需要提交，则还是常规的add、commit、push。
    ```sh
    git add 文件
    git commit -m "提交日志"
    git push
    ```
### 组件库导入（Vant组件库）
    这里建议为了前期开发的便利性先一次性导入所有 Vant 组件，在最后做打包优化的时候配置按需加载以降低打包体积大小
    1、安装 Vant
    ```js
    npm i vant -S
    ```
    2、在main.js中加载注册Vant组件（注意要引入Vant样式文件）
    ```js
    // 全局引入（体积包比较大）
    import Vant from 'vant'
    import 'vant/lib/index.css' 
    Vue.use(Vant)
    // 引入vant组件,这里我们动态加载（推荐）
    import './utils/register-vant'
    ```
    3、按需加载组件
       在utils中新建Vant组件加载文件vant-register.js
    ```js
    import Vue from 'vue'
    // 引入组件
    import {
      Button, Cell, CellGroup
    } from 'vant'
    // 全剧注册组件
    Vue.use(Button).use(Cell).use(CellGroup)
    ```
### 样式处理
    1、样式初始化组件包
        Normalize.css只是一个很小的CSS文件，但它在默认的HTML元素样式上提供了跨浏览器的高度一致性。相比于传统的CSS reset，Normalize.css是一种现代的、为HTML5准备的优质替代方案Normalize.css现在已经被用于Twitter Bootstrap以及许许多多其他框架、工具和网站上
        1、安装
        ```sh
        npm i normalize.css
        ```
        2、在main.js中引入
        ```js
        import 'normalize.css'
        ```
        但是我们的项目不需要加载它,不是不需要，因为我们使用了第三方组件库Vant,它内置了normalize.css，所以我们不需要自己手动安装配置它了。
    2、配置Rem适配
    Vant中的样式默认使用`px`作为单位，如果需要使用rem单位推荐使用两个工具
        postcss-pxtorem:用于将单位转化为rem
        amfe-flexible:用于设置rem基准值
        使用amfe-flexible动态设置 REM 基准值（html 标签的字体大小）
        1、安装
        ```sh
        npm i amfe-flexible
        ```
        2、然后在 `main.js` 中加载执行该模块
        ```js
        import 'amfe-flexible'
        ```

        使用postcss-pxtorem将 px 转为 rem
        1、安装
        ```sh
        npm install postcss-pxtorem -D
        ```
        2、然后在**项目根目录**中创建`postcss.config.js`文件
        ```js
        module.exports = {
          plugins: {
            "postcss-pxtorem": {
              // 设计稿 375:37.5
              // 设计稿：750:75
              // Vant 是基于 375
              rootValue: 37.5,
              propList: ["*"]
            }
          }
        }
        ```
        3、**配置完毕，重新启动服务**
### 项目中axios配置
    1、安装axios
    ```sh
    npm i axios
    ```
    2、创建src/utils/request.js
    ```js
    import axios from "axios"
    // axios.create 方法：复制一个axios,可以直接配置基础路径
    const request = axios.create({
      baseURL: "请求基础路径"
    })
    // 最后导出我们复制的request
    export default request
    ```
    3、如何使用
    把每一个请求都封装成一个的独立功能函数，在独立的功能函数中导出请求方法，在需要的时候加载调用即可
    ```js
    import request from '@/utils/request.js'
    export const getData = () => {
      return request({
        url: '',
        method: 'GET'
      })
    }
    ```
    4、axios中的请求、响应拦截问题(包含处理token问题)
    ```js
    // 设置请求拦截
    request.interceptors.request.use(config => {
      const user = store.state.user
      if (user) {
        config.headers.Authorization = `Bearer ${user.token}`
      }
      return config
    }, error => {
      return Promise.reject(error)
    })
    // 设置响应拦截
    request.interceptors.response.use(function (response) {
      return response
    }, async function (error) {
      // 判断是否是401
      if (error.response && error.response.status === 401) {
        const user = store.state.user
        // 判断是否有refresh_token
        if (!user || !user.refresh_token) {
          // 没有refresh_token直接跳转登录
          reLogin()
          return
        }
        try {
          // 有refresh_token重新获取token
          const { data } = await axios({
            url: 'http://ttapi.research.itcast.cn/app/v1_0/authorizations',
            method: 'PUT',
            headers: {
              Authorization: `Bearer ${user.refresh_token}`
            }
          })
          // 重新赋值token
          store.commit('setUser', {
            ...user,
            token: data.data.token
          })
          // 继续之前的请求
          return request(error.config)
        } catch (err) {
          console.log('请求刷新失败', err)
          reLogin()
        }
      }
      return Promise.reject(error)
    })
    function reLogin () {
      router.push({
        name: 'Login',
        query: {
          redirect: router.currentRoute.fullPath
        }
      })
    }
    ```
## 项目中路由表的配置
    1、配置路由表
    ```js
    import Vue from 'vue'
    import VueRouter from 'vue-router'
    // 在全局注册路由
    Vue.use(VueRouter)
    // 实例化路由对象
    const router = new VueRouter({
      routes
    })
    // 配置路由规则
    const routes = [
      {
        path: '/login',
        name: 'Login',
        component: () => import('@/views/login')
      }
    ]
    // 导出路由
    export default router
    ```
    2、在main.js中引入路由并注册路由
    ```js
    import router from './router'
    new Vue({
      router,
      store,
      render: h => h(App)
    }).$mount('#app')
    ```
## 表单验证问题（vee-validate插件）https://logaretm.github.io/vee-validate/
    1、安装
    ```sh
    npm i vee-validate
    ```
    ```js
    2、创建 `utils/validation.js`
    import Vue from 'vue'
    // 加载需要使用的验证组件
    import { ValidationProvider, ValidationObserver, extend } from 'vee-validate'
    // 加载内置的验证规则
    import * as rules from 'vee-validate/dist/rules'
    // 加载中文语言包
    import { messages } from 'vee-validate/dist/locale/zh_CN.json'
    // 注册全局组件
    Vue.component('ValidationProvider', ValidationProvider)
    Vue.component('ValidationObserver', ValidationObserver)
    // 配置验证规则和中文提示
    Object.keys(rules).forEach(rule => {
      extend(rule, {
        ...rules[rule],
        message: messages[rule]
      })
    })
    ```
    3、在main.js加载执行
    ```js
    import './utils/validation.js'
    ```
    4、基本使用（使用内置验证规则或自定义验证规则）
        使用ValidationObserver把需要校验的整个表单包起来
        使用ValidationProvider把需要校验的具体表单元素包起来，例如 input
        通过ValidationProvider配置具体的校验规则
        - name配置验证字段的名称
        - rules验证规则
          - rules="requried"单个验证规则
          - rules="required|length:4"多个验证规则使用|分隔
        - v-slot="{ errors }"获取错误消息，使用errors[0]绑定展示错误消息
        ```js
        // 添加自定义验证规则
        extend('positive', value => {
          return value >= 0;
        });
        // 使用验证规则
        import { validate } from 'vee-validate'
        // 参数1：要验证的数据 参数2：验证规则
        // 参数3：一个可选的配置对象，例如配置错误消息字段名称 name
        // 返回值：{ valid, errors, ... }
        //          valid: 验证是否成功，成功 true，失败 false
        //          errors：一个数组，错误提示消息
        validate(mobile, 'required|mobile', {
          name: '手机号'
        })
        ```
    5、手动触发验证规则（通过this.$refs.myform.errors获取错误信息）
        给ValidationObserver组件添加一个ref属性
        调用组件的validate方法
        ```js
        const success = await this.$refs.form.validate()
        ```


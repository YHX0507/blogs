---
title: Vue移动端项目流程
comments: true
date: 2019-12-18 23:50:43
tags:
- Vue
- Vue2
---



# 一、项目初始化

## 使用 Vue CLI 创建项目

> 提示：该项目基于 VueCLI 版本 4 创建，如果你的版本低于 4，请升级继续下面的内容。
>
> ```sh
> # 查看 VueCLI 版本号
> vue --version
> 
> # npm 安装升级
> npm install @vue/cli
> 
> # Yarn 安装升级
> yarn global add @vue/cli
> ```

在命令行中输入以下命令创建 Vue 项目：

```sh
vue create vue-toutiao-webapp
```

```
Vue CLI v4.1.2
? Please pick a preset:
  default (babel, eslint)
> Manually select features
```

> default：默认勾选 babel、eslint，回车之后直接进入装包
>
> manually：自定义勾选特性配置，选择完毕之后，才会进入装包
>
> 选择第 2 种：手动选择特性，支持更多自定义选项

```
? Please pick a preset: Manually select features
? Check the features needed for your project:
 (*) Babel
 ( ) TypeScript
 ( ) Progressive Web App (PWA) Support
 (*) Router
 (*) Vuex
 (*) CSS Pre-processors
>(*) Linter / Formatter
 ( ) Unit Testing
 ( ) E2E Testing
```

> 分别选择：
>
> Babel：es6 转 es5
>
> Router：路由
>
> Vuex：数据容器，存储共享数据
>
> CSS Pre-processors：CSS 预处理器，后面会提示你选择 less、sass、stylus 等
>
> Linter / Formatter：代码格式校验

```
? Use history mode for router? (Requires proper server setup for index fallback in production) (Y/n) n
```

> 是否使用 history 路由模式，这里输入 n 不使用

```
? Pick a CSS pre-processor (PostCSS, Autoprefixer and CSS Modules are supported by default):
  Sass/SCSS (with dart-sass)
  Sass/SCSS (with node-sass)
> Less
  Stylus
```

> 选择 CSS 预处理器，这里选择我们熟悉的 Less

```sh
? Pick a linter / formatter config:
  ESLint with error prevention only
  ESLint + Airbnb config
> ESLint + Standard config
  ESLint + Prettier
```

> 选择校验工具，这里选择 ESLint + [Standard config](https://standardjs.com/)

```
? Pick additional lint features:
 (*) Lint on save
>(*) Lint and fix on commit
```

> 选择在什么时机下触发代码格式校验：
>
> - Lint on save：每当保存文件的时候
> - Lint and fix on commit：每当执行 `git commit` 提交的时候
>
> 这里建议两个都选上，更严谨。

```sh
? Where do you prefer placing config for Babel, ESLint, etc.? (Use arrow keys)
> In dedicated config files
  In package.json
```

> Babel、ESLint 等工具会有一些额外的配置文件，这里的意思是问你将这些工具相关的配置文件写到哪里：
>
> - In dedicated config files：分别保存到单独的配置文件
> - In package.json：保存到 package.json 文件中
>
> 这里建议选择第 1 个，保存到单独的配置文件，这样方便我们做自定义配置。

```sh
? Save this as a preset for future projects? (y/N) N
```

> 这里是问你是否需要将刚才选择的一系列配置保存起来，然后它可以帮你记住上面的一系列选择，以便下次直接重用。
>
> 这里根据自己需要输入 y 或者 n，我这里输入 n 不需要。

```sh
✨  Creating project in C:\Users\LPZ\Desktop\topline-m-fe89\topline-m-89.
�  Initializing git repository...
⚙  Installing CLI plugins. This might take a while...

[          ........] - extract:object-keys: sill extract json5@2.1.1
```

> 向导配置结束，开始装包。
>
> 安装包的时间可能较长，请耐心等待......

```sh
⚓  Running completion hooks...

�  Generating README.md...

�  Successfully created project topline-m-89.
�  Get started with the following commands:

 $ cd topline-m
 $ npm run serve
```

> 安装结束，命令提示你项目创建成功，按照命令行的提示在终端中分别输入：
>
> - `cd 你的项目`
> - `npm run serve`
>   - 如果你是 yarn 装的包就执行 `yarn serve`

```sh
 DONE  Compiled successfully in 7527ms


  App running at:
  - Local:   http://localhost:8080/
  - Network: http://192.168.10.216:8080/

  Note that the development build is not optimized.
  To create a production build, run npm run build.

```

> 启动成功，命令行中输出项目的 http 访问地址。
>
> 打开浏览器，输入其中任何一个地址进行访问。

{% img https://yhx0507.github.io/wxapp_static/app/image-20200105001849854.png %}

如果能看到该页面，恭喜你，项目创建成功了。

## 初始目录结构说明

项目创建好以后，下面我们来了解一下目录结构的含义：

```
.
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
```

> 关于 `lock` 文件
>
> - 如果你的项目使用的是 npm，则这里产生的是 `package-lock.json` 文件
> - 如果你的项目使用的 yarn，则这里生产的是 `yarn.lock` 文件

## 加入 Git 版本管理

几个好处：

- 代码备份
- 多人协作
- 历史记录

1、创建远程仓库（github、码云、coding。。。）

2、将本地仓库推到线上

正常的话我们需要创建 Git 仓库并提交历史记录。

```sh
git init
git add 文件
git commit "提交日志"
```

但是 Vue CLI 在生成项目的时候默认完成了 Git 仓库的初始化和初始提交，所以这里只需要 push 到线上即可。

```sh
git remote add 你的远程仓库地址
git push -u origin master
```

之后如果需要提交，则还是常规的 add、commit、push。

```sh
git add 文件
git commit -m "提交日志"
git push
```

## 调整初始目录结构

默认生成的目录结构不满足我们的开发需求，所以这里需要做一些自定义改动。

这里主要就是下面的两个工作：

- 删除初始化的默认文件
- 新增调整我们需要的目录结构

1、将 `App.vue` 修改为

```html
<template>
  <div id="app">
    <!-- 根路由出口 -->
    <router-view />
  </div>
</template>

<script>
export default {
  name: 'App'
}
</script>

<style scoped lang="less"></style>

```

2、将 `router/index.js` 修改为

```js
import Vue from 'vue'
import VueRouter from 'vue-router'

Vue.use(VueRouter)

const routes = []

const router = new VueRouter({
  routes
})

export default router

```

3、删除

- src/views/About.vue
- src/views/Home.vue
- src/components/HelloWorld.vue
- src/assets/logo.png

4、创建以下几个目录

- src/api
  - 存储接口封装
- src/utils
  - 存储一些工具模块
- src/styles
  - 存储样式

调整之后的目录结构如下。

```
.                                 
├── README.md                     
├── babel.config.js               
├── package-lock.json             
├── package.json                  
├── public                        
│   ├── favicon.ico               
│   └── index.html                
└── src                           
    ├── api
    ├── App.vue                   
    ├── assets                    
    ├── components                
    ├── main.js                   
    ├── router
    ├── utils
    ├── styles
    ├── store                     
    └── views                     
```

## 导入 Vant

### Vant 介绍

{% img https://yhx0507.github.io/wxapp_static/app/logo.png %}

- [官方文档](https://youzan.github.io/vant/#/zh-CN/)
- [GitHub 仓库](https://github.com/youzan/vant)

Vant 是杭州有赞商城前端开发团队开发的一个基于 Vue.js 的移动端组件库，它提供了丰富的常见的移动端功能组件，开箱即用。

- 60+ 高质量组件
- 90% 单元测试覆盖率
- 完善的中英文文档和示例
- 支持按需引入
- 支持主题定制
- 支持国际化
- 支持 TS
- 支持 SSR

### 导入

方式一. 自动按需引入组件

- 和方式二一样，都是按需引入，但是加载更方便一些（需要额外配置插件）
- 优点：打包体积小
- 缺点：每个组件在使用之前都需要手动加载注册

方式二. 手动按需引入组件

- 在不使用插件的情况下，可以手动引入需要的组件
- 优点：打包体积小
- 缺点：每个组件在使用之前都需要手动加载注册

方式三. 导入所有组件

  - Vant 支持一次性导入所有组件，引入所有组件会增加代码包体积，因此不推荐这种做法
  - 优点：导入一次，使用所有
  - 缺点：打包体积大

方式四. 通过 CDN 引入

- 使用 Vant 最简单的方法是直接在 html 文件中引入 CDN 链接，之后你可以通过全局变量`vant`访问到所有组件。
- 优点：适合一些演示、示例项目，一个 html 文件就可以跑起来
- 缺点：不适合在模块化系统中使用

这里建议为了前期开发的便利性先一次性导入所有 Vant 组件，在最后做打包优化的时候配置按需加载以降低打包体积大小。

下面是具体的操作步骤。

1、安装 Vant

```sh
npm i vant -S
```

2、在 `main.js` 中加载注册 Vant 组件

```js
import Vue from 'vue'
import Vant from 'vant'
import 'vant/lib/index.css'

Vue.use(Vant)
```

3、查阅文档使用组件

## 样式

### 预处理器文件结构

在 styles 中创建以下文件结构：

```
styles/
  ├── base.less       公共基础样式（全局样式）
  ├── index.less      组织加载全局样式
  ├── mixins.less     公共的 mixin（哪里使用哪里加载）
  ├── reset.less      重置默认样式（全局样式）
  └── variables.less  公共变量文件（哪里使用哪里加载）
```

然后在 `main.js` 中加载全局样式使其生效：

```js
// 加载全局样式
// 注意：该样式文件要放到第三方样式之后
import './styles/index.less'
```

### normalize.css

​	[Normalize.css](http://necolas.github.io/normalize.css/) 只是一个很小的CSS文件，但它在默认的HTML元素样式上提供了跨浏览器的高度一致性。相比于传统的`CSS reset`，`Normalize.css`是一种现代的、为HTML5准备的优质替代方案。`Normalize.css`现在已经被用于[Twitter Bootstrap](http://getbootstrap.com/)、[HTML5 Boilerplate](http://html5boilerplate.com/)、[GOV.UK](http://www.gov.uk/)、[Rdio](http://www.rdio.com/)、[CSS Tricks](http://css-tricks.com/) 以及许许多多其他框架、工具和网站上。

- **保护有用的浏览器默认样式**而不是完全去掉它们
- **一般化的样式**：为大部分HTML元素提供
- **修复浏览器自身的bug**并保证各浏览器的一致性
- **优化CSS可用性**：用一些小技巧
- **解释代码**：用注释和详细的文档来

`Normalize.css`支持包括手机浏览器在内的超多浏览器，同时对HTML5元素、排版、列表、嵌入的内容、表单和表格都进行了一般化。尽管这个项目基于一般化的原则，但我们还是在合适的地方使用了更实用的默认值。

1、安装

```sh
npm i normalize.css
```

2、在 `main.js` 中引入

```js
import 'normalize.css'
```

但是我们的项目不需要加载它，不是不需要，因为我们使用了第三方组件库 Vant，它内置了 normalize.css，所以我们不需要自己手动安装配置它了。

### 关于字体图标

Vant 内置了很多丰富的[字体图标](https://youzan.github.io/vant/#/zh-CN/icon)，如果其中没有满足我们需要的，推荐使用 [iconfont](https://www.iconfont.cn/)。

### 配置 REM 适配

Vant 中的样式默认使用 `px` 作为单位，如果需要使用 `rem` 单位，推荐使用以下两个工具：

- [postcss-pxtorem](https://github.com/cuth/postcss-pxtorem) 是一款 postcss 插件，用于将单位转化为 rem
- [lib-flexible](https://github.com/amfe/lib-flexible) 用于设置 rem 基准值

下面我们分别将这两个工具配置到项目中完成 REM 适配。

一、使用 [amfe-flexible](https://github.com/amfe/lib-flexible) 动态设置 REM 基准值（html 标签的字体大小）

1、安装

```sh
# yarn add amfe-flexible
npm i amfe-flexible
```

2、然后在 `main.js` 中加载执行该模块

```js
...
import 'amfe-flexible'
```

最后测试：在浏览器中切换不同的手机设备尺寸，观察 html 标签 `font-size` 的变化。

二、使用 [postcss-pxtorem](https://github.com/cuth/postcss-pxtorem) 将 px 转为 rem

1、安装

```sh
# yarn add -D postcss-pxtorem
# -D 是 --save-dev 的简写
npm install postcss-pxtorem -D
```

2、然后在**项目根目录**中创建 `postcss.config.js` 文件

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

最后测试：在浏览器中审查元素的样式查看是否已将 px 转换为 rem。

> 注意：
>
> - **只能转换单独的 .css|.less|.scss 之类的文件、.vue 文件中的 style 中的 px**
> - **不能转换行内样式中的 px**

### 关于设计稿的使用

Vant 的组件是基于 375 宽写的，而我们的设计稿是 750 宽，这样的话转换规则就出现了冲突。

解决办法就是把我们应该将就 Vant，将转换规则中的 `rootValue` 设置为 `37.5`。

之后在测量我们的设计稿的时候，就需要通过下面的方式来处理使用。

方式一：

- 如果你的设计稿 375，写的时候就是量多少写多少
- 如果你的设计稿 750，写的时候就是测量尺寸 ÷ 2

方式二（扩展）：

- 在 Photoshop 中把设计稿图像尺寸修改为 375，测量多少写多少
- 有个缺点：它也会把图片转为 375 下的，会导致模糊
- 如果你真的想要这样做，那就是 750 把图片切出来，然后在 375 下量尺寸（不用/2）

## 封装请求模块

这里我们直接把 axios 封装为一个请求模块，在需要的时候直接加载使用。

### axios

1、安装 axios

```sh
# yarn add axios
npm i axios
```

2、创建 `src/utils/request.js`

```js
/**
 * 封装 axios 请求模块
 */
import axios from "axios"

// axios.create 方法：复制一个 axios
const request = axios.create({
  baseURL: "http://ttapi.research.itcast.cn/" // 基础路径
})

export default request
```

3、如何使用

- 方式一（麻烦）：哪里使用，哪里 `import `加载
- 方式二（不利于接口维护）：我们可以把请求对象挂载到 `Vue.prototype` 原型对象中，然后在组件中通过 `this.xxx` 直接访问
- 方式三（推荐）：我们把每一个请求都封装成一个一个的独立功能函数，在需要的时候加载调用即可

在我们的项目中建议使用方式三，更推荐。

### 处理后端返回数据中包含超出 JS 安全整数范围数字问题

JavaScript 中可以处理的最大安全整数范围是

```js
Number.MAX_SAFE_INTEGER // 9007199254740991
```

```js
JSON.parse('{ "id": 9007199254740995 }') // { id: 9007199254740996 }
```

该项目所使用的后端接口数据中包含超出 JavaScript 安全整数范围的数字，所以也需要像之前的 PC 端项目一样使用 [json-bigint](https://github.com/sidorares/json-bigint) 将后端返回数据处理一下才能正确使用。

1、安装依赖

```sh
# yarn add json-bigint
npm i json-bigint
```

2、在 `utils/request.js`

```js
import jsonBig from 'json-bigint'
```

```js
// axios 开放了自定义转换后端返回数据的 API
// data 就是后端返回的原始数据
request.defaults.transformResponse = [function (data) {
  try {
    // 现在我们定制使用 json-bigint 来帮我们处理转换原始的 JSON 格式字符串
    // 这个方法类似于 JSON.parse，只不过它能把数据中的超出 JS 安全整数范围的数字给处理成正确的
    // 它内部有自己的算法，它会把大数字转为一个对象，我们在使用的时候把对象.toString() 就得到字符串形式的 id 了
    // 如果转换成功则返回成功的结果给请求使用
    // 如果转换失败则进入 catch，返回一个空对象
    return jsonBig.parse(data)

    // 它默认是这样的
    // return JSON.parse(data)
  } catch (err) {
    console.log('转换失败', err)
    return {}
  }
}]
```

# 二、用户登录注册

> 目标
>
> - 能实现基本登录功能
> - 能了解 vant 中提示组件的使用
> - 能理解 api 请求模块的封装
> - 能了解在 Vue 中处理表单验证的方式

{% img https://yhx0507.github.io/wxapp_static/app/1566431560209.png %}

流程：

- 创建登录组件并配置路由
- 布局
- 完成登录功能

## 准备

### 创建登录组件并配置路由

{% img https://yhx0507.github.io/wxapp_static/app/1570784873574.png %}

1、创建 `views/login/index.vue` 并写入以下内容：

```html
<template>
  <div class="login">
    登录组件
  </div>
</template>

<script>
  export default {
    name: "LoginPage",
    components: {},
    props: {},
    data() {
      return {};
    },
    computed: {},
    watch: {},
    created() {},
    methods: {}
  };
</script>

<style scoped></style>
```

2、然后在 `router/index.js` 中配置路由表：

```js
import Vue from 'vue'
import VueRouter from 'vue-router'
+ import Login from '@/views/login'

Vue.use(VueRouter)

// 配置路由表
const routes = [
+  {
+    path: '/login',
+    component: Login
+  }
]

const router = new VueRouter({
  routes
})

export default router

```

最后，访问 `/login` 查看是否能访问到登录页面组件。

### 布局结构

{% img https://yhx0507.github.io/wxapp_static/app/1570785466138.png %}

这里主要使用到三个 Vant 组件：

- [NavBar 导航栏](https://youzan.github.io/vant/#/zh-CN/nav-bar)
- [Field 输入框](https://youzan.github.io/vant/#/zh-CN/field)
- [Button 按钮](https://youzan.github.io/vant/#/zh-CN/button)

1、登录页面的模板

```html
<template>
  <div class="login">
    <!-- 导航栏 -->
    <van-nav-bar title="登录" />
    <!-- 导航栏 -->

    <!-- 表单 -->
    <van-cell-group>
      <van-field label="手机号" placeholder="请输入手机号" />

      <van-field label="验证码" placeholder="请输入验证码" />
    </van-cell-group>
    <!-- /表单 -->

    <!-- 登录按钮 -->
    <div class="login-btn-box">
      <van-button type="info">登录</van-button>
    </div>
    <!-- /登录按钮 -->
  </div>
</template>
```

### 布局样式

{% img https://yhx0507.github.io/wxapp_static/app/1570786015561.png %}

我们把设置登录页头部的样式写到全局（全局生效），因为其它页面也要使用。把非公共样式写到页面组件内部，避免和其它组件样式冲突。

下面是具体实现步骤。

一、添加全局样式设置导航栏

1、在 `styles/base.less` 并写入以下内容

```less
// 把全局公共样式写到这里

.van-nav-bar {
  background-color: #3196fa;
  .van-nav-bar__title {
    color: #fff;
  }
}
```

完了测试查看效果。

二、添加局部样式处理登录按钮居中

在 `views/login/index.vue` 组件中的 style 中添加

```less
.login-container {
  .login-btn-container {
    padding: 20px;
    .login-btn {
      width: 100%;
    }
  }
}
```

## 实现基本登录功能

{% img https://yhx0507.github.io/wxapp_static/app/login.gif %}

实现思路：

- 获取表单数据（根据接口要求使用 v-model 绑定）
- 注册点击登录的事件
- 表单验证
- 发请求提交
- 根据请求结果做下一步处理

### 数据绑定

**根据接口要求**绑定获取表单数据。

1、在登录页面组件的实例选项 data 中添加 `user` 数据字段

```js
...
data () {
  return {
    user: {
      mobile: '',
      code: ''
    }
  }
}
```

2、在表单中使用 `v-model` 绑定对应数据

```html
<!-- van-cell-group 仅仅是提供了一个上下外边框，能看到包裹的区域 -->
<van-cell-group>
  <van-field
    +
    v-model="user.mobile"
    required
    clearable
    label="手机号"
    placeholder="请输入手机号"
  />

  <van-field
    +
    v-model="user.code"
    type="password"
    label="验证码"
    placeholder="请输入验证码"
    required
  />
</van-cell-group>
```

最后测试。

> 一个小技巧：使用 VueDevtools 调试工具查看是否绑定成功。

### 请求登录

1、给登录按钮注册点击事件处理函数

```html
<van-button type="info" @click="onLogin">登录</van-button>
```

2、登录处理函数

```js
import request from '@/utils/request'

```

```js
async onLogin () {
  try {
    const res = await request({
      method: 'POST',
      url: '/app/v1_0/authorizations',
      data: this.user
    })
    console.log('登录成功', res)
  } catch (err) {
    console.log('登录失败', err)
  }
}

```

## 登录中提示

{% img https://yhx0507.github.io/wxapp_static/app/loging.gif %}

Vant 中内置了[Toast 轻提示](https://youzan.github.io/vant/#/zh-CN/toast)组件，可以实现移动端常见的提示效果。

```js
// 简单文字提示
Toast("提示内容");

// loading 转圈圈提示
Toast.loading({
  duration: 0, // 持续展示 toast
  message: "加载中...",
  forbidClick: true // 是否禁止背景点击
});

// 成功提示
Ttoast.success("成功文案");

// 失败提示
Toast.fail("失败文案");

```

> 提示：在组件中可以直接通过 `this.$toast` 调用。

另外需要注意的是：Toast 默认采用单例模式，即同一时间只会存在一个 Toast，如果需要在同一时间弹出多个 Toast，可以参考下面的示例

```js
Toast.allowMultiple();

const toast1 = Toast('第一个 Toast');
const toast2 = Toast.success('第二个 Toast');

toast1.clear();
toast2.clear();

```

下面是为我们的登录功能增加 toast 交互提示。

```js
async onLogin () {
  // 开始转圈圈
  this.$toast.loading({
    duration: 0, // 持续时间，0表示持续展示不停止
    forbidClick: true, // 是否禁止背景点击
    message: '登录中...' // 提示消息
  })

  try {
    const res = await request({
      method: 'POST',
      url: '/app/v1_0/authorizations',
      data: this.user
    })
    console.log('登录成功', res)
    // 提示 success 或者 fail 的时候，会先把其它的 toast 先清除
    this.$toast.success('登录成功')
  } catch (err) {
    console.log('登录失败', err)
    this.$toast.fail('登录失败，手机号或验证码错误')
  }
}

```

假如请求非常快的话就看不到 loading 效果了，这里可以手动将调试工具中的网络设置为慢速网络。

{% img https://yhx0507.github.io/wxapp_static/app/image-20200108104554588.png %}

测试结束，再把网络设置恢复为 `Online` 正常网络。

## 优化封装请求模块

我们建议将所有请求都封装为函数的方式来进行使用，这样做的主要目的是为了便于重用和管理维护。

这里我们先把登录中的请求封装到请求模块中。

1、创建 `src/api/user.js`

```js
/**
 * 用户相关的请求模块
 */
import request from "@/utils/request"

/**
 * 用户登录
 */
export const login = data => {
  return request({
    method: 'POST',
    url: '/app/v1_0/authorizations',
    data
  })
}

```

2、然后在登录页面中加载调用

```js
import { login } from "@/api/user";
```

```js
async onLogin () {
  // const loginToast = this.$toast.loading({
  this.$toast.loading({
    duration: 0, // 持续时间，0表示持续展示不停止
    forbidClick: true, // 是否禁止背景点击
    message: '登录中...' // 提示消息
  })

  try {
+    const res = await login(this.user)
    console.log('登录成功', res)
    // 提示 success 或者 fail 的时候，会先把其它的 toast 先清除
    this.$toast.success('登录成功')
  } catch (err) {
    console.log('登录失败', err)
    this.$toast.fail('登录失败，手机号或验证码错误')
  }
}

```

3、测试功能是否正常

**之后项目中所有的请求就都不要在组件中去直接发了，而是都采用上面的方式封装之后进行使用，这是一个建议的做法。**

## 发送验证码

### 使用倒计时组件

1、在 data 中添加数据用来控制倒计时的显示和隐藏

```js
data () {
  return {
    ...
    isCountDownShow: false
  }
}

```

2、使用倒计时组件

```html
<van-field
  v-model="user.code"
  placeholder="请输入验证码"
>
  <i class="icon icon-mima" slot="left-icon"></i>
  <van-count-down
    v-if="isCountDownShow"
    slot="button"
    :time="1000 * 5"
    format="ss s"
    @finish="isCountDownShow = false"
  />
  <van-button
    v-else
    slot="button"
    size="small"
    type="primary"
    round
    @click="onSendSmsCode"
  >发送验证码</van-button>
</van-field>

```

### 发送验证码

1、在 `api/user.js` 中添加封装数据接口

```js
export const getSmsCode = mobile => {
  return request({
    method: 'GET',
    url: `/app/v1_0/sms/codes/${mobile}`
  })
}

```

2、给发送验证码按钮注册点击事件

3、发送处理

```js
async onSendSmsCode () {
  // 1. 获取手机号
  const { mobile } = this.user
  // 2. 校验手机号是否有效

  // 3. 发送验证码
  try {
    // 显示倒计时
    this.isCountDownShow = true

    // 发送
    await getSmsCode(mobile)
  } catch (err) {
    console.log(err)

    // 发送失败，关闭倒计时
    this.isCountDownShow = false

    if (err.response.status === 429) {
      this.$toast('请勿频繁发送')
      return
    }

    this.$toast('发送失败')
  }
}

```



## 表单验证

### 表单验证方式介绍

常见的表单验证方式一般有下面几种：

- 使用 HTML5 自带的表单验证
- 使用第三方组件库
- 自己写

下面针对这些方式一一说明。

**方式一：HTML5 自带的表单验证（了解即可）**

```html
<form>
  <div>
    <label for="">用户名</label>
    <input type="text" required minlength="3" maxlength="5">
  </div>
  
  <div>
    <label for="">密码</label>
    <input type="password" required>
  </div>
  
  <div>
    <button>登录</button>
  </div>
</form>

```

优点：原生支持，使用简单

缺点：兼容不好，功能有限

**方式二：自己写**

- 例如之前学习的自定义表单验证
- 例如 Vue 官方 Cookbook 中的[表单校验](https://cn.vuejs.org/v2/cookbook/form-validation.html)

优点：控制灵活

缺点：手写麻烦，效率低

**方式三：使用组件库内置的验证**

- 例如 element 内置的验证功能
- 但是 Vant 没有提供

优点：和组件库配套使用，功能强大

缺点：依赖 Vue 生态，特定组件库

**方式四：使用专门的验证插件**

- [vuelidate](https://github.com/monterail/vuelidate)
- [VeeValidate](https://github.com/baianat/vee-validate)（推荐）
- ...

优点：功能强大

缺点：依赖 Vue 生态，特定组件库

我们项目中主要讲解使用 VeeValidate 作为我们的表单验证解决方案。

### 安装和配置

1、安装

```sh
# yarn add vee-validate
npm i vee-validate

```

2、创建 `utils/validation.js`

```js
import Vue from 'vue'

// 加载需要使用的验证组件
import { ValidationProvider, ValidationObserver, extend } from 'vee-validate'

// 加载内置的验证规则
import * as rules from 'vee-validate/dist/rules'

// 加载中文语言包
// 官方文档：https://logaretm.github.io/vee-validate/guide/rules.html#importing-the-rules
import { messages } from 'vee-validate/dist/locale/zh_CN.json'

// 注册全局组件
Vue.component('ValidationProvider', ValidationProvider)
Vue.component('ValidationObserver', ValidationObserver)

// 配置验证规则和中文提示消息
Object.keys(rules).forEach(rule => {
  extend(rule, {
    ...rules[rule],
    message: messages[rule]
  })
})


```

3、在 `main.js` 中加载执行

```js
import './utils/validation.js'
```

### 基本使用

1、使用 `ValidationObserver` 把需要校验的整个表单包起来

2、使用 `ValidationProvider` 把需要校验的具体表单元素包起来，例如 input

3、通过 `ValidationProvider` 配置具体的校验规则

- `name` 配置验证字段的名称
- `rules` 验证规则
  - `rules="requried"` 单个验证规则
  - `rules="required|length:4"` 多个验证规则使用 | 分隔

- `v-slot="{ errors }"` 获取错误消息，使用 `errors[0]` 绑定展示错误消息

下面是一个基本使用示例。

```html
<ValidationObserver ref="form">
  <ValidationProvider name="手机号" rules="required">
    <van-field
      class="form-item"
      v-model="user.mobile"
      clearable
      placeholder="请输入手机号"
    >
      <i class="icon icon-shouji" slot="left-icon"></i>
    </van-field>
    <!-- errors[0] 获取验证失败的错误消息 -->
    <span>{{ errors[0] }}</span>
  </ValidationProvider>

  <ValidationProvider name="验证码" rules="required">
    <van-field
      class="form-item"
      v-model="user.code"
      placeholder="请输入验证码"
    >
      <i class="icon icon-mima" slot="left-icon"></i>
      <van-count-down
        v-if="isCountDownShow"
        slot="button"
        :time="1000 * 60"
        format="ss s"
        @finish="isCountDownShow = false"
      />
      <van-button
        v-else
        slot="button"
        size="small"
        type="primary"
        round
        @click="onSendSmsCode"
      >发送验证码</van-button>
    </van-field>
  </ValidationProvider>
</ValidationObserver>

```

### 使用内置验证规则

例如：

- `required`：必填项
- `email`：必须是邮箱
- `length`：长度
- `min`：最小长度
- `max`：最大长度
- `min_value`：最小数字
- `max_value`：最大数字
- ...

所有的内置验证规则都在[这里查询](https://logaretm.github.io/vee-validate/guide/rules.html#rules)。

### 使用自定义验证规则

```js
import { extend } from 'vee-validate';

extend('positive', value => {
  return value >= 0;
});
```

```html
<ValidationProvider rules="positive" v-slot="{ errors }">
  <input v-model="value" type="text">
  <span>{{ errors[0] }}</span>
</ValidationProvider>
```

添加自定义手机号验证规则。

```js
extend('mobile', {
  validate: value => {
    return /^1(3|5|6|7|8|9)\d{9}$/.test(value)
  },
  message: '手机号码格式错误'
})
```

添加自定义验证码验证规则。

```js
extend('code', {
  validate: value => {
    return /^\d{6}$/.test(value)
  },
  message: '验证码格式错误'
})
```

自定义异步验证示例：

```js
// 验证方法必须返回一个 Promise
// 1. 在 Promise 中执行异步操作（定时器、ajax请求）
// 2. 成功 resolve(true)
// 3. 失败 reject('错误提示消息')
extend('async-test', value => {
  return new Promise(resolve => {
    setTimeout(() => {
      return Math.random() > 0.5 ? resolve(true) : reject('错误消息')
    }, 500)
  })
})
```

### 手动触发验证

一、手动触发验证

1、给 `ValidationObserver` 组件添加一个 `ref`

```html
<ValidationObserver ref="form">
```

2、调用组件的 `validate` 方法

```js
this.$refs.form.validate().then(success => {
  if (!success) {
    // 表单验证失败处理
  } else {
    // 表单验证通过处理
  }
})
```

这里我们可以看到表单验证的方法支持 Promise，所以我们可以结合 async-await 获取表单验证结果。

```js
const success = await this.$refs.form.validate()

if (!success) {
  // 表单验证失败处理
  
  // 使用 return 阻止代码往后执行，不用把后续代码往 else 中嵌套了
  return
}

// 表单验证成功处理

```

二、获取验证失败的错误消息并给出轻提示

通过 ValidationObserver 组件的 `errors` 可以获取到报错信息。

{% img https://yhx0507.github.io/wxapp_static/app/image-20200109093642365.png %}

> 通过调试工具也可以看到这个错误信息对象。

```js
const errors = this.$refs.form.errors
for (let key in errors) {
  const item = errors[key]
  if (item[0]) {
    this.$toast(item[0])

    // 找到第1个有错误的消息，给出提示，停止遍历
    return
  }
}
```

### 验证指定数据字段

```js
import { validate } from 'vee-validate'
```

```js
// 参数1：要验证的数据
// 参数2：验证规则
// 参数3：一个可选的配置对象，例如配置错误消息字段名称 name
// 返回值：{ valid, errors, ... }
//          valid: 验证是否成功，成功 true，失败 false
//          errors：一个数组，错误提示消息
const validateResult = await validate(mobile, 'required|mobile', {
  name: '手机号'
})

// 如果验证失败，提示错误消息，停止发送验证码
if (!validateResult.valid) {
  this.$toast(validateResult.errors[0])
  return
}
```

# 三、Token 处理

## Token 介绍

### HTTP 是无状态的

{% img https://yhx0507.github.io/wxapp_static/app/v2-dde997503ed9d450e2f39042d53d4307_hd.png %}

### 基于 Cookie 的状态保持

{% img https://yhx0507.github.io/wxapp_static/app/v2-85622297a93f493c891ffb90b67fd5e0_hd.png %}

{% img https://yhx0507.github.io/wxapp_static/app/v2-1f49734871c5e2da2d264d28ac310a65_hd.png %}

Cookie 在客户端本地，用户可以操作它，所以不适合存储对安全性要求比较高的数据。

### 基于 Session 的状态保持

{% img https://yhx0507.github.io/wxapp_static/app/image-20200108192548559.png %}

Session 不是新东西，它是基于 Cookie 的一个服务端技术，用一个生活中的例子来解释就好比我们逛超市存包。

- 你：客户端
- 超市：服务器
- 存物柜：服务端的 Session Store
  - 存储用户的状态等对安全性比较高的数据
- 你拿的那个凭据：访问服务端 Session 数据的凭证
  - 凭证是服务端给你的
  - 凭证是唯一的

Session 其实就是把对安全性要求比较高的状态数据放到了服务端，把访问数据的钥匙放到了用户的本地（Cookie）。

用户的登录状态肯定是安全性要求比较高的，所以通常会使用 Session 来存储用户的登录状态。

{% img https://yhx0507.github.io/wxapp_static/app/v2-0b02fa4a73a8072eb03cdf78270235e1_hd.png %}

1、用户向服务器发送用户名和密码。

2、服务器验证通过后，在当前对话（session）里面保存相关数据，比如用户角色、登录时间等等。

3、服务器向用户返回一个 session_id，写入用户的 Cookie。

4、用户随后的每一次请求，都会通过 Cookie，将 session_id 传回服务器。

5、服务器收到 session_id，找到前期保存的数据，由此得知用户的身份。

这种模式的问题在于，扩展性（scaling）不好。单机当然没有问题，如果是服务器集群，或者是跨域的服务导向架构，就要求 session 数据共享，每台服务器都能够读取 session。

举例来说，A 网站和 B 网站是同一家公司的关联服务。现在要求，用户只要在其中一个网站登录，再访问另一个网站就会自动登录，请问怎么实现？

一种解决方案是 session 数据持久化，写入数据库或别的持久层。各种服务收到请求后，都向持久层请求数据。这种方案的优点是架构清晰，缺点是工程量比较大。另外，持久层万一挂了，就会单点失败。

另一种方案是服务器索性不保存 session 数据了，所有数据都保存在客户端，每次请求都发回服务器。

### 基于 Token（令牌、凭据、凭证） 的状态保持

基于 Token 的的思路是，服务器认证以后，生成一个加密数据（令牌），发回给用户，数据内容就像下面这样。

```json
{
  "姓名": "张三",
  "角色": "管理员",
  "到期时间": "2018年7月1日0点0分"
}
```

以后，用户与服务端通信的时候，都要发回这个加密数据（令牌）。服务器通过解密获取明文数据就知道了用户身份。

服务器就不保存任何 session 数据了，也就是说，服务器变成无状态了，从而比较容易实现扩展。

基于 `token` 的鉴权机制类似于 HTTP 协议也是无状态的，它不需要在服务端去保留用户的认证信息或者会话信息。这就意味着基于 `token` 认证机制的应用不需要去考虑用户在哪一台服务器登录了，这就为应用的扩展提供了便利。

{% img https://yhx0507.github.io/wxapp_static/app/ezgif-5-ae2b96ba00b5.png %}

1、用户使用用户名密码来请求服务器

2、服务器进行验证用户的信息

3、服务器通过验证发送给用户一个 token

4、客户端存储 token，并在每次请求时附送上这个 token 值

5、服务端验证 token 值，并返回数据

以上就是基于 Token 认证的思路。

### JWT（JSON Web Token）

有了基于 Token 认证的思路了，该如何实现呢？例如

- 如何生成 Token？
- 如何加密？
- 如何解密？
- 如何规定数据格式？

如果大家都各自搞一套，太麻烦。所以社区中制定了一个具体实现：[JSON Web Token](https://jwt.io/)。

- Java
- PHP
- Ruby
- Python
- Node.js
- 。。。。

该方案主要是对 JSON 格式数据进行加解密以及数据格式的规范，方便前后端交互。

{% img https://yhx0507.github.io/wxapp_static/app/image-20200108012948915.png %}

Json web token (JWT), 是为了在网络应用环境间传递声明而执行的一种基于JSON的开放标准（[(RFC 7519](https://link.jianshu.com?t=https://tools.ietf.org/html/rfc7519)).该token被设计为紧凑且安全的，特别适用于分布式站点的单点登录（SSO）场景。JWT的声明一般被用来在身份提供者和服务提供者间传递被认证的用户身份信息，以便于从资源服务器获取资源，也可以增加一些额外的其它业务逻辑所必须的声明信息，该token也可直接被用于认证，也可被加密。

JWT 的一些特点如下：

（1）JWT 默认是不加密，但也是可以加密的。生成原始 Token 以后，可以用密钥再加密一次。

（2）JWT 不加密的情况下，不能将秘密数据写入 JWT。

（3）JWT 不仅可以用于认证，也可以用于交换信息。有效使用 JWT，可以降低服务器查询数据库的次数。

（4）JWT 的最大缺点是，由于服务器不保存 session 状态，因此无法在使用过程中废止某个 token，或者更改 token 的权限。也就是说，一旦 JWT 签发了，在到期之前就会始终有效，除非服务器部署额外的逻辑。

（5）JWT 本身包含了认证信息，一旦泄露，任何人都可以获得该令牌的所有权限。为了减少盗用，JWT 的有效期应该设置得比较短。对于一些比较重要的权限，使用时应该再次对用户进行认证。

（6）为了减少盗用，JWT 不应该使用 HTTP 协议明码传输，要使用 HTTPS 协议传输。

> 扩展阅读
>
> - http://www.ruanyifeng.com/blog/2018/07/json_web_token-tutorial.html

## 存储 Token

{% img https://yhx0507.github.io/wxapp_static/app/image-20200109192157006.png %}

我们项目中哪里需要使用呢？

- 请求需要权限的接口
- 底部导航栏中也需要使用判断登录状态给出不同的提示
  - 已登录：我的
  - 未登录：未登录
- 我的页面也要使用，判断登录状态给出不同的页面展示
- ...

往哪儿存？

- 本地存储（持久化）
  - 获取麻烦
  - 不是响应式
- Vuex 容器（推荐）
  - 获取方便
  - 响应式的

思路：

- 登录成功，将 token 存储到本地存储
- 为了持久化，还需要把 token 放到本地存储

下面是具体实现。

一、使用 Vuex 容器存储 token

1、在 `src/store/index.js` 中

```js
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)

export default new Vuex.Store({
  state: {
    // 登录用户，一个对象，包含 token 信息
+    user: null
  },
  mutations: {
+    setUser (state, data) {
+      state.user = data
+    }
  },
  actions: {
  },
  modules: {
  }
})

```

2、登录成功以后将后端返回的 token 相关数据存储到容器中

```js
async onLogin () {
  // const loginToast = this.$toast.loading({
  this.$toast.loading({
    duration: 0, // 持续时间，0表示持续展示不停止
    forbidClick: true, // 是否禁止背景点击
    message: '登录中...' // 提示消息
  })

  try {
    const res = await login(this.user)

    // res.data.data => { token: 'xxx', refresh_token: 'xxx' }
+    this.$store.commit('setUser', res.data.data)

    // 提示 success 或者 fail 的时候，会先把其它的 toast 先清除
    this.$toast.success('登录成功')
  } catch (err) {
    console.log('登录失败', err)
    this.$toast.fail('登录失败，手机号或验证码错误')
  }

  // 停止 loading，它会把当前页面中所有的 toast 都给清除
  // loginToast.clear()
}
```

完了，在浏览器中点击登录，通过 VueDevTools 查看数据是否能正常的存储到 Vuex 容器中。



二、持久化 token 存储

Vuex 容器中的数据只是为了方便在其他任何地方能获取登录状态数据，但是页面刷新还是会丢失数据状态，所以我们还要把数据进行持久化中以防止页面刷新丢失状态的问题。

前端持久化常见的方式就是：

- 本地存储
- Cookie

这里我们以使用本地存储持久化用户状态为例。

为了方便，这里先封装一个用于操作本地存储的工具模块。

1、创建 `src/utils/storage.js`

```js
/**
 * 封装操作本地存储的工具方法模块
 */

export const getItem = name => {
  const data = window.localStorage.getItem(name)
  try {
    return JSON.parse(data)
  } catch (err) {
    console.log('转换失败', err)
    return data
  }
}

export const setItem = (name, value) => {
  const data = typeof value === 'object'
    ? JSON.stringify(value)
    : value
  window.localStorage.setItem(name, data)
}

export const removeItem = name => {
  window.localStorage.removeItem(name)
}

```

2、然后在容器中使用使用本地存储持久化 token 数据

```js
import Vue from 'vue'
import Vuex from 'vuex'
+ import { setItem, getItem } from '@/utils/storage'

Vue.use(Vuex)

export default new Vuex.Store({
  state: {
    // 登录用户，一个对象，包含 token 信息
+    user: getItem('user')
    // user: null
  },
  mutations: {
    setUser (state, data) {
      state.user = data

      // 为了防止刷新丢失 state 中的 user 状态，我们把它放到本地存储
+      setItem('user', state.user)
    }
  },
  actions: {
  },
  modules: {
  }
})

```

测试：重新登录，查看本地存储是否有 token 数据，刷新浏览器，查看 Vuex 容器是否有 token 数据。

经过以上处理之后，接下来我们项目中所有需要使用 token 的业务，直接找 Vuex 容器拿，不需要关心数据从哪儿来的。

## 解析 Token

有时候我们需要使用到登录用户的相关信息，例如用户 ID，正常的话建议由后端返回，但是如果后端没有提供的话，我们也可以通过解析 JWT token 来获取。

> 提示：token 中一定有表示用户身份的信息，例如 ID。

这里主要使用到一个第三方工具包：[jwt-decode](https://github.com/auth0/jwt-decode)。

1、安装

```bash
npm i jwt-decode
```

2、然后在容器中

```js
import decodeJwt from 'jwt-decode'
```

```js
setUser (state, data) {
  // 解析 JWT 中的数据（需要使用用户ID）
  if (data && data.token) {
    data.id = decodeJwt(data.token).user_id
  }

  state.user = data

  // 为了防止刷新丢失 state 中的 user 状态，我们把它放到本地存储
  setItem('user', state.user)
},
```

之后就可以直接通过 `store.state.user.id` 来访问使用了。

## 发送 Token

很多接口都需要提供 token（用户的登录状态） 才能访问。

方式一：在每次请求的时候手动添加（麻烦）

```js
axios({
  method: "",
  url: "",
  headers: {
    token数据
  }
});

```

方式二：使用请求拦截器统一添加（推荐，更方便）

在 `src/utils/request.js` 中添加拦截器统一设置 token：

```js
import store from '@/store'

```

```js
// 请求拦截器
request.interceptors.request.use(function (config) {
  // config 请求配置对象，我们可以通过修改 config 来实现统一请求数据处理
  const { user } = store.state

  // 统一添加 token
  if (user) {
    // config.headers 获取操作请求头对象
    // Authorization 是后端要求的字段名称
    // 数据值后端要求提供：Bearer token数据
    //    注意：Bearer 后面有个空格
    // 老师，为啥？后端要求的
    config.headers.Authorization = `Bearer ${user.token}`
  }
  return config
}, function (error) {
  // Do something with request error
  return Promise.reject(error)
})

```

## 处理 Token 过期（在功能优化中进行讲解）

- 为什么 token 过期时间这么短？
  - 为了安全
- 过期了怎么办？
  - 通过登录页面重新登录获取 token
  - 使用 refresh_token 重新获取新的 token

{% img https://yhx0507.github.io/wxapp_static/app/1567481874811.png %}

在请求的响应拦截器中统一处理 token 过期：

```js
/**
 * 封装 axios 请求模块
 */
import axios from "axios";
import jsonBig from "json-bigint";
import store from "@/store";
import router from "@/router";

// axios.create 方法：复制一个 axios
const request = axios.create({
  baseURL: "http://ttapi.research.itcast.cn/" // 基础路径
});

/**
 * 配置处理后端返回数据中超出 js 安全整数范围问题
 */
request.defaults.transformResponse = [
  function(data) {
    try {
      return jsonBig.parse(data);
    } catch (err) {
      return {};
    }
  }
];

// 请求拦截器
request.interceptors.request.use(
  function(config) {
    const user = store.state.user;
    if (user) {
      config.headers.Authorization = `Bearer ${user.token}`;
    }
    // Do something before request is sent
    return config;
  },
  function(error) {
    // Do something with request error
    return Promise.reject(error);
  }
);

// 响应拦截器
request.interceptors.response.use(
  // 响应成功进入第1个函数
  // 该函数的参数是响应对象
  function(response) {
    // Any status code that lie within the range of 2xx cause this function to trigger
    // Do something with response data
    return response;
  },
  // 响应失败进入第2个函数，该函数的参数是错误对象
  async function(error) {
    // Any status codes that falls outside the range of 2xx cause this function to trigger
    // Do something with response error
    // 如果响应码是 401 ，则请求获取新的 token

    // 响应拦截器中的 error 就是那个响应的错误对象
    console.dir(error);
    if (error.response && error.response.status === 401) {
      // 校验是否有 refresh_token
      const user = store.state.user;

      if (!user || !user.refresh_token) {
        router.push("/login");

        // 代码不要往后执行了
        return;
      }

      // 如果有refresh_token，则请求获取新的 token
      try {
        const res = await axios({
          method: "PUT",
          url: "http://ttapi.research.itcast.cn/app/v1_0/authorizations",
          headers: {
            Authorization: `Bearer ${user.refresh_token}`
          }
        });

        // 如果获取成功，则把新的 token 更新到容器中
        console.log("刷新 token  成功", res);
        store.commit("setUser", {
          token: res.data.data.token, // 最新获取的可用 token
          refresh_token: user.refresh_token // 还是原来的 refresh_token
        });

        // 把之前失败的用户请求继续发出去
        // config 是一个对象，其中包含本次失败请求相关的那些配置信息，例如 url、method 都有
        // return 把 request 的请求结果继续返回给发请求的具体位置
        return request(error.config);
      } catch (err) {
        // 如果获取失败，直接跳转 登录页
        console.log("请求刷线 token 失败", err);
        router.push("/login");
      }
    }

    return Promise.reject(error);
  }
);

export default request;
```

# 四、个人中心（我的）

{% img https://yhx0507.github.io/wxapp_static/app/1566431827166.png %}

## TabBar

通过分析页面，我们可以看到，首页、问答、视频、我的 都使用的是同一个底部标签栏，我们没必要在每个页面中都写一个，所以为了通用方便，我们可以使用 Vue Router 的嵌套路由来处理。

- 父路由：一个空页面，包含一个 tabbar，中间留子路由出口
- 子路由
  - 首页
  - 问答
  - 视频
  - 我的

### 创建 tabbar 组件并配置路由

{% img https://yhx0507.github.io/wxapp_static/app/image-20200109153050432.png %}

这里主要使用到的 Vant 组件：

- [Tabbar 标签栏](https://youzan.github.io/vant/#/zh-CN/tabbar)

1、创建 `src/views/tabbar/index.vue`

```html
<template>
  <div class="tabbar-container">
    <!-- 子路由出口 -->
    <router-view />
    <!-- /子路由出口 -->

    <!-- tab-bar 标签栏 -->
    <van-tabbar route>
      <van-tabbar-item icon="wap-home-o" to="/">首页</van-tabbar-item>
      <van-tabbar-item icon="comment-o" to="/qa">问答</van-tabbar-item>
      <van-tabbar-item icon="video-o" to="/video">视频</van-tabbar-item>
      <van-tabbar-item
        icon="user-o"
        to="/my"
      >{{ $store.state.user ? '我的' : '未登录' }}</van-tabbar-item>
    </van-tabbar>
    <!-- /tab-bar 标签栏 -->
  </div>
</template>

<script>
export default {
  name: 'TabBar',
  components: {},
  props: {},
  data () {
    return {}
  },
  computed: {},
  watch: {},
  created () {},
  mounted () {},
  methods: {}
}
</script>

<style scoped></style>

```

2、然后将 tab-bar 组件配置到一级路由

```js
{
  path: '/',
  component: () => import('@/views/tab-bar')
}
```

访问 `/` 测试。

### 分别创建首页、问答、视频、我的页面组件

一、分别创建四个主页面

1、首页

```html
<template>
  <div class="home-container">首页</div>
</template>

<script>
export default {
  name: 'HomePage',
  components: {},
  props: {},
  data () {
    return {}
  },
  computed: {},
  watch: {},
  created () {},
  mounted () {},
  methods: {}
}
</script>

<style scoped></style>

```

2、问答

```html
<template>
  <div class="qa-container">问答</div>
</template>

<script>
export default {
  name: 'QaPage',
  components: {},
  props: {},
  data () {
    return {}
  },
  computed: {},
  watch: {},
  created () {},
  mounted () {},
  methods: {}
}
</script>

<style scoped></style>

```



3、视频

```html
<template>
  <div class="video-container">首页</div>
</template>

<script>
export default {
  name: 'VideoPage',
  components: {},
  props: {},
  data () {
    return {}
  },
  computed: {},
  watch: {},
  created () {},
  mounted () {},
  methods: {}
}
</script>

<style scoped></style>

```

4、我的

```html
<template>
  <div class="my-container">首页</div>
</template>

<script>
export default {
  name: 'MyPage',
  components: {},
  props: {},
  data () {
    return {}
  },
  computed: {},
  watch: {},
  created () {},
  mounted () {},
  methods: {}
}
</script>

<style scoped></style>

```

二、将四个主页面配置为 tab-bar 的子路由

```js
{
  path: '/',
  name: 'tab-bar',
  component: () => import('@/views/tab-bar'),
  children: [
    {
      path: '', // 默认子路由
      name: 'home',
      component: () => import('@/views/home')
    },
    {
      path: 'qa',
      name: 'qa',
      component: () => import('@/views/qa')
    },
    {
      path: 'video',
      name: 'video',
      component: () => import('@/views/video')
    },
    {
      path: 'my',
      name: 'my',
      component: () => import('@/views/my')
    }
  ]
}
```



## 我的页面布局

- 注册组件
  - Image
  - Grid
  - GridItem
  - Icon
- 把该页面需要的两个图片放到页面组件目录中
  - banner.png
  - mobile.png

```html
<template>
  <div class="my-container">
    <!-- 已登录：用户信息 -->
    <div class="user-info-wrap">
      <div class="base-info-wrap">
        <div class="avatar-title-wrap">
          <van-image
            class="avatar"
            round
            fit="cover"
            src="https://img.yzcdn.cn/vant/cat.jpeg"
          />
          <div class="title">黑马程序员</div>
        </div>
        <van-button round size="mini">编辑资料</van-button>
      </div>
      <van-grid class="data-info" :border="false">
        <van-grid-item>
          <span class="count">123</span>
          <span class="text">头条</span>
        </van-grid-item>
        <van-grid-item>
          <span class="count">123</span>
          <span class="text">关注</span>
        </van-grid-item>
        <van-grid-item>
          <span class="count">123</span>
          <span class="text">粉丝</span>
        </van-grid-item>
        <van-grid-item>
          <span class="count">123</span>
          <span class="text">获赞</span>
        </van-grid-item>
      </van-grid>
    </div>
    <!-- /已登录：用户信息 -->

    <!-- 未登录 -->
    <div class="not-login">
      <div class="mobile"></div>
      <div class="text">点击登录</div>
    </div>
    <!-- /未登录 -->

    <!-- 其它 -->
    <van-grid clickable :column-num="3">
      <van-grid-item text="我的收藏">
        <van-icon slot="icon" name="star-o" color="#eb5253" />
      </van-grid-item>
      <van-grid-item text="浏览历史">
        <van-icon slot="icon" name="browsing-history-o" color="#ffa023" />
      </van-grid-item>
      <van-grid-item text="作品">
        <van-icon slot="icon" name="edit" color="#678eff" />
      </van-grid-item>
    </van-grid>

    <van-cell-group :border="false">
      <van-cell title="消息通知" is-link />
      <van-cell title="小智同学" is-link />
    </van-cell-group>

    <van-cell-group>
      <van-cell
        style="text-align: center;"
        title="退出登录"
        clickable
      />
    </van-cell-group>
    <!-- /其它 -->
  </div>
</template>

<script>
export default {
  name: 'MyPage',
  components: {},
  props: {},
  data () {
    return {}
  },
  computed: {},
  watch: {},
  created () {},
  mounted () {},
  methods: {}
}
</script>

<style lang="less" scoped>
.my-container {
  .user-info-wrap {
    background: url("./banner.png") no-repeat;
    height: 182px;
    box-sizing: border-box;
    background-size: cover;
    padding: 40px 20px;
    font-size: 15px;
    color: #fff;
    .base-info-wrap {
      display: flex;
      justify-content: space-between;
      align-items: center;
      .avatar-title-wrap {
        display: flex;
        align-items: center;
        .avatar {
          margin-right: 15px;
          width: 66px;
          height: 66px;
          padding: 2px;
          background: #fff;
        }
      }
    }
    .data-info {
      ::v-deep .van-grid-item__content {
        background: none;
      }
    }
  }

  .not-login {
    background: url("./banner.png") no-repeat;
    height: 182px;
    box-sizing: border-box;
    background-size: cover;
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
    .mobile {
      background: url("./mobile.png") no-repeat;
      background-size: cover;
      width: 66px;
      height: 66px;
      margin-bottom: 10px;
    }
    .text {
      font-size: 14px;
      color: #fff;
    }
  }

  > .van-cell-group {
    margin-top: 10px;
  }
}
</style>

```



## 处理已登录和未登录的展示状态

- 未登录，展示登录按钮
- 已登录，展示登录用户信息

```html
<!-- 已登录：用户信息 -->
<div v-if="$store.state.user" class="user-info-wrap">
  ...
</div>
<!-- /已登录：用户信息 -->

<!-- 未登录 -->
<div v-else class="not-login" @click="$router.push('/login')">
  ...
</div>
<!-- /未登录 -->

<!-- 退出 -->
<van-cell-group v-if="$store.state.user">
  ...
</van-cell-group>
<!-- /退出 -->

```



## 展示当前登录用户信息

{% img https://yhx0507.github.io/wxapp_static/app/image-20200109133717775.png %}

步骤：

- 封装接口
- 请求获取数据
- 模板绑定

1、在 `api/user.js` 中添加封装数据接口

```js
/**
 * 获取当前登录用户个人信息
 */
export const getUserInfo = () => {
  return request({
    method: 'GET',
    url: '/app/v1_0/user'
  })
}

```

2、在 `views/my/index.vue` 请求加载数据

```js
+ import { getUserInfo } from '@/api/user'

export default {
  name: 'MyPage',
  components: {},
  props: {},
  data () {
    return {
+      user: {} // 用户信息
    }
  },
  computed: {},
  watch: {},
+++  created () {
    // 初始化的时候，如果用户登录了，我才请求获取当前登录用户的信息
    if (this.$store.state.user) {
      this.loadUser()
    }
  },
  mounted () {},
  methods: {
+++    async loadUser () {
      try {
        const { data } = await getUserInfo()
        this.user = data.data
      } catch (err) {
        console.log(err)
        this.$toast('获取数据失败')
      }
    }
  }
}

```

3、模板绑定

## 用户退出

{% img https://yhx0507.github.io/wxapp_static/app/out-1578559616164.gif %}

1、给退出按钮注册点击事件

2、退出处理

```js
async onLogout () {
  await this.$dialog.confirm({
    title: '退出提示',
    message: '确认退出吗？'
  })

  // 清除登录状态
  this.$store.commit('setUser', null)
}
```

最后测试。

# 五、用户页面

{% img https://yhx0507.github.io/wxapp_static/app/image-20200111104547462.png %}

## 准备

### 创建组件并配置路由

1、创建 `views/user/index.vue`

```html
<template>
  <div class="user-container">用户页面</div>
</template>

<script>
export default {
  name: 'UserPage',
  components: {},
  props: {},
  data () {
    return {}
  },
  computed: {},
  watch: {},
  created () {},
  mounted () {},
  methods: {}
}
</script>

<style scoped></style>

```

2、配置到根路由

```js
{
  path: '/user/:userId',
  name: 'user',
  component: () => import('@/views/user')
}
```

最后访问 `/user/用户ID` 测试。

### 页面布局

```html
<template>
  <div class="user-container">
    <!-- 导航栏 -->
    <van-nav-bar title="黑马头条号" left-arrow />
    <!-- /导航栏 -->

    <!-- 用户信息 -->
    <div class="user-info-container">
      <div class="row1">
        <van-image
          class="col1"
          fit="cover"
          round
          src="https://img.yzcdn.cn/vant/cat.jpeg"
        />
        <div class="col2">
          <div class="row1">
            <div class="item">
              <div class="count">123</div>
              <div class="text">发布</div>
            </div>
            <div class="item">
              <div class="count">123</div>
              <div class="text">关注</div>
            </div>
            <div class="item">
              <div class="count">123</div>
              <div class="text">粉丝</div>
            </div>
            <div class="item">
              <div class="count">123</div>
              <div class="text">获赞</div>
            </div>
          </div>
          <div class="action">
            <van-button
              type="primary"
              size="small"
            >私信</van-button>
            <van-button
              type="default"
              size="small"
            >编辑资料</van-button>
          </div>
        </div>
      </div>
      <div class="intro-wrap">
        <div>
          <span>认证：</span>
          <span>用户的认证信息</span>
        </div>
        <div>
          <span>简介：</span>
          <span>用户的简介信息</span>
        </div>
      </div>
    </div>
    <!-- /用户信息 -->

    <!-- 文章列表 -->
    <!-- /文章列表 -->
  </div>
</template>

<script>
export default {
  name: 'UserPage',
  components: {},
  props: {},
  data () {
    return {}
  },
  computed: {},
  watch: {},
  created () {},
  mounted () {},
  methods: {}
}
</script>

<style scoped lang="less">
.user-container {
  font-size: 14px;
  .user-info-container {
    padding: 12px;
    background-color: #fff;
    margin-bottom: 10px;
    >.row1 {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 10px;
      .item {
        text-align: center;
        .text {
          font-size: 12px;
        }
      }
      >.col1 {
        width: 80px;
        height: 80px;
      }
      >.col2 {
        display: flex;
        flex-direction: column;
        justify-content: space-evenly;
        width: 70%;
        height: 80px;
        padding: 0 12px;
        >.row1 {
          display: flex;
          justify-content: space-between;
        }
        .action {
          display: flex;
          justify-content: space-between;
          .van-button {
            width: 45%;
          }
        }
      }
    }
  }
}
</style>

```

## 展示用户信息

步骤：

- 封装数据接口
- 请求获取数据
- 模板绑定

1、在 `api/user.js` 中添加获取指定用户信息的数据接口

```js
// 获取指定用户信息
export const getUserById = userId => {
  return request({
    method: 'GET',
    url: `/app/v1_0/users/${userId}`
  })
}
```

2、在用户页面中请求获取数据

```js
+ import { getUserById } from '@/api/user'

export default {
  name: 'UserPage',
  components: {},
  props: {},
  data () {
    return {
+      user: {} // 用户信息
    }
  },
  computed: {},
  watch: {},
  created () {
+    this.loadUser()
  },
  mounted () {},
  methods: {
+++    async loadUser () {
      try {
        const { data } = await getUserById(this.$route.params.userId)
        this.user = data.data
      } catch (err) {
        console.log(err)
        this.$toast('获取用户数据失败')
      }
    }
  }
}
```

3、模板绑定

## 展示用户文章列表

### 列表组件

```js
<van-list
  v-model="loading"
  :finished="finished"
  finished-text="没有更多了"
  @load="onLoad"
>
  <van-cell
    v-for="item in list"
    :key="item"
    :title="item"
  />
</van-list>
export default {
  data() {
    return {
      list: [],
      loading: false,
      finished: false
    };
  },

  methods: {
    onLoad() {
      // 异步更新数据
      setTimeout(() => {
        for (let i = 0; i < 10; i++) {
          this.list.push(this.list.length + 1);
        }
        // 加载状态结束
        this.loading = false;

        // 数据全部加载完成
        if (this.list.length >= 40) {
          this.finished = true;
        }
      }, 500);
    }
  }
}
```



### 分析列表组件使用

1、List 的运行机制是什么？

List 会监听浏览器的滚动事件并计算列表的位置，当列表底部与可视区域的距离小于offset时，List 会触发一次 load 事件。

2、为什么 List 初始化后会立即触发 load 事件？

List 初始化后会触发一次 load 事件，用于加载第一屏的数据，这个特性可以通过`immediate-check`属性关闭。

3、为什么会连续触发 load 事件？

如果一次请求加载的数据条数较少，导致列表内容无法铺满当前屏幕，List 会继续触发 load 事件，直到内容铺满屏幕或数据全部加载完成。因此你需要调整每次获取的数据条数，理想情况下每次请求获取的数据条数应能够填满一屏高度。

loading 和 finished 分别是什么含义？

> `List`有以下三种状态，理解这些状态有助于你正确地使用`List`组件：
>
> - 非加载中，`loading`为`false`，此时会根据列表滚动位置判断是否触发`load`事件（列表内容不足一屏幕时，会直接触发）
> - 加载中，`loading`为`true`，表示正在发送异步请求，此时不会触发`load`事件
> - 加载完成，`finished`为`true`，此时不会触发`load`事件
>
> 在每次请求完毕后，需要手动将`loading`设置为`false`，表示本次 load 加载结束

4、使用 float 布局后一直触发加载？

若 List 的内容使用了 float 布局，可以在容器上添加van-clearfix类名来清除浮动，使得 List 能正确判断元素位置

### 展示文章列表

1、封装获取用户文章列表的数据接口

```js
/**
 * 获取指定用户的文章列表
 */
export const getArticlesByUser = (userId, params) => {
  return request({
    method: 'GET',
    url: `/app/v1_0/users/${userId}/articles`,
    params
  })
}

```

2、在用户页面中请求获取数据

```js
import { getUserById } from '@/api/user'
+ import { getArticlesByUser } from '@/api/article'

export default {
  name: 'UserPage',
  components: {},
  props: {},
  data () {
    return {
      user: {}, // 用户信息
      list: [], // 列表数据
      loading: false, // 控制上拉加载更多的 loading
      finished: false, // 控制是否加载结束了
+      page: 1 // 获取下一页数据的页码
    }
  },
  computed: {},
  watch: {},
  created () {
    this.loadUser()
  },
  mounted () {},
  methods: {
    async loadUser () {
      try {
        const { data } = await getUserById(this.$route.params.userId)
        this.user = data.data
      } catch (err) {
        console.log(err)
        this.$toast('获取用户数据失败')
      }
    },

+++    async onLoad () {
      // 1. 请求获取数据
      const { data } = await getArticlesByUser(this.$route.params.userId, {
        page: this.page, // 可选的，默认是第 1 页
        per_page: 20 // 可选的，默认每页 10 条
      })

      // 2. 把数据添加到列表中
      // list []
      // data.data.results []
      // ...[1, 2, 3] 会把数组给展开，所谓的展开就是一个一个的拿出来
      const { results } = data.data
      this.list.push(...results)

      // 3. 加载状态结束
      this.loading = false

      // 4. 判断数据是否全部加载完毕
      if (results.length) {
        this.page++ // 更新获取下一页数据的页码
      } else {
        this.finished = true // 没有数据了，不需要加载更多了
      }
    }
  }
}
```



# 六、首页—文章列表

{% img https://yhx0507.github.io/wxapp_static/app/1566539328996.png %}

该模块主要处理两部分：

- 频道列表
- 频道的文章列表

## 布局

```html
<template>
  <div class="home-container">
    <!-- 导航栏 -->
    <div class="nav-bar">
      <div class="logo"></div>
      <van-button
        class="search-btn"
        round
        type="info"
        size="small"
        icon="search"
      >搜索</van-button>
    </div>
    <!-- /导航栏 -->

    <!-- 频道列表 -->
    <van-tabs v-model="active">
      <van-tab title="标签 1">
      	<!-- TODO: 文章列表 -->
      </van-tab>
      <van-tab title="标签 2">内容 2</van-tab>
      <van-tab title="标签 3">内容 3</van-tab>
      <van-tab title="标签 4">内容 4</van-tab>
    </van-tabs>
    <!-- /频道列表 -->
  </div>
</template>

<script>
export default {
  name: 'HomePage',
  components: {},
  props: {},
  data () {
    return {
      active: 0 // 控制标签页的激活项
    }
  },
  computed: {},
  watch: {},
  created () {},
  mounted () {},
  methods: {}
}
</script>

<style scoped lang="less">
.home-container {
  .nav-bar {
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 0 15px;
    height: 46px;
    background-color: #3196fa;
    z-index: 1;
    .logo {
      background: url("./logo-light.png") no-repeat;
      background-size: cover;
      width: 100px;
      height: 30px;
    }
    .search-btn {
      background-color: #5babfb;
      width: 50%;
    }
  }
}
</style>

```



## 展示频道列表

思路：

- 封装请求方法
- 请求获取数据
- 模板绑定

1、在 `src/api/channel.js` 中新增一个请求方法

```js
/**
 * 获取用户频道列表
 * 如果登录了：获取用户频道列表
 * 没有登录：获取默认推荐的频道列表
 */
export const getUserChannels = () => {
  return request({
    method: 'GET',
    url: '/app/v1_0/user/channels'
  })
}

```

2、在首页中请求获取数据

```js
+ import { getUserChannels } from '@/api/channel'

export default {
  name: 'HomePage',
  components: {},
  props: {},
  data () {
    return {
      active: 0,
+      userChannels: [] // 用户频道列表
    }
  },
  computed: {},
  watch: {},
  created () {
+    this.loadUserChannels()
  },
  mounted () {},
  methods: {
+++    async loadUserChannels () {
      const { data } = await getUserChannels()
      this.userChannels = data.data.channels
    }
  }
}

```

> 提示：代码写完以后在浏览器的调试工具中查看是否能够加载获取到 channels 数据，确保能正常获取到数据以后，再进行模板绑定。

3、拿到数据，渲染模板

```html
<!-- 频道标签列表 -->
<van-tabs v-model="active">
  <van-tab
    :title="channel.name"
    :key="channel.id"
    v-for="channel in channels"
  >
    <div>{{ channel.name }} 频道内容</div>
  </van-tab>
</van-tabs>
<!-- /频道标签列表 -->

```

## 文章列表

我们想要的功能是：

- 当查看某个频道的时候才初始加载
- 加载过的不要重新加载

就像手机版今日头条中的列表一样。

{% img https://yhx0507.github.io/wxapp_static/app/image-20200111124415227.png %}

> 你的思路

{% img https://yhx0507.github.io/wxapp_static/app/image-20200111124530323.png %}

> 多个列表

{% img https://yhx0507.github.io/wxapp_static/app/image-20200111124539391.png %}

> 封装组件。

最好的方式就是封装组件，同样的外观，不同的数据。

### 文章列表组件

1、创建 `views/home/components/article-list.vue`

```html
<template>
  <div>{{ channel.name }}频道的文章列表</div>
</template>

<script>
export default {
  name: 'ArticleList',
  components: {},
  props: {
    // 频道对象，父组件提供
    channel: {
      type: Object,
      required: true
    }
  },
  data () {
    return {}
  },
  computed: {},
  watch: {},
  created () {},
  mounted () {},
  methods: {}
}
</script>

<style scoped></style>


```

2、在首页中注册使用

```html
<template>
  <div class="home-container">
    <!-- 导航栏 -->
    <van-nav-bar title="首页" />
    <!-- /导航栏 -->
    
    <!-- 频道列表 -->
    <van-tabs v-model="active">
      <van-tab
        :title="channel.name"
        v-for="channel in userChannels"
        :key="channel.id"
      >
+        <article-list :channel="channel" />
      </van-tab>
    </van-tabs>
    <!-- /频道列表 -->
  </div>
</template>

<script>
import { getUserChannels } from '@/api/user'
+ import ArticleList from './components/article-list'

export default {
  name: 'HomePage',
  components: {
+    ArticleList
  },
  ...
  ...
}
</script>


```



### 文章列表组件布局

这里主要使用到 [List 列表](https://youzan.github.io/vant/#/zh-CN/list) 组件，该组件支持上拉加载更多功能。

```html
<template>
  <van-list
    v-model="loading"
    :finished="finished"
    finished-text="没有更多了"
    @load="onLoad"
  >
    <van-cell
      v-for="(item, index) in list"
      :key="item"
      :title="item"
    />
  </van-list>
</template>

<script>
export default {
  name: 'ArticleList',
  components: {},
  props: {
    // 频道对象，父组件提供
    channel: {
      type: Object,
      required: true
    }
  },
  data () {
    return {
      list: [], // 列表数据
      loading: false,
      finished: false
    }
  },
  computed: {},
  watch: {},
  created () {},
  mounted () {},
  methods: {
    onLoad() {
      // 异步更新数据
      setTimeout(() => {
        for (let i = 0; i < 10; i++) {
          this.list.push(this.list.length + 1)
        }
        // 加载状态结束
        this.loading = false

        // 数据全部加载完成
        if (this.list.length >= 40) {
          this.finished = true
        }
      }, 500)
    }
  }
}
</script>

```

### 加载文章列表

实现思路：

- 封装获取数据接口
- 请求加载
- 模板绑定



1、创建 `src/api/article.js` 封装获取文章列表数据的接口

```js
/**
 * 文章接口模块
 */
import request from '@/utils/request'

/**
 * 获取频道的文章列表
 */
export const getArticles = params => {
  return request({
    method: 'GET',
    url: '/app/v1_1/articles',
    params
  })
}

```

> 注意：使用接口文档中最下面的 **频道新闻推荐\_V1.1**

2、然后在首页文章列表组件 `onload` 的时候请求加载文章列表

```html
<template>
  <van-list
    v-model="loading"
    :finished="finished"
    finished-text="没有更多了"
    @load="onLoad"
  >
    <van-cell
+      v-for="(article, index) in list"
+      :key="index"
+      :title="article.title"
    />
  </van-list>
</template>

<script>
+ import { getArticles } from '@/api/article'

export default {
  name: 'ArticleList',
  components: {},
  props: {
    // 频道对象，父组件提供
    channel: {
      type: Object,
      required: true
    }
  },
  data () {
    return {
      list: [], // 列表数据
      loading: false, // 上拉加载更多的 loading 状态
      finished: false, // 数据是否加载完毕
+      timestamp: null // 用于获取下一页数据的时间戳
    }
  },
  computed: {},
  watch: {},
  created () {},
  mounted () {},
  methods: {
+++    async onLoad () {
      // 1. 请求获取数据
      const { data } = await getArticles({
        channel_id: this.channel.id, // 频道id

        // 第1次使用 Date.now() 获取最新数据
        // 下一页的数据使用本次返回的数据中的 timestamp
        timestamp: this.timestamp || Date.now(), // 时间戳，请求新的推荐数据传当前的时间戳，请求历史推荐传指定的时间戳
        with_top: 1 // 是否包含置顶，进入页面第一次请求时要包含置顶文章，1-包含置顶，0-不包含
      })

      // 2. 把请求获取到的数据添加到数组列表中
      const results = data.data.results
      this.list.push(...results)

      // 3. 加载状态结束
      this.loading = false

      // 4. 数据全部加载完成
      if (results.length) {
        // 更新获取下一页数据的时间戳
        this.timestamp = data.data.pre_timestamp
      } else {
        // 没有数据了，把 finished 设置为 true，不再加载更多
        this.finished = true
      }
    }
  }
}
</script>



```

> 标签页组件本身就具有延迟渲染的功能：
>
> - 首次切换到标签时才触发内容渲染
> - 而且对于渲染过的就不再重新渲染

### 下拉刷新

{% img https://yhx0507.github.io/wxapp_static/app/list-pull.gif %}

这里主要使用到 Vant 中的 [PullRefresh 下拉刷新](https://youzan.github.io/vant/#/zh-CN/pull-refresh) 组件。

思路：

- 注册下拉刷新事件（组件）的处理函数
- 发送请求获取文章列表数据
- 把获取到的数据添加到当前频道的文章列表的顶部
- 提示用户刷新成功！

下拉刷新时会触发组件的 `refresh` 事件，在事件的回调函数中可以进行同步或异步操作，操作完成后将 `v-model` 设置为 `false`，表示加载完成。

```js
async onRefresh () {
  // 1. 请求获取数据
  const { data } = await getArticles({
    channel_id: this.channel.id, // 频道id
    timestamp: Date.now(), // 时间戳，请求新的推荐数据传当前的时间戳，请求历史推荐传指定的时间戳
    with_top: 1
  })

  // 2. 如果有最新数据，则把数据放到列表的顶部
  const { results } = data.data
  this.list.unshift(...results)

  // 3. 关闭下拉刷新的 loading 状态
  this.isLoading = false

  // 提示更新成功
  this.$toast(`更新了${results.length}条数据`)
}


```

## 样式调整

我们的需求：

- 头部固定定位
- 频道列表固定定位

1、让头部导航栏固定定位

```html
<van-nav-bar title="首页" fixed />


```

2、让频道列表固定定位

```less
.home-container {
  /deep/ .van-tabs__wrap {
    position: fixed;
    top: 46px;
    left: 0;
    right: 0;
    z-index: 1;
  }
}


```

> [关于 .vue 文件中的作用域样式](https://vue-loader.vuejs.org/zh/guide/scoped-css.html)

3、给页面容器添加上下内边距

```less
.home-container {
  padding-top: 90px;
  padding-bottom: 50px;
}

```



如果想要在父组件中影响子组件样式：

- 要么不要有作用域，那就是全局，影响任何组件

- 如果有作用域
  - 默认只能影响到自子组件的根节点
    - 审查元素找到子组件根节点类名使用
    - 或者手动给子组件添加一个 class，它会自动添加到子组件根节点的 class 中
  - 如果需要影响的更深，则使用深度选择器：`>>>`、`/deep/`、`::v-deep`
    - `>>>` 在 less、sass、stylus 等 CSS 预处理器中会报错
    - 所以建议使用 `/deep/` 或者 `::v-deep`

# 七、首页—频道编辑

{% img https://yhx0507.github.io/wxapp_static/app/1566541118035.png %}

## 准备

### 弹出层组件

Vant 中内置了 [Popup 弹出层](https://youzan.github.io/vant/#/zh-CN/popup) 组件。

1、在首页模板中的频道列表后面添加弹出层组件

```html
<van-popup
  v-model="isChannelEditShow"
  position="bottom"
  closeable
  close-icon-position="top-left"
  :style="{ height: '100%' }"
/>

```

2、然后在 `data`中添加一个数据用来控制弹层的显示和隐藏

```js
data () {
  return {
    ...
    isChannelEditShow: true // 这里我们先设置为 true 就能看到弹窗的页面了
  }
}

```

测试查看结果。

### 点击面包按钮展示弹出层

通过标签页的插槽插入一个面包菜单图标到频道列表中。

```html
<van-tabs v-model="active">
+  <van-icon
+    class="wap-nav"
+    slot="nav-right"
+    name="wap-nav"
+    @click="isChannelEditShow = true"
+  />
  <van-tab
    :title="channel.name"
    v-for="channel in userChannels"
    :key="channel.id"
  >
    <article-list :channel="channel" />
  </van-tab>
</van-tabs>

```

> 注意：我们这里是把面包按钮通过 van-tabs 组件的 nav-right 插槽插入进去的。

别忘了给它设置一下样式，定位到右侧不动：

```css
.wap-nav {
	position: fixed;
	right: 0;
	line-height: 44px;
	background: #fff;
}

```

测试查看结果。

### 频道编辑组件

1、创建 `views/home/components/channel-edit.vue`

```html
<template>
  <div class="channel-edit">频道编辑</div>
</template>

<script>
export default {
  name: 'ChannelEdit',
  components: {},
  props: {},
  data () {
    return {}
  },
  computed: {},
  watch: {},
  created () {},
  mounted () {},
  methods: {}
}
</script>

<style scoped></style>


```

2、在首页中加载注册

```js
import ChannelEdit from './components/channel-edit'

```

```js
export default {
  ...
  components: {
    ...
    ChannelEdit
  }
}

```



3、在弹出层中使用频道编辑组件

```html
<!-- 频道编辑 -->
<van-popup
  v-model="isChannelEditShow"
  position="bottom"
  closeable
  close-icon-position="top-left"
  :style="{ height: '100%' }"
>
+  <channel-edit />
</van-popup>
<!-- /频道编辑 -->

```



### 组件布局

```html
<template>
  <div class="channel-edit">
    <van-cell title="我的频道" :border="false">
      <van-button size="mini" round type="danger" plain>编辑</van-button>
    </van-cell>

    <van-grid :gutter="10">
      <van-grid-item
        v-for="value in 8"
        :key="value"
        text="文字"
      />
    </van-grid>

    <van-cell title="推荐频道" :border="false" />
    <van-grid :gutter="10">
      <van-grid-item
        v-for="value in 8"
        :key="value"
        text="文字"
      />
    </van-grid>
  </div>
</template>

<script>
export default {
  name: 'ChannelEdit',
  components: {},
  props: {},
  data () {
    return {}
  },
  computed: {},
  watch: {},
  created () {},
  mounted () {},
  methods: {}
}
</script>

<style scoped>
.channel-edit {
  padding: 40px 0;
}
</style>


```



## 展示我的频道

1、父组件给子组件传递数据

```html
<!-- 频道编辑 -->
<van-popup
  v-model="isChannelEditShow"
  position="bottom"
  closeable
  close-icon-position="top-left"
  :style="{ height: '100%' }"
>
  <channel-edit
+		:user-channels="userChannels"
	/>
</van-popup>
<!-- /频道编辑 -->


```



2、在子组件中声明接收父组件的 `userChannels` 频道列表数据并遍历展示

```html
<template>
  <div class="channel-edit">
    <van-cell title="我的频道" :border="false">
      <van-button size="mini" round type="danger" plain>编辑</van-button>
    </van-cell>

    <van-grid :gutter="10">
      <van-grid-item
+        v-for="channel in userChannels"
+        :key="channel.id"
+        :text="channel.name"
      />
    </van-grid>

    <van-cell title="推荐频道" :border="false" />
    <van-grid :gutter="10">
      <van-grid-item
        v-for="value in 8"
        :key="value"
        text="文字"
      />
    </van-grid>
  </div>
</template>

<script>
export default {
  name: 'ChannelEdit',
  components: {},
  props: {
+    userChannels: {
+      type: Array,
+      required: true
+    }
  },
  data () {
    return {}
  },
  computed: {},
  watch: {},
  created () {},
  mounted () {},
  methods: {}
}
</script>

<style scoped>
.channel-edit {
  padding: 40px 0;
}
</style>



```

> 简单写法：props: []
>
> 推荐写法：props: {}

## 展示推荐频道列表

{% img https://yhx0507.github.io/wxapp_static/app/1571040968593.png %}

没有用来获取推荐频道的数据接口，但是我们有获取所有频道列表的数据接口。

所以：`所有频道列表 - 我的频道 = 剩余推荐的频道`。

实现过程所以一共分为两大步：

- 获取所有频道
- 基于所有频道和我的频道计算获取剩余的推荐频道

### 获取所有频道

1、在 `src/api/channel.js` 中添加封装请求模块

```js
export const getAllChannels = () => {
  return request({
    method: 'GET',
    url: '/app/v1_0/channels'
  })
}


```

2、请求获取所有频道

```js
+ import { getAllChannels } from '@/api/channel'

export default {
  name: 'ChannelEdit',
  components: {},
  props: {
    userChannels: {
      type: Array,
      required: true
    }
  },
  data () {
    return {
+      allChannels: [] // 所有频道
    }
  },
  computed: {},
  watch: {},
  created () {
+    this.loadAllChannels()
  },
  mounted () {},
  methods: {
+    async loadAllChannels () {
+      const { data } = await getAllChannels()
+      this.allChannels = data.data.channels
+    }
  }
}


```



### 处理获取展示剩余推荐频道

思路：`剩余频道 = 所有频道 - 我的频道`

- 遍历所有频道
- 对每一个频道都判断：该频道是否属于我的频道
- 如果不属于我的频道，则收集起来
- 直到遍历结束，剩下来就是那些剩余的推荐频道

1、封装一个**计算属性**用来获取剩余频道

```js
computed: {
  remainingChannels () {
    const { allChannels, userChannels } = this
    // 剩余频道 = 所有频道 - 我的频道
    const channels = []
    // 遍历所有频道
    allChannels.forEach(item => {
      // 如果我的频道中不包含当前被遍历的频道，则要
      if (!userChannels.find(c => c.id === item.id)) {
        channels.push(item)
      }
    })
    return channels
  }
},
  


```

2、模板绑定进行展示

```html
<van-cell title="推荐频道" :border="false" />
<van-grid :gutter="10">
  <van-grid-item
    v-for="channel in remainingChannels"
    :key="channel.id"
    :text="channel.name"
  />
</van-grid>


```

## 添加频道

{% img https://yhx0507.github.io/wxapp_static/app/chanel-add.gif %}

思路：

- 给推荐频道列表中每一项注册点击事件
- 获取点击的频道项
- 将频道项添加到我的频道中
- ~~将当前点击的频道项从推荐频道中移除~~
  - 不需要删除，因为我们获取数据使用的是计算属性，当我频道发生改变，计算属性重新求值了

1、给推荐频道中的频道注册点击事件

```html
<!-- 推荐频道 -->
<van-cell title="推荐频道" :border="false" />
<van-grid :gutter="10">
  <van-grid-item
    v-for="channel in recommendChannels"
    :key="channel.id"
    :text="channel.name"
    + @click="onChannelAdd(channel)"
  />
</van-grid>
<!-- /推荐频道 -->

```

2、在添加频道事件处理函数中

```js
onChannelAdd (channel) {
  // 将点击的频道项添加到我的频道列表中
  this.channels.push(channel)

  // 不需要删除，我的频道改变，计算属性 recommendChannels 重新调用求值
}

```

然后你会神奇的发现点击的那个推荐频道跑到我的频道中了，我们并没有去手动的删除点击的这个推荐频道，但是它没了！主要是因为推荐频道是通过一个计算属性获取的，计算属性中使用了 channels（我的频道）数据，所以只要我的频道中的数据发生变化，那么计算属性就会重新运算获取最新的数据。

## 编辑频道

### 处理编辑状态

1、在 data 中添加数据用来控制关闭按钮的显示隐藏

```js
data () {
  return {
    ...
    isEditShow: false
  }
}


```



2、通过组件插槽将关闭图标放到频道项中

```html
<van-grid :gutter="10" clickable>
  <van-grid-item
    v-for="(channel, index) in userChannels"
    :key="channel.id"
    :text="channel.name"
    @click="onUserChannelClick(index)"
  >
    <van-icon v-show="isEditShow && index !== 0" slot="icon" name="close" />
  </van-grid-item>
</van-grid>


```

3、将关闭的图标定位到频道项的右上角

```less
/deep/ .van-grid-item__icon-wrapper {
  position: absolute;
  top: -14px;
  right: -5px;
  .van-icon-close {
    font-size: 14px;
  }
}


```

4、处理编辑按钮

```html
<van-button
  size="mini"
  round
  type="danger"
  plain
  @click="isEditShow = !isEditShow"
>{{ isEditShow ? '完成' : '编辑'}}</van-button>


```

最后测试。

### 删除频道

两个功能需求：

- 切换频道
- 删除频道

1、给我的频道中的频道项注册点击事件

2、处理函数如下

```js
onUserChannelClick (index) {
  // 如果是编辑状态，则执行删除操作
  if (this.isEditShow && index !== 0) {
    this.userChannels.splice(index, 1) // 从索引处开始(包括索引本身)，删除指定的个数
  }
  // 如果是非编辑状态，则执行切换频道操作
}


```

最后测试结果。

### 切换频道

1、在父组件中

```html
<!--
  这里使用 v-model 绑定了 active 数据
		子组件在内部需要声明 value 属性接收使用
		子组件需要在内部通过 this.$emit('input', 数据) 修改该数据
-->
<channel-edit
  :user-channels="userChannels"
	:value="active"
	@input="active = $event"
  @close="isChannelEditShow = false"
/>

<!-- 这里的写法等价于上面的写法 -->
<channel-edit
  :user-channels="userChannels"
	v-model="active"
  @close="isChannelEditShow = false"
/>


```

> [在组件上使用 v-model]([https://cn.vuejs.org/v2/guide/components-custom-events.html#%E8%87%AA%E5%AE%9A%E4%B9%89%E7%BB%84%E4%BB%B6%E7%9A%84-v-model](https://cn.vuejs.org/v2/guide/components-custom-events.html#自定义组件的-v-model))

2、然后在子组件中切换频道

```js
onUserChannelClick (index) {
  // 如果是编辑状态，则删除频道
  if (this.isEditShow) {
    this.userChannels.splice(index, 1)
  } else {
    // 如果是非编辑状态，则切换频道
    this.$emit('input', index) // 修改激活的标签
    this.$emit('close') // 关闭弹层
  }
}


```

3、最后测试结果

## 频道数据持久化

正常的话我们需要调用接口对数据进行持久化存储，但是这里的项目接口暂时有问题。

我们这里先用本地存储来处理一下。

1、当我们添加或是删除频道的时候都需要对频道数据进行持久化，一种方式就是在分别在添加和删除或者比的修改 channels 数据变化的地方调用本次存储存储数据；一种方式，我们可以使用 Vue 中的 watch 功能来监视频道数据的变化，当数据发生改变，就把数据重新存储到本地存储。

```js
import { getItem, setItem } from "@/utils/storage";


```

```js
watch: {
  // 当 userChannels 发生改变的时候，将该数据存储到本地存储
  userChannels () {
    setItem('user-channels', this.userChannels)
  }
},


```

2、因为我们把数据持久化到了本次存储，所以我们需要在页面初始化的时候优先从本地存储来获取频道数据

```js
// 简单一句话：优先使用本地的，没有就使用线上的
async loadUserChannels () {
  // 1. 定义一个变量用来存储频道列表
  let channels = []

  // 2. 获取本地存储的频道列表
  const localUserChannles = getItem('user-channels')

  // 3. 如果本地存储有，就使用本地存储的
  if (localUserChannles) {
    channels = localUserChannles
  } else {
    // 4. 如果本地存储没有，则请求获取接口推荐的频道列表
    const { data } = await getUserChannels()
    channels = data.data.channels
  }

  // 5. 最后，把数据赋值到当前组件中
  this.userChannels = channels
}

```

# 八、文章搜索

{% img https://yhx0507.github.io/wxapp_static/app/1566431801892.png %}

{% img https://yhx0507.github.io/wxapp_static/app/wx_20190828082340.jpg %}


## 准备

### 创建组件并配置路由

1、创建 `src/views/search/index.vue`

```html
<template>
  <div class="search-container">搜索页面</div>
</template>

<script>
  export default {
    name: "SearchPage",
    components: {},
    props: {},
    data() {
      return {};
    },
    computed: {},
    watch: {},
    created() {},
    methods: {}
  };
</script>

<style scoped></style>

```

2、然后把搜索页面的路由配置到根组件路由（一级路由）

```js
{
  path: '/search',
  omponent: Search
}

```

最后访问 `/search` 测试。

### 页面布局

```html
<template>
  <div class="search-container">
    <!-- 搜索栏 -->
    <form action="/">
      <van-search
        v-model="searchContent"
        placeholder="请输入搜索关键词"
        show-action
				background="#3296fa"
        @search="onSearch"
        @cancel="onCancel"
      />
    </form>
    <!-- /搜索栏 -->

    <!-- 联想建议 -->
    <van-cell-group>
      <van-cell icon="search" title="单元格" />
      <van-cell icon="search" title="单元格" />
      <van-cell icon="search" title="单元格" />
      <van-cell icon="search" title="单元格" />
      <van-cell icon="search" title="单元格" />
      <van-cell icon="search" title="单元格" />
    </van-cell-group>
    <!-- /联想建议 -->

    <!-- 历史记录 -->
    <van-cell title="历史记录">
      <van-icon name="delete" />
      <span>全部删除</span>
      &nbsp;&nbsp;
      <span>完成</span>
    </van-cell>
    <van-cell-group>
      <van-cell title="单元格">
        <van-icon name="close"></van-icon>
      </van-cell>
      <van-cell title="单元格">
        <van-icon name="close"></van-icon>
      </van-cell>
      <van-cell title="单元格">
        <van-icon name="close"></van-icon>
      </van-cell>
      <van-cell title="单元格">
        <van-icon name="close"></van-icon>
      </van-cell>
      <van-cell title="单元格">
        <van-icon name="close"></van-icon>
      </van-cell>
    </van-cell-group>
    <!-- /历史记录 -->

    <!-- 搜索结果 -->
    <van-list
      v-model="loading"
      :finished="finished"
      finished-text="没有更多了"
      @load="onLoad"
    >
      <van-cell
        v-for="item in list"
        :key="item"
        :title="item"
      />
    </van-list>
    <!-- /搜索结果 -->
  </div>
</template>

<script>
export default {
  name: 'SearchPage',
  components: {},
  props: {},
  data () {
    return {
      searchContent: '', // 搜索内容
      list: [],
      loading: false,
      finished: false
    }
  },
  computed: {},
  watch: {},
  created () {},
  mounted () {},
  methods: {
    onSearch () {
      console.log('onSearch')
    },
    onCancel () {
      console.log('onCancel')
    },

    onLoad () {
      // 异步更新数据
      setTimeout(() => {
        for (let i = 0; i < 10; i++) {
          this.list.push(this.list.length + 1)
        }
        // 加载状态结束
        this.loading = false

        // 数据全部加载完成
        if (this.list.length >= 40) {
          this.finished = true
        }
      }, 500)
    }
  }
}
</script>

<style scoped lang="less">
.search-container {
  padding-top: 54px;
  // 让搜索栏固定在顶部
  .search-form {
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    z-index: 1;
  }
  .van-search__action {
    color: #fff;
  }
}
</style>


```



## 处理(历史记录/联想建议/搜索结果)显示状态

1、在 `data` 中添加数据用来控制搜索结果的显示状态

```js
data () {
  ...
  isResultShow: false
}

```

2、在模板中绑定条件渲染

```html
<!-- 搜索结果 -->
<search-result v-if="isResultShow" />
<!-- /搜索结果 -->

<!-- 联想建议 -->
<van-cell-group v-else-if="searchText">
  ...
</van-cell-group>
<!-- /联想建议 -->

<!-- 历史记录 -->
<van-cell-group v-else>
  ...
</van-cell-group>
<!-- /历史记录 -->

```



## 搜索联想建议

思路：

- 当搜索输入变化的时候，请求加载联想建议的数据
- 将请求得到的结果绑定到模板中

1、封装数据接口，创建 `api/serach.js`

```js
export const getSuggestions = q => {
  return request({
    method: 'GET',
    url: '/app/v1_0/suggestion',
    params: {
      q
    }
  })
}

```

2、给搜索组件注册 `input` 事件

3、在事件处理函数中请求获取数据

```js
import SearchResult from './components/search-result'
+ import { getSuggestions } from '@/api/search'

export default {
  name: 'SearchPage',
  components: {
    SearchResult
  },
  props: {},
  data () {
    return {
      searchText: '',
      isResultShow: false,
+      suggestions: [] // 联想建议列表
    }
  },
  computed: {},
  watch: {},
  created () {},
  mounted () {},
  methods: {
    onSearch () {
      this.isResultShow = true
    },

+++    async onSearchInput () {
      const searchText = this.searchText
      if (!searchText) {
        return
      }
      const { data } = await getSuggestions(searchText)
      this.suggestions = data.data.options
    }
  }
}

```

4、模板绑定

```html
<!-- 联想建议 -->
<van-cell-group v-else-if="searchText">
  <van-cell
    :title="item"
    icon="search"
    :key="index"
    v-for="(item, index) in suggestions"
  />
</van-cell-group>
<!-- /联想建议 -->

```

### 搜索关键字高亮

如何将字符串中的指定字符在**网页**中高亮展示？

```js
"Hello World";


```

将需要高亮的字符包裹 HTML 标签，为其单独设置颜色。

```js
"Hello <span style="color: red">World</span>"


```

在 Vue 中如何渲染带有 HTML 标签的字符串？

```js
data () {
  return {
    htmlStr: 'Hello <span style="color: red">World</span>'
  }
}


```



```html
<div>{{ htmlStr }}</div>
<div v-html="htmlStr"></div>


```

{% img https://yhx0507.github.io/wxapp_static/app/image-20200112154732044.png %}

如何把字符串中指定字符统一替换为高亮（包裹了 HTML）的字符？

```js
const str = "Hello World"

// 结果：<span style="color: red">Hello</span> World
"Hello World".replace('Hello', '<span style="color: red">Hello</span>')

// 需要注意的是，replace 方法的字符串匹配只能替换第1个满足的字符
// <span style="color: red">Hello</span> World Hello abc
"Hello World Hello abc".replace('Hello', '<span style="color: red">Hello</span>')

// 如果想要全文替换，使用正则表达式
// g 全局
// i 忽略大小写
// <span style="color: red">Hello</span> World <span style="color: red">Hello</span> abc
"Hello World Hello abc".replace(/Hello/gi, '<span style="color: red">Hello</span>')


```

> 一个小扩展：使用字符串的 split 结合数组的 join 方法实现高亮
>
> ```js
> var str = "hello world 你好 hello";
> 
> // ["", " world 你好 ", ""]
> const arr = str.split("hello");
> 
> // "<span>hello</span> world 你好 <span>hello</span>"
> arr.join("<span>hello</span>");
> 
> 
> ```

下面是具体的处理。

1、在 methods 中添加一个方法处理高亮

```js
// 参数 source: 原始字符串
// 参数 keyword: 需要高亮的关键词
// 返回值：替换之后的高亮字符串
highlight (source, keyword) {
  // /searchContent/ 正则表达式中的一切内容都会当做字符串使用
  // 这里可以 new RegExp 方式根据字符串创建一个正则表达式
  // RegExp 是原生 JavaScript 的内置构造函数
  // 参数1：字符串，注意，这里不要加 //
  // 参数2：匹配模式，g 全局，i 忽略大小写
  const reg = new RegExp(keyword, 'gi')
  return source.replace(reg, `<span style="color: #3296fa">${keyword}</span>`)
},


```

2、然后在联想建议列表项中绑定调用

```html
<!-- 联想建议 -->
<van-cell-group v-else-if="searchContent">
  <van-cell
    icon="search"
    v-for="(item, index) in suggestions"
    :key="index"
    @click="onSearch(item)"
  >
    <div slot="title" v-html="highlight(item, searchContent)"></div>
  </van-cell>
</van-cell-group>
<!-- /联想建议 -->


```



### 防抖处理（最后处理）

1、安装 lodash

```sh
# yarn add lodash
npm i lodash


```

2、防抖处理

```js
// lodash 支持按需加载，有利于打包结果优化
import { debounce } from "lodash"


```

> 不建议下面这样使用，因为这样会加载整个模块。
>
> ```js
> import _ from 'lodash'
> _.debounce()
> 
> 
> ```

```js
// debounce 函数
// 参数1：函数
// 参数2：防抖时间
// 返回值：防抖之后的函数，和参数1功能是一样的
onSearchInput: debounce(async function () {
  const searchContent = this.searchContent
  if (!searchContent) {
    return
  }

  // 1. 请求获取数据
  const { data } = await getSuggestions(searchContent)

  // 2. 将数据添加到组件实例中
  this.suggestions = data.data.options

  // 3. 模板绑定
}, 200),


```

## 搜索结果

思路：

- 请求获取数据
- 将数据展示到模板中

一、获取搜索关键字

1、声明接收父组件中的搜索框输入的内容

```js
props: {
  q: {
    type: String,
    require: true
  }
},


```



2、在父组件给子组件传递数据

```html
<!-- 搜索结果 -->
<search-result v-if="isResultShow" :q="searchText" />
<!-- /搜索结果 -->


```



最后在调试工具中查看确认是否接收到 props 数据。

{% img https://yhx0507.github.io/wxapp_static/app/image-20200112162223915.png %}

二、请求获取数据

1、在 `api/serach.js` 添加封装获取搜索结果的请求方法

```js
/**
 * 获取搜索结果
 */
export function getSearch(params) {
  return request({
    method: "GET",
    url: "/app/v1_0/search",
    params
  })
}


```

2、请求获取

```js
+ import { getSearch } from '@/api/search'

export default {
  name: 'SearchResult',
  components: {},
  props: {
    q: {
      type: String,
      require: true
    }
  },
  data () {
    return {
      list: [],
      loading: false,
      finished: false,
+      page: 1,
+      perPage: 20
    }
  },
  computed: {},
  watch: {},
  created () {},
  mounted () {},
  methods: {
+++    async onLoad () {
      // 1. 请求获取数据
      const { data } = await getSearch({
        page: this.page, // 页码
        per_page: this.perPage, // 每页大小
        q: this.q // 搜索关键字
      })

      // 2. 将数据添加到列表中
      const { results } = data.data
      this.list.push(...results)

      // 3. 设置加载状态结束
      this.loading = false

      // 4. 判断数据是否加载完毕
      if (results.length) {
        this.page++ // 更新获取下一页数据的页码
      } else {
        this.finished = true // 没有数据了，将加载状态设置结束，不再 onLoad
      }
    }
  }
}

```

三、最后，模板绑定

```html
<van-list
  v-model="loading"
  :finished="finished"
  finished-text="没有更多了"
  @load="onLoad"
>
  <van-cell
+    v-for="(article, index) in list"
+    :key="index"
+    :title="article.title"
  />
</van-list>

```

## 搜索历史记录

### 添加历史记录

当发生搜索的时候我们才需要记录历史记录。

1、在 data 中添加一个数据用来存储历史记录

```js
data () {
  return {
    ...
    searchHistories: []
  }
}


```

2、在触发搜索的时候，记录历史记录

```js
onSearch (q) {
  // 1. 更新搜索文本框的数据
  this.searchContent = q

  // 2. 记录搜索历史记录
  const searchHistories = this.searchHistories
  const index = searchHistories.indexOf(q)
  if (index !== -1) {
    searchHistories.splice(index)
  }
  searchHistories.unshift(q)

  // 3. 展示搜索结果
  this.isSearchResultShow = true
},


```



### 展示历史记录

```html
<!-- 历史记录 -->
<van-cell-group v-else>
  <van-cell title="历史记录">
    <van-icon name="delete" />
    <span>全部删除</span>
    &nbsp;&nbsp;
    <span>完成</span>
  </van-cell>
  <van-cell
    :title="item"
    v-for="(item, index) in searchHistories"
    :key="index"
  >
    <van-icon name="close"></van-icon>
  </van-cell>
</van-cell-group>
<!-- /历史记录 -->


```



### 删除历史记录

一、处理删除相关元素的展示状态

1、在 data 中添加一个数据用来控制删除相关元素的显示状态

```js
data () {
  return {
    ...
    isDeleteShow: false
  }
}


```

2、绑定使用

```html
<!-- 历史记录 -->
<van-cell-group v-else>
  <van-cell title="历史记录">
    <template v-if="isDeleteShow">
      <span @click="searchHistories = []">全部删除</span>
      &nbsp;&nbsp;
      <span @click="isDeleteShow = false">完成</span>
    </template>
    <van-icon v-else name="delete" @click="isDeleteShow = true"></van-icon>
  </van-cell>
  <van-cell
    :title="item"
    v-for="(item, index) in searchHistories"
    :key="index"
    @click="onSearch(item)"
  >
    <van-icon
      v-show="isDeleteShow"
      name="close"
      @click="searchHistories.splice(index, 1)"
    ></van-icon>
  </van-cell>
</van-cell-group>
<!-- /历史记录 -->


```

二、处理删除操作

```html
<!-- 历史记录 -->
<van-cell-group v-else>
  <van-cell title="历史记录">
    <template v-if="isDeleteShow">
+      <span @click="searchHistories = []">全部删除</span>
      &nbsp;&nbsp;
      <span @click="isDeleteShow = false">完成</span>
    </template>
    <van-icon v-else name="delete" @click="isDeleteShow = true" />
  </van-cell>
  <van-cell
    :title="item"
    v-for="(item, index) in searchHistories"
    :key="index"
+    @click="onHistoryClick(item, index)"
  >
    <van-icon v-show="isDeleteShow" name="close"></van-icon>
  </van-cell>
</van-cell-group>
<!-- /历史记录 -->


```

```js
onHistoryClick (item, index) {
  // 如果是删除状态，则执行删除操作
  if (this.isDeleteShow) {
    this.searchHistories.splice(index, 1)
  } else {
    // 否则执行搜索操作
    this.onSearch(item)
  }
}


```

### 数据持久化

1、利用 watch 监视统一存储数据

```js
watch: {
  searchHistories (val) {
    // 同步到本地存储
    setItem('serach-histories', val)
  }
},


```



2、初始化的时候从本地存储获取数据

```js
data () {
  return {
    ...
    searchHistories: getItem('serach-histories') || [],
  }
}


```





## 扩展：函数防抖和函数节流

干嘛的？限制函数调用的频率。

为什么要限制？例如搜索的时候请求联想建议，没必要每次内容改变就发请求，当用户输入的很快的时候，中间的请求都是无意义的，浪费资源，没必要。

### 函数防抖（Debounce）

**概念：** `在事件被触发n秒后再执行，如果在这n秒内又被触发，则重新计时。`

**生活中的实例：** `如果有人进电梯（触发事件），那电梯将在10秒钟后出发（执行事件监听器），这时如果又有人进电梯了（在10秒内再次触发该事件），我们又得等10秒再出发（重新计时）。`

我们先使用第三方包 [lodash]() 来体验什么是函数防抖：

首先把 lodash 安装到项目中：

```sh
# yarn add lodash
npm i lodash


```

示例：

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <title>Document</title>
  </head>
  <body>
    <script src="./node_modules/lodash/lodash.js"></script>
    <script>
      // lodash 会在全局提供一个成员：_

      // _ 对象中有很多方法，其中有一个方法专门用于处理函数防抖
      // 方法名：debounce
      // 作用：函数防抖
      // 使用方式：

      function fn(foo) {
        console.log("hello", foo);
      }

      // 正常的函数调用：立即调用，而且是一定会调用
      // fn()
      // fn()
      // fn()

      // 我们可以使用函数防抖把一个正常的函数变得不正常
      // 两个参数：
      //   参数1：函数
      //   参数2：时间，单位是毫秒
      // 返回值：函数
      //   返回值函数的功能和 fn 和的功能是一样
      //   唯一的区别就是经过了防抖处理
      const newFn = _.debounce(fn, 1000);

      // 计时 1s
      newFn("a");

      // 当你不到 1s 的时候，再次调用
      // 先把之前的废掉，重新计时 1s
      newFn("b");

      newFn("b");
      newFn("b");
      // newFn()

      // he
    </script>
  </body>
</html>


```

### 函数防抖实现原理

函数防抖的实现原理：

```js
function fn(foo) {
  console.log("hello", foo);
}

const newFn = debounce(fn, 1000);

// 计时 1s
newFn(123);

// 如果在 1s 之内重新调用
//   先把之前的废除
//   重新计时
newFn("world");
// newFn()

function debounce(callback, time) {
  let timer = null;
  // 函数参数中的 ... 表示接收剩余参数
  // 它会把所有的参数收集到一个数组中
  return function(...args) {
    console.log(args);
    window.clearTimeout(timer);
    timer = setTimeout(() => {
      // 这里的 ... 表示数组展示操作符
      // args[0], args[1], args[2] .........
      callback(...args);
    }, time);
  };
}


```

### 函数节流（Throttle）

**概念：** `规定一个单位时间，在这个单位时间内，只能有一次触发事件的回调函数执行，如果在同一个单位时间内某事件被触发多次，只有一次能生效。`

生活中的例子：`函数节流就是开枪游戏的射速，就算一直按着鼠标射击，也只会在规定射速内射出子弹。`

我们先用 lodash 来体验节流的使用方式：

```js
function fn() {
  console.log("------ fire ------");
}

// 参数1：函数
// 参数2：间隔时间
// 返回值：函数（它的功能和保证的 fn 的功能是一样的，但是被进行了节流处理）
// 第1次直接调用，之后的按照一定频率进行调用
const newFn = _.throttle(fn, 2000);

// newFn()
// newFn()

setInterval(() => {
  console.log("鼠标点击");
  newFn();
}, 200);

// 一上来就调用一次
// newFn()

// // 之后的调用，开始计时 1s
// newFn()

// // 1s 之内所有的调用只有1次
// newFn()
// newFn()
// newFn()
// newFn()
// newFn()


```

### 函数节流实现原理

```js
function throttle(callback, interval) {
  // 最后一次的调用时间
  let lastTime = 0;

  // 定时器
  let timer = null;

  // 返回一个函数
  return function() {
    // 清除定时器
    clearTimeout(timer);

    // 当前最新时间
    let nowTime = Date.now();

    // 如果当前最新时间 - 上一次时间 >= 时间间隔
    // 或者没有上一次时间，那就立即调用
    if (nowTime - lastTime >= interval) {
      callback();

      // 记录最后一次的调用时间
      // 1
      lastTime = nowTime;
    } else {
      timer = setTimeout(() => {
        callback();
      }, interval);
    }
  };
}

const fn = throttle(函数, 1000);

//
fn();

fn();

fn();


```

### 函数节流或者防抖的相关场景

- 建议
- window.resize 事件处理
- 页面的滚动事件

### 总结

- 函数防抖和函数节流都是防止某一时间频繁触发，但是这两兄弟之间的特性却不一样。
  - search 搜索联想，用户在不断输入值时，用防抖来节约请求资源。
  - window 触发 resize 的时候，不断的调整浏览器窗口大小会不断的触发这个事件，用防抖来让其只触发一次
- 函数防抖是某一段时间内只执行一次，而函数节流是间隔时间执行。
  - 鼠标不断点击触发，mousedown(单位时间内只触发一次)
  - 监听滚动事件，比如是否滑到底部自动加载更多，用 throttle 来判断

## 扩展：lodash 函数库

lodash 是一个常用工具函数库，它里面封装了好多常用的工具函数：

- 官方文档： https://lodash.com/
- 翻译的中文文档：https://www.lodashjs.com/

> 在 lodash 之前还有一个 JavaScript 工具函数库：[underscore](https://underscorejs.org/)，lodash 模仿 underscope 自己重新实现了一遍，它相比 underscore 最大的好处是它支持按需加载。
>
> underscope 不支持按需加载。

# 九、文章详情

{% img https://yhx0507.github.io/wxapp_static/app/1566972493322.png %}

## 准备

### 创建组件并配置路由

1、创建 `views/article/index.vue` 组件

```html
<template>
  <div class="article-container">文章详情</div>
</template>

<script>
export default {
  name: 'ArticlePage',
  components: {},
  props: {
    articleId: {
      type: String,
      required: true
    }
  },
  data () {
    return {}
  },
  computed: {},
  watch: {},
  created () {},
  mounted () {},
  methods: {}
}
</script>

<style scoped lang="less"></style>


```

2、然后将该页面配置到根级路由

```js
{
  path: '/article/:articleId',
  name: 'article',
  component: () => import('@/views/article'),
  // 将路由动态参数映射到组件的 props 中，更推荐这种做法
  props: true
}

```

> [官方文档：路由 props 传参](https://router.vuejs.org/zh/guide/essentials/passing-props.html)

最后访问 `/article/*` 测试。

### 布局

一、注册组件

- [Loading 加载](https://youzan.github.io/vant/#/zh-CN/loading)

二、导入素材到文章组件目录

- no-network.png
- [github-markdown.css](https://raw.githubusercontent.com/sindresorhus/github-markdown-css/gh-pages/github-markdown.css)

三、布局

```html
<template>
  <div class="article-container">
    <!-- 导航栏 -->
    <van-nav-bar
      title="文章详情"
      left-arrow
      fixed
      @click-left="$router.back()"
    ></van-nav-bar>
    <!-- /导航栏 -->

    <!-- 加载中 -->
    <van-loading
      class="loading"
      color="#1989fa"
      vertical
    >
      <slot>加载中...</slot>
    </van-loading>
    <!-- /加载中 -->

    <!-- 文章详情 -->
    <div class="detail">
      <h3 class="title">一个合格的中级前端工程师需要掌握的 28 个 JavaScript 技巧</h3>
      <div class="author-wrap">
        <div class="base-info">
          <van-image
            class="avatar"
            round
            fit="cover"
            src="https://img.yzcdn.cn/vant/cat.jpeg"
          />
          <div class="text">
            <p class="name">黑马头条号</p>
            <p class="time">4 小时前</p>
          </div>
        </div>
        <van-button class="follow-btn" type="info" size="small" round>+ 关注</van-button>
      </div>
      <div class="markdown-body">
        <p>作为战斗在业务一线的前端，要想少加班，就要想办法提高工作效率。这里提一个小点，我们在业务开发过程中，经常会重复用到日期格式化、url参数转对象、浏览器类型判断、节流函数等一类函数，这些工具类函数，基本上在每个项目都会用到，为避免不同项目多次复制粘贴的麻烦，我们可以统一封装，发布到npm，以提高开发效率。</p>
        <p>使用 Object.prototype.toString 配合闭包，通过传入不同的判断类型来返回不同的判断函数，一行代码，简洁优雅灵活（注意传入 type 参数时首字母大写）</p>
        <p>使用 Object.prototype.toString 配合闭包，通过传入不同的判断类型来返回不同的判断函数，一行代码，简洁优雅灵活（注意传入 type 参数时首字母大写）</p>
        <p>使用 Object.prototype.toString 配合闭包，通过传入不同的判断类型来返回不同的判断函数，一行代码，简洁优雅灵活（注意传入 type 参数时首字母大写）</p>
        <p>使用 Object.prototype.toString 配合闭包，通过传入不同的判断类型来返回不同的判断函数，一行代码，简洁优雅灵活（注意传入 type 参数时首字母大写）</p>
      </div>
    </div>
    <!-- /文章详情 -->

    <!-- 加载失败提示 -->
    <div class="error">
      <img src="./no-network.png" alt="no-network">
      <p class="text">亲，网络不给力哦~</p>
      <van-button
        class="btn"
        type="default"
        size="small"
      >点击重试</van-button>
    </div>
    <!-- /加载失败提示 -->

    <!-- 底部区域 -->
    <div class="footer">
      <van-button
        class="write-btn"
        type="default"
        round
        size="small"
      >写评论</van-button>
      <van-icon
        class="comment-icon"
        name="comment-o"
        info="9"
      />
      <van-icon
        color="orange"
        name="star"
      />
      <van-icon
        color="#e5645f"
        name="good-job"
      />
      <van-icon class="share-icon" name="share" />
    </div>
    <!-- /底部区域 -->
  </div>
</template>

<script>
export default {
  name: 'ArticlePage',
  components: {},
  props: {
    articleId: {
      type: String,
      required: true
    }
  },
  data () {
    return {}
  },
  computed: {},
  watch: {},
  created () {},
  mounted () {},
  methods: {}
}
</script>

<style scoped lang="less">
@import "./github-markdown.css";

.article-container {
  padding: 46px 20px 50px;
  background: #fff;
  .loading {
    padding-top: 100px;
    text-align: center;
  }
  .detail {
    .title {
      margin: 0;
      padding-top: 24px;
      font-size: 20px;
      color: #3A3A3A;
    }
    .author-wrap {
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding: 20px 0;
      height: 40px;
      .base-info {
        display: flex;
        align-items: center;
        .avatar {
          width: 40px;
          height: 40px;
          margin-right: 8px;
        }
        .text {
          line-height: 1.5;
          .name {
            margin: 0;
            font-size: 14px;
          }
          .time {
            margin: 0;
            font-size: 12px;
            color: #999;
          }
        }
      }
      .follow-btn {
        width: 85px;
      }
    }
  }
  .error {
    padding-top: 100px;
    text-align: center;
    .text {
      font-size: 15px;
    }
    .btn {
      width: 30%;
    }
  }
  .footer {
    position: fixed;
    left: 0;
    right: 0;
    bottom: 0;
    box-sizing: border-box;
    display: flex;
    justify-content: space-around;
    align-items: center;
    height: 44px;
    border-top: 1px solid #d8d8d8;
    background-color: #fff;
    .write-btn {
      width: 160px;
    }
    .van-icon {
      font-size: 20px;
    }
    .comment-icon {
      bottom: -2px;
    }
    .share-icon {
      bottom: -2px;
    }
  }
}
</style>
```

## 页面 loading 组件

## 页面错误组件

## 图片预览

## 展示文章详情

这里我们主要实现两个主要功能：

- 获取展示文章详情
- 处理加载中 loading

一、请求并展示文章详情

1、在 `api/article.js` 中新增封装接口方法

```js
/**
 * 根据 id 获取指定文章
 */
export const getArticleById = articleId => {
  return request({
    method: 'GET',
    url: `/app/v1_0/articles/${articleId}`
  })
}

```

2、在组件中调用获取文章详情

```js
+ import { getArticleById } from '@/api/article'

export default {
  name: 'ArticlePage',
  components: {},
  props: {
    articleId: {
      type: String,
      required: true
    }
  },
  data () {
    return {
+      article: {} // 文章详情
    }
  },
  computed: {},
  watch: {},
  created () {
+    this.loadArticle()
  },
  mounted () {},
  methods: {
+++    async loadArticle () {
      try {
        const { data } = await getArticleById(this.articleId)
        this.article = data.data
      } catch (err) {
        console.log(err)
      }
    }
  }
}


```

3、模板绑定

```html
<!-- 文章详情 -->
<div class="detail">
  <h3 class="title">{{ article.title }}</h3>
  <div class="author-wrap">
    <div class="base-info">
      <van-image
        class="avatar"
        round
        fit="cover"
        :src="article.aut_photo"
      />
      <div class="text">
        <p class="name">{{ article.aut_name }}</p>
        <p class="time">{{ article.pubdate }}</p>
      </div>
    </div>
    <van-button class="follow-btn" type="info" size="small" round>+ 关注</van-button>
  </div>
  <div class="markdown-body" v-html="article.content"></div>
</div>
<!-- /文章详情 -->

<!-- 底部区域 -->
<div class="footer">
  <van-button
    class="write-btn"
    type="default"
    round
    size="small"
  >写评论</van-button>
  <van-icon
    class="comment-icon"
    name="comment-o"
    info="9"
  />
  <van-icon
    color="orange"
    :name="article.is_collected ? 'star' : 'star-o'"
    @click="onCollect"
  />
  <van-icon
    color="#e5645f"
    name="good-job"
    :name="article.attitude === 1 ? 'good-job' : 'good-job-o'"
  />
  <van-icon class="share-icon" name="share" />
</div>
<!-- /底部区域 -->

```



二、处理加载中 loading

需求：

- 加载中，显示 loading
- 加载成功，显示文章详情
- 加载失败，显示错误提示

步骤：

- 添加 loading 数据用来控制 loading 展示
- 在请求获取数据处理函数中处理 loading
  - 请求开始，loading = true
  - 请求结束，loading = false
- 在模板中绑定处理

下面是具体实现过程。

1、在 data 中添加数据用来控制加载中的 loading 状态

```js
data () {
  return {
    ...
    loading: true
  }
}

```

2、在请求获取文章的处理函数中处理 loading 状态

```js
async loadArticle () {
  this.loading = true
  try {
    const { data } = await getArticleById(this.articleId)
    this.article = data.data
  } catch (err) {
  }
  this.loading = false
}

```

3、在模板中绑定条件渲染

```html
<!-- 加载中 -->
<van-loading
  v-if="loading"
  class="loading"
  color="#1989fa"
  vertical
>
  <slot>加载中...</slot>
</van-loading>
<!-- /加载中 -->

<!-- 文章详情 -->
<div v-else-if="article.title" class="detail">
  ...
</div>
<!-- /文章详情 -->

<!-- 加载失败提示 -->
<div v-else class="error">
  ...
</div>
<!-- /加载失败提示 -->


```

## 文章收藏

思路：

- 给收藏按钮注册点击事件
- 如果已经收藏了，则取消收藏
- 如果没有收藏，则添加收藏

下面是具体实现。

1、在 `api/article.js` 添加封装数据接口

```js
/**
 * 收藏文章
 */
export const addCollect = target => {
  return request({
    method: 'POST',
    url: '/app/v1_0/article/collections',
    data: {
      target
    }
  })
}

/**
 * 取消收藏文章
 */
export const deleteCollect = target => {
  return request({
    method: 'DELETE',
    url: `/app/v1_0/article/collections/${target}`
  })
}


```

2、给收藏按钮注册点击事件

3、处理函数

```js
async onCollect () {
  // 这里 loading 不仅仅是为了交互提示，更重要的是请求期间禁用背景点击功能，防止用户不断的操作界面发出请求
  this.$toast.loading({
    duration: 0, // 持续展示 toast
    message: '操作中...',
    forbidClick: true // 是否禁止背景点击
  })

  try {
    // 如果已收藏，则取消收藏
    if (this.article.is_collected) {
      await deleteCollect(this.articleId)
      // this.article.is_collected = false
      this.$toast.success('取消收藏')
    } else {
      // 添加收藏
      await addCollect(this.articleId)
      // this.article.is_collected = true
      this.$toast.success('收藏成功')
    }
    this.article.is_collected = !this.article.is_collected
  } catch (err) {
    console.log(err)
    this.$toast.fail('操作失败')
  }
}


```



## 文章点赞

article 中的 `attitude` 表示用户对文章的态度

- `-1` 无态度
- `0` 不喜欢
- `1` 已点赞

思路：

- 给点赞按钮注册点击事件
- 如果已经点赞，则请求取消点赞
- 如果没有点赞，则请求点赞

1、添加封装数据接口

```js
/**
 * 点赞
 */
export const addLike = articleId => {
  return request({
    method: 'POST',
    url: '/app/v1_0/article/likings',
    data: {
      target: articleId
    }
  })
}

/**
 * 取消点赞
 */
export const deleteLike = articleId => {
  return request({
    method: 'DELETE',
    url: `/app/v1_0/article/likings/${articleId}`
  })
}


```

2、给点赞按钮注册点击事件

3、处理函数

```js
async onLike () {
  // 两个作用：1、交互提示 2、防止网络慢用户连续不断的点击按钮请求
  this.$toast.loading({
    duration: 0, // 持续展示 toast
    message: '操作中...',
    forbidClick: true // 是否禁止背景点击
  })

  try {
    // 如果已经点赞，则取消点赞
    if (this.article.attitude === 1) {
      await deleteLike(this.articleId)
      this.article.attitude = -1
      this.$toast.success('取消点赞')
    } else {
      // 否则添加点赞
      await addLike(this.articleId)
      this.article.attitude = 1
      this.$toast.success('点赞成功')
    }
  } catch (err) {
    console.log(err)
    this.$toast.fail('操作失败')
  }
}

```



## 关注用户

### 用户不能关注自己

什么情况下展示关注按钮？

- 如果用户没有登录
- 如果当前文章作者不是当前登录用户

```html
v-if="!$store.state.user || article.aut_id !== 当前登录用户.id"

```



下面是具体的实现。

一、获取当前登录用户的 ID

详见：[三、Token 处理—解析 Token](/03-Token处理?id=解析-token ':target=_blank')

二、条件绑定展示

```html
<van-button
  v-if="!$store.state.user || article.aut_id !== $store.state.user.id"
  class="follow-btn"
  :type="article.is_followed ? 'default' : 'info'"
  size="small"
  round
  :loading="isFollowLoading"
  @click="onFollow"
>{{ article.is_followed ? '已关注' : '+ 关注' }}</van-button>

```

最后测试。

### 关注|取消关注

思路：

- 给按钮注册点击事件
- 在事件处理函数中
  - 如果已关注，则取消关注
  - 如果没有关注，则添加关注

下面是具体实现。

1、在 `api/user.js` 中添加封装请求方法

```js
/**
 * 添加关注
 */
export const addFollow = userId => {
  return request({
    method: 'POST',
    url: '/app/v1_0/user/followings',
    data: {
      target: userId
    }
  })
}

/**
 * 取消关注
 */
export const deleteFollow = userId => {
  return request({
    method: 'DELETE',
    url: `/app/v1_0/user/followings/${userId}`
  })
}


```

2、给关注/取消关注按钮注册点击事件

3、在事件处理函数中

```js
import { addFollow, deleteFollow } from '@/api/user'


```

```js
async onFollow () {
  // 开启按钮的 loading 状态
  this.isFollowLoading = true

  try {
    // 如果已关注，则取消关注
    const authorId = this.article.aut_id
    if (this.article.is_followed) {
      await deleteFollow(authorId)
    } else {
      // 否则添加关注
      await addFollow(authorId)
    }

    // 更新视图
    this.article.is_followed = !this.article.is_followed
  } catch (err) {
    console.log(err)
    this.$toast.fail('操作失败')
  }

  // 关闭按钮的 loading 状态
  this.isFollowLoading = false
}


```

最后测试。

## 日期格式化

[moment](https://momentjs.com/) 是一个专门处理日期时间的一个纯 JavaScript 工具函数库，它不仅可以在浏览器中使用，也可以在 Node.js 中使用。

1、安装 moment

```sh
npm i moment


```

2、创建 `utils/datetime.js`

```js
/**
 * moment 日期处理封装
 */
import moment from 'moment'
import Vue from 'vue'

// 配置中文语言
moment.locale('zh-cn')

// 相对时间
Vue.filter('relativeTime', value => {
  // 参考文档：http://momentjs.cn/docs/#/manipulating/start-of/
  return moment(value).startOf('second').fromNow()
})

// 日期格式化
Vue.filter('datetime', (value, format = 'YYYY-MM-DD HH:mm:ss') => {
  return moment(value).format(format)
})


```

3、在 `main.js` 中加载初始化

```js
import './utils/datetime'

```

之后在任何组件的模板中直接调用过滤器函数即可，例如。

```html
<p class="time">{{ article.pubdate | relativeTime }}</p>

```

# 十、文章评论

{% img https://yhx0507.github.io/wxapp_static/app/1566972493322.png %}

## 文章评论

### 准备文章评论组件

为了更好的开发和维护，这里我们把文章评论单独封装到一个组件中来处理。

1、创建 `src/views/article/components/article-comment.vue`

```html
<template>
  <div class="article-comments">
    <!-- 评论列表 -->
    <van-list
      v-model="loading"
      :finished="finished"
      finished-text="没有更多了"
      @load="onLoad"
    >
      <van-cell v-for="item in list" :key="item" :title="item">
        <van-image
          slot="icon"
          round
          width="30"
          height="30"
          style="margin-right: 10px;"
          src="https://img.yzcdn.cn/vant/cat.jpeg"
        />
        <span style="color: #466b9d;" slot="title">hello</span>
        <div slot="label">
          <p style="color: #363636;">我出去跟别人说我的是。。。</p>
          <p>
            <span style="margin-right: 10px;">3天前</span>
            <van-button size="mini" type="default">回复</van-button>
          </p>
        </div>
        <van-icon slot="right-icon" name="like-o" />
      </van-cell>
    </van-list>
    <!-- 评论列表 -->

    <!-- 发布评论 -->
    <van-cell-group class="publish-wrap">
      <van-field clearable placeholder="请输入评论内容">
        <van-button slot="button" size="mini" type="info">发布</van-button>
      </van-field>
    </van-cell-group>
    <!-- /发布评论 -->
  </div>
</template>

<script>
  export default {
    name: "ArticleComment",
    props: {},
    data() {
      return {
        list: [], // 评论列表
        loading: false, // 上拉加载更多的 loading
        finished: false // 是否加载结束
      };
    },

    methods: {
      onLoad() {
        // 异步更新数据
        setTimeout(() => {
          for (let i = 0; i < 10; i++) {
            this.list.push(this.list.length + 1);
          }
          // 加载状态结束
          this.loading = false;

          // 数据全部加载完成
          if (this.list.length >= 40) {
            this.finished = true;
          }
        }, 500);
      }
    }
  };
</script>

<style scoped lang="less">
  .publish-wrap {
    position: fixed;
    left: 0;
    bottom: 0;
    width: 100%;
  }

  .van-list {
    margin-bottom: 45px;
  }
</style>

```

2、在文章详情页面中加载注册文章评论子组件

```js
import ArticleComment from './components/article-comment'

export default {
  ...
  components: {
    ArticleComment
  }
}

```

3、在文章详情页面的加载失败提示消息后面使用文章评论子组件

```html
<!-- 文章评论 -->
<article-comment />
<!-- /文章评论 -->

```

最终页面效果如下：

{% img https://yhx0507.github.io/wxapp_static/app/image-20191206152846065.png %}

### 展示文章评论列表

> 提示：有评论数据的文章 id：`139987`。

步骤：

- 封装接口
- 请求获取数据
- 处理模板

实现：

1、在 `api/comment.js` 中添加封装请求方法

```js
/**
 * 评论接口模块
 */
import request from "@/utils/request";

/**
 * 获取文章列表
 */
export function getComments(params) {
  return request({
    method: "GET",
    url: "/app/v1_0/comments",
    params
  });
}

```

2、请求获取数据

```js
data () {
  return {
    ...
    articleComment: { // 文章评论相关数据
      list: [],
      loading: false,
      finished: false,
      offset: null, // 请求下一页数据的页码
      totalCount: 0 // 总数据条数
    }
  }
}

```



```js
async onLoad () {
  const articleComment = this.articleComment
  // 1. 请求获取数据
  const { data } = await getComments({
    type: 'a', // 评论类型，a-对文章(article)的评论，c-对评论(comment)的回复
    source: this.articleId, // 源id，文章id或评论id
    offset: articleComment.offset, // 获取评论数据的偏移量，值为评论id，表示从此id的数据向后取，不传表示从第一页开始读取数据
    limit: 10 // 每页大小
  })

  // 2. 将数据添加到列表中
  const { results } = data.data
  articleComment.list.push(...results)

  // 更新总数据条数
  articleComment.totalCount = data.data.total_count

  // 3. 将加载更多的 loading 设置为 false
  articleComment.loading = false

  // 4. 判断是否还有数据
  if (results.length) {
    articleComment.offset = data.data.last_id // 更新获取下一页数据的页码
  } else {
    articleComment.finished = true // 没有数据了，关闭加载更多
  }
}

```

3、模板绑定

```html
<!-- 评论列表 -->
<van-list
  v-model="loading"
  :finished="finished"
  finished-text="没有更多了"
  @load="onLoad"
>
  <van-cell v-for="item in list" + :key="item.com_id.toString()">
    <van-image
      slot="icon"
      round
      width="30"
      height="30"
      style="margin-right: 10px;"
      +
      :src="item.aut_photo"
    />
    + <span style="color: #466b9d;" slot="title">{{ item.aut_name }}</span>
    <div slot="label">
      +
      <p style="color: #363636;">{{ item.content }}</p>
      <p>
        +
        <span style="margin-right: 10px;"
          >{{ item.pubdate | relativeTime }}</span
        >
        <van-button size="mini" type="default">回复</van-button>
      </p>
    </div>
    <van-icon slot="right-icon" name="like-o" />
  </van-cell>
</van-list>
<!-- 评论列表 -->

```

### 文章评论项组件

```html
<template>
  <van-cell class="comment-item">
    <!-- 评论作者头像 -->
    <van-image
      slot="icon"
      class="avatar"
      round
      :src="comment.aut_photo"
    />
    <!-- 评论作者头像 -->
    
    <!-- 评论作者名字 -->
    <span style="color: #466b9d;" slot="title">{{ comment.aut_name }}</span>
    <!-- 评论作者名字 -->
    
    <div slot="label">
      <!-- 评论内容 -->
      <p style="color: #363636;">{{ comment.content }}</p>
      <!-- /评论内容 -->
      
      <p>
        <!-- 评论发布日期 -->
        <span style="margin-right: 10px;">{{ comment.pubdate }}</span>
        <!-- 评论发布日期 -->
        
        <van-button
          size="mini"
          type="default"
        >回复 {{ comment.reply_count }}</van-button>
      </p>
    </div>
    <div slot="right-icon" class="like-container">
      <van-icon
        :color="comment.is_liking ? '#e5645f' : ''"
        :name="comment.is_liking ? 'good-job' : 'good-job-o'"
      />
      <span>{{ comment.like_count ? comment.like_count : '赞' }}</span>
    </div>
  </van-cell>
</template>

<script>
export default {
  name: 'CommentItem',
  components: {},
  props: {
    comment: {
      type: Object,
      required: true
    }
  },
  data () {
    return {}
  },
  computed: {},
  watch: {
  },
  created () {},
  methods: {}
}
</script>

<style scoped lang="less">
.comment-item {
  display: flex;
  align-items: flex-start;
  .avatar {
    width: 30px;
    height: 30px;
    margin-right: 10px;
  }
  .like-container {
    width: 30px;
    display: flex;
    align-items: center;
    justify-content: space-between;
    font-size: 13px;
  }
}
</style>


```



## 发布文章评论

## 发布组件

```html
<template>
  <div class="post-comment">
    <!--
      模板中的 $event 表示事件参数
     -->
    <van-field
      class="post-field"
      :value="value"
      @input="$emit('input', $event)"
      rows="2"
      autosize
      type="textarea"
      maxlength="50"
      placeholder="优质评论将会被优先展示"
      show-word-limit
    />
    <van-button
      type="primary"
      size="small"
      @click="$emit('click-post')"
    >发布</van-button>
  </div>
</template>

<script>
export default {
  name: 'PostComment',
  components: {},
  props: {
    value: {
      type: String,
      required: true
    }
  },
  data () {
    return {
      message: ''
    }
  },
  computed: {},
  watch: {},
  created () {},
  mounted () {},
  methods: {
    onInput (value) {
      this.$emit('input', value)
    }
  }
}
</script>

<style scoped lang="less">
.post-comment {
  display: flex;
  padding: 15px;
  align-items: flex-end;
  .post-field {
    background: #f5f7f9;
    margin-right: 15px;
  }
}
</style>



```

## 发布文章评论



步骤：

- 注册发布点击事件
- 请求提交表单
- 根据响应结果进行后续处理

一、使用弹层展示发布评论

1、添加弹层组件

```js
data () {
  return {
    ...
    isPostShow: false
  }
}


```

```html
<!-- 发布文章评论 -->
<van-popup
  v-model="isPostShow"
  position="bottom"
/>
<!-- /发布文章评论 -->


```

> 提示：不设置高度的时候，内容会自动撑开弹层高度

2、点击发评论按钮的时候显示弹层

```html
<van-button
  class="write-btn"
  type="default"
  round
  size="small"
  @click="isPostShow = true"
>写评论</van-button>


```

二、封装发布评论组件

1、创建 `post-comment.vue`

```html
<template>
  <div class="post-comment">
    <van-field
      class="post-field"
      v-model="message"
      rows="2"
      autosize
      type="textarea"
      maxlength="50"
      placeholder="优质评论将会被优先展示"
      show-word-limit
    />
    <van-button
      type="primary"
      size="small"
    >发布</van-button>
  </div>
</template>

<script>
export default {
  name: 'PostComment',
  components: {},
  props: {},
  data () {
    return {
      message: ''
    }
  },
  computed: {},
  watch: {},
  created () {},
  mounted () {},
  methods: {}
}
</script>

<style scoped lang="less">
.post-comment {
  display: flex;
  padding: 15px;
  align-items: flex-end;
  .post-field {
    background: #f5f7f9;
    margin-right: 15px;
  }
}
</style>



```

2、在详情页加载注册

3、在发布评论的弹层中使用

```html
<!-- 发布文章评论 -->
<van-popup
  v-model="isPostShow"
  position="bottom"
>
  <post-comment />
</van-popup>
<!-- /发布文章评论 -->


```

三、发布评论

1、在 `api/comment.js` 中添加封装数据接口

```js
/**
 * 添加评论或评论回复
 */
export function addComment(data) {
  return request({
    method: "POST",
    url: "/app/v1_0/comments",
    data
  });
}


```

2、绑定获取添加评论的输入框数据并且注册发布按钮的点击事件

```js
data () {
  return {
    ...
    inputComment: ''
  }
}


```

```html
<!-- 发布评论 -->
<van-cell-group class="publish-wrap">
  <van-field + v-model="inputComment" clearable placeholder="请输入评论内容">
    <van-button slot="button" size="mini" type="info" + @click="onAddComment"
      >发布</van-button
    >
  </van-field>
</van-cell-group>
<!-- /发布评论 -->


```

3、在事件处理函数中

```js
import {
	getComments,
+  addComment
} from '@/api/comment'


```

```js
async onAddComment () {
  const inputComment = this.inputComment.trim()

  // 非空校验
  if (!inputComment.length) {
    return
  }

  // 请求添加
  const res = await addComment({
    target: this.$route.params.articleId, // 评论的目标id（评论文章即为文章id，对评论进行回复则为评论id）
    content: inputComment // 评论内容
    // art_id // 文章id，对评论内容发表回复时，需要传递此参数，表明所属文章id。对文章进行评论，不要传此参数。
  })

  // 将发布的最新评论展示到列表顶部
  this.list.unshift(res.data.data.new_obj)

  // 清空文本框
  this.inputComment = ''
}


```

### 发布文章评论

1、封装数据接口

```js
/**
 * 发布评论
 */
export const addComment = data => {
  return request({
    method: 'POST',
    url: '/app/v1_0/comments',
    data
  })
}


```



2、给发布按钮点击事件

3、事件处理函数

```js
async onAddComment () {
  // 1. 拿到数据
  const postMessage = this.postMessage

  // 非空校验
  if (!postMessage) {
    return
  }

  this.$toast.loading({
    duration: 0, // 持续展示 toast
    message: '发布中...',
    forbidClick: true // 是否禁止背景点击
  })

  try {
    // 2. 请求提交
    const { data } = await addComment({
      target: this.articleId, // 评论的目标id（评论文章即为文章id，对评论进行回复则为评论id）
      content: postMessage
      // art_id: // 文章id，对评论内容发表回复时，需要传递此参数，表明所属文章id。对文章进行评论，不要传此参数。
    })

    // 关闭发布评论的弹层
    this.isPostShow = false

    // 将最新发布的评论展示到列表的顶部
    this.articleComment.list.unshift(data.data.new_obj)

    // 更新文章评论的总数量
    this.articleComment.totalCount++

    // 清空文本框
    this.postMessage = ''

    this.$toast.success('发布成功')
  } catch (err) {
    console.log(err)
    this.$toast.fail('发布失败')
  }
}


```



### 文章评论点赞

1、在 `api/comment.js` 中添加封装两个数据接口

```js
/**
 * 对评论或评论回复点赞
 */
export function addCommentLike(commentId) {
  return request({
    method: "POST",
    url: "/app/v1_0/comment/likings",
    data: {
      target: commentId
    }
  });
}

/**
 * 取消对评论或评论回复点赞
 */
export function deleteCommentLike(commentId) {
  return request({
    method: "DELETE",
    url: `/app/v1_0/comment/likings/${commentId}`
  });
}


```

2、然后给评论项中的 `like` 图标注册点击事件

```html
<van-icon
  slot="right-icon"
  color="red"
  +
  :name="item.is_liking ? 'like' : 'like-o'"
  +
  @click="onCommentLike(item)"
/>


```

3、在事件处理函数中

```js
import {
  getComments,
  addComment,
+  addCommentLike,
+  deleteCommentLike
} from '@/api/comment'


```

```js
async onCommentLike (comment) {
  // 如果已经赞了则取消点赞
  if (comment.is_liking) {
    await deleteCommentLike(comment.com_id)
  } else {
    // 如果没有赞，则点赞
    await addCommentLike(comment.com_id)
  }

  // 更新视图状态
  comment.is_liking = !comment.is_liking
  this.$toast('操作成功')
}


```

## 评论回复

### 展示回复弹层

一、在详情页中使用弹层用来展示文章的回复

1、在 data 中添加数据用来控制展示回复弹层的显示状态

```js
data () {
  return {
    ...
    isReplyShow: false
  }
}


```

2、在详情页中添加使用弹层组件

```html
<!-- 评论回复 -->
<van-popup
  v-model="isReplyShow"
  position="bottom"
  style="height: 95%"
>
  评论回复
</van-popup>
<!-- /评论回复 -->


```

二、当点击评论项组件中的回复按钮的时候展示弹层

1、在 `comment-item.vue` 组件中点击回复按钮的时候，对外发布自定义事件

```php+HTML
<van-button
  size="mini"
  type="default"
  @click="$emit('click-reply')"
>回复 {{ comment.reply_count }}</van-button>


```

2、在详情页组件中使用的位置监听处理

```html
<comment-item
  v-for="(comment, index) in articleComment.list"
  :key="index"
  :comment="comment"
  @click-reply="isReplyShow = true"
/>


```



### 布局

```html
<template>
  <div class="comment-reply">
    <!-- 导航栏 -->
    <van-nav-bar :title="`${comment.reply_count}条回复`">
      <van-icon
        slot="left"
        name="cross"
        @click="$emit('click-close')"
      />
    </van-nav-bar>
    <!-- /导航栏 -->

    <!-- 当前评论项 -->
    <!-- /当前评论项 -->

    <van-cell title="所有回复" />

    <!-- 评论的回复列表 -->
    <!-- /评论的回复列表 -->

    <!-- 底部 -->
    <!-- /底部 -->
  </div>
</template>

<script>
export default {
  name: 'CommentReply',
  components: {},
  props: {},
  data () {
    return {}
  },
  computed: {},
  watch: {},
  created () {},
  mounted () {},
  methods: {}
}
</script>

<style scoped></style>



```



### 展示点击回复的当前评论

一、让 `comment-reply.vue` 组件拿到点击回复的评论对象

1、在 `comment-item.vue` 组件中点击回复按钮的时候把评论对象给传出来

```html
<van-button
  size="mini"
  type="default"
  @click="$emit('click-reply', comment)"
>回复 {{ comment.reply_count }}</van-button>


```

2、在文章详情组件中接收处理

```js
data () {
  return {
    ...
    currentComment: {} // 点击回复的那个评论对象
  }
}


```

```html
<comment-item
  v-for="(comment, index) in articleComment.list"
  :key="index"
  :comment="comment"
  @click-reply="onReplyShow"
/>


```

```js
async onReplyShow (comment) {
  // 将子组件中传给我评论对象存储到当前组件
  this.currentComment = comment
  
  // 展示评论回复弹层
  this.isReplyShow = true
}


```

3、在详情组件中将 `currentComment` 传递给 `comment-reply.vue` 组件

```html
<!-- 评论回复 -->
<van-popup
  v-model="isReplyShow"
  position="bottom"
  style="height: 95%"
>
  <comment-reply
    @click-close="isReplyShow = false"
    :comment="currentComment"
  />
</van-popup>
<!-- /评论回复 -->


```

4、在 `comment-reply.vue` 组件中声明接收

```js
props: {
  comment: {
    type: Object,
    required: true
  }
},


```

最后使用调试工具查看 props 数据是否接收正确。

二、在 `comment-reply.vue` 组件中展示当前评论

1、加载注册 `comment-item.vue` 组件

2、使用展示

```html
<template>
  <div class="comment-reply">
    <!-- 导航栏 -->
    <van-nav-bar :title="`${comment.reply_count}条回复`">
      <van-icon
        slot="left"
        name="cross"
        @click="$emit('click-close')"
      />
    </van-nav-bar>
    <!-- /导航栏 -->

    <!-- 当前评论项 -->
    <comment-item :comment="comment" />
    <!-- /当前评论项 -->

    <!-- 评论的回复列表 -->
    <!-- /评论的回复列表 -->

    <!-- 底部 -->
    <!-- /底部 -->
  </div>
</template>


```





一：把点击回复的评论对象传递给评论回复组件

1、在 data 中添加一个数据用来存储点击回复的评论对象

```js
data () {
  return {
    ...
    currentComment: {} // 存储当前点击回复的评论对象
  }
}


```

2、在点击回复的处理函数中评论对象存储到数据中

```js
async onReplyShow (comment) {
+  this.currentComment = comment
  // 显示回复的弹层
  this.isReplyShow = true
}


```

3、把当前组件的 `currentComment` 传递给评论回复组件

```html
<!-- 评论回复 -->
<van-popup
  v-model="isReplyShow"
  get-container="body"
  round
  position="bottom"
  :style="{ height: '90%' }"
>
  <!-- 回复列表 -->
  + <comment-reply :comment="currentComment" />
  <!-- /回复列表 -->
</van-popup>
<!-- 评论回复 -->


```

4、在评论回复组件中声明 `props` 接收数据

```js
props: {
  comment: {
    type: Object,
    required: true
  }
},


```

测试：点击不同的评论回复按钮，查看子组件中的 props 数据 `comment` 是否是当前点击回复所在的评论对象。

二、数据绑定：在评论回复组件中展示当前评论

```html
<!-- 导航栏 -->
+
<van-nav-bar :title="comment.reply_count + '条回复'">
  <van-icon slot="left" name="cross" />
</van-nav-bar>
<!-- /导航栏 -->

<!-- 当前评论 -->
<van-cell title="当前评论">
  <van-image
    slot="icon"
    round
    width="30"
    height="30"
    style="margin-right: 10px;"
    :src="comment.aut_photo"
  />
  + <span style="color: #466b9d;" slot="title">{{ comment.aut_name }}</span>
  <div slot="label">
    +
    <p style="color: #363636;">{{ comment.content }}</p>
    <p>
      +
      <span style="margin-right: 10px;"
        >{{ comment.pubdate | relativeTime }}</span
      >
      <van-button size="mini" type="default" +
        >回复 {{ comment.reply_count }}</van-button
      >
    </p>
  </div>
  <van-icon slot="right-icon" />
</van-cell>
<!-- /当前评论 -->


```

### 展示评论回复列表

```html
<template>
  <div class="comment-reply">
    <!-- 导航栏 -->
    <van-nav-bar :title="`${comment.reply_count}条回复`">
      <van-icon
        slot="left"
        name="cross"
        @click="$emit('click-close')"
      />
    </van-nav-bar>
    <!-- /导航栏 -->

    <!-- 当前评论项 -->
    <comment-item :comment="comment" />
    <!-- /当前评论项 -->

    <van-cell title="所有回复" />

    <!-- 评论的回复列表 -->
    <van-list
      v-model="loading"
      :finished="finished"
      finished-text="没有更多了"
      @load="onLoad"
    >
      <comment-item
        v-for="(comment, index) in list"
        :key="index"
        :comment="comment"
      />
    </van-list>
    <!-- /评论的回复列表 -->

    <!-- 底部 -->
    <!-- /底部 -->
  </div>
</template>

<script>
import CommentItem from './comment-item'
import { getComments } from '@/api/comment'

export default {
  name: 'CommentReply',
  components: {
    CommentItem
  },
  props: {
    comment: {
      type: Object,
      required: true
    }
  },
  data () {
    return {
      list: [],
      loading: false,
      finished: false,
      offset: null,
      limit: 20
    }
  },
  computed: {},
  watch: {},
  created () {},
  mounted () {},
  methods: {
    async onLoad () {
      // 1. 请求获取数据
      const { data } = await getComments({
        type: 'c', // 评论类型，a-对文章(article)的评论，c-对评论(comment)的回复
        source: this.comment.com_id.toString(), // 源id，文章id或评论id
        offset: this.offset, // 获取评论数据的偏移量，值为评论id，表示从此id的数据向后取，不传表示从第一页开始读取数据
        limit: this.limit // 获取的评论数据个数，不传表示采用后端服务设定的默认每页数据量
      })

      // 2. 将数据添加到列表中
      const { results } = data.data
      this.list.push(...results)

      // 3. 关闭 loading
      this.loading = false

      // 4. 判断数据是否加载完毕
      if (results.length) {
        this.offset = data.data.last_id
      } else {
        this.finished = true
      }
    }
  }
}
</script>

<style scoped></style>



```



### 解决弹层中组件内容不更新问题

弹层组件：

- 如果初始的条件是 false，则弹层的内容不会渲染
- 程序运行期间，当条件变为 true 的时候，弹层才渲染了内容
- 之后切换弹层的展示，弹层只是通过 CSS 控制隐藏和显示

弹层渲染出来以后就只是简单的切换显示和隐藏，里面的内容也不再重新渲染了，所以会导致我们的评论的回复列表不会动态更新了。解决办法就是在每次弹层显示的时候重新渲染组件。

```html
<!-- 评论回复 -->
<van-popup
  v-model="isReplyShow"
  get-container="body"
  round
  position="bottom"
  :style="{ height: '90%' }"
>
  <!-- 回复列表 -->
  <comment-reply :comment="currentComment" + v-if="isReplyShow" />
  <!-- /回复列表 -->
</van-popup>
<!-- 评论回复 -->


```

### 在评论回复组件中关闭弹层

1、在评论回复组件中发布一个自定义事件

```html
<!-- 导航栏 -->
<van-nav-bar :title="comment.reply_count + '条回复'">
  <van-icon slot="left" name="cross" @click="$emit('close-reply')" />
</van-nav-bar>
<!-- /导航栏 -->


```

2、在文章评论组件中监听处理

```html
<!-- 评论回复 -->
<van-popup
  v-model="isReplyShow"
  get-container="body"
  round
  position="bottom"
  :style="{ height: '90%' }"
>
  <!-- 回复列表 -->
  <comment-reply
    :comment="currentComment"
    v-if="isReplyShow"
    +
    @close-reply="isReplyShow = false"
  />
  <!-- /回复列表 -->
</van-popup>
<!-- 评论回复 -->

```

### 发布回复

# 十一、编辑用户资料

{% img https://yhx0507.github.io/wxapp_static/app/1566431661910.png %}

## 准备

### 创建组件并配置路由

1、创建 `views/user/index.vue`

```html
<template>
  <div>
    <van-nav-bar title="个人信息" left-arrow right-text="保存" />
    <van-cell-group>
      <van-cell title="头像" is-link>
        <van-image
          round
          width="30"
          height="30"
          fit="cover"
          src="http://toutiao.meiduo.site/FgSTA3msGyxp5-Oufnm5c0kjVgW7"
        />
      </van-cell>
      <van-cell title="昵称" value="abc" is-link />
      <van-cell title="性别" value="男" is-link />
      <van-cell title="生日" value="2019-9-27" is-link />
    </van-cell-group>
  </div>
</template>

<script>
  export default {
    name: "UserIndex"
  };
</script>

```

2、将该页面配置到根路由

```js
{
  name: 'user-profile',
  path: '/user/profile',
  component: () => import('@/views/user-profile')
}

```



## 展示用户资料

思路：

- 请求获取数据
- 模板绑定

1、在 `api/user.js` 中添加封装数据接口

```js
// 获取用户资料
export const updateUserPhoto = data => {
  return request({
    method: 'PATCH',
    url: '/app/v1_0/user/photo',
    data
  })
}

```

2、在 `views/user/index.vue` 组件中请求获取数据

```js
import { getUserProfile } from '@/api/user'

export default {
  name: 'UserProfile',
  components: {},
  props: {},
  data () {
    return {
      user: {}, // 用户资料
      isPreviewShow: false,
      images: [] // 预览的图片列表
    }
  },
  computed: {},
  watch: {},
  created () {
    this.loadUserProfile()
  },
  mounted () {},
  methods: {
    async loadUserProfile () {
      try {
        const { data } = await getUserProfile()
        this.user = data.data
      } catch (err) {
        console.log(err)
        this.$toast.fail('获取数据失败')
      }
    }
  }
}

```

3、模板绑定

## 修改头像

思路：

一、点击选择文件

二、图片预览

三、保存修改

### 点击选择文件

1、在模板中添加一个 file 类型的 input

```html
<input ref="file" type="file" hidden>

```

> 提示
>
> 如果是移动端，则会提示用户拍照、照片、文件

2、当点击头像的时候手动调用 input 的 click 点击事件

```html
<van-cell is-link title="头像" @click="onAvatarClick">
  <van-image
    class="avatar"
    round
    :src="user.photo"
  />
</van-cell>

```

```js
onAvatarClick () {
  // 手动触发 input 的点击事件
  this.$refs['file'].click()
},

```

最后测试。

### 图片上传预览

方式一：结合服务器的图片上传预览

- 优点：兼容好
- 缺点：麻烦，需要结合服务端

{% img https://yhx0507.github.io/wxapp_static/app/1567067894388.png %}

方式二：纯客户端实现上传图片预览

- 优点：简单方便
- 缺点：兼容性

```js
// 获取文文件对象
const file = file类型input.files[0]

// 设置图片的 src
img.src = window.URL.createObjectURL(file)


```



下面我们使用纯客户端的方式处理用户头像上传预览。

1、在模板中添加预览组件

```html
<!-- 头像预览 -->
<van-image-preview v-model="isPreviewShow" :images="images">
  <van-nav-bar
    slot="cover"
    left-text="取消"
    right-text="确定"
    @click-left="isPreviewShow = false"
    @click-right="onUpdateAvatar"
  />
</van-image-preview>
<!-- /头像预览 -->


```

```js
data {
  ...
  isPreviewShow: false, // 是否显示图片预览
  images: [] // 预览的图片列表
}


```

2、给 file-input 注册 change 事件

```html
<input ref="file" type="file" hidden @change="onFileChange">


```

change 事件只有在用户所选的文件发生变化之后会出触发，如果选择了同一个文件不会触发。所以为了解决该问题，我们可以在图片预览关闭的时候，手动将 file-input 的 value 清除。

```html
<!-- 头像预览 -->
    <van-image-preview v-model="isPreviewShow" :images="images" @close="$refs.file.value = ''">
      <van-nav-bar
        slot="cover"
        left-text="取消"
        right-text="确定"
        @click-left="isPreviewShow = false"
        @click-right="onUpdateAvatar"
      />
    </van-image-preview>
    <!-- /头像预览 -->


```



3、在事件处理函数中

```js
onFileChange () {
  // 1. 拿到 file 类型 input 选择的文件对象
  const fileObj = this.file.files[0]

  // 2. 使用 window.URL.createObjectURL(file) 得到文件数据
  const fileData = window.URL.createObjectURL(fileObj)

  // 3. 将 img.src = 第2步的结果
  this.images = [fileData] // 这里直接重置数组，防止出现多个预览图片
  this.isPreviewShow = true // 显示图片预览
},


```

最后测试。

### 保存更新

1、在 `api/user.js` 中添加封装数据接口

```js
/**
 * 更新用户头像
 */
export function updateUserPhoto (data) {
  return request({
    method: 'PATCH',
    url: '/app/v1_0/user/photo',
    data
  })
}



```

> 总结：
>
> - 如果 `Content-Type` 是 `application/json`，则传递普通 JavaScript 对象，例如 `{}`
> - 如果 `Content-Type` 是 `multipart/form-data`，则传递 [FormData](https://developer.mozilla.org/zh-CN/docs/Web/API/FormData) 对象，这种情况常见于文件上传的数据接口。

2、给图片预览中的确定注册点击事件

3、在事件处理函数中

```js
async onUpdateAvatar () {
  // 1. 构造包含文件数据的表单对象 FormData
  // 注意：含有文件的数据务必要放到 FormData 中
  const fd = new FormData()
  fd.append('photo', this.file.files[0])

  this.$toast.loading({
    duration: 0, // 持续展示 toast
    message: '保存中...',
    forbidClick: true // 是否禁止背景点击
  })

  // 2. 请求提交
  try {
    const { data } = await updateUserPhoto(fd)

    // 更新页面
    this.user.photo = data.data.photo

    // 关闭图片预览
    this.isPreviewShow = false

    this.$toast.success('更新成功')
  } catch (err) {
    console.log(err)
    this.$toast.fail('更新失败')
  }

  // 3. 根据响应结果执行后续处理
}


```



## 修改昵称

### 布局

### 更新

1、封装 API 请求方法

```js
// 更新用户资料
export const updateUserProfile = data => {
  return request({
    method: 'PATCH',
    url: '/app/v1_0/user/profile',
    data
  })
}


```



2、封装业务请求(更新用户昵称、性别、生日等都使用该方法)

```js
// field: 要修改的数据字段
// value：数据值
async updateUserProfile (field, value) {
  this.$toast.loading({
    duration: 0, // 持续展示 toast
    message: '更新中...',
    forbidClick: true // 是否禁止背景点击
  })

  try {
    await updateUserProfile({
      [field]: value // 注意属性名使用中括号包裹，否则会当做字符串来使用而不是变量
    })
    this.$toast.success('更新成功')
  } catch (err) {
    console.log(err)
    this.$toast.fail('更新失败')
  }
},


```



3、注册确定按钮点击事件

4、在事件处理函数中

```js
async onUpdateName () {
  // 请求提交表单
  await this.updateUserProfile('name', this.inputName)

  // 更新视图
  this.user.name = this.inputName

  // 关闭弹层
  this.isEditNameShow = false
}

```



## 修改性别

## 修改生日

# 十二、我的收藏/历史/作品

{% img https://yhx0507.github.io/wxapp_static/app/image-20200203094246747.png %}

## 准备

## 我的收藏

## 我的历史

## 我的作品



# 十三、小智同学

{% img https://yhx0507.github.io/wxapp_static/app/1566431856572.png %}

## WebSocket

{% img https://yhx0507.github.io/wxapp_static/app/bg2017051501.png %}

- WebSocket 是一种数据通信协议，类似于我们常见的 http
- 既然有 http，为啥还要 WebSocket
- http 通信是单向的
  - 请求 + 响应
  - 没有请求也就没有响应

初次接触 WebSocket 的人，都会问同样的问题：我们已经有了 HTTP 协议，为什么还需要另一个协议？它能带来什么好处？

答案很简单，因为 HTTP 协议有一个缺陷：通信只能由客户端发起。

举例来说，我们想了解今天的天气，只能是客户端向服务器发出请求，服务器返回查询结果。HTTP 协议做不到服务器主动向客户端推送信息。

这种单向请求的特点，注定了如果服务器有连续的状态变化，客户端要获知就非常麻烦。我们只能使用["轮询"](https://www.pubnub.com/blog/2014-12-01-http-long-polling/)：每隔一段时候，就发出一个询问，了解服务器有没有新的信息。最典型的场景就是聊天室。

轮询的效率低，非常浪费资源（因为必须不停连接，或者 HTTP 连接始终打开）。因此，工程师们一直在思考，有没有更好的方法。WebSocket 就是这样发明的。

WebSocket 协议在 2008 年诞生，2011 年成为国际标准。所有浏览器都已经支持了。

它的最大特点就是，**服务器可以主动向客户端推送信息**，**客户端也可以主动向服务器发送信息**，是真正的**双向平等对话**，属于[服务器推送技术](https://en.wikipedia.org/wiki/Push_technology)的一种。

简单理解：

HTTP 打电话：

- 我问一句
- 你回答一句
- 没有提问就没有回答，即便对方主动给你说话，我也是个聋子听不见

WebSocket 打电话：

- 双向对话

{% img https://yhx0507.github.io/wxapp_static/app/bg2017051502.png %}

> HTTP 和 WebSocket 通信模型

其他特点包括：

（1）建立在 TCP 协议之上，服务器端的实现比较容易。

（2）与 HTTP 协议有着良好的兼容性。默认端口也是 80 和 443，并且握手阶段（第 1 次建立连接）采用 HTTP 协议，因此握手时不容易屏蔽，能通过各种 HTTP 代理服务器。

（3）数据格式比较轻量，性能开销小，通信高效。

（4）可以发送文本，也可以发送二进制数据。

（5）没有同源跨域限制，客户端可以与任意服务器通信。

（6）协议标识符是`ws`（如果加密，则为`wss`），服务器网址就是 URL。

（7）浏览器专门为 WebSocket 通信提供了一个请求对象 `WebSocket`

```
ws://example.com:80/some/path

```

{% img https://yhx0507.github.io/wxapp_static/app/bg2017051503.jpg %}

## 使用原生 WebSocket（了解）

> 参考文档：
>
> - [MDN - WebSocket](https://developer.mozilla.org/zh-CN/docs/Web/API/WebSocket)

浏览器为 HTTP 通信提供了 `XMLHttpRequest` 对象，同样的，也为 WebSocket 通信提供了一个通信操作接口：`WebSocket`。

通信模型：

- 拨号（建立连接）
- 通话（双向通信）
- 结束通话（关闭连接）

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <title>Document</title>
  </head>
  <body>
    <script>
      // WebSocet 通信模型

      // 1. 拨打电话（建立连接）
      // 注意：wss://echo.websocket.org 是一个在线的测试接口，仅用于 WebSocket 协议通信测试使用
      var ws = new WebSocket("wss://echo.websocket.org");

      // 当连接建立成功，触发 open 事件
      ws.onopen = function(evt) {
        console.log("建立连接成功 ...");

        // 连接建立成功以后，就可以使用这个连接对象通信了
        // send 方法发送数据
        ws.send("Hello WebSockets!");
      };

      // 当接收到对方发送的消息的时候，触发 message 事件
      // 我们可以通过回调函数的 evt.data 获取对方发送的数据内容
      ws.onmessage = function(evt) {
        console.log("接收到消息: " + evt.data);

        // 当不需要通信的时候，可以手动的关闭连接
        // ws.close();
      };

      // 当连接断开的时候触发 close 事件
      ws.onclose = function(evt) {
        console.log("连接已关闭.");
      };
    </script>
  </body>
</html>

```

怎么查看 WebSocket 请求日志：

{% img https://yhx0507.github.io/wxapp_static/app/1569575005360.png %}

> 朝上的绿色箭头是发出去的消息
>
> 朝下的红色箭头是收到的消息

## Socket.IO（了解体验）

### 介绍

- 原生的 WebSocket 使用比较麻烦，所以推荐使用一个封装好的解决方案：socket.io
- socket.io 提供了服务端 + 客户端的实现
  - 客户端：浏览器
  - 服务端：Java、Python、PHP、。。。。Node.js
- 对于前端开发者来说，只需要关心她的客户端如何使用即可
- 注意：Socket.io 必须前后端配套使用
  - 实际工作中，socket.io 已经成为了各大后端开发的 WebSocket 通信主流解决方案
- [GitHub 仓库](https://github.com/socketio/socket.io)
- [官网](https://socket.io/)

Socket.io 和 WebSocket 就好比它就好比 axios 和 XMLHTTPRequest 的关系。

### 基本使用

官方的 Hello World：https://socket.io/get-started/chat/。

服务端代码：

```js
var app = require("express")();
var http = require("http").createServer(app);
var io = require("socket.io")(http);

app.get("/", function(req, res) {
  res.sendFile(__dirname + "/index.html");
});

io.on("connection", function(socket) {
  socket.on("disconnect", function() {
    console.log("user disconnected");
  });

  socket.on("chat message", function(msg) {
    io.emit("chat message", msg);
  });
});

http.listen(3000, "0.0.0.0", function() {
  console.log("listening on *:3000");
});

```

客户端代码：

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Socket.IO chat</title>
    <style>
      * {
        margin: 0;
        padding: 0;
        box-sizing: border-box;
      }
      body {
        font: 13px Helvetica, Arial;
      }
      form {
        background: #000;
        padding: 3px;
        position: fixed;
        bottom: 0;
        width: 100%;
      }
      form input {
        border: 0;
        padding: 10px;
        width: 90%;
        margin-right: 0.5%;
      }
      form button {
        width: 9%;
        background: rgb(130, 224, 255);
        border: none;
        padding: 10px;
      }
      #messages {
        list-style-type: none;
        margin: 0;
        padding: 0;
      }
      #messages li {
        padding: 5px 10px;
      }
      #messages li:nth-child(odd) {
        background: #eee;
      }
    </style>
  </head>
  <body>
    <!-- 消息列表 -->
    <ul id="messages"></ul>

    <!-- 发送消息的表单 -->
    <form action="">
      <input id="m" autocomplete="off" /><button>Send</button>
    </form>

    <!-- SocketIO 提供了一个客户端实现：socket.io.js -->
    <script src="/socket.io/socket.io.js"></script>
    <script src="https://code.jquery.com/jquery-1.11.1.js"></script>
    <script>
      // 建立连接，得到 socket 通信对象
      var socket = io();

      socket.on("connect", () => {
        console.log("建立连接成功了");
      });

      $("form").submit(function(e) {
        e.preventDefault(); // prevents page reloading
        socket.emit("chat message", $("#m").val());
        $("#m").val("");
        return false;
      });

      socket.on("chat message", function(msg) {
        $("#messages").append($("<li>").text(msg));
      });
    </script>
  </body>
</html>

```

### 扩展：ngrok 代理

https://lipengzhou.com/posts/9c28d833/

### 总结

我们只需要关注客户端怎么使用：

- 怎么建立连接
- 怎么发送消息
- 怎么接收消息

建立连接：

```js
const socket = io("连接地址");

```

发送数据：

```js
// 发送指定类型的消息
// 数据可以是任何类型
socket.emit("消息类型", 数据);

```

接收消息：

```js
// 消息类型
// 回调函数参数获取消息数据
socket.on("消息类型", data => {
  console.log(data);
});

```

> 消息类型由后端给出，他会告诉你收发消息的类型，就好比 HTTP 接口中的请求路径一样。

## 小智同学

### 准备

创建 `chat/index.vue`：

```html
<template>
  <div>小智同学</div>
</template>

<script>
  export default {
    name: "ChatIndex"
  };
</script>

<style></style>

```

然后配置路由：

```js
...
import Chat from '@/views/chat'

Vue.use(VueRouter)

const router = new VueRouter({
  // 配置路由表
  routes: [
    ...
    {
      name: 'user-chat',
      path: '/user/chat',
      component: Chat
    }
  ]
})

export default router



```

最后，在我的页面中点击 “小智同学” 跳转到聊天页面：

```html
<!-- 其它 -->
<van-cell-group>
  <van-grid clickable>
    <van-grid-item icon="star" text="我的收藏" />
    <van-grid-item icon="chat" text="我的评论" />
    <van-grid-item icon="like" text="我的点赞" />
    <van-grid-item icon="browsing-history" text="浏览历史" />
  </van-grid>
</van-cell-group>
<van-cell-group>
  <van-cell title="消息通知" is-link />
  <van-cell title="用户反馈" is-link />
  + <van-cell title="小智同学" is-link @click="$router.push('/chat')" />
  <van-cell title="系统设置" is-link to="/settings" />
</van-cell-group>
<!-- /其它 -->


```

### 布局

```html
<template>
  <div class="chat-container">
    <!-- 导航栏 -->
    <van-nav-bar
      title="小智同学"
      left-arrow
      @click-left="$router.back()"
      fixed
    />
    <!-- /导航栏 -->

    <!-- 消息列表 -->
    <div class="message-list" ref="message-list">
      <div
        class="message-item"
        :class="{ reverse: item % 3 === 0 }"
        v-for="item in 20"
        :key="item"
      >
        <van-image
          class="avatar"
          slot="icon"
          round
          width="30"
          height="30"
          src="https://img.yzcdn.cn/vant/cat.jpeg"
        />
        <div class="title">
          <span>{{ `hello${item}` }}</span>
        </div>
      </div>
    </div>
    <!-- /消息列表 -->

    <!-- 发送消息 -->
    <van-cell-group class="send-message">
      <van-field v-model="message" center clearable>
        <van-button slot="button" size="small" type="primary">发送</van-button>
      </van-field>
    </van-cell-group>
    <!-- /发送消息 -->
  </div>
</template>

<script>
  export default {
    data() {
      return {
        message: ""
      };
    },

    mounted() {
      window.list = this.$refs["message-list"];
    }
  };
</script>

<style scoped lang="less">
  .chat-container {
    position: absolute;
    width: 100%;
    height: 100%;
    padding: 46px 0 50px 0;
    top: 0;
    left: 0;
    box-sizing: border-box;
    background: #f5f5f6;
    .message-list {
      height: 100%;
      overflow-y: scroll;
      .message-item {
        display: flex;
        align-items: center;
        padding: 10px;
        .title {
          background: #fff;
          padding: 5px;
          border-radius: 6px;
        }
        .avatar {
          margin-right: 5px;
        }
      }
      .reverse {
        flex-direction: row-reverse;
        .title {
          margin-right: 5px;
        }
      }
    }

    .send-message {
      position: fixed;
      bottom: 0;
      left: 0;
      right: 0;
      background: #f5f5f6 !important;
    }
  }
</style>


```

```html
<template>
  <div class="page-user-chat">
    <van-nav-bar
      fixed
      left-arrow
      @click-left="$router.back()"
      title="小智同学"
    ></van-nav-bar>
    <div class="chat-list" ref="list">
      <div
        class="chat-item"
        :class="{left:item.name==='xz',right:item.name==='self'}"
        v-for="(item,i) in list"
        :key="i"
      >
        <van-image v-if="item.name==='xz'" fit="cover" round :src="xzAvatar" />
        <div class="chat-pao">{{item.msg}}</div>
        <van-image
          v-if="item.name==='self'"
          fit="cover"
          round
          src="https://img.yzcdn.cn/vant/cat.jpeg"
        />
      </div>
    </div>
    <div class="reply-container van-hairline--top">
      <van-field v-model="value" placeholder="说点什么...">
        <van-loading
          v-if="commentLoading"
          slot="button"
          type="spinner"
          size="16px"
        ></van-loading>
        <span
          v-else
          @click="send()"
          slot="button"
          style="font-size:12px;color:#999"
          >提交</span
        >
      </van-field>
    </div>
  </div>
</template>

<script>
  import xzAvatar from "@/assets/images/xz.png";
  import io from "socket.io-client";
  import { userLocal } from "@/utils/local";
  export default {
    data() {
      return {
        value: "",
        list: null,
        xzAvatar,
        commentLoading: false
      };
    },
    created() {
      this.list = [];
      this.socket = io("http://ttapi.research.itcast.cn", {
        query: {
          token: userLocal.getUser().token
        }
      });
      this.socket.on("connect", () => {
        // 建了链接后默认  小智给你打招呼
        this.list.push({ name: "xz", msg: "你好" });
      });
      this.socket.on("message", data => {
        // 接受机器人消息
        this.list.push({ name: "xz", msg: data.msg });
        this.scrollBottom();
      });
    },
    deactivated() {
      this.socket.close();
    },
    methods: {
      scrollBottom() {
        this.$nextTick(() => {
          this.$refs.list.scrollTop = this.$refs.list.scrollHeight;
        });
      },
      send() {
        this.socket.emit("message", { msg: this.value, timestamp: Date.now() });
        this.list.push({ name: "self", msg: this.value });
        this.value = "";
        this.scrollBottom();
      }
    }
  };
</script>

<style scoped lang="less">
  .page-user-chat {
    height: 100%;
    width: 100%;
    position: absolute;
    left: 0;
    top: 0;
    box-sizing: border-box;
    padding: 46px 0 50px 0;
    .chat-list {
      height: 100%;
      overflow-y: scroll;
      .chat-item {
        padding: 10px;
        .van-image {
          vertical-align: top;
        }
        .chat-pao {
          vertical-align: top;
          display: inline-block;
          min-width: 20px;
          max-width: 60%;
          min-height: 40px;
          line-height: 40px;
          border: 0.5px solid #c2d9ea;
          border-radius: 4px;
          position: relative;
          padding: 0 10px;
          background-color: #e0effb;
          word-break: break-all;
          font-size: 14px;
          color: #333;
          &::before {
            content: "";
            width: 10px;
            height: 10px;
            position: absolute;
            top: 13px;
            border-top: 1px solid #c2d9ea;
            border-right: 1px solid #c2d9ea;
            background: #e0effb;
          }
        }
      }
    }
  }
  .chat-item.right {
    text-align: right;
    .chat-pao {
      margin-left: 0;
      margin-right: 15px;
      &::before {
        right: -6px;
        transform: rotate(45deg);
      }
    }
  }
  .chat-item.left {
    text-align: left;
    .chat-pao {
      margin-left: 15px;
      margin-right: 0;
      &::before {
        left: -6px;
        transform: rotate(-135deg);
      }
    }
  }
  .van-image {
    width: 40px;
    height: 40px;
  }
  .reply-container {
    position: fixed;
    left: 0;
    bottom: 0;
    height: 44px;
    width: 100%;
    background: #f5f5f5;
    z-index: 9999;
  }
</style>


```

### 建立连接

步骤：

- 安装
  - https://github.com/socketio/socket.io-client
- 导入
- 建立连接

1、安装依赖

```sh
# yarn add socket.io-client
npm i socket.io-client


```

2、在初始化的时候建立连接

```js
import io from "socket.io-client";

export default {
  data() {
    return {
      message: ""
    };
  },

  created() {
    // 建立连接
    const socket = io("http://ttapi.research.itcast.cn");

    // 当客户端与服务器建立连接成功，触发 connect 事件
    socket.on("connect", () => {
      console.log("建立连接成功！");
    });
  }
};


```

### 收发消息

```js
import io from 'socket.io-client'

export default {
  data () {
    return {
      message: '',
+      socket: null
    }
  },

  created () {
    // 建立连接
    const socket = io('http://ttapi.research.itcast.cn')

    // 把 socket 存储到 data 中，然后就可以在 methods 中访问到了
+    this.socket = socket

    // 当客户端与服务器建立连接成功，触发 connect 事件
    socket.on('connect', () => {
      console.log('建立连接成功！')
    })

    // 监听接收服务端消息
+    socket.on('message', data => {
+      console.log('收到服务器消息：', data)
+    })
  },

  methods: {
+    onSendMessage () {
+      const message = this.message.trim()
+      if (!message) {
+        return
+      }
+
+      // 发送消息
+      this.socket.emit('message', {
+        msg: message,
+        timestamp: Date.now()
+      })
    }
  }
}


```

### 展示消息列表

```html
<template>
  <div class="chat-container">
    <!-- 导航栏 -->
    <van-nav-bar
      title="小智同学"
      left-arrow
      @click-left="$router.back()"
      fixed
    />
    <!-- /导航栏 -->

    <!-- 消息列表 -->
    <div class="message-list" ref="message-list">
      <div
        class="message-item"
        +
        :class="{ reverse: item.isMe }"
        +
        v-for="(item, index) in messages"
        +
        :key="index"
      >
        <van-image
          class="avatar"
          slot="icon"
          round
          width="30"
          height="30"
          +
          :src="item.photo"
        />
        <div class="title">+ <span>{{ item.message }}</span></div>
      </div>
    </div>
    <!-- /消息列表 -->

    <!-- 发送消息 -->
    <van-cell-group class="send-message">
      <van-field v-model="message" center clearable>
        <van-button
          slot="button"
          size="small"
          type="primary"
          @click="onSendMessage"
          >发送</van-button
        >
      </van-field>
    </van-cell-group>
    <!-- /发送消息 -->
  </div>
</template>

<script>
  import io from 'socket.io-client'

  export default {
    name: 'ChatIndex',
    data () {
      return {
        message: '',
        socket: null,
        // [ { message: '消息数据', isMe: true, photo: '头像' }, ]
  +      messages: [] // 存储所有的消息列表
      }
    },

    created () {
      // 建立连接
      const socket = io('http://ttapi.research.itcast.cn')

      // 把 socket 存储到 data 中，然后就可以在 methods 中访问到了
      this.socket = socket

      // 当客户端与服务器建立连接成功，触发 connect 事件
      socket.on('connect', () => {
        console.log('建立连接成功！')
      })

      // 监听接收服务端消息
      socket.on('message', data => {
        console.log('收到服务器消息：', data)
  +      this.messages.push({
  +        message: data.msg,
  +        isMe: false,
  +        photo: 'http://toutiao.meiduo.site/FkBUsGwtrHKjoF0NPLzeilckol1-'
  +      })
      })
    },

    methods: {
      onSendMessage () {
        const message = this.message.trim()
        if (!message) {
          return
        }

        // 发送消息
        this.socket.emit('message', {
          msg: message,
          timestamp: Date.now()
        })

        // 把消息存储到数组中
  +      this.messages.push({
  +        message,
  +        isMe: true,
  +        photo: 'https://img.yzcdn.cn/vant/cat.jpeg'
  +      })

        // 清空文本框
        this.message = ''
      }
    }
  }
</script>


```

### 数据持久化

```js
import io from 'socket.io-client'
+ import { getItem, setItem } from '@/utils/storage'

export default {
  name: 'ChatIndex',
  data () {
    return {
      message: '',
      socket: null,
      // [ { message: '消息数据', isMe: true, photo: '头像' }, ]
+      messages: getItem('chat-messages') || []
    }
  },

+  watch: {
+    messages (newValue) {
+      setItem('chat-messages', newValue)
+    }
+  },

  created () {
    // 建立连接
    const socket = io('http://ttapi.research.itcast.cn')

    // 把 socket 存储到 data 中，然后就可以在 methods 中访问到了
    this.socket = socket

    // 当客户端与服务器建立连接成功，触发 connect 事件
    socket.on('connect', () => {
      console.log('建立连接成功！')
    })

    // 监听接收服务端消息
    socket.on('message', data => {
      console.log('收到服务器消息：', data)
      this.messages.push({
        message: data.msg,
        isMe: false,
        photo: 'http://toutiao.meiduo.site/FkBUsGwtrHKjoF0NPLzeilckol1-'
      })
    })
  },

  methods: {
    onSendMessage () {
      const message = this.message.trim()
      if (!message) {
        return
      }

      // 发送消息
      this.socket.emit('message', {
        msg: message,
        timestamp: Date.now()
      })

      // 把消息存储到数组中
      this.messages.push({
        message,
        isMe: true,
        photo: 'https://img.yzcdn.cn/vant/cat.jpeg'
      })

      // 清空文本框
      this.message = ''
    }
  }
}


```

### 消息列表自动滚动到底部

公式：DOM.scrollTop = DOM.scrollHeight

```js
import io from 'socket.io-client'
import { getItem, setItem } from '@/utils/storage'

export default {
  name: 'ChatIndex',
  data () {
    return {
      message: '',
      socket: null,
      // [ { message: '消息数据', isMe: true, photo: '头像' }, ]
      messages: getItem('chat-messages') || []
    }
  },

  watch: {
    messages (newValue) {
      setItem('chat-messages', newValue)

      // 让列表滚动到最底部
+      const messageList = this.$refs['message-list']
+
+      // 这里需要把操作 DOM 的这个代码放到 $nextTick 中
+      // 为啥？明天再说
+      this.$nextTick(() => {
+        messageList.scrollTop = messageList.scrollHeight
+      })
    }
  },

  created () {
    // 建立连接
    const socket = io('http://ttapi.research.itcast.cn')

    // 把 socket 存储到 data 中，然后就可以在 methods 中访问到了
    this.socket = socket

    // 当客户端与服务器建立连接成功，触发 connect 事件
    socket.on('connect', () => {
      console.log('建立连接成功！')
    })

    // 监听接收服务端消息
    socket.on('message', data => {
      console.log('收到服务器消息：', data)
      this.messages.push({
        message: data.msg,
        isMe: false,
        photo: 'http://toutiao.meiduo.site/FkBUsGwtrHKjoF0NPLzeilckol1-'
      })
    })
  },

+  mounted () {
+    const messageList = this.$refs['message-list']
+    messageList.scrollTop = messageList.scrollHeight
+  },

  methods: {
    onSendMessage () {
      const message = this.message.trim()
      if (!message) {
        return
      }

      // 发送消息
      this.socket.emit('message', {
        msg: message,
        timestamp: Date.now()
      })

      // 把消息存储到数组中
      this.messages.push({
        message,
        isMe: true,
        photo: 'https://img.yzcdn.cn/vant/cat.jpeg'
      })

      // 清空文本框
      this.message = ''
    }
  }
}


```

### $nextTick 的作用

```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
</head>
<body>
	<div id="app">
		<h1 ref="message">{{ message }}</h1>
		<button @click="changeMessage">点击改变 message</button>
	</div>
	<script src="https://cn.vuejs.org/js/vue.js"></script>
	<script>
		new Vue({
			el: '#app',
			data: {
				message: 'hello'
			},
			methods: {
				changeMessage () {
					// 修改数据（响应式），数据改变会影响视图更新
					// 注意：数据影响视图不是即时的
					this.message = 'world'
					
					this.hello()

					// 修改完数据之后，马上操作数据影响的这个 DOM
					// console.log(this.$refs.message.innerHTML)

					// 什么场景下才需要使用这个 API？当你想要在修改数据之后马上操作数据影响的DOM。

					// 为了解决这个问题，Vue 提供了一个特殊的 API：$nextTick
					// 该函数中的代码会确保本次数据更新视图之后才执行
					// 官方文档说明：https://cn.vuejs.org/v2/api/#vm-nextTick
					// 该代码不是更新视图 DOM，更新视图 DOM 是 Vue 的事儿
					// 简单理解：你可以把它当作一个定时器，它会在内部完成 DOM 修改之后触发调用
					this.$nextTick(() => {
						console.log(this.$refs.message.innerHTML)
					})
				},

				hello () {
					console.log(this.$refs.message.innerHTML)
				}
			}
		})
	</script>
</body>
</html>


```

# 十四、功能优化

## 处理 token 过期

```js
// 响应拦截器
request.interceptors.response.use(
  // 响应成功进入第1个函数
  // 该函数的参数是响应对象
  function (response) {
    // Any status code that lie within the range of 2xx cause this function to trigger
    // Do something with response data
    return response
  },
  // 响应失败进入第2个函数，该函数的参数是错误对象
  async function (error) {
    // Any status codes that falls outside the range of 2xx cause this function to trigger
    // Do something with response error
    // 如果响应码是 401 ，则请求获取新的 token

    // 响应拦截器中的 error 就是那个响应的错误对象
    console.dir(error)
    if (error.response && error.response.status === 401) {
      // 校验是否有 refresh_token
      const user = store.state.user

      if (!user || !user.refresh_token) {
        // router.push('/login')
+        redirectLogin()

        // 代码不要往后执行了
        return
      }

      // 如果有refresh_token，则请求获取新的 token
      try {
        const res = await axios({
          method: 'PUT',
          url: 'http://ttapi.research.itcast.cn/app/v1_0/authorizations',
          headers: {
            Authorization: `Bearer ${user.refresh_token}`
          }
        })

        // 如果获取成功，则把新的 token 更新到容器中
        console.log('刷新 token  成功', res)
        store.commit('setUser', {
          token: res.data.data.token, // 最新获取的可用 token
          refresh_token: user.refresh_token // 还是原来的 refresh_token
        })

        // 把之前失败的用户请求继续发出去
        // config 是一个对象，其中包含本次失败请求相关的那些配置信息，例如 url、method 都有
        // return 把 request 的请求结果继续返回给发请求的具体位置
        return request(error.config)
      } catch (err) {
        // 如果获取失败，直接跳转 登录页
        console.log('请求刷线 token 失败', err)
        // router.push('/login')
+        redirectLogin()
      }
    }

    return Promise.reject(error)
  }
)

+ function redirectLogin () {
+   // router.currentRoute 当前路由对象，和你在组件中访问的 this.$route 是同一个东西
    // query 参数的数据格式就是：?key=value&key=value
+   router.push('/login?redirect=' + router.currentRoute.fullPath)
+ }


```



## 登录成功跳转回原来页面

首先在响应拦截器中：

```js
// 响应拦截器
request.interceptors.response.use(
  // 响应成功进入第1个函数
  // 该函数的参数是响应对象
  function (response) {
    // Any status code that lie within the range of 2xx cause this function to trigger
    // Do something with response data
    return response
  },
  // 响应失败进入第2个函数，该函数的参数是错误对象
  async function (error) {
    // Any status codes that falls outside the range of 2xx cause this function to trigger
    // Do something with response error
    // 如果响应码是 401 ，则请求获取新的 token

    // 响应拦截器中的 error 就是那个响应的错误对象
    console.dir(error)
    if (error.response && error.response.status === 401) {
      // 校验是否有 refresh_token
      const user = store.state.user

      if (!user || !user.refresh_token) {
        // router.push('/login')
+        redirectLogin()

        // 代码不要往后执行了
        return
      }

      // 如果有refresh_token，则请求获取新的 token
      try {
        const res = await axios({
          method: 'PUT',
          url: 'http://ttapi.research.itcast.cn/app/v1_0/authorizations',
          headers: {
            Authorization: `Bearer ${user.refresh_token}`
          }
        })

        // 如果获取成功，则把新的 token 更新到容器中
        console.log('刷新 token  成功', res)
        store.commit('setUser', {
          token: res.data.data.token, // 最新获取的可用 token
          refresh_token: user.refresh_token // 还是原来的 refresh_token
        })

        // 把之前失败的用户请求继续发出去
        // config 是一个对象，其中包含本次失败请求相关的那些配置信息，例如 url、method 都有
        // return 把 request 的请求结果继续返回给发请求的具体位置
        return request(error.config)
      } catch (err) {
        // 如果获取失败，直接跳转 登录页
        console.log('请求刷线 token 失败', err)
        // router.push('/login')
+        redirectLogin()
      }
    }

    return Promise.reject(error)
  }
)

+ function redirectLogin () {
+   // router.currentRoute 当前路由对象，和你在组件中访问的 this.$route 是同一个东西
    // query 参数的数据格式就是：?key=value&key=value
+   router.push('/login?redirect=' + router.currentRoute.fullPath)
+ }

```

然后在登录成功以后：

```js
const redirect = this.$route.query.redirect || "/";
this.$router.push(redirect);

```



## 组件缓存

默认组件在切换的时候会销毁，有了组件缓存就可以帮我们把组件缓存起来而不销毁。

> 参考文档：
>
> - [在动态组件上使用 `keep-alive`](https://cn.vuejs.org/v2/guide/components-dynamic-async.html#在动态组件上使用-keep-alive)

```html
<!-- 失活的组件将会被缓存！-->
<keep-alive>
  动态组件
</keep-alive>

```

> 注意：`<keep-alive>` 要求被切换到的组件都有自己的名字，不论是通过组件的 `name` 选项还是局部/全局注册。

1、在 App.vue 对根路由组件启用组件缓存

```html
<keep-alive>
  <router-view />
</keep-alive>

```

2、在 `views/tabbar/index.vue` 对子路由也启用组件缓存

```html
<keep-alive>
  <router-view />
</keep-alive>

```

### 禁用组件缓存

> 参考文档：
>
> - https://cn.vuejs.org/v2/api/#keep-alive

手动销毁的方式需要修改组件内部的代码，这里介绍一种更灵活的方式来配置哪些组件缓存，哪些组件不缓存。

`include` 和 `exclude` 属性允许组件有条件地缓存。二者都可以用逗号分隔字符串、正则表达式或一个数组来表示：

```html
<!-- 逗号分隔字符串 -->
<keep-alive include="a,b">
  <component :is="view"></component>
</keep-alive>

<!-- 正则表达式 (使用 `v-bind`) -->
<keep-alive :include="/a|b/">
  <component :is="view"></component>
</keep-alive>

<!-- 数组 (使用 `v-bind`) -->
<keep-alive :include="['a', 'b']">
  <component :is="view"></component>
</keep-alive>

```

匹配首先检查组件自身的 `name` 选项，如果 `name` 选项不可用，则匹配它的局部注册名称 (父组件 `components` 选项的键值)。匿名组件不能被匹配。

总结：

- 根据需要设置需要缓存的组件
- 缓存需要占用更大的内容，当缓存组件过多的时候容易造成页面卡顿
- 把不需要缓存的组件排除缓存

### 动态缓存

# 十五、打包发布

## 构建打包

```sh
npm run build

# yarn run build
yarn build

```

VueCLI 会把打包结果存储到项目的 `dist` 目录中。

项目中默认配置了生产环境不允许有 `console.log()`，如果有该代码，则打包就会报错。这里我们为了方便，建议手动关闭该规则。

找到项目根目录下的 `.eslintrc` 文件

```js
module.exports = {
  root: true,
  env: {
    node: true
  },
  'extends': [
    'plugin:vue/essential',
    '@vue/standard'
  ],
  rules: {
    // 错误代号: 报错级别
    // process.env.NODE_ENV === 'production' 可以判定当前代码运行环境
    // 如果你的开发环境，则它是 false，如果是生产环境，则它是 true

    // 关闭 console.log 校验
    'no-console': 'off',
    // 'no-console': process.env.NODE_ENV === 'production' ? 'error' : 'off',
    'no-debugger': process.env.NODE_ENV === 'production' ? 'error' : 'off',
    'no-unused-vars': process.env.NODE_ENV === 'production' ? 'error' : 'off'
  },
  parserOptions: {
    parser: 'babel-eslint'
  }
}


```

然后重新打包。

## 测试打包结果

将 dist 放到一个 Web 服务器中运行测试。

- Ngxin
- Apache
- tomcat
- IIS
- 。。。。
- Node.js

这里推荐使用 Vue 官方推荐的一个命令行 http 服务工具：[serve](https://github.com/zeit/serve)。

安装：

```sh
# yarn global add serve
npm install -g serve

```

使用：

```sh
# dist 是运行 Web 服务根目录
serve -s dist

```

> serve 默认占用 5000 端口并启动一个服务

然后在浏览器中访问给出的地址访问测试。

> 提示
>
> `npm run serve` 运行的开发中的源代码（src 中的代码）
>
> 这里使用 `serve` 工具启动运行的是打包结果 dist 中的代码，主要是用来测试打包结果是否可以正常运行。

## 部署

- 公司有专门的 devops，说白了就是运维
  - 有些公司没有专门的运维人员，那就后端负责
- 你只需要把打包结果给人家就可以了



依赖知识：

- 买一个服务器
- Linux 操作系统、常用 Linux 命令
- 域名
- 搭建服务器环境
- 把本地的 Web 网站放到远程服务器
- 维护更新。。。。



## 扩展：GitHub Pages

GitHub Pages 可以帮我们托管静态网站。

GitHub Pages 中的域名：

- 如果你的仓库名字叫：你的用户名.github.io，则访问地址是：https://lipengzhou.github.io/
- 如果是其他的名字，则访问地址是：https://lipengzhou.github.io/仓库名称/

- 自定义域名
  - 首先你得有一个自己的域名
  - 然后将域名解析配置 CNAME 到：`你的用户名.github.io`
  - 在托管的仓库根目录放一个文件

> 提示：
>
> GitHub Pages 默认的域名强制开启 https，自定义域名可以选择 http 或者 https。

使用 GitHub Pages + GitHub Actions 实现自动部署。



### GitHub Pages

### 部署 Vue 项目到 GitHub Pages

GitHub Pages 只能托管静态资源，我们的 Vue 项目需要打包才能产生静态资源。

有了静态资源（dist），把它放到 GitHub 仓库中，开启 GitHub Pages 托管。



一种方式：

- 在本地打包：`npm run build`，得到 `dist` 资源
- 把 dist 推送到 GitHub 仓库中
  - 我们可以新建一个仓库（不推荐，维护麻烦）
  - 也可以使用我们的源代码仓库（推荐，放到 gh-pages 分支中）
- 把编译结果（dist）放到源代码仓库的 gh-pages（该分支名具有特殊含义，GitHub Pages 服务的要求）
- 然后把 gh-pages 分支托管到 GitHub Pages 服务中



注意一：如果你的域名在一个子目录下，则必须配置项目的 publicPath。

````
a.com

a.com/a

.com
.cn
.io

.com/a
.com/a/b
.cn/a
.io/a

````



注意二：GitHub Pages 默认域名强制开启 https 协议，https 协议下的网站不能访问 http 接口地址。

解决办法：

- 把接口更新到 https
- 使用自定义域名，可以不开启 https



### 结合 GitHub Actions 自动部署 Vue 项目

- 手动打包
- 手动推送
- 太麻烦

使用 GitHub Actions 就可以自动起来了。

一、关于域名

GitHub 虽然有默认免费域名，但是只能有一个根路径的域名，而且都必须强制开启 https，所以如果你的项目中有 http 接口是不行的。

所以建议购买一个自己的域名。

推荐阿里云的万网。



二、在域名解析记录中添加一个 CNAME 到 `你的GitHub用户名.github.io`。

{% img https://yhx0507.github.io/wxapp_static/app/006tNbRwly1gbmotawimpj329q0u010o.jpg %}

{% img https://yhx0507.github.io/wxapp_static/app/006tNbRwly1gbmotr8teyj327b0u0n49.jpg %}

{% img https://yhx0507.github.io/wxapp_static/app/image-20200206114558224.png %}

>记录类型：CNAME
>
>主机记录：
>
>- `@`: 访问地址：`lipengzhou.com`
>- `www` 访问地址：`www.lipengzhou.com`
>- `abc` → `abc.lipengzhou`
>
>解析线路：默认
>
>记录值：`你的GitHub用户名.github.io`
>
>TTL: 10分钟，默认的

{% img https://yhx0507.github.io/wxapp_static/app/image-20200206114832875.png %}

> 添加成功，确认是否能看到添加的这条记录。

然后在项目的 `public` 目录中添加一个文件 `CNAME` 写入你的自定义域名

例如：

```
toutiao89.lipengzhou.com
```

三、生成 GitHub Token

{% img https://yhx0507.github.io/wxapp_static/app/image-20200206115237278.png %}

{% img https://yhx0507.github.io/wxapp_static/app/image-20200206115311948.png %}

{% img https://yhx0507.github.io/wxapp_static/app/Snipaste_2020-12-18_22-47-10.png %}

{% img https://yhx0507.github.io/wxapp_static/app/image-20200206115515608.png %}

{% img https://yhx0507.github.io/wxapp_static/app/image-20200206115537061.png %}

> 滚动到页面底部，点击 Generate token

{% img https://yhx0507.github.io/wxapp_static/app/image-20200206115641953.png %}

> 复制绿色背景中的内容，自己保管起来。
>
> 注意：该内容只显示一次，之后不再显示，如果忘了，就重新生成。

四、将 token 配置到项目的 secrets 中

{% img https://yhx0507.github.io/wxapp_static/app/image-20200206120118601.png %}

> Name：最好和我的一致，如果你修改了则下面的脚本内容也要修改。
>
> Value：填写上一步生成的那个 token。

{% img https://yhx0507.github.io/wxapp_static/app/image-20200206120155955.png %}

> 如图所示，添加成功了。

五、配置 GitHub Actions

在项目中创建 `.github/workflows/main.yml` 并写入下面的配置内容。

```sh
name: build and deploy

# 当 master 分支 push 代码的时候触发 workflow
on:
  push:
    branches:
    - master

jobs:
  build-deploy:
    runs-on: ubuntu-latest
    steps:
    # 下载仓库代码
    - uses: actions/checkout@v2
    
    # 缓存依赖
    - name: Cache dependencies
      uses: actions/cache@v1
      with:
        path: ~/.npm
        key: ${{ runner.os }}-node-${{ hashFiles('**/package-lock.json') }}
        restore-keys: |
          ${{ runner.os }}-node-
    
    # 安装依赖
    - run: npm ci
    
    # 打包构建
    - run: npm run build
    
    # 发布到 GitHub Pages
    - name: Deploy
      uses: peaceiris/actions-gh-pages@v2
      env:
        PERSONAL_TOKEN: ${{ secrets.ACCESS_TOKEN }}
        PUBLISH_BRANCH: gh-pages
        PUBLISH_DIR: ./dist


```

六、推送源代码

```sh
git add
git commit
git push

```

七、查看部署状态

{% img https://yhx0507.github.io/wxapp_static/app/Snipaste_2020-12-18_22-53-16.png %}

{% img https://yhx0507.github.io/wxapp_static/app/Snipaste_2020-12-18_22-54-15.png %}

> 自动部署已触发

{% img https://yhx0507.github.io/wxapp_static/app/Snipaste_2020-12-18_22-55-17.png %}

{% img https://yhx0507.github.io/wxapp_static/app/Snipaste_2020-12-18_22-56-00.png %}

> 当所有的任务都完成变为绿色的对勾之后，就表示本次自动部署成功了。
>
> 当有新的源码 push 推送过来，会再次触发自动部署。
>
> 如果自动部署失败，会有红色的 ×，它还会给你发一封邮件告诉你部署失败了。



部署成功以后，进入 settings 中查看 GitHub Pages 服务是否正常。

{% img https://yhx0507.github.io/wxapp_static/app/Snipaste_2020-12-18_22-56-58.png %}

最后在浏览器中访问你的域名。

# 十六、Vue 项目构建优化

大多数 Vue 项目是基于 VueCLI 搭建的，而 VueCLI 的底层建筑是 webpack。webpack 是现在主流的功能强大的模块化打包工具，在使用 webpack 时，如果不注意性能优化，有非常大的可能会产生性能问题，性能问题主要分为**开发时打包构建速度慢**、**开发调试时的重复性工作**、以及**输出文件质量不高**等，因此性能优化也主要从这些方面来分析。

本文主要针对是在 Vue 项目中关于底层建筑 webpack 优化以及 Vue 本身相关的一些优化事宜。

## 把 VueCLI 升级到最新稳定版

尤其是 VueCLI 3 之前创建的项目，强烈建议升级到最新版 Vue CLI。

## 分析打包结果

- 了解打包的内容
- 找出最大的模块是什么
- 查找错误到达的模块
- 优化它！

### vue-cli-service build

> 参考：
>
> - https://cli.vuejs.org/zh/guide/cli-service.html#vue-cli-service-build

```
用法：vue-cli-service build [options] [entry|pattern]

选项：

  --mode        指定环境模式 (默认值：production)
  --dest        指定输出目录 (默认值：dist)
  --modern      面向现代浏览器带自动回退地构建应用
  --target      app | lib | wc | wc-async (默认值：app)
  --name        库或 Web Components 模式下的名字 (默认值：package.json 中的 "name" 字段或入口文件名)
  --no-clean    在构建项目之前不清除目标目录
  --report      生成 report.html 以帮助分析包内容
  --report-json 生成 report.json 以帮助分析包内容
  --watch       监听文件变化

```

`vue-cli-service build` 会在 `dist/` 目录产生一个可用于生产环境的包，带有 JS/CSS/HTML 的压缩，和为更好的缓存而做的自动的 vendor chunk splitting。它的 chunk manifest 会内联在 HTML 里。

这里还有一些有用的命令参数：

- `--modern` 使用[现代模式](https://cli.vuejs.org/zh/guide/browser-compatibility.html#现代模式)构建应用，为现代浏览器交付原生支持的 ES2015 代码，并生成一个兼容老浏览器的包用来自动回退。
- `--target` 允许你将项目中的任何组件以一个库或 Web Components 组件的方式进行构建。更多细节请查阅[构建目标](https://cli.vuejs.org/zh/guide/build-targets.html)。
- `--report` 和 `--report-json` 会根据构建统计生成报告，它会帮助你分析包中包含的模块们的大小。
  - 内部使用的 [Webpack Bundle Analyzer](https://github.com/webpack-contrib/webpack-bundle-analyzer) 插件。

### 使用图形界面打包

> 自带分析功能

在命令行中执行 `vue ui` 启动 VueCLI 图形界面。

```sh
vue ui

```



## Gzip 压缩

网站加载的速度很大程序取决于网站资源文件的大小，减少要传输的文件的大小可以使网站不仅加载更快，而且对于那些宽带是按量计费的用户来说也更友好。

HTTP协议上的GZIP编码是一种用来改进WEB应用程序性能的技术。大流量的WEB站点常常使用GZIP[压缩技术](https://baike.baidu.com/item/压缩技术)来让用户感受更快的速度。这一般是指WWW服务器中安装的一个功能，当有人来访问这个服务器中的网站时，服务器中的这个功能就将网页内容压缩后传输到来访的电脑浏览器中显示出来.一般对纯文本内容可压缩到原大小的40%.这样传输就快了，效果就是你点击网址后会很快的显示出来.当然这也会增加服务器的负载. 一般服务器中都安装有这个功能模块的。

Gzip 是什么？

```
一种文件压缩格式。

```



开启 Gzip 有什么好处？

```
Gzip 开启以后会将输出到用户浏览器的数据进行压缩的处理，这样就会减小通过网络传输的数据量，提高文件传输的速度。

```

> 注意：`gzip`不一定适用于所有文件的压缩。例如，文本文件压缩得非常好，通常会缩小两倍以上。另一方面，诸如JPEG或PNG文件之类的图像已经按其性质进行压缩，使用`gzip`压缩很难有好的压缩效果或者甚至没有效果。压缩文件会占用服务器资源，因此最好只压缩那些压缩效果好的文件。
>
> 资源太小的文件也不会压缩。



如何开启？

不同服务器软件配置不一样，具体由部署项目的人负责，一般是运维、后端开发人员，如果想要自行配置，可自行百度查询。大多数服务器软件都是默认开启的。

- Nginx
- Tomcat
- Apache
- IIS
- ...

> 实际工作中，如果服务器软件没有开启，可以和负责运维部署的人员沟通。



如何检测内容是否已开启了 Gzip 压缩？查看响应头中是否有下面的字段信息。

```
Content-Encoding: gzip

```



前端没有调整配置服务器软件麻烦，该怎么测试？

使用 VueCLI 官方推荐的 [serve](https://github.com/zeit/serve) 命令行工具。

```sh
# 1、如果你已经安装了就不需要安装了
npm install --global serve

# 使用该命令检测是否已安装或者是否安装成功，如果能看到输出一个版本号，则证明安装好了
serve --version

# 启动 Web 服务
serve -s dist

# 使用 -u 参数禁用 Gzip 压缩
serve -s -u ./

```

### Gzip 压缩原理

不是每个浏览器都支持gzip的，如何知道客户端是否支持gzip呢，请求头中有个 `Accept-Encoding` 来标识对压缩的支持。客户端http请求头声明浏览器支持的压缩方式，服务端配置启用压缩，压缩的文件类型，压缩方式。当客户端请求到服务端的时候，服务器解析请求头，如果客户端支持gzip压缩，响应时对请求的资源进行压缩并返回给客户端，浏览器按照自己的方式解析，在http响应头，我们可以看到 `Content-encoding: gzip`，这是指服务端使用了  `gzip` 的压缩方式。

## 在 Vue CLI 中自定义 webpack 配置

> 参考文档：
>
> - https://cli.vuejs.org/zh/guide/webpack.html 

## CDN

- CDN 主要针对的静态资源
- 我们可以把我们自己的代码放到 CDN 上
- 也可以使用一些免费的第三方 CDN 服务
  - 一般提供的都是一些常用的第三方包的 CDN 服
  - 例如 bootcdn

- 我们最好把项目中比较大的第三方包都使用 CDN 链接
  - 不仅能提高构建的速度
  - 还能提高页面的响应速度
- 当我们在项目中使用 CDN 链接之后，就没必要下载打包第三方包了

## 路由懒加载

属于首屏加载优化的一种方案。

默认情况下所有页面资源都会打包到一起，如果页面过多会导致单个文件体积过大。使用路由懒加载可以优化这种情况。它的思路就是把每个页面的资源都独立的生成一个资源文件，只有当访问到该页面的时候才会请求加载。

## 异步组件

和路由懒加载是一个道理。

```js
import MyComponent from 'my-component'

new Vue({
  // ...
  components: {
    'my-component': MyComponent
  }
})

```

```js
// import MyComponent from 'my-component'
const MyComponent = () => import('my-component')

new Vue({
  // ...
  components: {
    // 'my-component': MyComponent
    'my-component': () => import('my-component')
  }
})

```



## prefetch

[link rel="prefetch”](https://developer.mozilla.org/en-US/docs/Web/HTTP/Link_prefetching_FAQ) 是一种 resource hint，用来告诉浏览器在页面加载完成后，利用空闲时间提前获取用户未来可能会访问的内容。

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <link rel="prefetch" href="./h5.html">
</head>
<body>
  <div>Hello Index page</div>
  <a href="./h5.html">go to H5 Page</a>
</body>
</html>

```

默认情况下，一个 Vue CLI 应用会为所有作为 async chunk 生成的 JavaScript 文件 ([通过动态 `import()` 按需 code splitting](https://webpack.js.org/guides/code-splitting/#dynamic-imports) 的产物) 自动生成 prefetch 提示。

这些提示会被 [@vue/preload-webpack-plugin](https://github.com/vuejs/preload-webpack-plugin) 注入，并且可以通过 `chainWebpack` 的 `config.plugin('prefetch')` 进行修改和删除。

示例：

```js
// vue.config.js
module.exports = {
  chainWebpack: config => {
    // 移除 prefetch 插件
    config.plugins.delete('prefetch')

    // 或者
    // 修改它的选项：
    config.plugin('prefetch').tap(options => {
      options[0].fileBlacklist = options[0].fileBlacklist || []
      options[0].fileBlacklist.push(/myasyncRoute(.)+?\.js$/)
      return options
    })
  }
}

```

当 prefetch 插件被禁用时，你可以通过 webpack 的内联注释手动选定要提前获取的代码区块：

```js
import(/* webpackPrefetch: true */ './someAsyncComponent.vue')


```

webpack 的运行时会在父级区块被加载之后注入 prefetch 链接。

> 提示
>
> Prefetch 链接将会消耗带宽。如果你的应用很大且有很多 async chunk，而用户主要使用的是对带宽较敏感的移动端，那么你可能需要关掉 prefetch 链接并手动选择要提前获取的代码区块。

使用建议：

- 如果是 PC 端项目建议默认开启
- 如果是移动端项目建议关闭，帮用户节省流量

## 按需加载第三方组件

- 例如 Vant、element 都支持按需加载

## 缓存和并行处理

VueCLI 内置了：

- `cache-loader` 会默认为 Vue/Babel/TypeScript 编译开启。文件会缓存在 `node_modules/.cache` 中——如果你遇到了编译方面的问题，记得先删掉缓存目录之后再试试看。
- `thread-loader` 会在多核 CPU 的机器上为 Babel/TypeScript 转译开启

## 参考链接

# 十七、移动 App 开发

## [移动 App 开发相关概念](./移动App开发介绍.md)

## 混合应用开发（DCloud HTML5+ App）

- 百分之九十五的技术还是 Web、百分之五的技术是原生应用

## 安装 HBuilder

访问 HBuilder 的下载页面：https://www.dcloud.io/hbuilderx.html

{% img https://yhx0507.github.io/wxapp_static/app/1570587926151.png %}

> 选择 DOWNLOAD

{% img https://yhx0507.github.io/wxapp_static/app/1570587995718.png %}

> 根据自己的操作系统下载对应的安装包。
>
> - 标准版：可直接用于 web 开发、markdown、字处理场景。做 App 仍需要安装插件
>
> - App 开发板：在标准版的基础之上预置了 App/uni-app 开发所需的插件，开箱即用
>
> 如果是开发 App 建议下载安装 App 开发版即可。

{% img https://yhx0507.github.io/wxapp_static/app/1570588350152.png %}

> 下载的文件是一个压缩包

{% img https://yhx0507.github.io/wxapp_static/app/1570588463176.png %}

> 解压到当前文件夹（或者其它目录都可以）

{% img https://yhx0507.github.io/wxapp_static/app/1570588604586.png %}

> 进入 HBuilderX 目录中，找到 `HBuilderX.exe` 并打开。

{% img https://yhx0507.github.io/wxapp_static/app/1570590042920.png %}

## Hello World

### 创建项目

在 Hbuilder 的菜单栏中依次找到：文件 -> 新建 -> 项目

{% img https://yhx0507.github.io/wxapp_static/app/1570590394808.png %}

> - 项目类型：选择 5+App
> - 项目名称：给项目起个名字
> - 存储目录：设置项目的存储目录
> - 项目模板：选择默认模板
>
> 配置好以后，点击“创建”

{% img https://yhx0507.github.io/wxapp_static/app/1570590670255.png %}

> 创建完成。

项目目录结构说明：

- `css`：存储项目中的样式文件资源

- `img`：存储项目中的图片文件资源

- `js`：存储项目中的 js 文件资源
- `unpackage`：存储不需要打包的文件资源
- `index.html`：项目首页
- `manifest.json`：打包 App 的配置文件

为了待会儿我们真机调试运行能够看到页面效果，我们这里在项目的 `index.html` 中添加一个标签：

```html
<h1>Hello World</h1>

```

### 真机调试运行

这里我们主要了解如何把上一步创建好的项目运行到手机上（仅用于开发测试）。

#### Android

常见问题：

- HBuilder/HBuilderX真机运行、手机运行、真机联调常见问题：https://ask.dcloud.net.cn/article/97

- 确保手机开启了 USB 调试模式
- 建议下载安装 360 手机助手，如果 360 手机助手能够连接到手机，基本就 OK 了

1、在手机的设置中找到并打开开发者选项

在手机的设置中找到开发者选项：

{% img https://yhx0507.github.io/wxapp_static/app/1570591544471.png %}

> 提示：大多数安卓手机的设置中默认没有开发者选项，如果没有就打开百度搜索：你的手机品牌型号开发者选项，例如：小米 9 开发者选项，然后根据搜索到的结果找到并打开开发者选项。

2、启用开发者选项和 USB 调试

{% img https://yhx0507.github.io/wxapp_static/app/1570591838627.png %}

3、使用数据线连接手机和电脑

4、在 HBuilder 的菜单栏中依次选择：运行 -> 运行到手机或模拟器 -> 你的手机设备

{% img https://yhx0507.github.io/wxapp_static/app/1570591970145.png %}

> 提示：如果没有发现你的手机设备，请参考：[HBuilder/HBuilderX 真机运行、手机运行、真机联调常见问题](http://ask.dcloud.net.cn/article/97)

{% img https://yhx0507.github.io/wxapp_static/app/1570592155354.png %}

> 运行到设备之后，Hbuilder 中会自动打开一个控制台并输出运行日志。

然后 Hbuilder 需要安装调试基座（应用）到手机中，这个时候手机会提示你是否允许安装该应用，选择允许即可。

调试基座安装好以后，Hbuilder 会自动打开并将项目运行到这个 App 中。

{% img https://yhx0507.github.io/wxapp_static/app/1570592635004.png %}

然后我们在 `index.html` 新增一些内容：

```html
<p>杨柳青青江水平</p>
<p>闻郎江上唱歌声</p>
<p>东边日出西边雨</p>
<p>道是无晴却有晴</p>

```

{% img https://yhx0507.github.io/wxapp_static/app/1570592783359.png %}

我们可以看到，手机 App 中的内容也自动更新了。

> 如果没有自动更新，就手动退出并重新打开。

#### iOS

1、下载安装 [iTunes](https://www.apple.com/cn/itunes/download/)

- Windows 用户：不要安装微软商店版
- 最新版的 macOS 不需要安装

2、使用数据线连接到电脑

3、确保 iTunes 能够正常连接到手机

4、在 Hbuilder 中选择运行到你的 iPhone 设备

5、它会在手机中安装一个用于调试的测试 App：Hbuilder

6、在手机上设置 Hbuilder 为受信任的开发者

> 注意：只有安装了未受信任的应用才能在设置中找到设备管理选项。

因为 iPhone 的权限比较高，所以打开 Hbuilder 会提示“未受信任的企业级开发者”，所以这里需要手动设置该应用为受信任的开发者。

{% img https://yhx0507.github.io/wxapp_static/app/ios-trustapp-01.jpg %}

{% img https://yhx0507.github.io/wxapp_static/app/ios-trustapp-02.jpg %}

> 打开手机的设置

{% img https://yhx0507.github.io/wxapp_static/app/ios-trustapp-03.jpg %}

> 在设置中找到通用并打开

{% img https://yhx0507.github.io/wxapp_static/app/ios-trustapp-04.jpg %}

> 打开设备管理

{% img https://yhx0507.github.io/wxapp_static/app/ios-trustapp-05.jpg %}

> 打开名称为 Digital Heaven... 的选项

{% img https://yhx0507.github.io/wxapp_static/app/ios-trustapp-06.jpg %}

> 选择“信任 Digital Heaven...”

{% img https://yhx0507.github.io/wxapp_static/app/ios-trustapp-07.jpg %}

> 选择信任

设置好以后就可以正常打开了，之后的其它操作和 Android 类似。

### 访问 HTML5 + API

- [HTML5+ API Reference](http://www.html5plus.org/doc/h5p.html)

### 打包发布

- 配置 [manifest.json](http://ask.dcloud.net.cn/article/94) 文件
- 在 HBuilder 中找到：发行 -> 原生 App（云打包）
- 等待一段时间，得到打包结果安装包，然后安装到手机上测试
- 最后根据需要发布到对应的手机应用商店

项目中的文件都被打包到应用安装包中了，只要安装了这个应用就不需要请求下载这些文件，除非是一些请求接口的功能必须是联网的。这种方式好处是一些核心文件不需要联网下载，但是更新麻烦，如果你修改了代码，则需要重新打包。

还有一种方式，我们把应用部署为网站，然后在这个壳儿里面加载我们的应用的那个网址。

如果是开发测试，则将 `manifest.json` 文件中的 `lanuchpath` 修改为你的局域网地址。

如果是发布部署，则将 `luanchpath` 设置为线上的地址。

#### 配置 manifest

- [Manifest.json 文档说明 manifest 配置](http://ask.dcloud.net.cn/article/94)

#### 打包

- 本地打包
- 云打包

#### 发布

## 使用 Vue.js 开发 HTML5+ App

一、将你的项目部署到服务端

二、真机调试运行

1、启动本地开发 Web 服务

```sh
# yarn serve
npm run serve
```

2、启动手机真机调试

在 HBuilder 中，菜单栏 → 运行 → 运行到手机或模拟器 → 你的手机设备

3、编码测试。。。和原来在 PC 端的开发是一样的，只不过运行到了手机中



### 打包发布

一、打包手机安装包

1、将 `app/manifest.json` 中的 `launch_path` 改为线上地址：`http://toutiao89.lipengzhou.com`。

2、在 HBuilder 中加载项目中的 app 目录，菜单栏 → 发行 → 原生 App 云打包

3、配置打包规则

4、等待打包结果

5、根据需要发行到手机的应用商店供用户下载（安卓安装包可以直接放到自己的服务器供用户下载）



二、更新 App 内容？

只需要更新我们的网站就可以了。

```
将本地代码推送到 GitHub。
触发 GitHub Actions 自动部署

部署好以后，网站就更新了。
```



手机 App 中的网页可能会有缓存问题，解决办法就是：结合前端 + 后端禁用缓存。

参考地址：https://ask.dcloud.net.cn/question/31327

所以一般应用或是网站的更新都会选择在夜深人静的时候。
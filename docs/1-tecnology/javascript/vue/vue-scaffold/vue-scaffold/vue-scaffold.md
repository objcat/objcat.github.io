# 🍎 简介

本文主要介绍`vue`脚手架要如何搭建, 包含了`vue2`和`vue3`

# 🍎 安装环境

## 🌲 安装nodejs

[跳转 node_env](../../../../../3-program/env/node/node_env.md)

# 🍎 搭建脚手架项目

## 🌲 create-vue ⭐️⭐️⭐️⭐️⭐️

使用`create-vue`可以方便的构建出`vue2/vue3`的项目, 而且可以选择式的导入需要的依赖, 而且它使用`vite`来做打包工具

[跳转 create-vue](../create-vue/create-vue.md)

## 🌲 vue-cli ⭐️⭐️⭐️

我们可以使用`vue-cli`来搭建项目, 它使用`webpack`作为打包工具

[跳转 vue-cli](../vue-cli/vue-cli.md)

## 🌲 vue ui ⭐️⭐️⭐️

也可以使用 vueui 进行创建

```shell
vue ui
```

系统会自动帮你启动一个服务帮你创建项目

![](images/Pasted%20image%2020231015093819.png)

按照提示去创建就可以了

## 🌲 vite创建 ⭐️⭐️⭐️⭐️

[跳转 vite](../../vite/vite/vite.md)

## 🌲 hbuilder创建 ⭐️⭐️⭐️

实际上也是基于`vue-cli`的

![](images/Pasted%20image%2020221208134651.png)

我们打开`HbuilderX`然后在里面新建项目, 我们可以看到嘎嘎简单啊, 选择一个项目后点创建就可以了, 上图是vue3-cli, 同样的向下翻也可以看到vue2-cli, 创建都是一样的, 我们这边也创建了个vue2-cli的项目, 你可以按需求创建, 那我们就开始学习脚手架语法吧

我们可以看到工程结构比普通项目稍微复杂一些

![](images/Pasted%20image%2020221208134924.png)

- main.js 工程入口
- App.vue 初始页面文件

我们下面就来对比一下vue2和vue3的初始页面的区别

> vue2

```html
<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <HelloWorld msg="Welcome to Your Vue.js App"/>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'

export default {
  name: 'app',
  components: {
    HelloWorld
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
```

> vue3

```html
<script setup>
// This starter template is using Vue 3 <script setup> SFCs
// Check out https://v3.vuejs.org/api/sfc-script-setup.html#sfc-script-setup
import HelloWorld from './components/HelloWorld.vue'
</script>

<template>
  <img alt="Vue logo" src="./assets/logo.png" />
  <HelloWorld msg="Hello Vue 3 + Vite" />
</template>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
```

经过对比我们可以发现, vue3的script标签上面有一个setup属性, 这是vue3与vue2最大的区别, 一般叫它setup语法糖, 是用来改进vue结构引入的优化, 同时对vue的性能上也有帮助

# 🍎 语法基础

[跳转vue-syntax](../../vue-syntax/vue-syntax/vue-syntax.md)

# 🍎 VSCode

我们平时工作中多使用VSCode, 首先进行下载

https://code.visualstudio.com

安装完毕后用它打开工程

[跳转 vscode](../../../../../7-software/IDE/vscode/vscode/vscode.md)

# 🍎 样式配置

## 🌲 normalize.css

一般在写`css`的时候都会去网上找到初始化`css`样式的代码, 常用的有

https://meyerweb.com/eric/tools/css/reset/

http://necolas.github.io/normalize.css/?spm=a2c6h.12873639.0.0.6d231e22tlgUni

我们这里使用yarn直接安装的

```shell
yarn add normalize.css
```

然后在main.ts中导入, 使全局生效

```ts
import { createApp } from 'vue'
import App from './App.vue'
import router from './router'
import 'normalize.css'

createApp(App).use(router).mount('#app')
```

## 🌲 sass

一行代码安装

```shell
yarn add sass
```

# 🍎 Design

一般写网页之前都要用到一套UI框架, 常用的有`element-ui`, `Ant Design`, `TDesign`等

## 🌲 element UI

> Vue2

https://element.eleme.cn

> Vue3

[element-plus](../../../../css/ui/element-ui/element-plus/element-plus/element-plus.md)

## 🌲 Ant Design

https://www.antdv.com/components/overview-cn

## 🌲 TDesign

https://tdesign.tencent.com

## 🌲 tailwindcss

中文文档

https://www.tailwindcss.cn/docs

英文文档

https://tailwindcss.com/docs/upgrade-guide#configure-content-sources

首先进行安装

```shell
yarn add -D tailwindcss@latest postcss@latest autoprefixer@latest
```

初始化配置文件

```shell
npx tailwindcss init -p
```

配置 Tailwind 来移除生产环境下没有使用到的样式声明`purge`

```ts
// tailwind.config.js
module.exports = {
  purge: ['./index.html', './src/**/*.{vue,js,ts,jsx,tsx}'],
darkMode: false, // or 'media' or 'class'
theme: {
  extend: {},
},
variants: {
  extend: {},
},
plugins: [],
}
```

在3.0的时候配置会改变 变成

```ts
module.exports = {
  content: [
    './public/**/*.html',
    './src/**/*.{js,jsx,ts,tsx,vue}',
  ],
  corePlugins: {
    preflight: false,
  },
  theme: {
    extend: {},
  },
  plugins: [],
}

```

然后在`src`目录创建`styles`文件夹来管理css样式, 创建一个`tailwind.css`里面加入这三个

```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```

然后在main.ts中引入即可

```ts
import { createApp } from 'vue'
import App from './App.vue'
import router from './router'
import './styles/tailwind.css'

createApp(App).use(router).mount('#app')
```

在搭配使用`element-plus`时可能会发现ui被初始化, 这个时候就需要进行一个配置

https://www.tailwindcss.cn/docs/preflight

```ts
module.exports = {
 corePlugins: {
    preflight: false,
  }
}
```

关闭后就没有了初始化样式, 之后还需要导入第三方的reset样式, 比如`🌸 reset`章节介绍的

# 🍎 核心组件

## 🌲 vue-router

[跳转 vue-router](../../vue-router/vue-router/vue-router.md)

# 🍎 环境变量

## 🌲 webpack

有时候我们希望在不同环境中的常量值是不同的, 那我们就需要使用到环境变量了, 我们可以在main.js中打印一下

```js
console.log(process.env);
// {NODE_ENV: 'development', BASE_URL: '/'}
```

可以看到默认的环境变量有两个, 那我们要怎么增加呢, 也很简单, 我们只需要在项目下建立两个文件`.env.development`和`.env.production`

![](images/Pasted%20image%2020230629093818.png)

分别配置好我们的变量

```js
# just a flag
ENV = 'development'
# api接口请求地址
VUE_APP_BASE_API = 'http://127.0.0.1:8085'

# just a flag
ENV = 'production'
# api接口请求地址
VUE_APP_BASE_API = 'http://www.objcat.com'
```

然后我们在main.js中就可以使用了

```js
console.log(process.env);
console.log(process.env.VUE_APP_BASE_API)

{VUE_APP_BASE_API: 'http://127.0.0.1:8085', NODE_ENV: 'development', BASE_URL: '/'}
```

我们可以看到ENV变量是不显示的, 我并不知道为什么会这样, 但我经过尝试发现变量需要遵循下面的命名规则

```js
VUE_APP_XXX
比如
VUE_APP_A
VUE_APP_A_B
```

# 🍎 vue中使用ts

首先随便定义个ts, 并导出

```ts
class Student {
    name: string
    age: number
    // 构造函数 / 初始化方法
    constructor(name: string, age: number) {
        this.name = name
        this.age = age
    }
}

export {
    Student
}
```

然后在vue中就可以直接使用了

```vue
<script lang="ts" setup>
import { Student } from '@/pages/main/model/test.ts'
let stu = new Student("张三", 1)
console.log(stu.name)
</script>
```

# 🍎 打包

## 🌲 打包命令

我们查看package.json很容易就能看到打包的命令

![](images/Pasted%20image%2020221212095117.png)

我们只需要运行`npm run build`

```shell
npm run build

> vue3_cli_default@0.0.0 build
> vite build

vite v2.5.3 building for production...
✓ 9 modules transformed.
dist/index.html                  0.42 KiB
dist/assets/index.8a2ad377.js    0.90 KiB / brotli: 0.45 KiB
dist/assets/vendor.6ab47f8f.js   46.81 KiB / brotli: 16.74 KiB
```

打包后我们看一下目录结构

![](images/Pasted%20image%2020221212095406.png)

我们可以看到里面有一个`index.html`就是我们的主页了, 我们来访问一下

![](images/Pasted%20image%2020240401092551.png)

发现是不通的, 不要着急, 我们可以把他放到nginx中来测试一下

![](images/Pasted%20image%2020240401092741.png)

可以看到还是不行

## 🌲 打包白屏问题

打包后我们可能会遇到白屏, 这时候我们就要开始解决问题了, 首先我们打开控制台

### 🌸 原因1: 本地运行

当我们没有使用服务器来打开, 本地运行`index.html`的时候就会出现这个错误, 所以你需要把dist放在`tomcat`或者`nginx`中

![](images/Pasted%20image%2020221212095633.png)

### 🌸  原因2: base

![](images/Pasted%20image%2020221212095824.png)

然后我们查看一下源码

```html
<head>
<meta charset="UTF-8">
<link rel="icon" href="/favicon.ico">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Vite App</title>
<script type="module" crossorigin="" src="/assets/index.8a2ad377.js"></script>
<link rel="modulepreload" href="/assets/vendor.6ab47f8f.js">
</head>
```

我们发现路径前面缺少了`"."`, 我们需要在配置文件上进行修改, 我们打开`vite.config.js`, 添加`base`属性, 网上是这么告诉我们修改的, 但其实这还是有点问题的, 我们继续向下看

```js
import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'

// https://vitejs.dev/config/
export default defineConfig({
  base: "./",
  plugins: [vue()]
})
```

然后我们重新打包运行

```html
<head>
<meta charset="UTF-8">
<link rel="icon" href="./favicon.ico">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Vite App</title>
<script type="module" crossorigin="" src="./assets/index.8a2ad377.js"></script>
<link rel="modulepreload" href="./assets/vendor.6ab47f8f.js">
</head>
```

可以看到页面显示正常了, 我随便插入了一张图片, 嘿嘿

![](images/Pasted%20image%2020240401092939.png)

但是这个`./`是有一个大坑的, 如果使用`nginx`部署会出现二级以上路由无法访问的问题, 报错提示是

```
failed to load module script
```

所以我们还是要使用`/`, 而`nginx`配置如下, 可以看到`nginx`的`location /`和`base: "/"`对应上就没有问题了

```nginx
# 配置vue项目
location / {
	root /usr/share/nginx/html/dist;
	index index.html index.htm;
	try_files $uri $uri/ /index.html;
}
```

## 🌲 打包白屏无报错

没打包之前是没有问题的, 打包后白屏并且无报错

![](images/Pasted%20image%2020240410001406.png)

我们来看一下路由配置

```js
import { createRouter, createWebHistory } from 'vue-router';

const modules = import.meta.glob("@/views/**/*.vue");
const _import = (file) => modules[`/src/views/${file}.vue`]

const router = createRouter({
  history: createWebHistory(import.meta.env.BASE_URL),
  routes: [
    {
      path: '/',
      name: 'home',
      component: _import("home/HomeView")
    },
    {
      path: '/qqpd',
      name: 'qqpd',
      component: _import("soft/QQPhotoDownload")
    }
  ]
})

export default router
```

主页仅仅是一个`router-view`

```vue
<template>
  <router-view></router-view>
</template>
```

更换路由模式, 我们更换成

```js
import { createRouter, createWebHashHistory } from 'vue-router';

const modules = import.meta.glob("@/views/**/*.vue");
const _import = (file) => modules[`/src/views/${file}.vue`]

const router = createRouter({
  history: createWebHashHistory(import.meta.env.BASE_URL),
  routes: [
    {
      path: '/',
      name: 'home',
      component: _import("home/HomeView")
    },
    {
      path: '/qqpd',
      name: 'qqpd',
      component: _import("soft/QQPhotoDownload")
    }
  ]
})

export default router

```

发现页面可以显示出来了

![](images/Pasted%20image%2020240410001253.png)

但是由于`hash`这种模式比较反人类, 所以不采用, 还是回去寻找解决方案, 后来无意间看到`createWebHistory`参数中有个`BASE_URL`, 给它换成和网站一个路径的就没问题了

比如我的网站路径是`https://www.objcat.com/dist`

那么我的`BASE_URL`就是`/dist`

```js
import { createRouter, createWebHistory } from 'vue-router';

const modules = import.meta.glob("@/views/**/*.vue");
const _import = (file) => modules[`/src/views/${file}.vue`]

const router = createRouter({
  history: createWebHistory("/dist"),
  routes: [
    {
      path: '/',
      name: 'home',
      component: _import("home/HomeView")
    },
    {
      path: '/qqpd',
      name: 'qqpd',
      component: _import("soft/QQPhotoDownload")
    }
  ]
})

export default router

```

## 🌲 刷新404问题

很神奇的是在本地没有任何问题, 放到nginx上会发现第一次加载也没问题, 但是刷新就会404

```js
import { createRouter, createWebHistory } from 'vue-router';

const modules = import.meta.glob("@/views/**/*.vue");
const _import = (file) => modules[`/src/views/${file}.vue`]

const router = createRouter({
  history: createWebHistory("/dist"),
  routes: [
    {
      path: '/',
      name: '',
      component: _import("layout/layout"),
      redirect: { name: "web_navi" },
      children: [
        {
          path: 'web_navi',
          name: 'web_navi',
          // route level code-splitting
          // this generates a separate chunk (About.[hash].js) for this route
          // which is lazy-loaded when the route is visited.
          component: _import("content/web_navi")
        }
      ]
    },
    {
      path: '/home',
      name: 'home',
      component: _import("home/HomeView")
    },
    {
      path: '/qqpd',
      name: 'qqpd',
      component: _import("soft/QQPhotoDownload")
    }
  ]
})

export default router
```

http://localhost/dist

直接访问本地路径, 会被重定义为`http://localhost/dist/web_navi`, 这个我是通过子路由嵌套设置的, 第一次加载也是没有问题的

![](images/Pasted%20image%2020240410101718.png)

但是刷新之后就会404

![](images/Pasted%20image%2020240410101733.png)

感觉上就能看出来, 应该是nginx的配置问题

经过多方查看, 找到了原因, 其实我们的浏览器分为前端路由和后端路由, 所谓前端路由就是`vue-router`的一套机制, 它可以利用单页面来进行路由的跳转, 从而实现了一套页面可以实现无数的页面, 但是部署在服务器上就有了后端路由的概念, 所谓后端路由是指`输入网址然后按回车`或者`刷新页面`, 这时候页面就会被发送到`nginx`服务器, 我们上面的错误就基于这个原因, 知道原因后那么我们就来配置`nginx`

最终还是找到了解决方案, 那就当找不到文件的时候 让他去找`/dist/index.html`, 很多人会配置成`/index.html`这是定向到了`nginx`的主文件, 而我的vue主文件在`html/dist/index.html`

```nginx
    location / {
	    root   /usr/share/nginx/html;
	    index  index.html index.htm;
	    try_files $uri $uri/ /dist/index.html;
    }
```

这么做每次访问根路径下面的一个dist子路径, 不是很舒服, 但是能用

http://localhost/dist

## 🌲 设置nginx就是主页

上面我们已经可以实现了`http://localhost/dist`来访问主页了, 但是这么做可能不太舒服, 我们还是想把`http://localhost`设置成主页, 所以我们再改`nginx`

```
location / {
	root   /usr/share/nginx/html/dist;
	index  index.html index.htm;
	try_files $uri $uri/ /dist/index.html;
}
```

第一次访问没问题

http://localhost

刷新后直接500

![](images/Pasted%20image%2020240410104446.png)

显示的错误是

```
Failed to load resource: the server responded with a status of 500 (Internal Server Error)
```

然后我尝试了修改

```
location / {
	root   /usr/share/nginx/html/dist;
	index  index.html index.htm;
	try_files $uri $uri/ /index.html;
}
```

还是第一次能访问, 刷新就报错

```
index-CeD1PKaS.js:1 Failed to load module script: Expected a JavaScript module script but the server responded with a MIME type of "text/html". Strict MIME type checking is enforced for module scripts per HTML spec.
```

后来把路由里面的路径去掉了

```
history: createWebHistory("/dist")

history: createWebHistory()
```

发现项目可以运行了, 但是这种是有缺点的, 因为`nginx`里面存放的文件无法下载了, 因为路径都被重定向到`index.html`都被`vue`接管了, 所以打开就是白屏, 然后报错

```
Failed to load module script: Expected a JavaScript module script but the server responded with a MIME type of "text/html". Strict MIME type checking is enforced for module scripts per HTML spec.
```

我们想解决办法肯定是配置`location`, 我们使用`alias`来指定路径进行下载, 可以打破`vue`的接管

```
location /qqpddownload {
	alias /usr/share/nginx/html/qqpddownload;
}
```

### 🌸 新尝试直接覆盖nginx目录

后来打算尝试一种新的方法, 既然配置路径这么麻烦, 倒不如回归本质, 直接把`dist`覆盖到`html`, 完全取代`nginx`的默认页面

首先我把`/dist`去掉了

```
history: createWebHistory("/dist")

history: createWebHistory()
```

然后直接把dist中的内容全都覆盖到html中

![](images/Pasted%20image%2020240410114154.png)
然后配置`nginx`

```
location / {
	root   /usr/share/nginx/html;
	index  index.html index.htm;
	try_files $uri $uri/ /index.html;
}
```

发现这种配置方法是完全没问题的, 无论打开或刷新或下载文件都是正常的

倒回来讲, 如果最上面路由中不去掉`/dist`, 那么访问`http://localhost`会自动跳到`http://localhost/dist/web_navi`

然后报错, 所以我们可以看到`createWebHistory("/dist")`的作用就是在我们的路径里拼接`/dist`路径

```
index-BJ665o_D.js:1 Failed to load module script: Expected a JavaScript module script but the server responded with a MIME type of "text/html". Strict MIME type checking is enforced for module scripts per HTML spec.
```

## 🌲 经验总结

我总结出的经验就是`nginx`部署`vue`路由要配对其实不难, 分为两种, 我们先看一下路径

![](images/Pasted%20image%2020240410124437.png)

![](images/Pasted%20image%2020240410124639.png)

我们的vue项目的主页在`dist/index.html`

### 🌸 localhost直跳主页

直接跳主页, 这个不需要去配置`createWebHistory`直接就是默认的就行

```
history: createWebHistory()
```

然后配置`nginx`, 由于要直接跳去`/dist`所以`root`就设置`/usr/share/nginx/html/dist`

```
location / {
	root   /usr/share/nginx/html/dist;
	index  index.html index.htm;
	try_files $uri $uri/ /index.html;
}
```

`try_files`是指如果找不到文件就去`/index.html`, 注意这个是相对路径, 因为我们设置`root`为`dist`, 所以直接读取`dist`下面的`index.html`, 就不会报404了

### 🌸 localhost不跳转主页

这种情况是进来是`nginx`主页, 你手动输入`http://localhost/dist`才跳转到网页

这个时候你就需要配置`createWebHistory`

```
history: createWebHistory("/dist")
```

然后配置`nginx`, 由于不需要跳转网页, 那么默认就是进入`nginx`的首页

```
location / {
	root   /usr/share/nginx/html;
	index  index.html index.htm;
	try_files $uri $uri/ /dist/index.html;
}
```

`try_files`是指如果找不到文件就去`/dist/index.html`, 注意这个和上面就不一样了, 因为我们设置`root`为`html`, 所以读取的时候比如要加上`dist`的路径, 也就是`dist`下面的`index.html`, 就不会报`404`了

## 🌲 首次打开白屏, 刷新回复正常

可以看到错误信息是下面的, 这个可能要使用useRouter在main.js中重新定向一下了

```
Failed to load module script: Expected a JavaScript module script but the server responded with a MIME type of "text/html". Strict MIME type checking is enforced for module scripts per HTML spec.
index-DQyJw4ml.js:31 TypeError: Failed to fetch dynamically imported module: http://www.objcat.com/assets/layout-0aDdxZsg.js
F @ index-DQyJw4ml.js:31
web_navi-2aanxH9o.js:1 Failed to load module script: Expected a JavaScript module script but the server responded with a MIME type of "text/html". Strict MIME type checking is enforced for module scripts per HTML spec.
www.objcat.com/:1 Uncaught (in promise) TypeError: Failed to fetch dynamically imported module: http://www.objcat.com/assets/web_navi-2aanxH9o.js
navibar-X3mGW1LJ.js:1 Failed to load module script: Expected a JavaScript module script but the server responded with a MIME type of "text/html". Strict MIME type checking is enforced for module scripts per HTML spec.
sidebar-BME_TUDj.js:1 Failed to load module script: Expected a JavaScript module script but the server responded with a MIME type of "text/html". Strict MIME type checking is enforced for module scripts per HTML spec.
```

## 🌲 去掉`console.log`

https://www.bilibili.com/video/BV1eK4y167pr?vd_source=6a3366073b40e666199d2ecb6a5f4e17

使用它可以在开发环境方便的调试我们的项目, 但是在生产环境上可能会造成内存泄漏, 所以我们需要在生产环境中把它去掉, 使用`terser`即可

### 🌸  vue-cli

众所周知`vue-cli`使用`webpack`作为打包工具, 内部已经集成了`terser`, 我们可以在`vue.config.js`中方便的进行配置

```js
const { defineConfig } = require('@vue/cli-service')
module.exports = defineConfig({
  transpileDependencies: true,
  terser: {
    terserOptions: {
      compress: {
        drop_console: true,
        drop_debugger: true
      }
    }
  }
})
```

### 🌸  vite

我们使用`create-vue`来创建的项目一般都默认使用`vite`作为构建工具, 里面默认是没有`terser`的, 但是我们可以额外引入

```js
npm i terser
yarn add terser
```

然后我们可以在项目中`vite.config.js`中直接配置

```js
import { fileURLToPath, URL } from 'node:url'

import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'

// https://vitejs.dev/config/
export default defineConfig({
  plugins: [
    vue(),
  ],
  build: {
    minify: `terser`,
    terserOptions: {
      compress: {
        drop_console: true,
        drop_debugger: true
      }
    }
  },
  resolve: {
    alias: {
      '@': fileURLToPath(new URL('./src', import.meta.url))
    }
  },
})
```

## 🌲 页面过大

我们打包之后会发现下面有一个`warning`

```
dist/assets/web_navi-w9c1EYRp.js          4.17 kB │ gzip:   1.56 kB
dist/assets/flex_teach-Dh1SUUi_.js       29.30 kB │ gzip:   4.67 kB
dist/assets/apimap-C_HrJmEu.js           30.84 kB │ gzip:  12.19 kB
dist/assets/index-CdzD1zbS.js         1,255.34 kB │ gzip: 313.02 kB

(!) Some chunks are larger than 500 kB after minification. Consider:
- Using dynamic import() to code-split the application
- Use build.rollupOptions.output.manualChunks to improve chunking: https://rollupjs.org/configuration-options/#output-manualchunks
- Adjust chunk size limit for this warning via build.chunkSizeWarningLimit.
✓ built in 2.76s
✨  Done in 3.70s.
```

我们可以看到`index-CdzD1zbS.js`是非常大的, 超过了1M, 这有什么影响呢? 答案就是加载网页很慢

![](images/Pasted%20image%2020240421154017.png)




# 🍎 后台管理框架

## 🌲 Ant Design Pro - Vue

https://pro.antdv.com/docs/getting-started

# 🍎 FAQ

## 🌲 Failed to mount component

非常容易出现的一个问题, 报错如下

```js
main.js?t=1697437341022:29 [Vue warn]: Failed to mount component: template or render function not defined.

found in

---> <Anonymous>
       <App> at /Users/objcat/project/Java/gulimall2024/renren-fast-vite/src/App.vue
         <Root>
(anonymous)	@	main.js?t=1697437341022:29
Show 34 more frames
```

经过检查多是路由配置问题, 在`import`前加上`() => `问题解决

```json
{ path: '/login', component: import('@/views/common/login.vue'), name: 'login', meta: { title: '登录' } }


{ path: '/login', component: () => import('@/views/common/login.vue'), name: 'login', meta: { title: '登录' } }
```

## 🌲 @问题

`vite.config.ts`中这么设置就可以使用相对路径符号`@`来导入组件了

```ts
import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'
import path from 'path'
// https://vitejs.dev/config/
export default defineConfig({
  plugins: [vue()],
  resolve: {
      alias: {
        "@": path.join(__dirname, "./src"),
      },
      // extensions: [".ts", ".js"],
    },
})
```

然后我们会发现'path'有一个找不到的报错, 但貌似不影响使用我这里是安装了`@types/node`

```shell
pnpm add @types/node -D
```

然后我们会发现`path`没有错误了, 但是当我们导入组件的时候我们会发现另外一个错误

```vue
<script setup lang="ts">
import HelloWorld from "@/components/HelloWorld.vue"
</script>

<template>
	<HelloWorld></HelloWorld>
</template>
	
<style scoped>

</style>
```

`HelloWorld`报红了, 说`can found module`

这时候我们只需要配置一下`tsconfig.json`即可

```json
{
  "compilerOptions": {
    ......
    "baseUrl": ".",
    "paths": {
      "@/*": [ "src/*" ],
    }
  }
}
```

上述问题也存在于导入ts的时候, 我们下面就来演示一下如何导入ts

```ts
// src/tools/zykit/ZYRequest/index.ts
// 以前导入方式
import { ZYRequest } from '@/tools/zykit/ZYRequest/index.ts'
// 修改后的导入方式
import { ZYRequest } from '@/tools/zykit/ZYRequest'
```

## 🌲 ERR_OSSL_EVP_UNSUPPORTED

问题如下

```shell
objcat@yuanjun-2 mall4v % npm run dev                               

> mall4v@0.1.0 dev
> vue-cli-service serve

 INFO  Starting development server...
10% building 2/5 modules 3 active ...ject/Js/mall4v/node_modules/babel-loader/lib/index.js!/Users/objcat/project/Js/mall4v/src/main.jsError: error:0308010C:digital envelope routines::unsupported

Error: error:0308010C:digital envelope routines::unsupported
    at new Hash (node:internal/crypto/hash:71:19)
    at Object.createHash (node:crypto:133:10)
    at module.exports (/Users/objcat/project/Js/mall4v/node_modules/webpack/lib/util/createHash.js:135:53)
    at NormalModule._initBuildHash (/Users/objcat/project/Js/mall4v/node_modules/webpack/lib/NormalModule.js:417:16)
    at handleParseError (/Users/objcat/project/Js/mall4v/node_modules/webpack/lib/NormalModule.js:471:10)
    at /Users/objcat/project/Js/mall4v/node_modules/webpack/lib/NormalModule.js:503:5
    at /Users/objcat/project/Js/mall4v/node_modules/webpack/lib/NormalModule.js:358:12
    at /Users/objcat/project/Js/mall4v/node_modules/loader-runner/lib/LoaderRunner.js:373:3
    at iterateNormalLoaders (/Users/objcat/project/Js/mall4v/node_modules/loader-runner/lib/LoaderRunner.js:214:10)
    at Array.<anonymous> (/Users/objcat/project/Js/mall4v/node_modules/loader-runner/lib/LoaderRunner.js:205:4)
    at Storage.finished (/Users/objcat/project/Js/mall4v/node_modules/enhanced-resolve/lib/CachedInputFileSystem.js:55:16)
    at /Users/objcat/project/Js/mall4v/node_modules/enhanced-resolve/lib/CachedInputFileSystem.js:91:9
    at /Users/objcat/project/Js/mall4v/node_modules/graceful-fs/graceful-fs.js:123:16
    at FSReqCallback.readFileAfterClose [as oncomplete] (node:internal/fs/read_file_context:68:3) {
  opensslErrorStack: [ 'error:03000086:digital envelope routines::initialization error' ],
  library: 'digital envelope routines',
  reason: 'unsupported',
  code: 'ERR_OSSL_EVP_UNSUPPORTED'
}
```

这个问题是由于我跟新nodejs 18版本导致的, 我们需要配置环境变量

> Win

```
$env:NODE_OPTIONS="--openssl-legacy-provider"
```

> Mac/Linux

```
export NODE_OPTIONS=--openssl-legacy-provider
```

## 🌲 ERR_OSSL_EVP_UNSUPPORTED

```
"http" directive is not allowed here in /etc/nginx/conf.d/default.conf:1
```

子文件中不能写`http`

## 🌲 __VUE_PROD_HYDRATION_MISMATCH_DETAILS__

https://blog.csdn.net/weixin_61498310/article/details/137975828

```
runtime-core.esm-bundler.js:4844 Feature flag __VUE_PROD_HYDRATION_MISMATCH_DETAILS__ is not explicitly defined. You are running the esm-bundler build of Vue, which expects these compile-time feature flags to be globally injected via the bundler config in order to get better tree-shaking in the production bundle.

For more details, see https://link.vuejs.org/feature-flags.
```

大体意思就是：

```
此__VUE_PROD_HYDRATION_MISMATCH_DETAILS__ 未被明确定义。您正在运行Vue的esm-bundler构建版本，这个版本期望在编译时通过bundler配置全局注入这些特性标志，以便在生产捆绑包中获得更好的tree-shaking效果。

在Vue 3.4版本中引入了特性标志__VUE_PROD_HYDRATION_MISMATCH_DETAILS__，是一个编译时的特征标志（feature flag），它用于控制在生产环境下服务器端渲染（SSR）过程中hydration（激活）阶段的错误处理行为。
　　在Vue应用进行SSR时，服务器会预先生成HTML字符串发送到客户端，然后客户端的JavaScript执行时会对这个静态HTML进行 hydration，即把静态HTML转换成可响应数据变化的动态DOM。
　　在这个过程中，如果服务器生成的初始HTML结构与客户端Vue组件渲染出的结构不匹配，就会发生hydration不匹配错误。
　　默认情况下，在生产环境中，Vue通常会简化这类错误报告以提高性能和减少包体积。然而，启用 __VUE_PROD_HYDRATION_MISMATCH_DETAILS__ 标志后，即使在生产环境下，当发生hydration不匹配错误时，Vue也会输出详细的错误信息，这对于调试和排查此类问题非常有用。
```

- vite

```js
import { defineConfig } from 'vite'
 
export default defineConfig({
  define: {
    // enable hydration mismatch details in production build
    __VUE_PROD_HYDRATION_MISMATCH_DETAILS__: 'true'
  }
})
```

 - webpack
 
```js
const { defineConfig } = require("@vue/cli-service");
module.exports = defineConfig({
  transpileDependencies: true,
  chainWebpack: (config) => {
    config.plugin('define').tap((definitions) => {
      Object.assign(definitions[0], {
        __VUE_PROD_HYDRATION_MISMATCH_DETAILS__: "false"
      })
      return definitions;
    });
  },
  css: {
    loaderOptions: {
      sass: {
        additionalData: `
        @import "@/assets/css/util.scss";
        @import "@/assets/css/index.scss";
        `,
      },
    },
  },
});
```
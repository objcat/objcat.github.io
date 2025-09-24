# 🍎 简介

Vite（法语意为 "快速的"，发音 /vit/，发音同 "veet"）是一种新型前端构建工具，能够显著提升前端开发体验。它主要由两部分组成：
- 一个开发服务器，它基于 原生 ES 模块 提供了 丰富的内建功能，如速度快到惊人的 模块热替换（HMR）。
- 一套构建指令，它使用 Rollup 打包你的代码，并且它是预配置的，可输出用于生产环境的高度优化过的静态资源。
Vite 是一种具有明确建议的工具，具备合理的默认设置。您可以在 功能指南 中了解 Vite 的各种可能性。通过 插件，Vite 支持与其他框架或工具的集成。如有需要，您可以通过 配置部分 自定义适应你的项目。Vite 还提供了强大的扩展性，可通过其 插件 API 和 JavaScript API 进行扩展，并提供完整的类型支持。你可以在 为什么选 Vite 部分深入了解该项目的设计理念。

https://cn.vite.dev/guide/

# 🍎 快速开始

## 🌲 创建项目

我多写了几个, 你选一个就行

```shell
npm create vite
npm create vite@latest
yarn create vite
```

首先让你起个名

```
Project name: … vite-project
```

然后会问你用什么框架, 可以看到`vite`已经支持这么多了

```shell
yarn create v1.22.19
[1/4] 🔍  Resolving packages...
[2/4] 🚚  Fetching packages...
[3/4] 🔗  Linking dependencies...
[4/4] 🔨  Building fresh packages...
success Installed "create-vite@4.4.1" with binaries:
      - create-vite
      - cva
✔ Project name: … vite-project
? Select a framework: › - Use arrow-keys. Return to submit.
    Vanilla
❯   Vue
    React
    Preact
    Lit
    Svelte
    Solid
    Qwik
    Others
```

我们选择`vue`, 然后会让你选择语言, 我们选择最简单的`js`

```shell
? Select a variant: › - Use arrow-keys. Return to submit.
    TypeScript
❯   JavaScript
    Customize with create-vue ↗
    Nuxt ↗
```

然后我就创建完成了

## 🌲 package.json

我们看一下依赖

```json
{
  "name": "vite-project",
  "private": true,
  "version": "0.0.0",
  "type": "module",
  "scripts": {
    "dev": "vite",
    "build": "vite build",
    "preview": "vite preview"
  },
  "dependencies": {
    "vue": "^3.3.4"
  },
  "devDependencies": {
    "@vitejs/plugin-vue": "^4.2.3",
    "vite": "^4.4.5"
  }
}
```

## 🌲 拉取依赖

```shell
# 使用npm
npm i
# 使用yarn
yarn
```

## 🌲 运行

```shell
# 使用npm
npm run dev
# 使用yarn
yarn dev
```

# 🍎 proxy

这个东西我可要说一说, 因为在开发中经常会用它来跨域, 如果不知道跨域可以在电脑上搜索一下, 我这里就不赘述了

我们接下来直接上例子, 比如我们在浏览器中访问百度

```ts
<script setup lang="tsx">
import axios from 'axios'
axios.get("http://www.baidu.com").then(res => {
  console.log(res);
})
</script>
```

这个代码会报什么错误呢? 答案就是会出现跨域问题

```c
Access to XMLHttpRequest at 'http://www.baidu.com/' from origin 'http://localhost:5173' has been blocked by CORS policy: No 'Access-Control-Allow-Origin' header is present on the requested resource.

App.vue:3   GET http://www.baidu.com/ net::ERR_FAILED 200 (OK)
```

然后我们就可以用这个`proxy`来解决, 我们找到`vite.config.js`

```ts
import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'
import vueJsx from '@vitejs/plugin-vue-jsx'

// https://vite.dev/config/
export default defineConfig({
  plugins: [vue(), vueJsx()],
  server: {
    proxy: {
      '/baidu': {
        target: 'https://www.baidu.com', // 你的后端地址
        changeOrigin: true,              // 是否更改 Origin 头
        rewrite: (path) => path.replace(/^\/baidu/, '') // 去掉 /api 前缀
      }
    }
  }
})
```

我们在`server -> proxy`上配置了一个`/baidu`, 这告诉了`vite`, 当我的接口请求`/baidu`的时候帮我转到`https://www.baidu.com`并帮我免除跨域

- rewrite 这个要重点说一下, 如果不配置这个参数, 转到的地址就是`https://www.baidu.com/baidu`, 这肯定是访问不通的, 所以我们可以发现它`proxy`的规则是发现了固定地址后就在前面拼上`target`, 而我们不想要这个`/baidu`的字符串拼接到我们的地址上, 所以这里给它替换掉了

# 🍎 环境变量

https://cn.vitejs.dev/guide/env-and-mode.html#env-variables

## 🌲 发现错误

由于`vite`上`process`被抛弃了, 我们使用`process.env`会出现错误

```
index.js?t=1688353238312:19 Uncaught ReferenceError: process is not defined
    at index.js?t=1688353238312:19:33
```

文档上有写`vite`默认集成`dotenv`, 所以它会在你的项目中读取三个文件`.env, .env.development, .env.production`

## 🌲 新建配置文件

如果没有这些文件我们需要新建

![](images/Pasted%20image%2020231016125624.png)

## 🌲 书写配置文件

然后随便写个测试key

```
VITE_SOME_KEY=env
```

需要注意的是`vite`规定必须以`VITE_`开头的才会暴漏给客户端

## 🌲 读取配置文件

我们读取环境变量使用`import.meta.env`和`webpack`的`process.env`差不多, 就是前缀不一样

```js
console.log(import.meta.env)
// {BASE_URL: '/', MODE: 'development', DEV: true, PROD: false, SSR: false}
```

切换环境其实比较简单, 就是修改`package.json`中的命令即可, 使用`--mode`来指定环境

```json
"scripts": {
    "dev": "vite",
    "prod": "vite --mode production",
    "build": "vite build",
    "preview": "vite preview --port 4173"
}
```

设置成`production`就是生产环境, `development`就是开发环境

# 🍎 webpack -> vite

本章节主要讲`webpack`切换到`vite`后遇到的问题

## 🌲 获取文件列表

```js
// webpack
const svgFiles = require.context('./svg', true, /\.svg$/)
// vite
const svgFiles = import.meta.globEager('./svg/*.svg');
```

## 🌲 使用svg

https://www.jianshu.com/p/e997dc4eb3cb

文章中说, 在`vite`上不能使用`svg-sprite-loader`插件, 但我目前还没兴趣了解

后来看见了另外一个文章, 说支持`vite`

https://www.jianshu.com/p/4cfd3f17bac3

两个工具分别是
https://www.npmjs.com/package/svg-sprite-loader#examples
https://www.npmjs.com/package/vite-svg-loader

## 🌲 导入

vite不支持`没有后缀名`的导入, 是龙盘着, 是虎卧着

```js
// webpack
import Test from './test';
// vite
import Test from './test.vue';
```

vite仅支持下面的导入方式

## 🌲 递归导入问题

在当前vue文件中导入自己, webpack没问题, vite就会出现下面的报错

```
ReferenceError: Cannot access 'SubMenu' before initialization
```

我的代码是这样的

```js
import SubMenu from './main-sidebar-sub-menu.vue'

components: {
    SubMenu
},
```

这样导入就会报错, 我们可以替换成下面的导入方式

```js
components: {
    SubMenu: () => import("./main-sidebar-sub-menu.vue")
},
```

我们发现报错解决了

# 🍎 创建vue2项目

以为使用vite工具不能创建vue2项目, 但不代表vue2不能使用vite, 我们用`create-vue`就可以创建基于vite的vue2项目了

[跳转 create-vue](../../vue-scaffold/create-vue/create-vue.md)

# 🍎 vue2转vue3工具

据说挺好我还没用

https://github.com/thx/gogocode

# 🍎 FAQ

## 🌲 内网地址无法访问

配置一下`--host`即可, 然后用手机就能访问了 http://192.168.2.191:5173

```
"scripts": {
    "dev": "vite --host 0.0.0.0",
```




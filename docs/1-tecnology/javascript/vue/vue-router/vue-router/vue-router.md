# 🍎 简介

https://router.vuejs.org/zh/

为 Vue.js 提供富有表现力、可配置的、方便的路由

# 🍎 `vue2`与`vue3`配置区别

如果你使用`vue-cli`来创建项目那么官方会默认给你配置好, 新手研究我也推荐从`cli`创建的项目开始, 我们来看一下官方默认配置, 看不懂也没关系, 这段是给能看懂的人看的, 详细学习请看下面的模块

> 🌲 vue2

```js
import Vue from 'vue'
import VueRouter from 'vue-router'
import HomeView from '../views/HomeView.vue'

Vue.use(VueRouter)

const routes = [
  {
    path: '/',
    name: 'home',
    component: HomeView
  },
  {
    path: '/about',
    name: 'about',
    // route level code-splitting
    // this generates a separate chunk (about.[hash].js) for this route
    // which is lazy-loaded when the route is visited.
    component: () => import(/* webpackChunkName: "about" */ '../views/AboutView.vue')
  }
]

const router = new VueRouter({
  mode: 'history',
  base: process.env.BASE_URL,
  routes
})

export default router
```

> 🌲 vue3

我们可以看到打框架没变, 但是小细节很多, `vue3`不需要使用`Vue.use(VueRouter)`

```js
import { createRouter, createWebHistory } from 'vue-router'
import HomeView from '../views/HomeView.vue'

const routes = [
  {
    path: '/',
    name: 'home',
    component: HomeView
  },
  {
    path: '/about',
    name: 'about',
    // route level code-splitting
    // this generates a separate chunk (about.[hash].js) for this route
    // which is lazy-loaded when the route is visited.
    component: () => import(/* webpackChunkName: "about" */ '../views/AboutView.vue')
  }
]

const router = createRouter({
  history: createWebHistory(process.env.BASE_URL),
  routes
})

export default router

```

如果使用`vite`做打包工具, 会出现process找不到的问题, 我们需要用`history: createWebHistory(import.meta.env.BASE_URL)`代替

# 🍎 安装

https://router.vuejs.org/zh/installation.html

> 🌲 vue2

vue2需要使用低版本的, 否则会不兼容

```shell
yarn add vue-router@3.5.2
```

> 🌲 vue3

vue3直接安装最新版本

```shell
npm install vue-router
yarn add vue-router
```

# 🍎 快速开始

我们就使用`vue2`来做演示

## 🌲 新建路由文件夹

配置相对来说比较简单, 我们来看一下, 首先在`src`下面创建一个`router`文件夹, 然后新建一个`index.js`, 然后再创建两个测试页面, 我放在了`page`文件夹中了

![](images/Pasted%20image%2020230626142228.png)

代码如下

```js
import Vue from 'vue'
import VueRouter from 'vue-router'
import Home from '@/page/home.vue'
import About from '@/page/about.vue'

Vue.use(VueRouter);

const routes = [
    {
        path: "/",
        name: "Home",
        component: () => import("@/views/home.vue"),
    },
    {
        path: "/about",
        name: "About",
        component: () => import("@/views/about.vue"),
    },
];

const router = new VueRouter({
	mode: 'history',
	routes: routes
})

export default router
```

- path 访问的路径, 

如果是`/`那么就是根路径 对应的访问地址是 http://localhost
如果是`/about` 对应的访问地址是 http://localhost/about

- name 路由的名称, 我们跳转路由的时候不仅可以使用路径跳转, 也支持使用名称跳转, 所以还是推荐使用名称跳转

- component 路由组件 导入格式为`() => import("@/views/about.vue")`

- mode 可以选两种模式`hash`默认的, 使用这种模式url中会存在`#`, 使用`history`后, `url`就是正常的

新建的测试页面代码如下`home.vue`

```vue
<template>
	<h1>Home</h1>
</template>
```

```vue
<template>
	<h1>About</h1>
</template>
```

## 🌲 配置main.js

然后我们在main.js中配置一下即可

```js
import Vue from 'vue'
import VueRouter from 'vue-router'
import App from './App.vue'
import router from '@/router'

Vue.config.productionTip = false

new Vue({
	router,
	render: h => h(App),
}).$mount('#app')
```

## 🌲 配置App.vue

最后一步是我们要配置一下`<router-view>`组件, 也就是我们的路由入口

```vue
<template>
	<router-view></router-view>
</template>
```

## 🌲 测试

然后我们测试一下

![](images/Pasted%20image%2020230626142703.png)

![](images/Pasted%20image%2020230626142713.png)

这就是最简单的路由配置了

# 🍎 功能

## 🌲 路由

上面已经讲过了, 我们使用`<router-view>`来加载默认路由

```vue
<template>
	<router-view></router-view>
</template>
```

然而我们还有另外一种方式来加载

```vue
<template>
	<router-view v-slot="{ Component }">
        <component :is="Component"></component>
    </router-view>
</template>
```

有些人到这里就看不懂了, 那我就来说一说, 在使用`router-view`时, 我们可以使用`v-slot`来把组件引用过来, 比如`Component`是`home.vue`这个页面, 然后我们使用`<component>`标签使用`:is`来加载这个组件(页面), 这样就达到了路由的效果了, 是不是还挺简单呢

## 🌲 跳转

跳转是我们最常用的功能, 在`vue-router`中使用`<router-link>`来进行跳转

```vue
<template>
	<div>
		<h1>Home</h1>
		<router-link to="/about">about</router-link>
	</div>
</template>
```

比如我在主页中写一个跳转到关于页面的链接, 我们可以看到, to后面接路径即可, 我们还可以使用`this.$router.push`来进行跳转

```vue
<template>
	<div>
		<h1 @click="click">Home</h1>
	</div>
</template>

<script>
	export default {
		methods: {
			click() {
				this.$router.push("/about");
			}
		}
	}
</script>
```

我们也可以根据名字来进行跳转

```vue
<template>
	<div>
		<h1 @click="click">Home</h1>
	</div>
</template>

<script>
	export default {
		methods: {
			click() {
				this.$router.push({"name": 'Home'});
			}
		}
	}
</script>
```

## 🌲 嵌套路由

所谓嵌套路由就是路由下面还有子路由, 我们使用`children`属性来配置子路由, 我们先从最简单的开始学习

### 🌸 基础

子路由顾名思义就是路由下面的路由, 某些情况下我们希望可以在父路由的页面中嵌套一些组件, 来变成一个新页面, 比如我们后台管理系统中点击菜单来切换右边的功能视图, 这个通常就是使用子路由来实现的, 我们在上面的基础上来写例子

首先我们来改造一下页面 我们给`main.vue`外面包裹一个`div`, 给它一个红色的背景, 那么效果就是这样

```vue
<template>
    <div class="home">
        <h1>Home</h1>
    </div>
</template>

<style lang="scss">
    .home {
        background-color: red;
    }

    h1 {
        padding: 0;
        margin: 0;
    }
</style>
```

我们来看一下效果是这样

![](images/Pasted%20image%2020230712150650.png)

然后我们给主页下面配置两个子路由, 新建`book.vue, paint.vue`

```js
const routes = [
    {
        path: "/",
        name: "Home",
        component: () => import("@/views/home.vue"),
        children: [
            {
                path: "book",
                name: "book",
                component: () => import("@/views/book.vue"),
            },
            {
                path: "paint",
                name: "paint",
                component: () => import("@/views/paint.vue"),
            },
        ]
    },
    {
        path: "/about",
        name: "About",
        component: () => import("@/views/about.vue"),
    },
];
```

一个是图书页面

```vue
<template>
    <div style="background-color: cyan;">
        <span>我是图书页面</span>
        <p>鹅鹅鹅， 曲项向天歌。 白毛浮绿水， 红掌拨清波</p>
    </div>
</template>
```

一个是绘画页面

```vue
<template>
    <div style="background-color: cyan">
        <span>我是美术页面</span>
        <img src="@/assets/1.jpg" />
    </div>
</template>
```

配置完我们试着访问一下, 发现没有任何作用

http://localhost:5173/book

![](images/Pasted%20image%2020230712152055.png)

该啥样还是啥样, 你这不是闹呢吗, 快点说下面应该怎么配置, 其实很简单啊, 你`Home`页面是父路由, 你想让子路由显示在父页面上, 就要加上`<router-view>`来嵌入啊, 我们现在就嵌入一下

```vue
<template>
    <div class="home">
        <h1>Home</h1>
        <router-view></router-view>
    </div>
</template>

<style lang="scss">
    .home {
        background-color: red;
    }

    h1 {
        padding: 0;
        margin: 0;
    }
</style>
```

嵌入的`<router-view>`就是子路由, 系统自动给你配的, 我们刷新一下页面发现可以了

![](images/Pasted%20image%2020230712152505.png)

然后我们来访问另外一个绘画页面

![](images/Pasted%20image%2020230712152532.png)

发现页可以正常访问, 那么诱人会问, 如果我不用子路由这个不是也能实现吗, 那它多啥? 答案是方便管理, 你单独去拼接是要比父子路由凌乱的

### 🌸 redirect

在我们使用子路由配置后, 我们发现访问主页还是那个Home

http://localhost:5173

![](images/Pasted%20image%2020230713131956.png)

但是我们主页上只有一个Home的标签, 根本没有内容, 所以会有这样一种需求, 我们想要访问主页直接默认去访问book, 那么就要用到`redirect`重定向了, 很简单, 在路由里直接配置

```js
const routes = [
    {
        path: "/",
        name: "Home",
        redirect: "/book",
        component: () => import("@/views/home.vue"),
        children: [
            {
                path: "book",
                name: "book",
                component: () => import("@/views/book.vue"),
            },
            {
                path: "paint",
                name: "paint",
                component: () => import("@/views/paint.vue"),
            },
        ],
    },
    {
        path: "/about",
        name: "About",
        component: () => import("@/views/about.vue"),
    },
];
```

这次我们访问主页的时候直接就会跳转到`book`的网址了, 值得注意的是`redirect`必须写绝对路径, 而`children`中最好写相对路径, 因为方便, 所谓相对路径就是不加`/`的

http://localhost:5173/book

### 🌸 meta

meta成为路由元信息, 可以存储一些数据, 方便我们在页面中拿到, 我们现在就来配置一下

```js
const routes = [
    {
        path: "/",
        name: "Home",
        redirect: "/book",
        meta: {
            title: "张三"
        },
        component: () => import("@/views/home.vue"),
        children: [
            {
                path: "book",
                name: "book",
                component: () => import("@/views/book.vue"),
            },
            {
                path: "paint",
                name: "paint",
                component: () => import("@/views/paint.vue"),
            },
        ],
    },
    {
        path: "/about",
        name: "About",
        component: () => import("@/views/about.vue"),
    },
];
```

配置好meta后我们在页面中访问一下, 我们使用`this.$route.meta`来访问

```vue
<template>
    <div class="home">
        <h1>Home</h1>
        <router-view></router-view>
    </div>
</template>

<script>
    export default {
        mounted() {
            console.log(this.$route.meta)
            // {title: '张三'}
        }
    }
</script>

<style lang="scss">
    .home {
        background-color: red;
    }

    h1 {
        padding: 0;
        margin: 0;
    }
</style>
```

# 🍎 技巧

## 🌲 封装导入

我们可以定义函数来简化导入, 比如正常导入是这样的

```js
component: () => import('@/views/HomeView.vue')
```

我们也可以改写成这样, 定义一个方法传入名字

```js
const _import = (file) => () => import(`@/views/${file}.vue`)

component: _import("HomeView")
```

这样我们能够导入了

## 🌲 多层导入

随着业务的发展, 我们不可能把所有页面都放在`views`的根目录中, 肯定是要建立文件夹的, 比如我把`HomeView`放入了`main`文件夹, 那么我们要这么引入

```js
component: _import("main/HomeView")
```

但是当我们传入`路径/名字`的时候`vue`会报错, 因为`vite`只支持一层导入

```js
vue-router.esm.js:2316 Error: Unknown variable dynamic import: ../views/main/HomeView.vue
```

想实现多层导入, 我们需要使用`glob`导入法

## 🌲 官方glob导入法(推荐)

对于上述不能导入两层的问题, `vite`也给我们提供了解决方案, 那就是用官方提供的导入方法

```js
// 取出所有的页面
const modules = import.meta.glob("@/views/**/*.vue");
// 按照路径导入
component: modules["/src/views/HomeView.vue"]
```

但是我们发现拼接路径也挺麻烦的, 所以我们可以优化一下

```js
const modules = import.meta.glob("@/views/**/*.vue");
const _import = (file) => modules[`/src/views/${file}.vue`]

component: _import("HomeView")

component: _import("sys/user")

component: _import("product/category")
```

是不是非常爽呢?

我们来打印一下`modules`

```
/src/views/AboutView.vue : () => import("/src/views/AboutView.vue")
/src/views/HomeView.vue : () => import("/src/views/HomeView.vue")
/src/views/product/brand.vue : () => import("/src/views/product/brand.vue")
/src/views/product/category.vue : () => import("/src/views/product/category.vue")
/src/views/sys/role.vue : () => import("/src/views/sys/role.vue")
/src/views/sys/user.vue : () => import("/src/views/sys/user.vue")
```

可以看到它就是个字典, key是`路径`, 值是`导入的组件`

# 🍎 功能

## 🌲 导航守卫

导航守卫是我们拦截跳转的地方

```js
router.beforeEach((to, from, next) => {
  console.log(to);
  console.log(from);
  next();
})
```

我们可以看到导航守卫可以`hook`到每一次的路由跳转, 而且我们从信息中能够看出来, 第一次路由加载的时候是从`name`为`null`的路由跳转到`home`

里面有三个参数, 我们分别来看一下

### 🌸 to 去哪

```json
{"name":"home","meta":{},"path":"/","hash":"","query":{},"params":{},"fullPath":"/","matched":[{"path":"","regex":{"keys":[]},"components":{"default":{"staticRenderFns":[null],"_compiled":true,"__file":"/Users/objcat/project/js/test-vue-scaffold/test-vue2-vite-js-dynamic-router/src/views/HomeView.vue","beforeCreate":[null],"beforeDestroy":[null]}},"alias":[],"instances":{},"enteredCbs":{},"name":"home","meta":{},"props":{}}]}
```

### 🌸  from 从哪来

```json
{"name":null,"meta":{},"path":"/","hash":"","query":{},"params":{},"fullPath":"/","matched":[]}
```

### 🌸  next 跳转方法

跳转方法是为了让路由有动作而产生的, 如果不执行这个`next()`那么路由就不会跳到`home`, 而是在空白页面停留

如果调用`next();`就是执行默认路由, 这个操作是不会再次走`beforeEach`, 我们可以把他认为是`正常跳转并退出`

如果调用`next({name: xxxx});`就是跳转到制定名称的路由, 这个操作会再次走`beforeEach`, 如果不设置跳出的话会死循环

我们可以看下面的例子

```js
router.beforeEach((to, from, next) => {
  console.log(to);
  console.log(from);
  if (to.name == 'home') {
    next({name: "about"});
  } else {
    next();
  }
})
```

我写的逻辑是拦截跳转到`home`的请求, 让它跳转到`about`, 其他请求就正常走, 所以顺序是这样的

```
home
/

about
/
跳出
```

明白了吧, 其实是执行两次路由, 最后通过`next();`来跳出, 如果不理解就多执行几次

# 🍎 动态路由

[跳转 vue-dynamic-router](../vue-dynamic-router/vue-dynamic-router.md)

# 🍎 Vue3

在vue3中router会有所不同

## 🌲 路由跳转

```js
<script setup>
import { RouterLink, RouterView } from 'vue-router'
</script>

<RouterLink to="/">Home</RouterLink>

<RouterView />
```

# 🍎 FAQ

## 🌲 Failed to fetch dynamically imported module

报错如下

```
Upload.vue:21 [vue-router] Failed to resolve async component default: TypeError: Failed to fetch dynamically imported module: http://localhost:5173/src/views/upload/UploadDemo23.vue
```

问题经常出现在找不到`vue`文件, 查看路径下是否有`UploadDemo23.vue`, 如果没有就把路由中的文件路径更改成正确的

## 🌲 Avoided redundant navigation to current location: "/".

报错如下

```shell
vue.runtime.esm.js:4605 [Vue warn]: Error in v-on handler (Promise/async): "NavigationDuplicated: Avoided redundant navigation to current location: "/"."

found in

---> <ElMenuItem> at packages/menu/src/menu-item.vue
       <ElMenu> at packages/menu/src/menu.vue
         <ElCol>
           <ElRow>
             <HomeView> at /Users/objcat/project/Js/test-vue-scaffold/test-vue2-vite-js-dynamic-router/src/views/HomeView.vue
               <App> at /Users/objcat/project/Js/test-vue-scaffold/test-vue2-vite-js-dynamic-router/src/App.vue
                 <Root>
warn @ vue.runtime.esm.js:4605
logError @ vue.runtime.esm.js:3045
globalHandleError @ vue.runtime.esm.js:3041
handleError @ vue.runtime.esm.js:3008
（匿名） @ vue.runtime.esm.js:3019
显示另外 5 个框架
收起
vue.runtime.esm.js:3049 NavigationDuplicated: Avoided redundant navigation to current location: "/".
    at createRouterError (http://localhost:5173/node_modules/.vite/deps/vue-router.js?v=af46055c:1480:15)
    at createNavigationDuplicatedError (http://localhost:5173/node_modules/.vite/deps/vue-router.js?v=af46055c:1454:15)
    at HTML5History2.confirmTransition (http://localhost:5173/node_modules/.vite/deps/vue-router.js?v=af46055c:1705:18)
    at HTML5History2.transitionTo (http://localhost:5173/node_modules/.vite/deps/vue-router.js?v=af46055c:1647:8)
    at HTML5History2.push2 [as push] (http://localhost:5173/node_modules/.vite/deps/vue-router.js?v=af46055c:1911:10)
    at http://localhost:5173/node_modules/.vite/deps/vue-router.js?v=af46055c:2257:24
    at new Promise (<anonymous>)
    at VueRouter2.push (http://localhost:5173/node_modules/.vite/deps/vue-router.js?v=af46055c:2256:12)
    at click (http://localhost:5173/src/views/HomeView.vue?t=1697530454009:24:345)
    at invokeWithErrorHandling (http://localhost:5173/node_modules/.vite/deps/chunk-HAIF5GEN.js?v=af46055c:1968:26)
```

### 🌸 解法1: 忽略错误

问题经常出现在当前页面`push`到当前页面, 导致出错, 我们可以忽略这个错误

```js
import VueRouter from 'vue-router'

Vue.use(VueRouter)

// 忽略重复导航错误
const originalPush = VueRouter.prototype.push
VueRouter.prototype.push = function push(location) {
  return originalPush.call(this, location).catch(err => err)
}
```
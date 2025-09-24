# 🍎 简介

https://router.vuejs.org/zh/guide/advanced/dynamic-routing.html

动态路由就是配置在后台数据库中的路由, 前端请求路由接口后直接加载, 这样可以动态的改变路由页面, 在后台管理系统中很常见

# 🍎 快速开始

## 🌲 基础路由

想配置动态路由就先要有基础路由做支撑, 我们先把静态路由配置完

```js
import Vue from 'vue'
import VueRouter from 'vue-router'

Vue.use(VueRouter)

const modules = import.meta.glob("@/views/**/*.vue");
const _import = (file) => modules[`/src/views/${file}.vue`]

const routes = [
  {
    path: '/',
    name: 'home',
    component: _import("HomeView")
  },
  {
    path: '/about',
    name: 'about',
    // route level code-splitting
    // this generates a separate chunk (About.[hash].js) for this route
    // which is lazy-loaded when the route is visited.
    component: () => import('@/views/AboutView.vue')
  }
];

const router = new VueRouter({
  mode: 'history',
  base: import.meta.env.BASE_URL,
  routes: routes
})

export default router
```

为了方便我们就使用`vue`官方的默认路由

## 🌲 了解动态路由原理

所谓动态路由就是动态配置的路由, 并没有那么高大上, 主要依靠两个函数`router.addRoute() 和 router.removeRoute()`, 到这里有人会说了, 这不就是用方法来添加路由么, 对就是这玩意, 知道原理了我们就可以动手进行配置了

## 🌲 最简单的动态路由

我们在路由守卫中添加一个简单的动态路由先试试, 首先我们新建两个页面, 方便在动态路由中对应

![](images/Pasted%20image%2020231017124051.png)

然后填写内容

- role.vue

```vue
<template>
    <div>
        <h1 @click="$router.push({ name: 'sys-user' })">当前页面role点击跳转到user</h1>
    </div>
</template>
```

- user.vue

```vue
<template>
    <div>
        <h1 @click="$router.push({name: 'sys-role'})">当前页面user点击跳转到role</h1>
    </div>
</template>
```

然后我们针对这两个页面去配置路由

```js
router.beforeEach((to, from, next) => {
  // 到哪去
  console.log("----to", to);
  // 从哪来
  console.log("----from", from);

  // 判断是否加载过动态路由了
  if (router.options.isLoadedDynamicRotes) {
    console.log("正常跳转");
    // 如果加载过直接跳转
    next();
  } else {
    console.log("添加动态路由");

    // 添加第一个动态路由
    router.addRoute({
      path: '/sys/user',
      name: 'sys-user',
      component: _import("sys/user")
    })

    // 添加第二个动态路由
    router.addRoute({
      path: '/sys/role',
      name: 'sys-role',
      component: _import("sys/role")
    })
    // 加载完动态路由后, 把标记置为true, 只要不刷新页面那么都会默认使用配置好的动态路由
    router.options.isLoadedDynamicRotes = true;
    // 加载完动态路由后跳转到当前前往的页面
    next(to);
  }
})
```

够简单么? 使用`router.addRoute`配置的就是动态路由, 然后我们访问一下试试

http://localhost:5173/sys/user

发现是可以访问通的, 点击上面的标题可以跳转到`role`

![](images/Pasted%20image%2020231017124447.png)

但是你要注意的是`next(to);`是个有说法的方法, 如果配置成`next();`那么它什么都不会做, 会在空白页面, 所以我们想让动态路由配置好后就跳转过去要使用`next(to);`

## 🌲 addRoutes

我们也可以使用`addRoutes`来进行添加, 但是这种添加方式会警告`[vue-router] router.addRoutes() is deprecated and has been removed in Vue Router 4. Use router.addRoute() instead.`

```js
let tmpRoutes = []
tmpRoutes.push({
  path: '/sys/user',
  name: 'sys/user',
  component: _import("sys/user")
});

tmpRoutes.push({
  path: '/sys/role',
  name: 'sys/role',
  component: _import("sys/role")
});

router.addRoutes(tmpRoutes);
```

## 🌲 获取静态路由

- 选项式

```js
$router.options.routes.map((item) => {
  console.log(item)
})
```

- 组合式

```ts
<script lang="ts" setup>
import { useRouter } from 'vue-router'
const router = useRouter()
router.options.routes.map((item) => {
  console.log(item)
})
</script>
```

# 🍎 根据后台数据配置简单动态路由

所谓简单路由就是层级不多, 最多是一层, 那么就可以使用这种方法来配置

学了上面的简单路由, 那我们可以继续学习一下根据「后台的菜单数据」来配置路由

## 🌲 准备路由数据

上面如果会了, 那我们就可以模拟一下从服务器读取数据了

因为我们还没有服务器, 所以我们直接写一串`json`来动态配置路由, 实际应用的时候是从服务器请求的

```json
{
  "code": 0,
  "msg": "success",
  "menuList": [{
	"id": 0,
	"name": "系统管理",
	"list": [
	  {
		"id": 1,
		"name": "管理员列表",
		"url": "sys/user"
	  },
	  {
		"id": 2,
		"name": "角色管理",
		"url": "sys/role"
	  }]
  },
  {
	"id": 3,
	"name": "商品系统",
	"list": [
	  {
		"id": 4,
		"name": "分类维护",
		"url": "product/category"
	  },
	  {
		"id": 5,
		"name": "品牌管理",
		"url": "product/brand"
	  },
	  {
		"id": 6,
		"name": "测试空路由",
		"url": ""
	  }]
```

## 🌲 配置路由守卫

那我们就开始配置吧

```js
import Vue from 'vue'
import VueRouter from 'vue-router'

Vue.use(VueRouter)

const modules = import.meta.glob("@/views/**/*.vue");
const _import = (file) => modules[`/src/views/${file}.vue`]

const routes = [
  {
    path: '/',
    name: 'home',
    component: _import("HomeView")
  },
  {
    path: '/about',
    name: 'about',
    // route level code-splitting
    // this generates a separate chunk (About.[hash].js) for this route
    // which is lazy-loaded when the route is visited.
    component: _import("AboutView")
  }
];

const router = new VueRouter({
  mode: 'history',
  base: import.meta.env.BASE_URL,
  routes: routes
})

router.beforeEach((to, from, next) => {
  console.log("----to", to);
  console.log("----from", from);

  // 判断是否加载过动态路由了
  if (router.options.isLoadedDynamicRotes) {
    // 如果加载过直接跳转
    next();
  } else {
    // 如果没有加载过就去加载
    // 模拟网络请求
    new Promise((resolve, reject) => {
      setTimeout(() => {
        const data = {
          "code": 0,
          "msg": "success",
          "menuList": [{
            "id": 0,
            "name": "系统管理",
            "list": [{
              "id": 1,
              "name": "管理员列表",
              "url": "sys/user"
            }, {
              "id": 2,
              "name": "角色管理",
              "url": "sys/role"
            }]
          },
          {
            "id": 3,
            "name": "商品系统",
            "list": [{
              "id": 4,
              "name": "分类维护",
              "url": "product/category"
            },
            {
              "id": 5,
              "name": "品牌管理",
              "url": "product/brand"
            },
            {
              "id": 6,
              "name": "测试空路由",
              "url": ""
            }]
          }]
        }
        resolve(data)
      }, 500);
    }).then((data) => {
      // 网络请求成功后, 我们开始配置路由
      if (data && data.code == 0) {
        // 首先打开动态路由标记, 说明动态路由被加载了
        router.options.isLoadedDynamicRotes = true;
        // 获取menuList路由表
        const menuList = data.menuList
        // 双循环遍历路由
        for (let i = 0; i < menuList.length; i++) {
          let children = menuList[i].list;
          for (let j = 0; j < children.length; j++) {
            let obj = children[j];
            if (obj.url == undefined || obj.url == '' || obj.url == null) {
              console.log(`「${obj.name}」的url为空, 不予加入路由表`);
              continue;
            }
            router.addRoute({
              // 给url前面拼接一个"/", 否则无法跳转, 访问路径 如: /sys/role
              path: "/" + obj.url,
              // 名称设置为url 如: sys/role
              name: obj.url,
              component: _import(obj.url) || null
            },)
          }
        }
        // 保存数据列表
        // sessionStorage.setItem("menu", JSON.stringify(routes.concat(menuList)));
        sessionStorage.setItem("menu", JSON.stringify(menuList));
      }
      next(to);
    })
  }
})

export default router
```

## 🌲 渲染到页面

渲染也比较简单, 我们来做一下

首先我们要在路由下面保存数据

```js
sessionStorage.setItem("menu", JSON.stringify(menuList));
}
next(to);
```

然后在页面打开的时候从`created`中读取即可, 读取完毕后渲染到页面上

```vue
<template>
  <div>
    <el-row class="tac">
      <el-col :span="12">
        <h5>动态路由</h5>
        <el-menu class="el-menu-vertical-demo" @open="handleOpen" @close="handleClose">
          <!-- home 静态路由 -->
          <el-menu-item @click="$router.push({name: 'main', replace: true})">
            <template slot="title">
              <i class="el-icon-s-home"></i>
              <span>home</span>
            </template>
          </el-menu-item>
          <!-- about 静态路由 -->
          <el-menu-item @click="$router.push({name: 'about'})">
            <template slot="title">
              <i class="el-icon-platform-eleme"></i>
              <span>about</span>
            </template>
          </el-menu-item>
          <!-- about 动态路由 -->
          <el-submenu v-for="item in menu" :index="item.id + ''">
            <template slot="title">
              <i class="el-icon-location"></i>
              <span>{{ item.name }}</span>
            </template>
            <el-menu-item v-for="item2 in item.list" @click="itemClick(item2)">{{ item2.name }}</el-menu-item>
          </el-submenu>
        </el-menu>
      </el-col>
    </el-row>
  </div>
</template>

<script>
export default {
  data() {
    return {
      menu: []
    }
  },
  methods: {
    handleOpen(key, keyPath) {
      console.log(key, keyPath);
    },
    handleClose(key, keyPath) {
      console.log(key, keyPath);
    },
    itemClick(item) {
      this.$router.push({name: item.url});
    }
  },
  created() {
    this.menu = JSON.parse(sessionStorage.getItem("menu"));
  }
}
</script>
```

我们可以看到页面上既有静态渲染, 又有动态渲染, 那是因为静态路由和动态路由的配置规则有些不同, 所以还是分开配为好

# 🍎 配置复杂动态路由

所谓复杂路由我是这么定义的

1. 层级比较多, 简单路由只有一层目录, 比如`sys`目录, 目录下就是一个一个的菜单`user, role`, 复杂路由是目录中可能存在子目录, 菜单存在于任何目录中, 这个时候双循环就不行了, 需要使用递归去找

# 🍎 案例

## 🌲 renren

我们来看一下人人是怎么返回动态路由数据的

```json
{
	"msg": "success",
	"menuList": [{
		"menuId": 1,
		"parentId": 0,
		"parentName": null,
		"name": "系统管理",
		"url": null,
		"perms": null,
		"type": 0,
		"icon": "system",
		"orderNum": 0,
		"open": null,
		"list": [{
			"menuId": 2,
			"parentId": 1,
			"parentName": null,
			"name": "管理员列表",
			"url": "sys/user",
			"perms": null,
			"type": 1,
			"icon": "admin",
			"orderNum": 1,
			"open": null,
			"list": []
		}, {
			"menuId": 3,
			"parentId": 1,
			"parentName": null,
			"name": "角色管理",
			"url": "sys/role",
			"perms": null,
			"type": 1,
			"icon": "role",
			"orderNum": 2,
			"open": null,
			"list": []
		}, {
			"menuId": 4,
			"parentId": 1,
			"parentName": null,
			"name": "菜单管理",
			"url": "sys/menu",
			"perms": null,
			"type": 1,
			"icon": "menu",
			"orderNum": 3,
			"open": null,
			"list": []
		}, {
			"menuId": 5,
			"parentId": 1,
			"parentName": null,
			"name": "SQL监控",
			"url": "http://localhost:8080/renren-fast/druid/sql.html",
			"perms": null,
			"type": 1,
			"icon": "sql",
			"orderNum": 4,
			"open": null,
			"list": []
		}, {
			"menuId": 6,
			"parentId": 1,
			"parentName": null,
			"name": "定时任务",
			"url": "job/schedule",
			"perms": null,
			"type": 1,
			"icon": "job",
			"orderNum": 5,
			"open": null,
			"list": []
		}, {
			"menuId": 27,
			"parentId": 1,
			"parentName": null,
			"name": "参数管理",
			"url": "sys/config",
			"perms": "sys:config:list,sys:config:info,sys:config:save,sys:config:update,sys:config:delete",
			"type": 1,
			"icon": "config",
			"orderNum": 6,
			"open": null,
			"list": []
		}, {
			"menuId": 30,
			"parentId": 1,
			"parentName": null,
			"name": "文件上传",
			"url": "oss/oss",
			"perms": "sys:oss:all",
			"type": 1,
			"icon": "oss",
			"orderNum": 6,
			"open": null,
			"list": []
		}, {
			"menuId": 29,
			"parentId": 1,
			"parentName": null,
			"name": "系统日志",
			"url": "sys/log",
			"perms": "sys:log:list",
			"type": 1,
			"icon": "log",
			"orderNum": 7,
			"open": null,
			"list": []
		}]
	}, {
		"menuId": 76,
		"parentId": 0,
		"parentName": null,
		"name": "商品系统",
		"url": "",
		"perms": "",
		"type": 0,
		"icon": "editor",
		"orderNum": 0,
		"open": null,
		"list": [{
			"menuId": 77,
			"parentId": 76,
			"parentName": null,
			"name": "分类维护",
			"url": "product/category",
			"perms": "",
			"type": 1,
			"icon": "menu",
			"orderNum": 0,
			"open": null,
			"list": []
		}, {
			"menuId": 78,
			"parentId": 76,
			"parentName": null,
			"name": "品牌管理",
			"url": "product/brand",
			"perms": "",
			"type": 1,
			"icon": "menu",
			"orderNum": 0,
			"open": null,
			"list": []
		}]
	}],
	"code": 0,
	"permissions": ["sys:schedule:info", "sys:menu:update", "sys:menu:delete", "sys:config:info", "sys:menu:list", "sys:config:save", "sys:config:update", "sys:schedule:resume", "sys:user:delete", "sys:config:list", "sys:user:update", "sys:role:list", "sys:menu:info", "sys:menu:select", "sys:schedule:update", "sys:schedule:save", "sys:role:select", "sys:user:list", "sys:menu:save", "sys:role:save", "sys:schedule:log", "sys:role:info", "sys:schedule:delete", "sys:role:update", "sys:schedule:list", "sys:user:info", "sys:schedule:run", "sys:config:delete", "sys:role:delete", "sys:user:save", "sys:schedule:pause", "sys:log:list", "sys:oss:all"]
}
```
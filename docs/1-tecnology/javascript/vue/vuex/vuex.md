# 🍎 简介

Vuex 是一个专为 Vue.js 应用程序开发的状态管理模式 + 库。它采用集中式存储管理应用的所有组件的状态，并以相应的规则保证状态以一种可预测的方式发生变化。

https://vuex.vuejs.org/zh/

# 🍎 官方默认配置

如果你使用`vue-cli`来创建项目那么官方会默认给你配置好, 我们来看一下`store/index.js`的官方默认配置

## 🌲 vue2

```js
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)

export default new Vuex.Store({
  state: {
  },
  getters: {
  },
  mutations: {
  },
  actions: {
  },
  modules: {
  }
})
```

## 🌲 vue3

我们可以发现vue3中没有`Vue.use(Vuex)`所以不需要引入`Vue`模块这一点让配置比较方便

```js
import { createStore } from 'vuex'

export default createStore({
  state: {
  },
  getters: {
  },
  mutations: {
  },
  actions: {
  },
  modules: {
  }
})
```

# 🍎 简介

https://vuex.vuejs.org/zh/

Vuex 是一个专为 Vue.js 应用程序开发的状态管理模式 + 库。它采用集中式存储管理应用的所有组件的状态，并以相应的规则保证状态以一种可预测的方式发生变化。

# 🍎 快速开始

## 🌲 导入依赖包

-  vue2

注意如果你的vue版本是2, 那么vuex要安装低版本的

```shell
yarn add vuex@3.6.2
```

使用高版本的会报错

```
vue.runtime.esm.js:4605 [Vue warn]: Error in render: "TypeError: Cannot read properties of undefined (reading 'state')"
```

- vue3

如果是vue3就安装最新版本的

```shell
yarn add vuex@next
```

## 🌲 创建Store

- vue2

首先我们在项目`src`目录下创建`store`文件夹, 然后建立`index.js`文件, 在里面创建一个`Vuex.Store`对象并导出

```js
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)

const store = new Vuex.Store({
	state() {
		return {
			name: "张三"
		}
	}
})

export default store
```

![](images/Pasted%20image%2020230621111325.png)

- vue3

在vue3中我们也创建出这个文件, 然后里面的代码有点不一样, 但是好理解

```js
import { createStore } from 'vuex'

const store = createStore({
  state: {
    name: "张三",
    age: 18
  }
})

export default store
```

## 🌲 在main.js中使用

- vue2

没啥好说的就是把`store`引入进来, 然后配置到`Vue`对象上

```js
import Vue from 'vue'
import App from './App.vue'
import store from '@/store'

Vue.config.productionTip = false

new Vue({
	store,
	render: h => h(App),
}).$mount('#app')
```

- vue3

```js
import { createApp } from 'vue'
import App from './App.vue'
import store from '@/store'

createApp(App).use(store).mount('#app')
```

## 🌲 使用变量

- vue2

最简单的使用方法是直接通过`$store`调用

```vue
<template>
	<h1>{{ this.$store.state.name }}</h1>
	<h1>{{ this.$store.state["name"] }}</h1>
</template>

<script>

</script>

<style>

</style>
```

如果读取错误就是版本高了

- vue3

vue3在store的使用上有点不一样, 因为没有this

```vue
<template>
    <h1>{{ $store.state.name }}</h1>
    <h1>{{ $store.state["name"] }}</h1>
</template>

<script setup>

</script>

<style lang="scss" scoped></style>
```

## 🌲 改变变量

改变名字我们在`h1`上添加一个点击方法, 然后直接对name赋值就可以了

- vue2

```vue
<template>
	<h1 @click="click">{{ this.$store.state.name }}</h1>
</template>

<script>
	import { mapState } from 'vuex'
	export default {
		methods: {
			click() {
				this.$store.state.name = "李四";
				// 或
				this.$store.state["name"] = "李四";
			}
		}
	}
</script>

<style>

</style>
```

但是我们这么做会有一个小问题, 那就是不规范, 在严格模式下系统会给我们报错, 我们把`store`设置成严格模式

```js
const store = new Vuex.Store({
	state() {
		return {
			name: "张三"
		}
	},
	strict: true
})

export default store
```

```shell
vue.runtime.esm.js:4605 [Vue warn]: Error in callback for watcher "function () { return this._data.$$state }": "Error: [vuex] do not mutate vuex store state outside mutation handlers."

(found in <Root>)
warn @ vue.runtime.esm.js:4605
logError @ vue.runtime.esm.js:3045
globalHandleError @ vue.runtime.esm.js:3041
handleError @ vue.runtime.esm.js:3008
invokeWithErrorHandling @ vue.runtime.esm.js:3024
Watcher.run @ vue.runtime.esm.js:3534
Watcher.update @ vue.runtime.esm.js:3510
Dep.notify @ vue.runtime.esm.js:720
reactiveSetter @ vue.runtime.esm.js:954
click @ App.vue:16
invokeWithErrorHandling @ vue.runtime.esm.js:3017
invoker @ vue.runtime.esm.js:1815
original_1._wrapper @ vue.runtime.esm.js:7473
vue.runtime.esm.js:3049 Error: [vuex] do not mutate vuex store state outside mutation handlers.
    at assert (vuex.esm.js:135:1)
    at store._vm.$watch.deep (vuex.esm.js:893:1)
    at invokeWithErrorHandling (vue.runtime.esm.js:3017:1)
    at Watcher.run (vue.runtime.esm.js:3534:1)
    at Watcher.update (vue.runtime.esm.js:3510:1)
    at Dep.notify (vue.runtime.esm.js:720:1)
    at Object.reactiveSetter [as name] (vue.runtime.esm.js:954:1)
    at VueComponent.click (App.vue:16:1)
    at invokeWithErrorHandling (vue.runtime.esm.js:3017:1)
    at HTMLHeadingElement.invoker (vue.runtime.esm.js:1815:1)
```

怎么解决呢?  很简单`vuex`规定改变变量的值需要使用`commit`来操作, 我们需要改写一下`store`

```js
const store = new Vuex.Store({
	state() {
		return {
			name: "张三"
		}
	},
	mutations: {
		xset_name(state, name) {
			state.name = name
		}
	},
	strict: true
})

export default store
```

我们添加了一个`mutations`模块, 这个里面是写方法的同`methods`类似, 然后我们添加一个改变名称的方法`xset_name`

然后我们就可以在代码里面使用了

```vue
<template>
	<h1 @click="click">{{ this.$store.state["name"] }}</h1>
</template>

<script>
	import { mapState } from 'vuex'
	export default {
		methods: {
			click() {
				this.$store.commit("xset_name", "李四");
			}
		}
	}
</script>

<style>
```

这就是我们最基础的使用`vuex`的教程了

- vue3

vue3中store的写法几乎没变, 所以我们只看代码里要怎么写

```vue
<template>
    <h1 @click="click">{{ $store.state.name }}</h1>
</template>

<script setup>
    import { useStore } from "vuex";
    const store = useStore();
    function click() {
        store.commit("xset_name", "李四");
    }
</script>

<style lang="scss" scoped></style>

```

# 🍎 模块

## 🌲 state

### 🌸 mapState

我们可以通过`mapState`辅助函数让使用变量变得更容易, 我们首先添加一个新的变量`age`

```js
const store = new Vuex.Store({
	state() {
		return {
			name: "张三",
			age: 18
		}
	},
	mutations: {
		xset_name(state, name) {
			state.name = name
		}
	},
	strict: true
})

export default store
```

使用方法是直接把`mapState`用在计算属性上

```vue
<template>
	<h1 @click="click">{{ name + age }} </h1>
</template>

<script>
	import { mapState } from 'vuex'
	export default {
		methods: {
			click() {
				this.$store.commit("xset_name", "李四");
			}
		},
		computed: mapState(['name', 'age'])
	}
</script>

<style>

</style>
```

### 🌸 ...mapState

上面的写法还行, 但是有点问题就是它会占用整个`computed`, 我们可以使用`对象展开运算符`来优化一下

```vue
<template>
	<h1 @click="click">{{ name + age }} </h1>
</template>

<script>
	import { mapState } from 'vuex'
	export default {
		methods: {
			click() {
				this.$store.commit("xset_name", "李四");
			}
		},
		computed: {
			...mapState(['name', 'age'])
		}
	}
</script>

<style>

</style>
```

### 🌸 mapState中实现计算属性

有时候我们赋值之前需要再处理一下, 我们可以这么写, 我们把`mapState`中的`小括号`变成`花括号`, 然后我们就可以描述更多信息了

```vue
<template>
	<h1 @click="click">{{ name + age }} </h1>
</template>

<script>
	import { mapState } from 'vuex'
	export default {
		data() {
			return {
				"role": "同学"
			}
		},
		methods: {
			click() {
				this.$store.commit("xset_name", "李四");
			}
		},
		computed: {
			...mapState({
				name: function(state) {
					return state.name + this.role
				},
				age: state => state.age
			})
		}
	}
</script>

<style>

</style>
```

比如我得name定义为一个函数, 里面对返回值进行处理

## 🌲 Mutations

我们可以使用它简化方法的调用

### 🌸 mapMutations

可以看到与`mapState`类似, 我们在`methods`中使用`mapMutations`函数后, 就可以正常的使用`xset_name`方法了, 而无需`commit`

```vue
<template>
	<h1 @click="click">{{ name + age }} </h1>
</template>

<script>
	import { mapState, mapMutations } from 'vuex'
	export default {
		methods: {
			...mapMutations(['xset_name']),
			click() {
				this.xset_name("李四")
			}
		},
		computed: {
			...mapState(['name', 'age'])
		}
	}
</script>

<style>

</style>
```

我们也可以使用花括号的形式给方法起别名

```vue
<template>
	<h1 @click="click">{{ name + age }} </h1>
</template>

<script>
	import { mapState, mapMutations } from 'vuex'
	export default {
		methods: {
			...mapMutations({setName: 'xset_name'}),
			click() {
				this.setName("李四")
			}
		},
		computed: {
			...mapState(['name', 'age'])
		}
	}
</script>

<style>

</style>
```

不多废话了, 太他吗简单了

### 🌸 commit

在上面的快速开始已经学过了, 执行方法需要使用`commit`

```
this.$store.commit("xset_name", "李四");
```

## 🌲 Actions

### 🌸 mapActions

Actions顾名思义也是方法的意思, 不过Actions强调的是一组动作

```js
const store = new Vuex.Store({
	state() {
		return {
			name: "张三",
			age: 18
		}
	},
	mutations: {
		xset_name(state, name) {
			state.name = name
		},
		xset_age(state, age) {
			state.age = age
		}
	},
	actions: {
		changeUserInfo(context, arr) {
			context.commit('xset_name', arr[0]);
			context.commit('xset_age', arr[1]);
		}
	},
	strict: true
})

export default store
```

比如这里我们创建一个`changeUserInfo`方法来改变两个变量的值, 然后我们看代码页面

```vue
<template>
	<h1 @click="click">{{ name + age }} </h1>
</template>

<script>
	import { mapState, mapMutations, mapActions } from 'vuex'
	export default {
		methods: {
			...mapActions(['changeUserInfo']),
			click() {
				this.changeUserInfo(["李四", 20])
			}
		},
		computed: {
			...mapState(['name', 'age'])
		}
	}
</script>

<style>

</style>
```

我们直接使用`changeUserInfo`就可以同时修改两个值了, 这算是封装的一种, 可以把一堆操作都放在`vuex`中处理, 比如登录之后保存用户资料可能需要很多步骤, 如此就能简化我们的操作了


### 🌸 dispatch

如果不使用辅助函数的话, 我们执行动作需要使用`dispatch`

```js
this.$store.dispatch('changeUserInfo', ["李四", 20])
```

## 🌲 modules

有时候`store`比较大, 为了方便做功能拆分我们可能会按功能分成几个模块

### 🌸 创建模块

创建模块比较简单, 我们一起来看看

创建`user.js`, 在`store`文件夹下创建一个`modules`文件夹然后创建`js`

```js
export default {
	namespaced: true,
	state: {
		name: "张三",
		age: 18
	},
	mutations: {
		xset_name(state, name) {
			state.name = name
		},
		xset_age(state, age) {
			state.age = age
		}
	},
	actions: {
		changeUserInfo(context, arr) {
			context.commit('xset_name', arr[0]);
			context.commit('xset_age', arr[1]);
		}
	}
}
```

把`user.js`加载到主模块上

```js
import Vue from 'vue'
import Vuex from 'vuex'
import user from './modules/user'

Vue.use(Vuex)

const store = new Vuex.Store({
	modules: {
		user
	},
	strict: true,
})

export default store
```

文件目录如图, 可以看到我们是在`store`下面创建了一个`modules`文件夹

![](images/Pasted%20image%2020230625133813.png)

### 🌸 state

```js
this.$store.state.user.name
// 或
...mapState('user', ['name'])
<h1 @click="click">{{ name }} </h1>

```

### 🌸 mutations

```js
this.$store.commit('user/xset_name', "李四");
// 或
...mapMutations('user', ['xset_name'])
his.xset_name("李四")
```

### 🌸 actions

```js
this.$store.dispatch('user/changeUserInfo', ["李四", 18])
// 或
...mapActions('user', ['changeUserInfo'])
this.changeUserInfo(["李四", 18])
```




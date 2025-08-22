# 🍎 简介

Vue (发音为 /vjuː/，类似 view) 是一款用于构建用户界面的 JavaScript 框架。它基于标准 HTML、CSS 和 JavaScript 构建，并提供了一套声明式的、组件化的编程模型，帮助你高效地开发用户界面。无论是简单还是复杂的界面，Vue 都可以胜任。

# 🍎 版本

`Vue`分为2和3两个版本, 前者使用比较多, 市面上会的人也多, 后者比较新, 语法有改动, 并且引入了新的API风格, 作为前端来说需要同时掌握以应对不同的开发环境

# 🍎 项目分类

`Vue`项目分为两种形式`单页面`和`脚手架`, 所谓单页面就是在`HTML`中使用`<script></script>`标签进行引入`vue.js`这种适合单个页面进行开发的项目, 而另外一种方式是使用`Vue`官方给我们提供的一个项目结构进行开发, 适合比较大的多页面项目场景, 第二种功能上是更全面的, 本文也会针对这两种形式进行说明

# 🍎 IDE

这里推荐`HbuilderX`, 是国内对`vue`支持比较好的, 而且他们的`uni-app`可以开发小程序和移动端(移动端不推荐使用uni-app开发问题较多), 我们首先下载`HbuilderX`

https://www.dcloud.io/hbuilderx.html

其他的IDE还有`vscode`和`webstorm`, 前者用途广泛是我们工作中要使用的, 但是它不适合初学因为配置插件比较麻烦, 所以我们先学会用一个, 学会了使用其他的都不难

# 🍎 API风格

详情可以参考官网上的介绍

https://cn.vuejs.org/guide/introduction.html#api-styles

## 🌲 选项式(Options API)

所谓选项式就是`vue2`一直以来的写法, 当然`vue3`也可以写, 即使用`el, data, methods...`分为若干个模块然后进行编码的方式, 这种方式的优点在于会的人多, 写起来熟练

```html
<body>
	<div id="app">
		<h1>{{ title }}</h1>
	</div>
	<script type="text/javascript">
		new Vue({
			el: "#app",
			data() {
				return {
					title: "hello world!"
				}
			}
		});
	</script>
</body>
```

## 🌲 组合式(Composition API)

组合式是vue3提出的, 利用`setup`语法把变量, 方法, 组件等逻辑写在一起, 在可读性上可能有一些提升

组合式分为脚手架和非脚手架, 非脚手架就是单页面的, 脚手架就是vue给我们提供的开发环境, 可以更好的利用组件

### 🌸 非脚手架

非脚手架就是应用于单页面的vue3写法, 我们可以看到利用`setup()`函数就能方便的定义变量了, 而不需要使用`data()`

```html
<html>
	<head>
		<meta charset="utf-8" />
		<title></title>
		<script src="https://cdn.bootcdn.net/ajax/libs/vue/3.2.45/vue.global.prod.min.js"></script>
	</head>
	<body>
		<div id="app">
			<h1>{{ title }}</h1>
		</div>
		<script type="text/javascript">
			Vue.createApp({
				setup() {
					let title = "hello";
					return {
						title
					}
				}
			}).mount("#app");
		</script>
	</body>
</html>
```

### 🌸 脚手架

在`script`标签上添加setup是脚手架专用的形式, 单页面是不能使用的, 这也是vue3的主流写法

```html
<template>
	<h1>{{ title }}</h1>
</template>

<script setup>
	let title = "hello";
</script>

<style scoped>

</style>
```

同样的我们的脚手架也可以使用`setup()函数`的那种写法, 只不过不常用, 我们只需要去掉`setup`属性就能回归选项式写法了, 当然我们还需要借助`defineComponent`来开辟一块空间来写代码

```vue
<template>
	<h1>{{ title }}</h1>
</template>

<script>
	export default {
        setup() {
            let title = "hello";
            return {
                title
            }
        }
    }
</script>

<style scoped>

</style>
```

或

```html
<template>
	<h1>{{ title }}</h1>
</template>

<script lang="ts">
	import { defineComponent } from 'vue';
	export default defineComponent({
		setup() {
			let title = "hello";
			return {
				title
			}
		}
	})
</script>

<style scoped>

</style>
```

上面的代码仅仅是为了表达区分`选项式`和`组合式`的区别, 如果还没有真正开始学习, 所以看不懂也没关系, 下面才是真正开始学习

# 🍎 基础

学习的话最好是从简单的单页面开始, 也就是html内嵌vue的方式, 因为不需要搭建任何环境就能够运行, 学习起来比较方便, 这也是vue对比react方便的地方, react是不支持这种单页面嵌入开发的

## 🌲 新建项目

我们打开`HbuilderX`新建一个普通项目, 我们这里就选最基础的`基本HTML`项目, 目的是方便学习

![](images/Pasted%20image%2020221128134358.png)

创建后结构是这个样子

![](images/Pasted%20image%2020221128134301.png)

## 🌲 外链嵌入vue.js

新建完毕后我们就可以嵌入vue了, 下面的例子为多版本演示方便对比, 一般我们外链嵌入是需要在下面两个网址里搜索链接

https://cdn.bootcdn.net

https://www.jsdelivr.com

> vue2

我们翻阅官方文档

https://v2.cn.vuejs.org/v2/guide/installation.html#CDN

可以在上面的网站查看到官方推荐的版本, 写文章的时候官方推荐的是`2.7.10`这个版本, 该版本会随着时间更新, 所以学习的时候可以根据上面的网站跟新版本, 引入的方式也有两种, 一种是cdn加速, 一种是本地文件, 前者方便学习但不稳定, 后者存放在自己的项目目录比较稳定, 适合生产环境, 我们先使用前者学习

我们想嵌入vue只需要在页面中引入下面的代码即可

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8" />
		<title></title>
		<script src="https://cdn.bootcdn.net/ajax/libs/vue/2.7.10/vue.min.js"></script>
	</head>
	<body>
		
	</body>
</html>
```

> vue3

vue3嵌入环境也很简单, 仅仅是换了一个地址, 需要注意的是这个地址需要是`vue.global`开放组件的, 否则可能某些功能无法使用

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8" />
		<title></title>
		<script src="https://cdn.bootcdn.net/ajax/libs/vue/3.2.45/vue.global.prod.min.js"></script>
	</head>
	<body>
		
	</body>
</html>
```

## 🌲 本地嵌入vue.js

本地嵌入只需要打开网址把`js`代码复制到本地即可

https://cdn.bootcdn.net/ajax/libs/vue/2.7.10/vue.min.js

我们打开上面的网站, 复制代码, 然后在本地新建一个vue.min.js

![](images/Pasted%20image%2020221129134838.png)

然后我们直接在index.html中引入即可

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8" />
		<title></title>
		<script src="js/vue.min.js"></script>
	</head>
	<body>
	
	</body>
</html>
```

我们会发现加载数据会稍微快一点, 因为本地的不需要访问外网就能使用, vue3的我就不演示了, 在本地创建个`vue.global.prod.min.js`文件然后把vue3的代码拷贝过来就行

## 🌲 创建Vue实例

https://v2.cn.vuejs.org/v2/guide/instance.html#创建一个-Vue-实例

我们想使用vue的功能光引入是不够的, 我们需要在项目中创建出vue实例来管理我们的页面

> vue2

在vue2中我们使用`new Vue({});`来创建一个vue实例

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8" />
		<title></title>
		<script src="https://cdn.jsdelivr.net/npm/vue@2.7.10/dist/vue.js"></script>
	</head>
	<body>
		<script type="text/javascript">
			new Vue({
				
			});
		</script>
	</body>
</html>
```

我们也可以打印出这个实例看一下, 我们定义一个vm来接收Vue实例

```js
const vm = new Vue({
	
});
console.log(vm);
```

![](images/Pasted%20image%2020221128140117.png)

可以看到Vue创建成功了, 里面的东西我们会慢慢去学

> vue3

我们可以看到Vue3不再采用`new`, 而是使用`Vue.createApp`方法来创建实例

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
		<script src="https://cdn.bootcdn.net/ajax/libs/vue/3.2.45/vue.global.prod.js"></script>
	</head>
	<body>
		<div id="app">
			{{title}}
		</div>
		<script type="text/javascript">
			Vue.createApp({

			});
		</script>
	</body>
</html>
```

## 🌲 el

`el即element(元素)`在我们网页的开发中用来指标签, 是用来确认我们vue实例作用范围的一个属性, 我们需要配置它来指定vue作用于哪些标签, 一般情况下我们会配置一个id为app的div作为根标签

> vue2

```html
<body>
	<div id="app">
		
	</div>
	<script type="text/javascript">
		new Vue({
			el: "#app"
		});
	</script>
</body>
```

这样就指定了根标签是`id="app"`的div, 我们的作用域也在这里, 其他地方vue不能监管到

> vue3

在vue3中, 我们使用`mount`来绑定根标签, 它跟el的作用一样

```html
<body>
	<div id="app">
		
	</div>
	<script type="text/javascript">
		Vue.createApp({
		
		}).mount("#app");
	</script>
</body>
```

## 🌲 data

> vue2

data顾名思义就是我们数据变量存放的地方, 我们可以在vue结构中定义并使用它, 下面我们来声明一个`hello world!`

```html
<body>
	<div id="app">
		<h1>{{ title }}</h1>
	</div>
	<script type="text/javascript">
		new Vue({
			el: "#app",
			data() {
				return {
					title: "hello world"
				}
			}
		});
	</script>
</body>
```

我们使用`data()`来定义一个data函数, 然后在return里面定义对象, 对象里面的属性叫做`title`, 这个属性的值是`hello world!`

> vue3

我们可以看到vue3也可以使用`data()`来管理数据

```html
<body>
	<div id="app">
		<h1>{{ title }}</h1>
	</div>
	<script type="text/javascript">
		Vue.createApp({
			data() {
				return {
					title: "hello world"
				}
			}
		}).mount("#app");
	</script>
</body>
```

> vue3 setup

我们可以看到使用setup语法需要使用`return`把变量暴露出去, 否则不生效

```html
<body>
	<div id="app">
		<h1>{{ title }}</h1>
	</div>
	<script type="text/javascript">
		Vue.createApp({
			setup() {
				let title = "hello world";
				return {
					title
				}
			}
		}).mount("#app");
	</script>
</body>
```

> vue3 setup 脚手架

在脚手架中我们可以使用另外一种语法糖写法, 我们可以看到这种更加方便, 脱离了选项式的结构, 所以称为组合式

```html
<template>
	<h1>{{ title }}</h1>
</template>

<script lang="ts" setup>
	let title = "hello world";
</script>

<style scoped>

</style>
```

## 🌲 方法

> vue2

我们可以使用`methods`在vue中定义方法

```html
<body>
	<div id="app">
		{{ hello() }}
	</div>
	<script type="text/javascript">
		new Vue({
			el: "#app",
			data() {
				return {
					title: "hello world!"
				}
			},
			methods: {
				hello() {
					return "hello";
				}
			}
		});
	</script>
</body>
```

我们定义了一个叫`hello`的方法, 然后在里面返回`hello`, 之后我们同样可以在模板语法里面调用这个方法渲染值, 效果是这样的

![](images/Pasted%20image%2020221228091541.png)

> vue3 setup

我们可以看到使用`setup`语法把值和函数都能写到一起了, 阅读起来非常方便, 我们定义方法后需要在`return`中把方法暴露出去, 否则html中是无法使用的

```html
<body>
	<div id="app">
		{{ hello() }}
	</div>
	<script type="text/javascript">
		Vue.createApp({
			setup() {
				let hello = () => {
					return "hello";
				}
				return {
					title: "hello vue3",
					hello
				}
			}
		}).mount("#app");
	</script>
</body>
```

这种写法虽然看起来很方便, 但是有一个非常大的问题就是值不是响应式的, 也无法访问this, 因此我们如果想实现响应式数据需要使用Vue提供的类`ref`, 这里只是简单演示, 在ref章节会详细来说

```vue
<body>
	<div id="app">
		<h1 @click="click">{{ title }}</h1>
	</div>
	<script type="text/javascript">
		Vue.createApp({
			setup() {
				let title = Vue.ref("hello world")
				let click = () => {
					console.log("123");
					title.value = "123456";
				}
				return {
					title,
					click
				}
			}
		}).mount("#app");
	</script>
</body>
```

因为单页面嵌入vue在setup上面的文档极少, 所以单页面还是推荐使用选项式来写, 而脚手架使用vue3则推荐组合式

## 🌲 计算属性

计算属性和方法差不多, 但是它不能有参数, 只是对内部变量进行处理的一个方法, 比如我们定义了一个time变量, 想要在打印hello world的时候拼接上时间

> vue2

```html
<body>
	<div id="app">
		{{ hello }}
	</div>
	<script type="text/javascript">
		new Vue({
			el: "#app",
			data() {
				return {
					title: "hello world!",
					time: "2022年11月28日14:39:30"
				}
			},
			computed: {
				hello() {
					return this.title + " " + this.time;
				}
			}
		});
	</script>
</body>
```

值得注意的是, 计算属性不需要使用`()`来调用, 直接使用方法名即可

> vue3 setup

```html
<body>
	<div id="app">
		<h1>{{ hello }}</h1>
	</div>
	<script type="text/javascript">
		console.log(Vue);
		Vue.createApp({
			setup() {
				let title = "hello world!";
				let time = "2022年11月28日14:39:30";

				let hello = Vue.computed(() => {
					return title + " " + time;
				})

				return {
					title,
					hello
				}
			}
		}).mount("#app");
	</script>
</body>
```

> vue2 脚手架

计算属性中也可以使用getter/setter我们来看一下

```vue
<template>
  <h1 @click="click">{{hello}}</h1>
</template>

<script>

export default {
  data() {
    return {
      name: "张三"
    }
  },
  methods: {
    click() {
      this.name = "李四"
    }
  },
  computed: {
    hello: {
      get() {
        return this.name
      },
      set(val) {
        this.name = val
      }
    }
  }
}
</script>
```

> vue3 setup 脚手架

```html
<template>
	<h1>{{ hello }}</h1>
</template>

<script lang="ts" setup>
	import { computed } from 'vue'
	let title = "hello world!";
	let time = "2022年11月28日14:39:30";

	let hello = computed(() => {
		return title + " " + time;
	})
</script>

<style scoped>

</style>
```

## 🌲 监听属性

我们使用watch来监听data中声明的值, 名称必须和data里面的值一样, 如例子所示当number被修改时会在watch的函数中进行回调

> vue3

```html
<body>
	<div id="app">
		<h1>{{ number }}</h1>
		<button @click="number++">+1</button>
	</div>
	<script type="text/javascript">
		Vue.createApp({
			data() {
				return {
					number: 0
				}
			},
			watch: {
				number(newValue, oldValue) {
					console.log("新值: " + newValue + " 旧值: " + oldValue);
				}
			},
		}).mount("#app");
	</script>
</body>
```

> vue3 setup

在setup语法中watch的使用有点不同

```html
<body>
	<div id="app">
		<h1 @click="change">{{ number }}</h1>
	</div>
	<script type="text/javascript">
		Vue.createApp({
			setup() {
				let number = Vue.ref(0)
				
				Vue.watch(number, (newValue, oldValue) => {
					console.log("新值: " + newValue + " 旧值: " + oldValue);
				})
				
				let change = () => {
					number.value++;
				}

				return {
					number,
					change
				}
			}
		}).mount("#app");
	</script>
</body>
```

> vue3 setup 脚手架

我们再来看一下脚手架的形式

```html
<template>
	<h1 @click="change">{{ number }}</h1>
</template>

<script lang="ts" setup>
	import { ref, watch } from 'vue'

	let number = ref(0)

	watch(number, (newValue, oldValue) => {
		console.log("新值: " + newValue + " 旧值: " + oldValue);
	})

	let change = () => {
		number.value++;
	}
</script>

<style scoped>

</style>
```

## 🌲 监听属性

# 🍎 语法

下面是一些常用的语法, 这里使用最新的vue3进行演示, vue2与之相差无几

## 🌲 {{}}

`{{}}即模板插值语法`, 把vue管理的数据渲染到`element`上, 我们在hello world章节已经学习过了

## 🌲 v-text

与`模板插值`一样, 我们可以使用`v-text`来渲染内容

```html
<body>
	<div id="app">
		<h1 v-text="title"></h1>
	</div>
	<script>
		Vue.createApp({
			data() {
				return {
					title: "hello world!"
				}
			}
		}).mount('#app');
	</script>
</body>
```

## 🌲 v-html

可以渲染上去一段html

```html
<body>
	<div id="app">
		<h1 v-html="title"></h1>
	</div>
	<script>
		Vue.createApp({
			data() {
				return {
					title: "<h1>你好 我是html</h1>"
				}
			}
		}).mount('#app');
	</script>
</body>
```

## 🌲 v-on

`v-on即事件绑定指令`, 我们可以使用它轻松的绑定`点击方法`等事件

https://www.jianshu.com/p/0b95a319a7c7

我们使用click来指定点击事件, 有三种写法, `v-on:事件`, `@事件`, `@[动态事件]`, 第二种是第一种的缩写, 第三种可以绑定动态的事件名称

```html
<body>
	<div id="app">
		<h1 v-on:click="go">普通点击</h1>
		<h1 @click="go">点击缩写</h1>
		<h1 @[event_name]="go">动态绑定</h1>
	</div>
	<script>
		Vue.createApp({
			data() {
				return {
					event_name: "click"
				}
			},
			methods: {
				go() {
					console.log("go方法执行")
				}
			}
		}).mount('#app');
	</script>
</body>
```

事件绑定更多细节请看文章最下方脚手架详解

## 🌲 v-bind

`v-bind即绑定指令`, 我们可以使用它来绑定一些属性, 比如`class`或`id`, 让vue拥有动态给html标签上绑定属性的能力, 我们一起来看看吧

### 🌸 动态绑定id

我们使用`v-bind`来实现动态绑定, 很简单在需要绑定的属性前面加上`v-bind:`这个属性就变成了一个动态的

```html
<body>
	<div id="app">
		<h1 v-bind:id="id">{{ title }}</h1>
	</div>
	<script type="text/javascript">
		Vue.createApp({
			data() {
				return {
					title: "hello world!",
					id: "test-id"
				}
			}
		}).mount("#app");
	</script>
</body>
```

但是由于`v-bind`过于冗长, vue也提供了我们第二种写法, 就是可以省略前面的`v-bind`

```html
<body>
	<div id="app">
		<h1 :id="id">{{ title }}</h1>
	</div>
	<script type="text/javascript">
		Vue.createApp({
			data() {
				return {
					title: "hello world!",
					id: "test-id"
				}
			}
		}).mount("#app");
	</script>
</body>
```

这样写起来就比较清爽了

### 🌸 动态绑定class

原理和动态绑定id一样

```html
<body>
	<div id="app">
		<h1 :class="cls">{{ title }}</h1>
	</div>
	<script type="text/javascript">
		Vue.createApp({
			data() {
				return {
					title: "hello world!",
					cls: "test-class"
				}
			}
		}).mount("#app");
	</script>
</body>
```

### 🌸 动态绑定style

```html
<body>
	<div id="app" >
		<div :style="style"></div>
	</div>
	<script type="text/javascript">
		Vue.createApp({
			data() {
				return {
					title: "hello",
					style: {
						"background-color": "red",
						width: "100px"
					},
					height: "200px"
				}
			}
		}).mount("#app");
	</script>
</body>
```

我们也可以动态绑定单个`style`

```html
<html>
	<head>
		<meta charset="utf-8" />
		<title></title>
		<script src="https://cdn.bootcdn.net/ajax/libs/vue/3.2.45/vue.global.prod.min.js"></script>
		<style>
			.test-div {
				background-color: red;
				width: 100px;
				height: 100px;
			}
		</style>
	</head>
	<body>
		<div id="app">
			<div class="test-div" :style="{'background-color': color}"></div>
		</div>
		<script type="text/javascript">
			Vue.createApp({
				data() {
					return {
						title: "hello",
						color: "blue"
					}
				}
			}).mount("#app");
		</script>
	</body>
</html>
```

### 🌸 动态绑定多个属性

我们可以自己构造对象然后使用`v-bind`绑定多个属性

```html
<body>
	<div id="app">
		<h1 v-bind="attributed">{{ title }}</h1>
	</div>
	<script type="text/javascript">
		Vue.createApp({
			data() {
				return {
					title: "hello world!",
					attributed: {
						id: "test-id",
						class: "test-class"
					}
				}
			}
		}).mount("#app");
	</script>
</body>
```

### 🌸 拼接属性

```
我们使用 `xxx-${变量}` 来动态拼接属性 我们直接看下面的例子 我利用数组中的数字来动态生成了每个h1的属性
```

```html
<body>
	<div id="app">
		<h1 v-for="item in arr" :class="`test-class-${item}`">{{ item }}</h1>
	</div>
	<script type="text/javascript">
		Vue.createApp({
			data() {
				return {
					title: "hello world!",
					arr: [1, 2, 3, 4]
				}
			}
		}).mount("#app");
	</script>
</body>
```

生成的html是这个样子的

![](images/Pasted%20image%2020221229095627.png)

## 🌲 v-if

`v-if即条件语句`, 用于控制标签是否显示, 这个显示是通过移除/加入目标标签来实现的

```html
<body>
	<div id="app">
		<h1 v-if="show">{{ title }}</h1>
		<button type="button" @click="show = !show">显示</button>
	</div>
	<script type="text/javascript">
		Vue.createApp({
			data() {
				return {
					title: "hello vue3",
					show: false
				}
			}
		}).mount("#app");
	</script>
</body>
```

## 🌲 v-show

`v-show`即控制显示, 我们把`v-if`换成`v-show`会发现也可以机型显示和隐藏, 区别在于`v-show`是通过`display: none;`来实现隐藏的

```html
<body>
	<div id="app">
		<h1 v-show="show">{{ title }}</h1>
		<button type="button" @click="show = !show">显示</button>
	</div>
	<script type="text/javascript">
		Vue.createApp({
			data() {
				return {
					title: "hello vue3",
					show: false
				}
			}
		}).mount("#app");
	</script>
</body>
```

## 🌲 v-for

`v-for即循环语句`, 会把所在标签按照数据数量进行循环

```html
<body>
	<div id="app">
		<h1 v-for="item, index in items">行:{{ index }} 值:{{ item }}</h1>
	</div>
	<script type="text/javascript">
		Vue.createApp({
			data() {
				return {
					items: ["张三", "李四", "王五", "赵六"]
				}
			}
		}).mount("#app");
	</script>
</body>
```

## 🌲 v-cloak

有时我们刷新页面会看到一个没有渲染完的元素在上面`{{ msg }}`, 这可能是js文件还没下载完成, 而html先显示了出来, 体验不是太好, 所以这时就可以使用这个东西了

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
		<script src="../js/vue.global.prod.min.js"></script>
		<style>
			[v-cloak] {
				display: none !important;
			}
		</style>
	</head>
	<body>
		<div id="app" v-cloak>
			<h1>{{ title }}</h1>
		</div>

		<script type="text/javascript">
			Vue.createApp({
				setup() {
					let title = Vue.ref("123");
					return {
						title
					}
				}
			}).mount("#app");
		</script>
	</body>
</html>
```

# 🍎 导出导入

## 🌲 新建js文件

我们在开发中会经常导入一些组件或工具, js中导出使用`export`, 导入使用`import`, 我们使用vue的脚手架形式简单举例, 比如我们创建了一个工具类叫`test.js`

```js
var obj = {"name": "张三"}
```

目录如图

![](images/Pasted%20image%2020230627134844.png)

我们可以看到是在`utils/testUtil`下面的

## 🌲 直接导入

我们在`main.js`中尝试直接导入这个`js`, 导入使用`import`关键字然后+名字+路径

```js
import test from '@/utils/testUtil/test.js'
console.log(test)
```

打印之后我们会发现结果是`{}`空对象, 这是因为js规定想从别的文件导入必须先设置导出,

## 🌲 设置导出

导出方式有两种`export default`和`export`, 前者一般用于导出一个对象, 后者常用于导出多个对象, 我们先看第一个

### 🌸 export default

我们尝试把obj导出来

```js
var obj = {"name": "张三"}
export default obj
```

然后我们发现控制台可以输出了

![](images/Pasted%20image%2020230627135912.png)

### 🌸 export

然后我们看`export`, 我们也尝试把obj导出来

```js
var obj = {"name": "张三"}
export obj
```

我们发现这么写会报错, 所以在使用export导出时需要使用下面的写法

```js
export var obj = {"name": "张三"}
// 或
var obj = {"name": "张三"}
export {
    obj
}
```

然后我们运行一下发现导入的东西没打印出来

```js
import test from '@/utils/testUtil/test.js'
console.log(test) // undefined
```

这就是`export`和`export default`的第二个区别, 我们在使用export导出后, 相应的import也要进行改写

```js
import { obj } from '@/utils/testUtil/test.js'
console.log(obj) // {name: '张三'}
```

我们必须使用中括号里面加变量的形式来接收, 并且名字要和导出的变量名对上

我们来尝试导出多个

```js
var obj = {"name": "张三"}
var obj2 = {"name": "李四"}
export {
    obj,
    obj2
}
```

然后我们来导入

```js
import { obj, obj2 } from '@/utils/testUtil/test.js'
console.log(obj, obj2) // {name: '张三'} {name: '李四'}
```

可以看到能够打印出来, 而且我们可以有选择的引入, 比如我们定义了一堆工具, 最后只想到处obj2, 那没问题中括号里只写obj2就可以了

## 🌲 无文件名导入

这个其实很简单, 就是引入路径的时候只引入文件夹名, 不引入js文件名, 那有人会问了, 这怎么可能找到, 其实还是可以找到的, 因为导入的时候如果没有找到js文件名就会去文件夹下找`index.js`, 这就是导入原理了, 我们一起来看看

我们首先把test.js改成index.js运行一下发现`报错了`

![](images/Pasted%20image%2020230627141403.png)

遇事不要慌, 原因是你导入的时候用的test.js这个文件名, 我们刚把它改了当然找不到了

所以我们导入要变成下面这个样子

```js
import { obj, obj2 } from '@/utils/testUtil/index.js'
console.log(obj, obj2)
```

这种写法也可以写成

```js
import { obj, obj2 } from '@/utils/testUtil'
console.log(obj, obj2)
```

这就是让新手大犯迷糊的`文件夹名`导入了, 我觉得这种方式并不好, 因为增加了新手学习的复杂度, 知道就行了

# 🍎 脚手架

经过上面的学习, 我们已经对vue的基本语法有一些了解了, 下面我们就来看一下更高级的脚手架项目, 脚手架是vue官方提供的框架, 可以帮助开发者更好的开发vue, 我们在公司一般用的就是这种项目, 因为它管理多页面要比单页面方便很多, 是的脚手架也是分为vue2和vue3, 我们先来看看怎么创建吧

## 🌲 创建项目

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

## 🌲 hello world

我们首先要学的还是这个, 下面我们就分别用vue2和vue3来实现吧

> vue2

```html
<template>
	<div id="app">
		<h1>{{ title }}</h1>
	</div>
</template>

<script>
	export default {
		name: 'app',
		data() {
			return {
				title: "hello world!"
			}
		}
	}
</script>

<style>

</style>
```

> vue3

```html
<script setup>
	let title = "hello world!";
</script>

<template>
	<h1>{{ title }}</h1>
</template>

<style>

</style>
```

我们可以发现vue3比vue2更加清爽, 有了setup语法后, 我们不需要再指定根标签了

## 🌲 ref

这个是vue3中最重要的, 也就是数据的绑定, 我们先看看vue2是怎么实现的

> vue2

vue2实现起来很简单, 因为使用的是`选项式API`风格, 我给h1绑定了一个点击方法, 当点击h1的时候就会触发`change`方法, 然后在方法里面直接修改title的值就可以了

```html
<template>
	<div id="app">
		<h1 @click="change">{{ title }}</h1>
	</div>
</template>

<script>
	export default {
		name: 'app',
		data() {
			return {
				title: "hello world!"
			}
		},
		methods: {
			change() {
				this.title = "123";
			}
		}
	}
</script>

<style>

</style>
```

> vue3 setup

在vue3里面我们也可以跟vue2一样使用选项式的API风格, 但一般我们都会选择更新的组合式API, 然而使用同样的逻辑点击h1发现没有任何反应

```html
<body>
	<div id="app">
		<h1 @click="change">{{ title }}</h1>
	</div>
	<script type="text/javascript">
		Vue.createApp({
			setup() {
				let title = "hello world";

				function change() {
					title = "123";
				}
				
				return {
					title,
					change
				}
			}
		}).mount("#app");
	</script>
</body>
```

因为`setup`是`组合式API`, 我们需要借用ref来实现数据绑定

```html
<body>
	<div id="app">
		<h1 @click="change">{{ title }}</h1>
	</div>
	<script type="text/javascript">
		Vue.createApp({
			setup() {
				let title = Vue.ref("hello world");

				function change() {
					title.value = "123";
				}
				
				return {
					title,
					change
				}
			}
		}).mount("#app");
	</script>
</body>
```

我们想要改变ref中的值需要修改它的`.value`属性

> vue3 setup 脚手架

我们再来看看脚手架的形式, 可以看到大同小异, 用vue3还是尽量用这种组合式吧, 写法稍微方便一些

```html
<script setup>
	import { ref } from 'vue'
	let title = ref("hello world!");
	function change() {
		console.log("123");
		title.value = "123";
	}
</script>

<template>
	<h1 @click="change">{{ title }}</h1>
</template>

<style>

</style>
```

## 🌲 更多vue3脚手架详解

[vue3-scaffold](../vue3/vue3-scaffold.md)

# 🍎 路径补全

[index.md](../../vscode/plugin/自动补全路径/index.md.md)
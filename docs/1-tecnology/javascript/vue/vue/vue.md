# 🍎 简介

![](images/Vue.js_Logo_2.svg)

Vue (发音为 /vjuː/，类似 view) 是一款用于构建用户界面的 JavaScript 框架。它基于标准 HTML、CSS 和 JavaScript 构建，并提供了一套声明式的、组件化的编程模型，帮助你高效地开发用户界面。无论是简单还是复杂的界面，Vue 都可以胜任。

# 🍎 版本

`Vue`分为`2`和`3`两个版本, 前者使用比较多, 市面上会的人也多, 后者比较新, 语法有改动, 并且引入了新的`API`风格, 作为前端来说需要同时掌握以应对不同的开发环境, 我的建议是先学`vue2`然后可以很方便的过渡到`vue3`

# 🍎 项目分类

`Vue`项目分为两种形式`单页面`和`脚手架`, 所谓单页面就是在`HTML`中使用`<script>`标签导入`vue.js`, 这种形式适合`单个页面`进行开发, 而另外一种方式是使用`vue`官方给我们提供的一个`脚手架`进行开发, 适合比较大的`多页面`项目, 本文使用单页面的开发方式进行说明

# 🍎 IDE

如果是学习这里推荐`HbuilderX`, 是国内对`vue`支持比较好的, 开箱即用, 而且他们的`uni-app`可以开发小程序和移动端(移动端不推荐使用uni-app开发问题较多), 本文也是基于`HbuilderX`进行讲解的, 我们首先下载`HbuilderX`

https://www.dcloud.io/hbuilderx.html

其他的常用IDE还有`vscode`和`webstorm`, 如果你会他们也可以用它们来进行学习

# 🍎 API风格

详情可以参考官网上的介绍

https://cn.vuejs.org/guide/introduction.html#api-styles

## 🌲 选项式(Options API)

所谓选项式就是`vue2`一直以来的写法, 当然`vue3`也兼容, 即使用`el, data, methods...`分为若干个模块然后进行编码的方式, 这种方式的优点在于会的人多, 写起来熟练

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

组合式是`vue3`提出的, 利用`setup语法糖`把变量, 方法, 组件等逻辑写在一起, 在可读性上`可能`有一些提升

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

当然如果在脚手架中还可以使用`setup`语法糖

# 🍎 快速开始

学习的话最好是从简单的单页面开始, 也就是`html`内嵌`vue`的方式, 因为不需要搭建任何环境就能够运行, 学习起来比较方便, 这也是`vue`对比`react`方便的地方, `react`是不支持这种单页面嵌入开发的

## 🌲 新建项目

我们打开`HbuilderX`新建一个普通项目, 我们这里就选最基础的`基本HTML`项目, 目的是方便学习

![](images/Pasted%20image%2020221128134358.png)

创建后结构是这个样子

![](images/Pasted%20image%2020221128134301.png)

## 🌲 外链嵌入`vue.js`

新建完毕后我们就可以嵌入`vue`了, 下面的例子为多版本演示方便对比, 一般我们外链嵌入是需要在下面两个网址里搜索链接

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

`vue3`嵌入环境也很简单, 仅仅是换了一个地址, 需要注意的是这个地址需要是`vue.global`开放组件的, 否则可能某些功能无法使用, 你在使用的时候也可以用最新的版本

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

我们可以看到`vue3`不再采用`new`, 而是使用`Vue.createApp`方法来创建实例

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
		<script src="https://cdn.bootcdn.net/ajax/libs/vue/3.2.45/vue.global.prod.js"></script>
	</head>
	<body>
		<script type="text/javascript">
			Vue.createApp({

			});
		</script>
	</body>
</html>
```

## 🌲 el/mount

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

在vue3中, 我们使用`mount`来绑定根标签, 它跟`el`的作用一样

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

`data`顾名思义就是我们数据变量存放的地方, 我们可以在vue结构中定义并使用它, 下面我们来声明一个`hello world!`

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

我们使用`data()`来定义一个`data`函数, 然后在`return`后面定义对象, 对象里面的属性叫做`title`, 这个属性的值是`hello world!`

> vue3

我们可以看到`vue3`也可以使用`data()`来管理数据

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

我们可以看到使用`setup`语法更加自然, 但在最后还是需要使用`return`把变量暴露出去, 否则外面识别不到

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

## 🌲 方法

> vue2

我们可以使用`methods`在`vue`中定义方法

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

> vue3 setup

我们可以看到使用`setup`语法把值和函数都能写到一起了, 阅读起来非常方便, 我们定义方法后也需要在`return`中把方法暴露出去, 否则`html`中是无法使用的

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

这种写法虽然看起来很方便, 但是有一个非常大的问题就是值不是响应式的, 也无法访问`this,` 因此我们如果想在`setup`中实现响应式数据需要使用`vue`提供的`ref`, 这里只是简单演示, 在`ref`章节会详细来说

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

因为单页面嵌入`vue`使用`setup`上面的文档极少, 边写还得边查, 浪费时间, 所以单页面还是推荐使用选项式来写

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

## 🌲 监听属性

我们使用`watch`来监听`data`中声明的值, 名称必须和`data`里面的值一样, 如例子所示当`number`被修改时会在`watch`的函数中进行回调

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

在`setup`语法中`watch`的使用有点不同

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

# 🍎 语法

下面是一些常用的语法, 这里使用最新的`vue3`进行演示, 演示还是用大家最熟悉的选项式, 也就是`vue2`的模式

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

我们使用 `xxx-${变量}` 来动态拼接属性 我们直接看下面的例子 我利用数组中的数字来动态生成了每个h1的属性

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

`v-show`即控制显示, 我们把`v-if`换成`v-show`会发现也可以机型显示和隐藏, 区别在于`v-show`是通过`display: none;`来实现隐藏的, 这种方式可能比直接移除元素效率更高, 所以频繁需要隐藏显示的可以使用该语法实现

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

<body>
	<div id="app">
		<h1 v-for="(item, key) in dic">行:{{ index }} 值:{{ item }}</h1>
	</div>
	<script type="text/javascript">
		Vue.createApp({
			data() {
				return {
					dic: {"name": "张三", "age": "18"}
				}
			}
		}).mount("#app");
	</script>
</body>
```

## 🌲 v-cloak

有时我们刷新页面会看到一个没有渲染完的元素在上面`{{ msg }}`, 这可能是js文件还没下载完成, 而html先显示了出来, 体验不是太好, 所以这时就可以使用这个东西了, 使用它别忘了在`style`中添加样式

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

## 🌲 refs

`refs`是`vue`中方便获取元素的一个方法, 我们一起来看看吧, 我们有个子组件叫`HomeView`, 我可以在标签上用ref属性来标记它, 然后在方法中我们就可以使用`this.$refs.hello`来获取到该元素

```vue
<script>
import HomeView from './views/HomeView.vue';
export default {
  components: {
    HomeView
  },

  methods: {
    click() {
      console.log(this.$refs.hello);
    }
  }
}
</script>

<template>
  <div>
    <HomeView ref="hello"></HomeView>
    <button @click="click">点击</button>
  </div>
</template>

<style scoped></style>
```

我们点击看一下

```js
VueComponent {_uid: 2, _isVue: true, __v_skip: true, _scope: EffectScope2, $options: {…}, …}
```

然后我们能做什么用呢? 很多用处哈, 这里我们可以使用它来调用子组件中的方法, 我们在子组件中写一个改变颜色的方法

```vue
<script>
export default {
  methods: {
    changeColor() {
        this.$refs.div1.style.backgroundColor = 'blue';
    }
  }
}
</script>

<template>
  <div style="background-color: red; width: 100px; height: 100px;" ref="div1"></div>
</template>
```

然后我们在父组件中调用它

```js
click() {
  this.$refs.hello.changeColor();
}
```

然后我们就可以看到组件的颜色改变了

# 🍎 vue2和3区别

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

我们可以发现`vue3`比`vue2`更加清爽, 有了`setup`语法后, 我们不需要再指定根标签了

## 🌲 ref

这个是`vue3`中最重要的, 也就是数据的绑定, 我们先看看`vue2`是怎么实现的

> vue2

`vue2`实现起来很简单, 因为使用的是`选项式API`风格, 我给h1绑定了一个点击方法, 当点击h1的时候就会触发`change`方法, 然后在方法里面直接修改title的值就可以了

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

# 🍎 脚手架

经过上面的学习, 我们已经对vue的基本语法有一些了解了, 下面我们就来看一下更高级的脚手架项目, 脚手架是vue官方提供的框架, 可以帮助开发者更好的开发`vue`, 我们在公司开发一般都用这种项目, 因为它管理多页面要比单页面方便很多, 是的脚手架也是分为`vue2`和`vue3`, 我们先来看看怎么创建吧

[跳转 vue-scaffold](../vue-scaffold/vue-scaffold/vue-scaffold.md)

# 🍎 插件

[自动补全路径.md](../../vscode/plugin/自动补全路径/自动补全路径.md.md)

# 🍎 双向数据绑定

在`vue2`和`vue3`中实现双向数据绑定的原理是不同的, 在`vue2`中使用了`defineProperty`来实现, 而`vue3`中使用了`ES6`的`Proxy`来实现, 在性能上得到了很大的提升


# 🍎 简介

本文主要写`vue2`以及`vue3`的语法

# 🍎 语法分类

## 🌲 选项式语法(Vue2原生写法)

### 🌸 `export default`写法

选项式语法就是`vue2`的原生语法, 在`vue3`中也是兼容的, 都有用, 所以不要担心, 放心学

```vue
<script>
export default {
  data() {
    return {
      title: "hello world!"
    }
  }
}
</script>

<template>
  <div>
    <h1>{{ title }}</h1>
  </div>
</template>
```

### 🌸 `defineComponent`写法

跟`export default`大差不差

```vue
<script>
import { defineComponent } from 'vue'
export default defineComponent({
    data() {
        return {
            title: "hello world!"
        }
    }
});
</script>

<template>
    <div>
        <h1>{{ title }}</h1>
    </div>
</template>
```

## 🌲 组合式语法(Vue3推荐写法)

`setup`语法是`vue3`的一大改进可以让`组合式API`更加方便, 同时它也被迁移到`vue2.7`使得`vue2`也有使用`组合式API`的能力

### 🌸 `export default+setup`写法

这种写法和选项式语法是相近的, 就是在`export default`中写一个`setup()`函数, 最后使用`return`把变量或方法导出

```vue
<script>
export default {
  setup() {
    let title = "hello world!";
    return {
      title
    }
  }
}
</script>
```

### 🌸 `defineComponent+setup`写法

https://cn.vuejs.org/api/general.html#definecomponent

乍眼一看, 这种写法比上面的`export default`麻烦, 但是`vue`官方给出了解释, 这种方法比较容易去识别类型

```vue
<template>
    <h1>{{ title }}</h1>
</template>
  
<script>
import { defineComponent } from 'vue'
export default defineComponent({
    setup() {
        const title = "hello world!";
        return {
            title
        }
    }
});
</script>
  
<style scoped></style>
```

### 🌸 `setup`语法糖

我们可以省略`export default`, 直接在`script`标签上加`setup`属性, 虽然是个语法糖, 但简化了大量写法, 这是一个重大的突破

```vue
<script setup>
const title = "hello world";
</script>

<template>
  <div>
    <h1>{{ title }}</h1>
  </div>
</template>
```

# 🍎 `vue2`与`vue3`区别

## 🌲 主文件

> vue2

```js
import Vue from 'vue'
import { createPinia, PiniaVuePlugin } from 'pinia'

import App from './App.vue'
import router from './router'

Vue.use(PiniaVuePlugin)

new Vue({
  router,
  pinia: createPinia(),
  render: (h) => h(App)
}).$mount('#app')
```

> vue3

```js
import { createApp } from 'vue'
import { createPinia } from 'pinia'

import App from './App.vue'
import router from './router'

const app = createApp(App)

app.use(createPinia())
app.use(router)

app.mount('#app')
```

## 🌲 根标签

`vue2`如果在`<template>`中存在两个或两个以上标签, 是需要在最外层加一层`div`的, 否则会报错, 而`vue3`不需要, 这一点`react`也一样, 一个渲染内容中不许存在两个标签, 这是可以使用`幽灵标签`包裹

> vue2

```vue
<template>
  <div>
    <h1>123</h1>
    <h1>456</h1>
  </div>
</template>
```

如果不加外层div会出现一个错误

```shell
13:56:34 [vite] Internal server error: Component template should contain exactly one root element. If you are using v-if on multiple elements, use v-else-if to chain them instead.
```

> vue3

```vue
<template>
	<h1>123</h1>
	<h1>456</h1>
</template>
```

# 🍎 模板语法说明

这里我不会区分`vue2`还是`vue3`因为在`vue2.7`中也兼容了组合式API, 所以只有出现不兼容的问题我才会提出来, 否则就是都兼容, 我这里使用的是`vue 2.7.14`, 使用的语言是以`js`为主的, 关于`ts`只说差异, 点到为止

# 🍎 模板语法(选项式)

选项式是`vue2`的原生语法, 当然`vue3`也是兼容的, 我们在这里就一笔带过了, 因为我们之前在单页面就写过了

## 🌲 基本结构

我们把`主页面`里面的东西都删掉, 这就是`vue`文件的基本结构了

```vue
<script>

</script>

<template>
  
</template>

<style scoped>

</style>
```

## 🌲 插值 {{}}

```vue
<template>
    <h1>{{ title }}</h1>
</template>

<script>
export default {
    data() {
        return {
            title: "hello world!"
        }
    }
};
</script>
```

插值也可以支持表达式

```vue
<h1>{{ title == "hello world!" ? "123" : "456" }}</h1>
```

# 🍎 模板语法(组合式)

组合式中有一个代表性的关键字是`setup`, `setup`语法分为两种, 一种是放在`<script>`标签中的语法糖, 一种是定义在`export default`中的`setup()`, 我们主要使用`setup语法糖`, 因为它更简便

## 🌲 基本结构

我们把`主页面`里面的东西都删掉, 这就是`vue`文件的基本结构了

```vue
<script setup>

</script>

<template>
  
</template>

<style scoped>

</style>
```

最基础的结构知道了, 我们就可以在上面写代码了

## 🌲 插值 {{}}

```vue
<script setup>
const title = "hello world!"
</script>

<template>
  <h1>{{ title }}</h1>
</template>

<style scoped>
</style>
```

插值也可以支持表达式

```vue
<script setup>
const title = "hello world!"
</script>

<template>
  <h1>{{ title == "hello world!" ? "123" : "456" }}</h1>
</template>

<style scoped>
</style>
```

如果你使用`ts`, 那你需要这样写, 标签上只需要加上`lang="ts"`, 定义变量的时候`ts`会让你强制指定变量的类型

```vue
<script lang="ts" setup>
const title: string = "hello world!"
</script>

<template>
  <h1>{{ title }}</h1>
</template>

<style scoped>
</style>
```

## 🌲 v-text

与花括号类似, 也是插值

```vue
<template>
    <h1 v-text="title"></h1>
</template>
```

## 🌲 v-html

可以插入一段`html`

```vue
<script setup>
const html = "<h1>hello world - html</h1>"
</script>

<template>
  <div v-html="html"></div>
</template>

<style scoped></style>
```

## 🌲 v-bind

### 🌸 普通绑定

可以绑定一个属性

```vue
<script setup>
const testId = "app2"
</script>

<template>
  <div>
    <h1 v-bind:id="testId">123</h1>
    <h1 :id="testId">123</h1>
  </div>
</template>

<style scoped></style>
```

绑定后我们看一下html源码, 可以发现确实绑定上了

```html
<div data-v-7a7a37b1>
  <h1 data-v-7a7a37b1 id="app2">123</h1>
  <h1 data-v-7a7a37b1 id="app2">123</h1>
</div>
```

为了方便`v-bind`在代码中也可以省略, 使用`:`代替

### 🌸 动态绑定

属性名字也支持动态绑定, 属性名和属性值都可以动态指定了

```vue
<script setup>
const my_id_name = "id";
const my_id_name2 = "id2";
const my_id = "app";
const my_id2 = "app2";
</script>

<template>
  <div>
    <h1 v-bind:[my_id_name]="my_id">123</h1>
    <h1 :[my_id_name2]="my_id2">456</h1>
  </div>
</template>

<style scoped></style>
```

可看到, 我们使用`my_id_name`变量取代了属性名, 使用`my_id`取代了属性值, 我们可以看一下绑定的标签

```html
<div data-v-7a7a37b1>
  <h1 data-v-7a7a37b1 id="app">123</h1>
  <h1 data-v-7a7a37b1 id2="app2">456</h1>
</div>
```

## 🌲 v-if

### 🌸 v-if

当布尔值为`true`的时候显示, 为`false`的时候隐藏

```vue
<script setup>
var a = true;
</script>

<template>
  <h1 v-if="a">123</h1>
</template>

<style scoped></style>
```

### 🌸 v-else

`a`如果是`true`就返回`123`, 否则返回`456`

```vue
<script setup>
var a = true;
</script>

<template>
  <h1 v-if="a">123</h1>
  <h1 v-else>456</h1>
</template>

<style scoped></style>
```

### 🌸 v-if-else

```vue
<script setup>
var a = "A";
</script>

<template>
  <h1 v-if="a == 'A'">this is A</h1>
  <h1 v-else-if="a == 'B'">this is B</h1>
  <h1 v-else>this is others</h1>
</template>

<style scoped></style>
```

## 🌲 v-show

简单版`v-if` , 不支持`else-if`和`else`

```vue
<script setup>
var a = true;
</script>

<template>
  <h1 v-show="a">123</h1>
</template>

<style scoped></style>
```

如果设置为`no`, 可以看到原理是`style="display: none;"`

```vue
<h1 data-v-7a7a37b1="" style="display: none;">123</h1>
```

## 🌲 v-on

### 🌸 点击

绑定事件, 三种方式效果相同

```vue
<script setup>
const event_name = "click"
function click1() {
  console.log("普通点击")
}

function click2() {
  console.log("点击缩写")
}

function click3() {
  console.log("动态绑定")
}
</script>

<template>
  <div>
    <h1 v-on:click="click1">普通点击</h1>
    <h1 @click="click2">点击缩写</h1>
    <h1 @[event_name]="click3">动态绑定</h1>
  </div>
</template>

<style scoped></style>
```

我们也可以使用箭头函数代替`click`也是可以的

```js
const click1 = () => {
  console.log("普通点击")
}
```

### 🌸 原生组件添加点击

在`vue`原生组件中添加点击事件有一个说道, 那就是无法响应, 比如我在`HelloWorld`上添加`click2`方法就是无法收到响应事件的

```html
<HelloWorld @click="click2"></HelloWorld>
```

解决的问题很容易, 我们只需要设置为`native`

```html
<HelloWorld @click.native="click2"></HelloWorld>
```

### 🌸 长按

触摸手势只有移动端才有, 所以此代码`pc`端是不通用的

```html
<script setup lang="ts">
	import { ref } from "vue"
	let title = ref("hello world1")
	let timer: any = null;

	function touchstart() {
		timer = setTimeout(() => {
			longPress();
		}, 500)
	}

	function touchmove() {
		clearTimeout(timer);
		timer = null;
	}

	function touchend() {
		clearTimeout(timer);
		if (timer != null) {
			console.log("普通点击");
		}
	}

	function longPress() {
		console.log("长按");
	}
</script>

<template>
	<h1 @touchstart="touchstart" @touchmove="touchmove" @touchend="touchend">{{ title }}</h1>
</template>

<style scoped>

</style>
```

所以我们要在移动端里测试长按, 我们用谷歌浏览器模拟一下

![](images/Pasted%20image%2020231025141241.png)

### 🌸 监听按键

我们发现当焦点在`textarea`中才可以监听按键

```vue
<script setup>
function testKeyup(e) {
  console.log(e);
  // 监听按键获取
  let text = e.target.value;
  console.log(text)
}
</script>

<template>
  <textarea @keyup="testKeyup">监听按键</textarea>
</template>
```

### 🌸 事件修饰符

https://cn.vuejs.org/guide/essentials/event-handling.html#event-modifiers

```html
.stop
.prevent
.self
.capture
.once
.passive

<!-- 单击事件将停止传递 -->
<a @click.stop="doThis"></a>

<!-- 提交事件将不再重新加载页面 -->
<form @submit.prevent="onSubmit"></form>

<!-- 修饰语可以使用链式书写 -->
<a @click.stop.prevent="doThat"></a>

<!-- 也可以只有修饰符 不加方法 -->
<form @submit.prevent></form>

<!-- 仅当 event.target 是元素本身时才会触发事件处理器 -->
<!-- 例如：事件处理器不来自子元素 -->
<div @click.self="doThat">...</div>
```

## 🌲 v-for

```vue
<script setup>
var a = [1, 2, 3]
</script>

<template>
  <div>
    <h1 v-for="item in a" :key="item">{{ item }}</h1>
  </div>
</template>

<style scoped></style>
```

## 🌲 ref声明变量

`vue3`里面的变量默认不是响应式的, 如果想把文字改成响应式, 需要使用`ref`, 当然在`vue2.7`也是能用的

```vue
<template>
  <button @click="change">{{ msg }}</button>
</template>

<script setup>
let msg = "hello world!"
function change() {
  msg = "123"
}
</script>

<style scoped>
button {
  width: 200px;
  height: 40px;
}
</style>
```

这种情况下, 点击按钮是不会改变文字的, 下面我们就使用ref语法来改写

```vue
<template>
  <button @click="change">{{ msg }}</button>
</template>

<script setup>
import { ref } from 'vue';
let msg = ref("hello world!")
function change() {
  msg.value = "123"
}
</script>

<style scoped>
button {
  width: 200px;
  height: 40px;
}
</style>
```

这么该改写后, 就可以响应了

## 🌲 ref引用组件

### 🌸 引用组件

`ref`不仅可以`声明响应式变量`也可以`引用组件`, 所以一起讲比较好, 在`vue3`中使用`ref`来实现`refs(vue2的用法 即this.refs.xxx`, 也就是引用某一个组件, 当然也可以引用某一个HTML元素, 太简单, 这里只说组件, 我们先新建一个`TestView`

```vue
<template>
    <h1>我是test组件</h1>
</template>
```

然后我们在`App.vue`中引用它

```vue
<template>
  <TestView ref="testRef"></TestView>
</template>

<script setup lang="ts">
import { ref } from "vue";
import TestView from "@/views/TestView.vue";
const testRef = ref(null);
console.log(testRef);
</script>
```

打印出的ref我们看看是啥

![](images/Pasted%20image%2020240712191045.png)

可以看到就是一个纯粹的ref对象, 我们可以看到value中是有值的, 说明引用成功了

如果没引用成功是这样的, 比如我们把ref故意改成`testRef2`, 那么你使用`testRef`就引用不到

```vue
<TestView ref="testRef2"></TestView>
```

![](images/Pasted%20image%2020240712191146.png)

引用不到的时候我们可以看到都是空的

### 🌸 调用组件中的方法

引用组件后我们就可以做很多事情了, 比如调用组件中的某个方法或属性, 比如我们在组件中定义一个方法和一个属性

```vue
<template>
  <h1>我是test组件</h1>
</template>

<script lang="ts" setup>
const name = "张三";
const say = () => {
  console.log("hello");
};
defineExpose({ name, say });
</script>
```

然后我们使用`defineExpose`导属性和方法

然后我们在`App.vue`调用`TestView`中的方法和属性

```vue
<template>
  <TestView ref="testRef"></TestView>
  <button @click="click">点击</button>
</template>

<script setup lang="ts">
import { ref } from "vue";
import TestView from "@/views/TestView.vue";
const testRef = ref<any>(null);
const click = () => {
  console.log(testRef.value.name);
  testRef.value.say();
}
</script>
```

### 🌸 ref开始生效的生命周期

`ref`是在`mounted`后才引用成功的, 所以你直接在`setup`中打印属性就不行的

```vue
<template>
  <TestView ref="testRef"></TestView>
</template>

<script setup lang="ts">
import { ref } from "vue";
import TestView from "@/views/TestView.vue";
const testRef = ref<any>(null);
console.log(testRef.value.name);
</script>
```

你会发现报错了

```shell
TypeError: Cannot read properties of null (reading 'name')
```

我们需要在`mounted`中进行打印

```vue
<template>
  <TestView ref="testRef"></TestView>
</template>

<script setup lang="ts">
import { ref, onMounted } from "vue";
import TestView from "@/views/TestView.vue";
const testRef = ref<any>(null);
onMounted(() => {
  console.log(testRef.value.name);
})
</script>
```

### 🌸 for循环中的ref

看似简单的ref在for循环中会出现一个问题, 我们来试一下

```vue
<template>
  <TestView v-for="item in [1, 2, 3]" :ref="'testRef' + `${item}`"></TestView>
</template>

<script setup lang="ts">
import { ref, onMounted } from "vue";
import TestView from "@/views/TestView.vue";
const testRef1 = ref<any>(null);
const testRef2 = ref<any>(null);
const testRef3 = ref<any>(null);
onMounted(() => {
  console.log(testRef1);
  console.log(testRef2);
  console.log(testRef3);
});
</script>
```

我们看一下打印

![](images/Pasted%20image%2020240714182637.png)

打眼看上去没什么异常, 但是你有没有发现, 原来的value是个Object, 但现在value变成了数组, 这会出现什么问题呢? 我们想获取引用就不太好获取, 这时候有人会说了, 你只需要取数组里的第0个元素, 然后就可以获取到组件对象了, 就可以调用组件的方法和获取组件里面的值, 说的没错确实可以这么做, 但是这种做法有一个大坑, 就是编译到生产环境会出问题, 所以最有效的解决方案就是把ref手动收集到容器中, 比如字典或数组

```vue
<template>
  <TestView v-for="item in [1, 2, 3]" :ref="(r) => handleRef(r, item)"></TestView>
</template>

<script setup lang="ts">
import { ref, onMounted, reactive } from "vue";
import TestView from "@/views/TestView.vue";
const testRefs = reactive<any>([]);
onMounted(() => {
  console.log(testRefs);
});
function handleRef(r: any, item: number) {
  testRefs[item] = r;
}
</script>
```

![](images/Pasted%20image%2020240714183220.png)

或者你也可以保存到一个字典中, 怎么做随你

```vue
<template>
  <TestView v-for="item in [1, 2, 3]" :ref="(r) => handleRef(r, item)"></TestView>
</template>

<script setup lang="ts">
import { ref, onMounted, reactive } from "vue";
import TestView from "@/views/TestView.vue";
const testRefs = reactive<any>({});
onMounted(() => {
  console.log(testRefs);
});
function handleRef(r: any, item: number) {
  Object.assign(testRefs, {[item]: r});
}
</script>
```

## 🌲 shallowRef

shallowRef可以做到只监听对象本身, 至于对象内部发生什么改动并不关注, 有时候这样做可以节约系统的开销

```vue
<template>
    <button @click="change">{{ msg }}</button>
</template>

<script setup>
import { shallowRef } from 'vue';
let msg = shallowRef({ "name": "张三" })
function change() {
    msg.value.name = "123";
}
</script>

<style scoped>
button {
    width: 200px;
    height: 40px;
}
</style>
```

点击按钮发现值没有修改, 那么怎么让值修改呢

```vue
<template>
    <button @click="change">{{ msg }}</button>
</template>

<script setup>
import { shallowRef } from 'vue';
let msg = shallowRef({ "name": "张三" })
function change() {
    msg.value = { "name": "李四" }
}
</script>

<style scoped>
button {
    width: 200px;
    height: 40px;
}
</style>
```

直接赋值一个对象即可

## 🌲 triggerRef

`triggerRef`可以让视图强制刷新

```vue
<template>
    <button @click="change">{{ msg }}</button>
</template>

<script setup>
import { shallowRef, triggerRef } from 'vue';
let msg = shallowRef({ "name": "张三" })
function change() {
    msg.value.name = "李四"
    triggerRef(msg)
}
</script>

<style scoped>
button {
    width: 200px;
    height: 40px;
}
</style>
```

## 🌲 customRef

可以自定义ref

```vue
<template>
    <button @click="change">{{ msg }}</button>
</template>

<script lang="ts" setup>
import { customRef, triggerRef } from 'vue';

function my_ref<T>(value: T) {
    return customRef((track, trigger) => {
        return {
            get() {
                track()
                return value
            },
            set(newValue: T) {
                value = newValue
                trigger()
            }
        }
    })
}

let msg = my_ref({"name": "张三"})

function change() {
    msg.value.name = "123"
}

</script>

<style scoped>
button {
    width: 200px;
    height: 40px;
}
</style>
```

## 🌲 reactive

与ref功能类似, 都可以做响应式, 而且不需要.value, 但是它有一个问题, 就是不能修改自己本身

```vue
<template>
    <button @click="change">{{ msg }}</button>
</template>

<script lang="ts" setup>
import { reactive } from 'vue';

let msg = reactive({ "name": "张三" })

function change() {
    msg.name = "李四"
}

</script>

<style scoped>
button {
    width: 200px;
    height: 40px;
}
</style>
```

## 🌲 readonly

就是拷贝一份对象, 把它变成只读

```vue
<template>
    <button @click="change">{{ msg }}</button>
</template>

<script lang="ts" setup>
import { reactive, readonly } from 'vue';

let msg = reactive({ "name": "张三" })
let rmsg = readonly(msg)
rmsg.name = "123"

function change() {
    msg.name = "李四"
}

</script>

<style scoped>
button {
    width: 200px;
    height: 40px;
}
</style>
```

## 🌲 shallowReactive

经测试, shallowReactive只会修改浅层值, 不会修改深层值, 但是浅层和深层同时修改的时候, 深层值也会被修改

其实上面解释有点不正确, 因为实际上值是修改了的, 但是它只刷新第一层的值, 第二层的值是不会给刷新的

```vue
<template>
    <button @click="change">{{ msg }}</button>
    <button @click="change2">{{ msg }}</button>
</template>

<script lang="ts" setup>
import { shallowReactive } from 'vue';
let msg = shallowReactive({"name": "张三", "obj": {"age": "18", "obj": {"sex": "男"}}})
function change() {
    msg.name = "李四"
}
function change2() {
    msg.obj.age = "30"
}
</script>

<style scoped>
button {
    width: 200px;
    height: 40px;
}
</style>
```

## 🌲 toRef

顾名思义, 就是把普通的对象或者响应式对象转化成ref, 转化后, 如果之前不是响应式, 那么之后也不是. 如果之前就能响应, 那么转化之后也能响应

但我发现了个奇怪的情况, 就是响应的改变了之后会刷新不响应的, 比如先点1, 画面没有改变, 再点2, 两个按钮的文字全都变了, 也就是不响应的也会随着响应刷新而自动刷新, 我觉得这多多少少有点问题

```vue
<template>
    <button @click="change">{{ name }}</button>
    <button @click="change2">{{ name2 }}</button>
</template>

<script lang="ts" setup>
import { toRef, reactive } from 'vue';

const obj = {
    "name": "张三"
}

let name = toRef(obj, "name")
let name2 = toRef(reactive({"name": "王五"}), "name")

function change() {
    name.value = "李四"
}

function change2() {
    name2.value = "赵六"
}

</script>

<style scoped>
button {
    width: 200px;
    height: 40px;
}
</style>
```

## 🌲 toRefs

顾名思义, 就是toRef是吧对象或引用的某一个属性给变成ref, 这个就是把它自己本身也变成ref

```vue
<template>
    <button @click="change">{{ test_ref }}</button>
</template>

<script lang="ts" setup>
import { toRefs, reactive } from 'vue';

const test_rea = reactive({"name": "张三"})
let test_ref = toRefs(test_rea)


function change() {
    test_ref.name.value = "李四"
}

</script>

<style scoped>
button {
    width: 200px;
    height: 40px;
}
</style>
```

## 🌲 toRaw

toRaw就是把响应式又转化为不响应了, 但是与响应式联合使用的时候, 也会被强制刷新, 我觉得这多多少少有点问题

```vue
<template>
    <button @click="change">{{ test_rea }}</button>
    <button @click="change">{{ test_ref }}</button>
</template>

<script lang="ts" setup>
import { reactive, toRaw } from 'vue';

const test_rea = reactive({ "name": "张三" })
let test_ref = toRaw(test_rea)


function change() {
    test_rea.name = "王五"
    test_ref.name = "李四"
}

</script>

<style scoped>
button {
    width: 200px;
    height: 40px;
}
</style>
```

## 🌲 计算属性

```vue
<template>
    <button @click="change">{{ name }}</button>
</template>

<script lang="ts" setup>
import { computed, ref } from 'vue';

let firstName = ref("张")
let lastName = ref("三")

let name = computed(() => {
    return firstName.value + lastName.value;
})

function change() {
    firstName.value = "李"
    lastName.value = "四"
}

</script>

<style scoped>
button {
    width: 200px;
    height: 40px;
}
</style>
```

也可以使用另外一种写法, set和get

```vue
<template>
    <button @click="change">{{ name }}</button>
</template>

<script lang="ts" setup>
import { computed, ref } from 'vue';

let firstName = ref("张")
let lastName = ref("三")

let name = computed({
    get() {
        return firstName.value + lastName.value;
    },
    set() {

    }
})

function change() {
    firstName.value = "李"
    lastName.value = "四"
}

</script>

<style scoped>
button {
    width: 200px;
    height: 40px;
}
</style>
```

## 🌲 监听属性

### 🌸 监听ref

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

### 🌸 监听prop

还有另外一种情况, 想监听父组件传过来的`prop`, 这个时候`prop`是个值, 在监听的时候就会出警告, 我们这个时候就需要使用函数来包一层

```vue
<script lang="ts" setup>
import { watch } from "vue";

const props = defineProps(["name"]);

watch(
  () => props.name,
  (newVal, oldVal) => {
    console.log(newVal);
  }
);
```

懂的自然懂

# 🍎 模板语法(组合式)

我不喜欢区分`js和ts`, 下面的代码作为参考代码, 我不再向里面补充内容, 并在后续版本会删掉, 因为ts就是语法加强后的js, 最显著的地方是类型, 其次是ts的语言特性

我们可以看到和`js`相比就多了一个语言标识属性`lang="ts"`

```vue
<script lang="ts" setup>

</script>

<template>
  
</template>

<style scoped>

</style>
```

我们可以在上面学习了

## 🌲 插值 {{}}

我们可以看到使用`ts`后在`title`上我们可以指定类型, 其他用法没变

```vue
<script lang="ts" setup>
const title: string = "hello world!"
</script>

<template>
  <h1>{{ title }}</h1>
</template>

<style scoped>
</style>
```

而且使用vite作为构建工具, 即使在js项目中使用ts也完全能够进行编译, 太强大了

## 🌲 定义变量

重复的语法我不想写了, 来个绝活定义变量, 我们在`ts`中可以指定变量的类型, 这样`ts`可以在我们编译时检查出变量类型错误, 防止一些问题发生

```js
// 字符串
var my_string: string = "123"
// 数字
var my_number: number = 456
// 数组
var my_array: Array<number> = [1, 2, 3]
// 对象 / 字典
var my_obj: object = {"name": "张三"}
```

## 🌲 ref

```vue
<template>
    <button @click="change">{{ msg }}</button>
</template>
  
<script lang="ts" setup>
import { ref, Ref } from 'vue';
let msg: Ref = ref("hello world!")
function change() {
    msg.value = "123"
}
</script>
  
<style scoped>
button {
    width: 200px;
    height: 40px;
}
</style>
```

## 🌲 shallowRef

shallowRef可以做到只监听对象本身, 至于对象内部发生什么改动并不关注, 有时候这样做可以节约系统的开销

```vue
<template>
    <button @click="change">{{ msg }}</button>
</template>

<script lang="ts" setup>
import { Ref, shallowRef } from 'vue';
let msg: Ref = shallowRef({ "name": "张三" })
function change() {
    msg.value.name = "123";
}
</script>

<style scoped>
button {
    width: 200px;
    height: 40px;
}
</style>
```

点击按钮发现值没有修改, 那么怎么让值修改呢

```vue
<template>
    <button @click="change">{{ msg }}</button>
</template>

<script lang="ts" setup>
import { shallowRef, ShallowRef } from 'vue';
let msg: ShallowRef = shallowRef({ "name": "张三" })
function change() {
    msg.value = { "name": "李四" }
}
</script>

<style scoped>
button {
    width: 200px;
    height: 40px;
}
</style>
```

直接赋值一个对象即可

## 🌲 triggerRef

`triggerRef`可以让视图强制刷新

```vue
<template>
    <button @click="change">{{ msg }}</button>
</template>

<script lang="ts" setup>
import { shallowRef, ShallowRef, triggerRef } from 'vue';
let msg: ShallowRef = shallowRef({ "name": "张三" })
function change() {
    msg.value.name = "李四"
    triggerRef(msg)
}
</script>

<style scoped>
button {
    width: 200px;
    height: 40px;
}
</style>
```

## 🌲 customRef

可以自定义ref

```vue
<template>
    <button @click="change">{{ msg }}</button>
</template>

<script lang="ts" setup>
import { customRef, triggerRef } from 'vue';

function my_ref<T>(value: T) {
    return customRef((track, trigger) => {
        return {
            get() {
                track()
                return value
            },
            set(newValue: T) {
                value = newValue
                trigger()
            }
        }
    })
}

let msg = my_ref({"name": "张三"})

function change() {
    msg.value.name = "123"
}

</script>

<style scoped>
button {
    width: 200px;
    height: 40px;
}
</style>
```

## 🌲 reactive

与`ref`功能类似, 都可以做响应式, 而且不需要.value, 使用规则是简单类型使用ref, 数组和字典使用`reactive`, 但是它有一个问题, 就是不能修改自己本身

```vue
<template>
    <button @click="change">{{ msg }}</button>
</template>

<script lang="ts" setup>
import { reactive } from 'vue';

let msg = reactive({ "name": "张三" })

function change() {
    msg.name = "李四"
}

</script>

<style scoped>
button {
    width: 200px;
    height: 40px;
}
</style>
```

## 🌲 readonly

就是拷贝一份对象, 把它变成只读

```vue
<template>
    <button @click="change">{{ msg }}</button>
</template>

<script lang="ts" setup>
import { reactive, readonly } from 'vue';

let msg = reactive({ "name": "张三" })
let rmsg = readonly(msg)
rmsg.name = "123"

function change() {
    msg.name = "李四"
}

</script>

<style scoped>
button {
    width: 200px;
    height: 40px;
}
</style>
```

## 🌲 shallowReactive

经测试, shallowReactive只会修改浅层值, 不会修改深层值, 但是浅层和深层同时修改的时候, 深层值也会被修改

其实上面解释有点不正确, 因为实际上值是修改了的, 但是它只刷新第一层的值, 第二层的值是不会给刷新的

```vue
<template>
    <button @click="change">{{ msg }}</button>
    <button @click="change2">{{ msg }}</button>
</template>

<script lang="ts" setup>
import { shallowReactive } from 'vue';
let msg = shallowReactive({"name": "张三", "obj": {"age": "18", "obj": {"sex": "男"}}})
function change() {
    msg.name = "李四"
}
function change2() {
    msg.obj.age = "30"
}
</script>

<style scoped>
button {
    width: 200px;
    height: 40px;
}
</style>
```

## 🌲 toRef

顾名思义, 就是把普通的对象或者响应式对象转化成ref, 转化后, 如果之前不是响应式, 那么之后也不是. 如果之前就能响应, 那么转化之后也能响应

但我发现了个奇怪的情况, 就是响应的改变了之后会刷新不响应的, 比如先点1, 画面没有改变, 再点2, 两个按钮的文字全都变了, 也就是不响应的也会随着响应刷新而自动刷新, 我觉得这多多少少有点问题

```vue
<template>
    <button @click="change">{{ name }}</button>
    <button @click="change2">{{ name2 }}</button>
</template>

<script lang="ts" setup>
import { toRef, reactive } from 'vue';

const obj = {
    "name": "张三"
}

let name = toRef(obj, "name")
let name2 = toRef(reactive({"name": "王五"}), "name")

function change() {
    name.value = "李四"
}

function change2() {
    name2.value = "赵六"
}

</script>

<style scoped>
button {
    width: 200px;
    height: 40px;
}
</style>
```

## 🌲 toRefs

顾名思义, 就是toRef是吧对象或引用的某一个属性给变成ref, 这个就是把它自己本身也变成ref

```vue
<template>
    <button @click="change">{{ test_ref }}</button>
</template>

<script lang="ts" setup>
import { toRefs, reactive } from 'vue';

const test_rea = reactive({"name": "张三"})
let test_ref = toRefs(test_rea)


function change() {
    test_ref.name.value = "李四"
}

</script>

<style scoped>
button {
    width: 200px;
    height: 40px;
}
</style>
```

## 🌲 toRaw

toRaw就是把响应式又转化为不响应了, 但是与响应式联合使用的时候, 也会被强制刷新, 我觉得这多多少少有点问题

```vue
<template>
    <button @click="change">{{ test_rea }}</button>
    <button @click="change">{{ test_ref }}</button>
</template>

<script lang="ts" setup>
import { reactive, toRaw } from 'vue';

const test_rea = reactive({ "name": "张三" })
let test_ref = toRaw(test_rea)


function change() {
    test_rea.name = "王五"
    test_ref.name = "李四"
}

</script>

<style scoped>
button {
    width: 200px;
    height: 40px;
}
</style>
```

# 🍎 组件

由于组件比较多, 跳转学习

[跳转 vue-components](../vue-components/vue-components.md)

# 🍎 导出导入

## 🌲 新建js文件

我们在开发中会经常导入一些组件或工具, `js`中导出使用`export`, 导入使用`import`, 我们使用`vue`的脚手架形式简单举例, 比如我们创建了一个工具类叫`test.js`

```js
var obj = {"name": "张三"}
```

目录如图

![](../../vue/images/Pasted%20image%2020230627134844.png)

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

![](../../vue/images/Pasted%20image%2020230627135912.png)

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

![](../../vue/images/Pasted%20image%2020230627141403.png)

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

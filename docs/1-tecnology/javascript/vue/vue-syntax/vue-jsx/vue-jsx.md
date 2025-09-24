# 🍎 简介

JSX是一种JavaScript的语法扩展，首先运用于[React](https://baike.baidu.com/item/React/18077599?fromModule=lemma_inlink)中，其格式比较像是模版语言，但事实上完全是在[JavaScript](https://baike.baidu.com/item/JavaScript/321142?fromModule=lemma_inlink)内部实现的。元素是构成React应用的最小单位，JSX就是用来声明React当中的元素。React主要使用JSX来描述用户界面，但React并不强制要求使用JSX [1]，而JSX也在React之外的框架得到了广泛的支持，包括[Vue.js](https://baike.baidu.com/item/Vue.js/19884851?fromModule=lemma_inlink) [3]，[Solid](https://baike.baidu.com/item/Solid/9662803?fromModule=lemma_inlink) [2]等。

# 🍎 快速开始

我们在上面已经了解到了`jsx`是对`js`的扩展, 它并不是`React`的专利, 模板引擎都可以用, 只不过它最先应用于`React`, 所谓`jsx`语法就是可以在`js`中去写`html`代码, 已达到方便开发的目的

## 🌲 在vue中写jsx

为了方便教学, 我们使用改写的形式, 首先我们写一个正常的`vue模板语法`

```vue
<script setup lang="ts">

</script>

<template>
  <h1>hello world</h1>
</template>

<style scoped>

</style>
```

![](images/Pasted%20image%2020250721210819.png)

然后我们把这个东西改写成`jsx`语法要怎么写呢, 首先我们要把`lang`改成`tsx或者jsx`, 因为我写的是`typescript`所以我写的`tsx`, 主要不要写错了, 比如写成`tsX`, 否则会报错

```shell
Uncaught SyntaxError: The requested module '/src/App.vue?vue&type=script&setup=true&lang.tsX' does not provide an export named 'default' (at App.vue:1:144)
```

如果书写正确向我下面写的一样

```vue
<script setup lang="tsx">
const JsxComponent = () => <h1>hello world jsx</h1>
</script>

<template>
  <JsxComponent></JsxComponent>
</template>

<style scoped>

</style>
```

但是你会发现, 还是出现了错误, 不要慌

```
Uncaught ReferenceError: React is not defined
    at Proxy.renderJsx (App.vue:3:5)
    at Proxy._sfc_render (App.vue:8:6)
```

![](images/Pasted%20image%2020250721211423.png)

这是由于没安装`jsx`插件的原因, 首先安装`jsx`支持, 下面的命令选一个执行即可

```
npm i @vitejs/plugin-vue-jsx -D
yarn add @vitejs/plugin-vue-jsx -D
```

然后在`vite.config.ts`中配置插件

```ts
import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'
import vueJsx from '@vitejs/plugin-vue-jsx'

// https://vite.dev/config/
export default defineConfig({
  plugins: [vue(), vueJsx()],
})
```

然后重启项目发现可以渲染出来了

![](images/Pasted%20image%2020250721233346.png)

## 🌲 渲染多个元素

我们在上面已经成功的渲染了单元素, 那我们来试试渲染多元素吧

我们发现直接在上面放多个标签是不行的

```jsx
const JsxComponent = () => <h1>hello world jsx</h1><span>123</span>
```

会报如下错误

```shell
[vue/compiler-sfc] Adjacent JSX elements must be wrapped in an enclosing tag. Did you want a JSX fragment <>...</>? (2:51)
```

意思是比如有一个根标签, 和vue2的时候很像, 比如我们在外面套一个`div`

```jsx
const JsxComponent = () => <div><h1>hello world jsx</h1><span>123</span></div>
```

![](images/Pasted%20image%2020250721234357.png)

但是这样外层会多渲染出来一个div

![](images/Pasted%20image%2020250721234456.png)

如果不想这样, 我们也可以根据提示放一个`幽灵标签`

```jsx
const JsxComponent = () => <><h1>hello world jsx</h1><span>123</span></>
```

![](images/Pasted%20image%2020250721234527.png)

我们的多个元素如果也想换行的话也是可以的

```vue
<script setup lang="tsx">
const JsxComponent = () => <>
<h1>hello world jsx</h1>
<span>123</span>
</>
</script>

<template>
  <JsxComponent></JsxComponent>
</template>

<style scoped>

</style>
```

亲测完全没问题, 我们也可以在箭头函数里使用`return`

```vue
<script setup lang="tsx">
const JsxComponent = () => {
  return <>
    <h1>hello world jsx</h1>
    <span>123</span>
  </>
}
</script>

<template>
  <JsxComponent></JsxComponent>
</template>

<style scoped></style>
```

如果想更美观也可以加上括号, 这样就可以对齐了

```vue
<script setup lang="tsx">
const JsxComponent = () => {
  return (
    <>
      <h1>hello world jsx</h1>
      <span>123</span>
    </>
  )
}
</script>

<template>
  <JsxComponent></JsxComponent>
</template>

<style scoped></style>
```

渲染出来都是一样的

## 🌲 封装组件vue

### 🌸 setup

如果这个`jsx`组件很大的时候, 我们就不一定会在当前页面来渲染了, 这个时候我们可以把它封装成组件, 然后导出, 比如我创建了一个叫`Hello world`的组件

```vue
<script lang="tsx" setup>
const render = () => <h1>hello world jsx2</h1>
</script>

<template>
<render></render>
</template>
```

然后我们在父视图中直接使用即可

```vue
<script setup lang="tsx">
import HelloWorld from './components/HelloWorld.vue';
</script>

<template>
  <HelloWorld></HelloWorld>
</template>

<style scoped></style>
```

那如果我们想传值这个要怎么写呢, 当然很简单, 那就是`defineProps`

```vue
<script lang="tsx" setup>
const props = defineProps({title: {type: String, default: "无"}})
const render = () => <h1>hello world jsx2 {props.title}</h1>
</script>

<template>
<render></render>
</template>
```

渲染出效果是这样的

![](images/Pasted%20image%2020250721235929.png)

然后我们也可以给组件传值

```vue
<template>
  <HelloWorld title="zhangsan"></HelloWorld>
</template>
```

很明显值是可以传过来的

![](images/Pasted%20image%2020250722000618.png)

### 🌸 setup()

我们使用`setup()`的语法也是可以的, 只需要返回这个`jsx`

```vue
<script lang="tsx">
export default {
  setup() {
    const render = () => <h1>hello world jsx2</h1>
    return render
  }
}
</script>
```

当然你也可以直接`return () => <h1>hello world jsx2</h1>`, 我这里是为了给新手看的, 所以就分开写了

子组件用法我就不演示了, 都一个样, 下面是这种`vue2`选项式写法

```vue
<script lang="tsx">
export default {
  props: {
    title: {
      type: String,
      default: "无"
    }
  },
  setup(props) {
    const render = () => <h1>hello world jsx2 {props.title}</h1>
    return render
  }
}
</script>
```

## 🌲 封装组件jsx

除了使用`vue`, 我们还可以使用单独的`tsx/jsx`文件, 我们新建一个文件`HelloWorld.tsx`, 然后在里面导出一个函数

```tsx
export default () => <h1>hello world jsx component</h1>
```

然后我们在父视图中使用这个组件

```vue
<script setup lang="tsx">
import HelloWorld from './components/HelloWorld.tsx';
</script>

<template>
  <HelloWorld></HelloWorld>
</template>

<style scoped></style>
```

这种写法也可以进行传值. 只需要在前面加上props即可

```jsx
export default (props: any) => <h1>hello world jsx component {props.title}</h1>
```

上面已经可以应多很多问题了, 但是这种写法`不能在内部设置默认值`, 所以我们可以进行如下写法

```tsx
export default {
    props: {
        title: {
            type: String,
            default: "无"
        }
    },
    setup(props: any) {
        return () => <h1>hello world jsx component {props.title}</h1>
    }
}
```

这个看起来是不是很熟悉呢? 没错这就是vue中`<script>`标签中的写法, 我们也可以使用`render`函数来渲染

```tsx
export default {
    props: {
        title: {
            type: String,
            default: "无"
        }
    },
    render(props: any) {
        return <h1>hello world jsx component {props.title}</h1>
    }
}
```

我们可以看到不同的地方是render直接返回一个`jsx`, 而不用是一个函数

我们也可以使用vue提供的`defineComponent`

```tsx
import { defineComponent } from 'vue'

export default defineComponent(() => {
    return () => <h1>hello world jsx component</h1>
})
```

然后我们传值试一试

```vue
<HelloWorld title="zhangsan"></HelloWorld>
```

![](images/Pasted%20image%2020250722122824.png)

同样的`defineComponent`也可以使用结构

```tsx
import { defineComponent } from 'vue'

export default defineComponent({
    props: {
        title: {
            type: String,
            default: "无"
        }
    },
    setup(props) {
        return () => <h1>hello world jsx component {props.title}</h1>
    }
})
```

然后我们就可以在父视图中去传递参数了

```vue
<HelloWorld title="zhangsan"></HelloWorld>
```

我们可以看到参数是可以加载出来的

![](images/Pasted%20image%2020250722122824.png)



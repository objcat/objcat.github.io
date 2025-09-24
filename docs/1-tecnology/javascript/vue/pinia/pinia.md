# 🍎 简介

您将喜欢使用的 Vue 存储库

https://pinia.web3doc.top

# 🍎 与vuex对比

https://mp.weixin.qq.com/s?__biz=MzA5MTI0ODUzNQ==&mid=2652957572&idx=1&sn=c77f7ca8550aace7714b26d6781ccca3&chksm=8bab097cbcdc806a190092a0c083f36b47f9eb9d15951f22248598f5f6e7eedf667d35d67ed0&scene=27

## 🌲 没有module

`vuex`的`module模块化`一直是一个`槽点`, 学习成本略高, 配置有点麻烦, `pinia`中把它干掉了

## 🌲 没有mutations

这个东西存在本身就是一个鸡肋, 有了`actions`就够了

# 🍎 快速开始

我这里用`vue2`作为展示

## 🌲 导入依赖

```js
"dependencies": {
    "pinia": "^2.1.7",
}
```

## 🌲 配置

打开`main.js`

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

vue3直接使用命令安装`pinia`即可

```shell
yarn add pinia
```

我们可以看到vue3配置的更简洁

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

## 🌲 创建`store`

根据官方的创建方法, 我们在`src`目录下面创建一个`stores`文件夹, 然后里面放一个`store.js`

![](images/Pasted%20image%2020231025104242.png)

然后我们写入样例`demo`

```js
import { defineStore } from 'pinia'

export const useStore = defineStore({
  id: 'store',
  state: () => ({
    count: 0
  }),
  getters: {
    doubleCount: (state) => state.count * 2
  },
  actions: {
    increment() {
      this.count++
    }
  }
})
```

我们可以看到非常简洁

## 🌲 页面使用

我们可以使用`setup`语法方便的导入变量

```vue
<script setup>
import { useStore } from '@/stores/store.js'

const store = useStore();

function click() {
  store.count++;
}

</script>

<template>
  <div>
    <h1 @click="click">{{ store.count }}</h1>
    <h1>{{ store.doubleCount }}</h1>
  </div>
</template>

<style scoped></style>
```

## 🌲 变量

一旦我们把`count`定义成变量, 这个时候这个值就不是响应式的了, 想要搞成响应式需要一个函数`storeToRefs`

```vue
<script setup>
import { useStore } from '@/stores/store.js'
import { storeToRefs  } from 'pinia'

const store = useStore();

// let count = store.count;

let { count } = storeToRefs(store);

function click() {
    count.value++;
}

</script>

<template>
    <div>
        <h1 @click="click">{{ count }}</h1>
        <h1>{{ store.count }}</h1>
    </div>
</template>

<style scoped></style>
```

而且我们可以发现, 这个`count`是引用形式的, 它可以改变`store`中的值

## 🌲 执行方法

其中`store.count++`也可以换成定义好的方法, 是一样的效果

```js
store.increment();
```

## 🌲 修改多个值

我们可以使用`$patch`来同时修改多个值, 你如果不想用也可以一个一个修改, 都可以

```vue
<script setup>
import { useStore } from '@/stores/store.js'

const store = useStore();

function click() {
    store.count++;
}

function changeValue() {
    store.$patch({
        count: 100,
        name: "李四",
        age: 30
    });
}

</script>

<template>
    <div>
        <h1 @click="click">{{ store.count }}</h1>
        <h1>{{ store.doubleCount }}</h1>
        <h1>{{ store.name }}</h1>
        <h1>{{ store.age }}</h1>
        <button @click="changeValue">修改多个值</button>
    </div>
</template>

<style scoped></style>
```

## 🌲 重置状态

```vue
<script setup>

import { useStore } from '@/stores/store.js'

const store = useStore();

function click() {
    store.count++;
}

function reset() {
    store.$reset();
}

</script>

<template>
    <div>
        <h1 @click="click">{{ store.count }}</h1>
        <h1>{{ store.doubleCount }}</h1>
        <button @click="reset">重置状态</button>
    </div>
</template>

<style scoped></style>
```

## 🌲 多store

`pinia`没有`vuex`复杂的`module`概念, 取而代之的是多`store`

```js
import { defineStore } from 'pinia'

export const useStore2 = defineStore({
  id: 'store2',
  state: () => ({
    name: "张三"
  }),
  getters: {
    say: (state) => "hello world " + state.name
  }
})
```

然后使用

```vue
<script setup>
import { useStore } from '@/stores/store.js'
import { useStore2 } from '@/stores/store2.js'

const store = useStore();
const store2 = useStore2();

function click() {
  store.count++;
}

</script>

<template>
  <div>
    <h1 @click="click">{{ store.count }}</h1>
    <h1>{{ store.doubleCount }}</h1>
    <h1>{{ store2.name }}</h1>
    <h1>{{ store2.say }}</h1>
  </div>
</template>
```

# 🍎 options模式

除了可以在`组合式`中使用, `pinia`也考虑到向下兼容支持`选项式`

## 🌲 mapState

我们在`computed`中使用`...mapStores`来引用`useStore`, 使用`...mapState`来读取

```vue
<template>
    <h1 @click="click">{{ count }}</h1>
</template>
  
<script>
import { useStore } from '@/stores/store.js'
import { mapStores, mapState } from 'pinia'

export default {
    data() {
        return {
            title: "hello world!"
        }
    },
    computed: {
        ...mapStores(useStore),
        ...mapState(useStore, ['count']),
    },
    methods: {
        click() {
            this.count = 1;
        }
    }
}
</script>
```

我们点击按钮后会发现报错了

```js
[Vue warn]: Computed property "count" was assigned to but it has no setter.

found in

---> <App> at /Users/objcat/Desktop/test/test-pinia/src/App.vue
       <Root>
```

有时候变量会重名, 我们也可以给变量定义别名

```vue
<template>
    <h1 @click="click">{{ myCount }}</h1>
</template>
  
<script>
import { useStore } from '@/stores/store.js'
import { mapStores, mapWritableState, mapActions } from 'pinia'

export default {
    data() {
        return {
            title: "hello world!"
        }
    },
    computed: {
        ...mapStores(useStore),
        ...mapWritableState(useStore, {myCount: 'count'}),
    },
    methods: {
        ...mapActions(useStore, ['increment']),
        click() {
            this.increment();
        }
    }
}
</script>
```

## 🌲 mapWritableState

因为`mapState`默认不能写, 我们想要写可以使用`mapWritableState`

```vue
<template>
    <h1 @click="click">{{ count }}</h1>
</template>
  
<script>
import { useStore } from '@/stores/store.js'
import { mapStores, mapWritableState } from 'pinia'

export default {
    data() {
        return {
            title: "hello world!"
        }
    },
    computed: {
        ...mapStores(useStore),
        ...mapWritableState(useStore, ['count']),
    },
    methods: {
        click() {
            this.count = 1;
        }
    }
}
</script>
```

## 🌲 mapActions

我们可以使用`mapActions`调用方法

```vue
<template>
    <h1 @click="click">{{ count }}</h1>
</template>
  
<script>
import { useStore } from '@/stores/store.js'
import { mapStores, mapWritableState, mapActions } from 'pinia'

export default {
    data() {
        return {
            title: "hello world!"
        }
    },
    computed: {
        ...mapStores(useStore),
        ...mapWritableState(useStore, ['count']),
    },
    methods: {
        ...mapActions(useStore, ['increment']),
        click() {
            this.increment();
        }
    }
}
</script>
```

方法也支持定义别名

```js
methods: {
	...mapActions(useStore, { test: 'increment' }),
	click() {
		this.test();
	}
}
```

# 🍎 加载缓存

## 🌲 完全加载

我们可能会使用`pinia`来管理一些配置, 这些配置是实时存储的, 再次进入的时候要把配置读到`pinia`中, 我们就可以这样写

```js
import { defineStore } from 'pinia';
import JSON5 from 'json5';

export const useStore = defineStore({
  id: 'convert-store',
  state: () => ({
    json: '',
  }),
  actions: {
    setJson(value) {
      this.json = value;
      this.save();
    },
    loadAllData() {
      // 首先获取localStorage中的对应数据
      const str = localStorage.getItem('java-convert-store');
      // 不为空就加载数据
      if (str !== null && str !== undefined && str !== '') {
        var obj = JSON5.parse(str);
        this.$patch(obj);
      }
    },
    save() {
      localStorage.setItem('java-convert-store', JSON5.stringify(this.$state));
    },
  },
});

// 加载本地数据缓存
useStore().loadAllData();
```

如代码所示, 当触发`setJson`方法后我们就会调用`save`方法把`state`整体缓存到`localStorage`, 刷新页面后我们会把状态使用`loadAllData`加载到`state`上, 达到了实时修改配置的效果



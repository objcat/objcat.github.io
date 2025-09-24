# 🍎 简介

组件是`vue`中非常重要的概念, 传统`HTML`中对于`HTML代码块`的复用是很吃力的, `vue`使用组件管理这些`HTML代码块`, 让一处代码多处使用变成了可能

# 🍎 组件(选项式Vue2原生写法)

我们下面来学习组件相关的知识

## 🌲 八个生命周期函数

vue中有8个生命周期函数, vue2和vue3有所不同

```vue
<template>
	<div>
		<h1 id="hello">{{ title }}</h1>
		<button @click="update">更新</button>
		<button @click="dismiss">销毁</button>
	</div>
</template>

<script>
	export default {
		data() {
			return {
				title: "hello world"
			}
		},
		beforeCreate() {
			console.log("创建之前")
			console.log(document.querySelector("#hello"))
		},
		created() {
			console.log("创建完成")
			console.log(document.querySelector("#hello"))
		},
		beforeMount() {
			console.log("挂载之前")
			console.log(document.querySelector("#hello"))
		},
		mounted() {
			console.log("挂载完毕")
			console.log(document.querySelector("#hello"))
		},
		beforeUpdate() {
			console.log("更新之前")
			console.log(document.querySelector("#hello"))
		},
		updated() {
			console.log("更新完毕")
			console.log(document.querySelector("#hello"))
		},
		beforeUnmount() {
			console.log("vue3")
			console.log("卸载之前")
			console.log(document.querySelector("#hello"))
		},
		unmounted() {
			console.log("vue3")
			console.log("卸载完成")
			console.log(document.querySelector("#hello"))
		},
		beforeDestroy() {
			console.log("vue2")
			console.log("销毁之前")
			console.log(document.querySelector("#hello"))
		},
		destroyed() {
			console.log("vue2")
			console.log("销毁完成")
			console.log(document.querySelector("#hello"))
		},
		methods: {
			update() {
				this.title = "小白菜";
			},
			dismiss() {
				this.$emit("dismiss");
			}
		}
	}
</script>

<style scoped></style>
```

我们可以看到vue3没有`destroyed`, 取而代之的是`unmounted`, 唯一用到父组件的地方是移除当前组件

```vue
<template>
	<HelloWorld v-if="visiable" @dismiss="dismiss"></HelloWorld>
</template>

<script>
	import HelloWorld from './components/HelloWorld.vue'
	export default {
		data() {
			return {
				visiable: true
			}
		},
		components: {
			HelloWorld
		},
		methods: {
			dismiss() {
				this.visiable = false;
			}
		}
	}
</script>

<style scoped></style>
```

然后我们来看打印, 唯一的疑惑点在于`更新之前`为什么不是`hello world`而是`小白菜`, 这个以后再解决吧

```
创建之前
null
创建完成
null

挂载之前
null
挂载完毕
<h1 id=​"hello">​hello world​</h1>​

更新之前
<h1 id=​"hello">​小白菜​</h1>​
更新完毕
<h1 id=​"hello">​小白菜​</h1>​

vue3
卸载之前
<h1 id=​"hello">​小白菜​</h1>​
vue3
卸载完成
null
```

## 🌲 组件导入

我们现在有一个`HelloWorld`组件想要导入到`App.vue`中去

- HelloWorld.vue

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
  <h1>{{ title }}</h1>
</template>

<style scoped>

</style>
```

我们可以使用`import`导入, 然后使用`components`模块来加载

```vue
<template>
	<HelloWorld></HelloWorld>
</template>

<script>
import HelloWorld from "@/components/HelloWorld.vue"
export default {
	components: {
		HelloWorld
	}
}
</script>

<style scoped>

</style>
```

## 🌲 定义属性

我们可以在子组件中定义属性来接收父组件的传值

- HelloWorld.vue

```vue
<script>
	export default {
    data() {
      return {
        title: "hello world!"
      }
    },
    props: [
      'title'
    ]
  }
</script>

<template>
  <h1>{{ title }}</h1>
</template>

<style scoped>

</style>
```

然后我们发现报错了

```
main.js?t=1698289399317:13 [Vue warn]: The data property "title" is already declared as a prop. Use prop default value instead.

found in

---> <HelloWorldVue> at /Users/objcat/Desktop/test/test-pinia/src/components/HelloWorld.vue
       <App> at /Users/objcat/Desktop/test/test-pinia/src/App.vue
         <Root>
```

意思是`title`已经定义过了, 不能再次定义, 所以我们把参数改一个名字

```js
props: [
  'newTitle'
]
```

## 🌲 父向子传值(props)

### 🌸 静态传值

所谓静态传值就是传递一个常量, 比如`123`

```vue
<template>
    <HelloWorld newTitle="123"></HelloWorld>
</template>

<script>
import HelloWorld from '@/components/HelloWorld.vue';
export default {
    components: {
        HelloWorld
    }
}
</script>

<style scoped></style>
```

### 🌸 子组件收值

然后我们子组件`HelloWorld`中就可以直接使用这个值了, 我们直接用`模板语法`把`newTitle`渲染到`HTML`中

```vue
<script>
	export default {
    data() {
      return {
        title: "hello world!"
      }
    },
    props: [
      'newTitle'
    ]
  }
</script>

<template>
  <h1>{{ newTitle }}</h1>
</template>

<style scoped>

</style>
```

这个时候`title`就没有用了, 因为我们直接就使用外边传入的了

### 🌸 子组件收值拓展

我们在上面已经提到过了

#### 🌵 数组接收法

数据接收法优点是方便

```vue
<template>
  <h1>{{ newTitle }}</h1>
</template>

<script>
export default {
  props: ['newTitle']
}
</script>

<style scoped></style>
```

#### 🌵 字典接收法

字典接收法优点是严谨, 可以看到字典接收法可以更好的标注类型, 比如我们的`title`类型是`String`, 如果传入别的类型的值会警告

```vue
<template>
  <h1>{{ newTitle }}</h1>
</template>

<script>
export default {
  props: {
    newTitle: String
  }
}
</script>

<style scoped></style>
```

比如我们传入一个数字类型, 就会收到vue的警告

```
main.js?t=1713262945486:4 [Vue warn]: Invalid prop: type check failed for prop "newTitle". Expected String with value "123", got Number with value 123. 
  at <HelloWorld newTitle=123 > 
  at <App>
```

#### 🌵 默认值

字典接收法也可以设置成详细模式, 可以设置默认值等.

```vue
<template>
  <h1>{{ newTitle }}</h1>
</template>

<script>
export default {
  props: {
    newTitle: {
      type: String,
      default: 'hello'
    }
  }
}
</script>

<style scoped></style>
```

#### 🌵 数据类型

上面显示我们可以标记的数据类型

```json
props: {
  title: String,
  likes: Number,
  isPublished: Boolean,
  commentIds: Array,
  author: Object,
  callback: Function,
  contactsPromise: Promise // or any other constructor
}
```

### 🌸 动态传值

动态传值就是传递一个变量, 我们定义一个

```vue
<template>
    <HelloWorld :newTitle="newTitle"></HelloWorld>
</template>

<script>
import HelloWorld from "@/components/HelloWorld.vue"
export default {
    components: {
        HelloWorld
    },
    data() {
        return {
            newTitle: "123"
        }
    }
}
</script>

<style scoped></style>
```

### 🌸 修改值

动态传值的优势就是在传值后可以随意修改变量, 但是要注意的是`vue`里面的规则是父变量只能在外面去修改, 不能由子组件去修改

```vue
<template>
    <HelloWorld :newTitle="newTitle"></HelloWorld>
</template>

<script>
import HelloWorld from "@/components/HelloWorld.vue"
export default {
    components: {
        HelloWorld
    },
    data() {
        return {
            newTitle: "123"
        }
    },
    created() {
        setTimeout(() => {
            console.log("修改了");
            this.newTitle = "456";
        }, 2000);
    }
}
</script>

<style scoped></style>
```

### 🌸 子组件修改父组件的值

一句话叫不能`直接修改`, 想要修改就要子`向父传值`, 让`父去修改`

## 🌲 子向父传值($emit)

这种`$emit`的方式可以看做是子组件调用父组件的方法, 然后顺便把值传递过去

### 🌸 实验

我们刚才说了, 子组件不能直接修改父组件传过来的值, 那么为什么不能呢? 我们来试一试

假如我们想在子组件中修改这个`newTitle`要怎么做呢? 我们先演示一个错误的做法

```vue
<template>
  <div>
    <h1 @click="click">{{ newTitle }}</h1>
  </div>
</template>

<script>
export default {
  props: ['newTitle'],
  methods: {
    click() {
      this.newTitle = 456;
    }
  }
}
</script>

<style scoped>
</style>
```

我们给`h1`绑定了一个点击事件, 点击后修改`newTitle`, 我们发现可以正常修改但会报错

```
[Vue warn]: Avoid mutating a prop directly since the value will be overwritten whenever the parent component re-renders. Instead, use a data or computed property based on the prop's value. Prop being mutated: "newTitle"

found in
```

这个东西的意思是在子组件直接修改`props`是不安全的, 因为只修改了子组件, 没修改父组件, 所以在父组件更新后会出现值又变回原来的值的情况, 在开发中就很危险, 我们接下来就学习最常用的方法`emit`

### 🌸 子组件上定义回调方法

首先我们要在子组件上定义一个方法别名, 比如我这里叫`@sub-click`, 然后绑定一个父组件的方法, 我这里叫`changeTitle`

```vue
<template>
    <HelloWorld :newTitle="newTitle" @sub-click="changeTitle"></HelloWorld>
</template>

<script>
import HelloWorld from "@/components/HelloWorld.vue"
export default {
    components: {
        HelloWorld
    },
    data() {
        return {
            newTitle: "hello world"
        }
    },
    methods: {
        changeTitle(value1) {
            this.newTitle = value1;
        }
    }
}
</script>

<style scoped></style>
```

### 🌸 子组件触发方法

子传值需要使用`$emit`来调用父组件方法, 我们调用方法的时候无法直接调用方法本身, 所以要`emit`刚才定义的别名

```vue
<template>
  <h1 @click="click">{{ newTitle }}</h1>
</template>

<script>
export default {
  props: ['newTitle'],
  methods: {
    click() {
      this.$emit("sub-click", "小白菜啊")
    }
  }
}
</script>

<style scoped></style>
```

### 🌸 计算属性

我们也可以把`$emit`封装成计算属性, 原理还是`$emit`到父组件, 只不过用计算属性封装一下让它使用起来方便了一些

```vue
<template>
  <div>
    <h1 @click="click">{{ NewTitle }}</h1>
  </div>
</template>

<script>

export default {
  props: ['newTitle'],
  methods: {
    click() {
      this.NewTitle = "小白菜啊";
    }
  },
  computed: {
    NewTitle: {
      get() {
        return this.newTitle;
      },
      set(val) {
        this.$emit("sub-click", val);
      }
    }
  }
}
</script>

<style scoped>
</style>
```

### 🌸 传值易错点(注意)

但是有一种传值方式就容易引发错误, 那就是子组件中这种写法

```vue
<script>
	export default {
    data() {
      return {
        title: this.newTitle
      }
    },
    props: [
      'newTitle'
    ]
  }
</script>

<template>
  <h1>{{ newTitle }}</h1>
</template>

<style scoped>

</style>
```

乍眼一看没有任何问题, 就是把`newTitle`赋值给了`title`, 然后去渲染`title`, 但是这种写法有一个问题就是后期当你在父组件中改变`newTitle`的时候, 它不能被传递到子组件, 因为`data`不是响应式的, 只能接受初始化时传过来的值, 后期修改都会没有效果, 比如我们把子组件改成这样, 然后发现子组件中的标题2秒后没有变成`456`

怎么解决这个问题呢? 其实最方便的解法就是在子组件中

直接使用`newTitle`
直接使用`newTitle`
直接使用`newTitle`

重要的事情说三遍, 你就当传进去的值就在你控件上就不会有把它赋值到别的变量的意愿了

### 🌸 在适当的时候刷新

如果你确实想实现这种不跟随父组件响应的这种形式的页面我们也是有办法的, 只需要在子组件中定义一个刷新方法然后进行手动刷新

- HelloWorld.vue

我们给子组件中写一个方法叫`reloadata`, 用于刷新数据

```vue
<script>
	export default {
    data() {
      return {
        title: this.newTitle
      }
    },
    props: [
      'newTitle'
    ],
    methods: {
      reloadata() {
        console.log("已刷新");
        console.log(this.newTitle);
        this.title = this.newTitle;
      }
    }
  }
</script>

<template>
  <h1>{{ title }}</h1>
</template>

<style scoped>

</style>
```

然后我们在父组件中调用即可

```vue
<template>
    <HelloWorld :newTitle="newTitle" ref="HelloWorld"></HelloWorld>
</template>

<script>
import HelloWorld from "@/components/HelloWorld.vue"
export default {
    components: {
        HelloWorld
    },
    data() {
        return {
            newTitle: "123"
        }
    },
    created() {
        setTimeout(() => {
            console.log("修改了");
            this.newTitle = "456";
            this.$refs.HelloWorld.reloadata();
        }, 2000);
    }
}
</script>

<style scoped></style>
```

我们发现还是不能刷新的, 那是什么问题呢? 我们在打印中可以看到

```
修改了
已刷新
123
```

我们发现子组件中数据还是123, 我们明明传递过去了, 不卖关子了, 解决方案是这样

```js
created() {
	setTimeout(() => {
		console.log("修改了");
		this.newTitle = "456";
		this.$nextTick(() => {
			this.$refs.HelloWorld.reloadata();
		})
	}, 2000);
}
```

使用`$nextTick`, 当数据改变后再调用`reloadata`给组件赋值, 这样就能实现子组件的手动刷新了, 如果图方便就直接使用变量, 而不是赋值与子组件内的另外一个, 否则刷新就会很麻烦, 顺便提一下这个东西一般人遇不到, 但我不是一般人 `:)`

## 🌲 子向父传值(sync)

这个方法只支持`vue2`, `vue3`不支持`sync`语法糖了, 改用`v-model`

通过上面的方法我们已经可以搞定子向父传值了, 但是如果传递的值很多, 那么就需要定义n多个方法, 这显然不太方便, 所以我们可以使用另外一种方式`sync`, 翻译过来就是同步, 可以使用`update`关键字来快速修改父元素中的变量

### 🌸 子组件上使用sync绑定

```vue
<template>
    <HelloWorld :newTitle.sync="newTitle"></HelloWorld>
</template>

<script>
import HelloWorld from "@/components/HelloWorld.vue"
export default {
    components: {
        HelloWorld
    },
    data() {
        return {
            newTitle: "123"
        }
    }
}
</script>

<style scoped></style>
```

### 🌸 子组件调用update方法更新值

```vue
<template>
  <h1 @click="click">{{ newTitle }}</h1>
</template>

<script>
export default {
  props: ['newTitle'],
  methods: {
    click() {
      this.$emit("update:newTitle", "新标题")
    }
  }
}
</script>

<style scoped></style>
```

到此为止实现了子组件自己更新自己的值

## 🌲 子向父传值(v-model)

在`vue3`中可以直接使用`v-model`来同步父和子的变量了

```vue
<template>
    <HelloWorld v-model:newTitle="newTitle"></HelloWorld>
</template>

<script>
import HelloWorld from "@/components/HelloWorld.vue"
export default {
    components: {
        HelloWorld
    },
    data() {
        return {
            newTitle: "123"
        }
    }
}
</script>

<style scoped></style>
```

修改值的时候还是使用`this.$emit("update:newTitle", "新标题")`这个没有变化

## 🌲 父调用子方法

我们上面学会了子调用父方法, 那就是`$emit`, 那么父调用子方法要怎么做呢? 没错就是`ref`

### 🌸 在子组件中声明方法

比如我定义了一个叫`changeTitle`的方法用于改变标题

```vue
<template>
  <h1>{{ title }}</h1>
</template>

<script>
export default {
	data() {
		return {
			title: "hello world"
		}
	},
	methods: {
		changeTitle(value) {
			this.title = value;
		}
	}
}
</script>

<style scoped></style>
```

### 🌸 父组件中使用ref调用

在子组件中定义一个属性叫`ref`, 我们在页面中就能通过`this.$refs`来拿到子组件对象了, 然后顺理成章的调用子组件中的方法

```vue
<template>
    <HelloWorld ref="hello" @click="click"></HelloWorld>
</template>

<script>
import HelloWorld from "@/components/HelloWorld.vue"
export default {
    components: {
        HelloWorld
    },
	methods: {
		click() {
			this.$refs.hello.changeTitle("小白菜");
		}
	}
}
</script>

<style scoped></style>
```

## 🌲 slot

### 🌸 匿名slot

#### 🌵 子组件定义

```vue
<template>
  <div>
    <slot></slot>
  </div>
</template>

<script>

</script>

<style scoped></style>
```

#### 🌵 父组件使用

```vue
<template>
    <HelloWorld>
        <h1>hello</h1>
    </HelloWorld>
</template>

<script>
import HelloWorld from "@/components/HelloWorld.vue"
export default {
    components: {
        HelloWorld
    }
}
</script>

<style scoped></style>
```

### 🌸 指定名slot

#### 🌵 子组件定义

```vue
<template>
  <div>
    <slot name="head"></slot>
    <slot name="foot"></slot>
  </div>
</template>

<script>

</script>

<style scoped></style>
```

#### 🌵 父组件使用

```vue
<template>
    <HelloWorld>
        <template v-slot:head>
            <h1>我是头</h1>
        </template>

        <template v-slot:foot>
            <h1>我是脚</h1>
        </template>
    </HelloWorld>
</template>

<script>
import HelloWorld from "@/components/HelloWorld.vue"
export default {
    components: {
        HelloWorld
    }
}
</script>

<style scoped></style>
```

### 🌸 子组件通过slot向父传值

可以看到 我们子组件中使用 v-bind 的简写方式 `:`来绑定一个数据, 数据的名字叫data值是value

```vue
<template>
  <div>
    <slot name="head" :value="value1"></slot>
    <slot name="foot" :value="value2"></slot>
  </div>
</template>

<script>
export default {
  data() {
    return {
      value1: "我是第一个值",
      value2: "我是第二个值"
    }
  }
}
</script>

<style scoped></style>
```

我们在父组件中 可以直接使用`v-slot`来接收, 这个方法是很常用的, `element-ui`列表就使用这个传值方法

```vue
<template>
    <HelloWorld>
        <template v-slot:head="{ value }">
            <h1>我是头</h1>
            <h2>{{ value }}</h2>
        </template>

        <template v-slot:foot="{ value }">
            <h1>我是脚</h1>
            <h2>{{ value }}</h2>
        </template>
    </HelloWorld>
</template>

<script>
import HelloWorld from "@/components/HelloWorld.vue"
export default {
    components: {
        HelloWorld
    }
}
</script>

<style scoped></style>
```

# 🍎 组件(组合式Vue3推荐写法)

组合式我们就使用`setup`语法糖惊醒演示

## 🌲 六个生命周期函数

我们会发现使用`setup`语法后生命周期函数变成了6个, 少了`created`, 其实我们的`setup`里面执行的就是`created`

```vue
<template>
	<div>
		<h1 id="hello">{{ title }}</h1>
		<button @click="update">更新</button>
		<button @click="dismiss">销毁</button>
	</div>
</template>

<script setup>
	import {
		ref
	} from 'vue';

	import {
		onBeforeMount,
		onMounted,
		onBeforeUpdate,
		onUpdated,
		onBeforeUnmount,
		onUnmounted
	} from 'vue';

	const title = ref("hello world");

	const emit = defineEmits();

	const update = () => {
		title.value = "小白菜";
	};

	const dismiss = () => {
		emit('dismiss');
	};

	onBeforeMount(() => {
		console.log("挂载之前")
		console.log(document.querySelector("#hello"))
	})

	onMounted(() => {
		console.log("挂载完毕")
		console.log(document.querySelector("#hello"))
	})

	onBeforeUpdate(() => {
		console.log("更新之前")
		console.log(document.querySelector("#hello"))
	})

	onUpdated(() => {
		console.log("更新完毕")
		console.log(document.querySelector("#hello"))
	})

	onBeforeUnmount(() => {
		console.log("卸载之前")
		console.log(document.querySelector("#hello"))
	})

	onUnmounted(() => {
		console.log("卸载完成")
		console.log(document.querySelector("#hello"))
	})
</script>

<style scoped></style>
```

我们来看一下打印

```
挂载之前
null
挂载完毕
<h1 id=​"hello">​hello world​</h1>​

更新之前
<h1 id=​"hello">​小白菜​</h1>​
更新完毕
<h1 id=​"hello">​小白菜​</h1>​

vue3
卸载之前
<h1 id=​"hello">​小白菜​</h1>​
vue3
卸载完成
null
```

## 🌲 组件导入

可以发现`setup`语法导入的组件非常简洁, 直接使用即可, 不需要再使用`components`定义

```vue
<template>
	<HelloWorld></HelloWorld>
</template>

<script setup>
import HelloWorld from "@/components/HelloWorld.vue"
</script>

<style scoped>

</style>
```

我们也可以单独使用一个`<script>`标签来导入组件, 有两个`setup`是不行的, 但是我们可以使用一个不带`setup`的`script`标签导入组件

```vue
<template>
	<HelloWorld></HelloWorld>
</template>

<script>
	import HelloWorld from '@/components/HelloWorld.vue'
</script>

<script setup>
	// do something
</script>

<style scoped></style>
```

## 🌲 父向子传值(props)

### 🌸 静态传值

所谓静态传值就是传递一个常量, 比如`123`

```vue
<template>
	<HelloWorld newTitle="123"></HelloWorld>
</template>

<script setup>
import HelloWorld from "@/components/HelloWorld.vue"
</script>

<style scoped>

</style>
```

### 🌸 子组件收值

然后我们子组件`HelloWorld`中就可以直接使用这个值了, 在`setup`语法中我们直接使用`defineProps`来收值

```vue
<template>
	<h1>{{ newTitle }}</h1>
</template>

<script setup>
	import { defineProps } from "vue"
	defineProps(['newTitle']);
</script>

<style scoped>

</style>
```

### 🌸 动态传值

```vue
<template>
	<HelloWorld :newTitle="newTitle"></HelloWorld>
</template>

<script setup>
import HelloWorld from "@/components/HelloWorld.vue"
import { ref } from "vue"
let newTitle = ref("hello world");
</script>

<style scoped>

</style>
```

### 🌸 修改值

`setup`里面就是我们的`created`声明周期函数, 可以直接写代码

```vue
<template>
	<HelloWorld :newTitle="newTitle"></HelloWorld>
</template>

<script setup>
import HelloWorld from "@/components/HelloWorld.vue"
import { ref } from "vue"
const newTitle = ref("hello world");
setTimeout(() => {
	newTitle.value = "小白菜";
}, 2000);
</script>

<style scoped>

</style>
```

### 🌸 子组件修改父组件的值

一句话叫不能`直接修改`, 想要修改就要子`向父传值`, 让`父去修改

## 🌲 子向父传值($emit)

### 🌸 子组件上定义回调方法

首先我们要在子组件上定义一个方法别名, 比如我这里叫`@sub-click`, 然后绑定一个父组件的方法, 我这里叫`changeTitle`

```vue
<template>
	<HelloWorld :newTitle="newTitle" @sub-click="changeTitle"></HelloWorld>
</template>

<script setup>
	import HelloWorld from "@/components/HelloWorld.vue"
	import {
		ref
	} from "vue"
	const newTitle = ref("hello world");
	const changeTitle = (value) => {
		newTitle.value = value;
	}
</script>

<style scoped>

</style>
```

### 🌸 子组件触发方法

子传值需要使用`$emit`来调用父组件方法, 我们调用方法的时候无法直接调用方法本身, 所以要`emit`刚才定义的别名

```vue
<template>
	<h1 @click="click">{{ newTitle }}</h1>
</template>

<script setup>
	import {
		defineProps,
		defineEmits
	} from "vue"
	defineProps(['newTitle']);
	const emit = defineEmits();
	const click = () => {
		emit('sub-click', '小白菜啊');
	};
</script>

<style scoped>

</style>
```

### 🌸 计算属性

我们也可以把`$emit`封装成计算属性, 原理还是`$emit`到父组件, 只不过用计算属性封装一下让它使用起来方便了一些

```vue
<template>
	<h1 @click="click">{{ NewTitle }}</h1>
</template>

<script setup>
	import {
		defineProps,
		defineEmits,
		computed
	} from "vue"

	const props = defineProps(['newTitle']);
	const emit = defineEmits();

	const click = () => {
		NewTitle.value = "小白菜啊"
	};

	const NewTitle = computed({
		set(val) {
			emit('sub-click', val);
		},
		get() {
			return props.newTitle;
		}
	})
</script>

<style scoped>

</style>
```

## 🌲 子向父传值(v-model)

在`vue3`中可以直接使用`v-model`来同步父和子的变量了

```vue
<template>
	<h1 @click="click">{{ NewTitle }}</h1>
</template>

<script setup>
	import {
		defineProps,
		defineEmits,
		computed
	} from "vue"

	const props = defineProps(['newTitle']);
	const emit = defineEmits();

	const click = () => {
		NewTitle.value = "小白菜啊"
	};

	const NewTitle = computed({
		set(val) {
			emit('update:newTitle', val);
		},
		get() {
			return props.newTitle;
		}
	})
</script>

<style scoped>

</style>
```

## 🌲 父调用子方法

我们上面学会了子调用父方法, 那就是`$emit`, 那么父调用子方法要怎么做呢? 没错就是`ref`

### 🌸 在子组件中声明方法

比如我定义了一个叫`changeTitle`的方法用于改变标题, 在`setup`语法中需要使用`defineExpose`把方法暴漏出去, 否则父组件无法使用

```vue
<template>
	<h1>{{ title }}</h1>
</template>

<script setup>
	import {
		defineProps,
		defineEmits,
		ref,
		defineExpose
	} from "vue"

	const title = ref("hello world");
	const changeTitle = (value) => {
		title.value = value;
	};

	defineExpose({
		changeTitle
	});
</script>

<style scoped>

</style>
```

### 🌸 父组件中使用ref调用

在子组件中定义一个属性叫`ref`, 在`setup`语法中不能再使用`this.$refs`, 取而代之的是定义一个`ref`类型的变量传递给`ref`, 调用的时候要`.value`

```vue
<template>
	<HelloWorld ref="hello" @click="click"></HelloWorld>
</template>

<script setup>
	import HelloWorld from "@/components/HelloWorld.vue"
	import {
		ref
	} from "vue"

	const hello = ref(null);
	const click = () => {
		hello.value.changeTitle("小白菜啊");
	};
</script>

<style scoped>

</style>
```

## 🌲 异步组件

### 🌸 组件分包

有时候一个组件会相当的庞大, 都放在主组件入口中会出现加载缓慢的情况, 那么要怎么优化呢, `vue`提供了组件分包机制, 下面就一起来看吧

普通我们导入组件都是这么导入的

```vue
<script setup>
import TestViewVue from '@/pages/test/components/TestView.vue';
</script>
```

异步组件要使用特有的函数

```vue
<script setup>
import { defineAsyncComponent } from 'vue';
const TestViewVue = defineAsyncComponent(() => import('@/pages/test/components/TestView.vue'))
</script>
```

使用的时候没有什么差别

打包的时候会多打出这个组件的js文件, 也就是从主js中拆分出来了

# 🍎 上传

上传是一个大功能, 要认真学习

## 🌲 声明

其实声明一个上传的组件是非常简单的, 就是一个`<input>`

```vue
<template>
  <input type="file" name="file" @change="change" />
</template>

<script setup>
let change = (e) => {
  console.log(e);
};
</script>
```

看一下

![](images/Pasted%20image%2020240423205618.png)

我们可以看到它打印的是一个`event`

```js
Event {isTrusted: true, _vts: 1713876954031, type: 'change', target: input, currentTarget: input, …}
```

我们的文件对象就在这个`event`的`target`中

```js
let change = (e) => {
  console.log(e.target.files[0]);
};
```

打印是这样的, 可以看到它是一个file对象

```
File {name: '6.jpg', lastModified: 1701665968473, lastModifiedDate: Mon Dec 04 2023 12:59:28 GMT+0800 (中国标准时间), webkitRelativePath: '', size: 411449, …}
```

## 🌲 限制大小

我们可以判断`file`的`size`来限制文件的大小

```vue
<template>
  <input type="file" name="file" @change="change" />
</template>

<script setup>
let change = (e) => {
  const file = e.target.files[0];
  console.log(file.size / 1024);
  if (file.size / 1024 > 100) {
    alert("文件不能大于100K");
  }
};
</script>
```

## 🌲 限制类型

我们可以判断`file`的`type`来限制文件类型

```vue
<template>
  <input type="file" name="file" @change="change" />
</template>

<script setup>
let change = (e) => {
  const file = e.target.files[0];
  console.log(file);
  console.log(file.type);
  if (file.type != 'image/jpeg') {
    alert("只能上传图片");
  }
};
</script>
```

## 🌲 转为blob

```vue
<template>
  <input type="file" name="file" @change="change" />
</template>

<script setup>
let change = (e) => {
  const file = e.target.files[0];
  console.log(new Blob([file]));
};
</script>
```

## 🌲 转为file

`blob`也可以转为`file`

```vue
<template>
  <input type="file" name="file" @change="change" />
</template>

<script setup>
let change = (e) => {
  const file = e.target.files[0];
  const blob = new Blob([file]);
  const file2 = new File([blob], "1.jpg");
  console.log(file2);
};
</script>
```

## 🌲 转为base64

我们可以把file转为`base64`

```vue
<template>
  <input type="file" name="file" @change="change" />
</template>

<script setup>
let change = (e) => {
  const file = e.target.files[0];
  const fr = new FileReader();
  fr.readAsDataURL(file);
  fr.onload = () => {
    console.log(fr.result);
  };
};
</script>
```

可以看到就是这种数据

```js
data:image/jpeg;base64,iVBORw0KGgoAAAANSUhEUgAADpoAAARgCAYAAABDz2R9AAABYWlDQ1BJQ0MgUHJvZmlsZQAAKJFjYGDiSSwoyGFhYGDIzSspCnJ3UoiIjFJgf87Aw8ACxGIMsonJxQWOAQE+QCUMMBoVfLvGwAiiL+uCzFrv0aWuLOc11aT+b3zRAiNHTPUogCsltTgZSP8BYpPkgqISBgZGAyA7oLykAMRuALJFioCOArKngNjpEPYKEDsJwt4DVhMS5AxkXwCyBZIzElOA7AdAtk4Skng6Ejs3pzQZ6gaQ63lS80KDgbQEEMswuDC4MvgAoQJDMIMRgzkQGzEEMjjj0GMC1uPMkM9QwFDJUMSQyZDOkMFQAtTtCBQpYMhhSAWyPRnyGJIZ9Bh0gGwjBgMgNgaFNXoYIsQKPzAwWEwCWtWMEIuNYWDYBvQXzzGEmHoX0Dt9DAxHnhQkFiXCQ5bxG0txmrERhM29nYGBddr//5/DGRjYNRkY/l7////39v///y5jYGC+xcBw4BsA7NVj15iGb4IAAABsZVhJZk1NACoAAAAIAAQBGgAFAAAAAQAAAD4BGwAFAAAAAQAAAEYBKAADAAAAAQACAACHaQAEAAAAAQAAAE4AAAAAAAAAk......
```

## 🌲 显示出来

这个也不难就是直接`v-bind`绑定就行

```vue
<template>
  <img :src="base64" />
  <input type="file" name="file" @change="change" />
</template>

<script setup>
import { ref } from "vue";
const base64 = ref("");

let change = (e) => {
  const file = e.target.files[0];
  const fr = new FileReader();
  fr.readAsDataURL(file);
  fr.onload = () => {
    base64.value = fr.result;
  };
};
</script>
```

我们来看一下效果

![](images/Pasted%20image%2020240423225358.png)

可以选择的图片显示出来了

## 🌲 上传

上传分为两种, 一种是二进制文件, 一种是转为`base64`字符串

### 🌸 上传单文件

上传文件也是比较简单的, 我们只需要传入一个`FormData`对象即可, axios会自动帮我们进行表单上传

```vue
<template>
  <input type="file" name="file" @change="change" />
</template>

<script setup>
import { ref } from "vue";
import axios from "axios";
const base64 = ref("");

let change = (e) => {
  const file = e.target.files[0];
  const formdata = new FormData();
  formdata.append("file", file);
  formdata.append("userName", "张三");
  axios.post("/upload", formdata).then((res) => {
    console.log(res);
  });
};
</script>
```

我们虽然没有`/upload`接口, 但是我们可以在浏览器上看到内容

![](images/Pasted%20image%2020240423231522.png)

可以看到我们是传递了两个东西, 一个是文件, 一个是字符串张三, 我们可以点击查看源代码来看格式规范

```
------WebKitFormBoundaryZUiAFdFZ6gYAMszQ
Content-Disposition: form-data; name="file"; filename="6.jpg"
Content-Type: image/jpeg


------WebKitFormBoundaryZUiAFdFZ6gYAMszQ
Content-Disposition: form-data; name="userName"

张三
------WebKitFormBoundaryZUiAFdFZ6gYAMszQ--
```

### 🌸 上传多文件

```vue
<template>
  <img :src="base64" />
  <input type="file" @change="change" multiple />
</template>

<script setup>
import { ref } from "vue";
import axios from "axios";
const base64 = ref("");

let change = (e) => {
  const formdata = new FormData();
  for (let i = 0; i < e.target.files.length; i++) {
    formdata.append("files", e.target.files[i]);
  }
  formdata.append("userName", "张三");
  axios
    .post("/upload", formdata)
    .then((res) => {
      console.log(res);
    });
};
</script>
```


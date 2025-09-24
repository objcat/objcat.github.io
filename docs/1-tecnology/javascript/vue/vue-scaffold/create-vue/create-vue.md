# 🍎 简介

`create-vue`是主流的快速构建`vue`的工具, 它使用`vite`作为构建工具

# 🍎 vue2

我们使用`vite+vu2`来举例

https://blog.csdn.net/qq991658923/article/details/127325711

https://github.com/ElanYoung/vite-vue2-js-starter-template/tree/master

## 🌲 创建项目

我们可以使用vue官方自带的工具创建

```shell
npm init vue@2
```

其原理就是安装并执行`create-vue`

https://www.npmjs.com/package/create-vue?activeTab=readme

所以我们也可以执行下面的操作

```shell
npm create vue@2
```

然后按需求选择就可以了, 最后创建出来的项目就是`vite`的, 如果执行的很慢建议配置一下镜像源

[跳转 npm](../../../../../4-package-manager/npm/npm/npm.md)

## 🌲 package.json

我们来看一下

```json
{
  "name": "test-vue2",
  "version": "0.0.0",
  "scripts": {
    "dev": "vite",
    "build": "vite build",
    "preview": "vite preview --port 4173"
  },
  "dependencies": {
    "pinia": "^2.1.7",
    "vue": "^2.7.7",
    "vue-router": "^3.5.4"
  },
  "devDependencies": {
    "@vitejs/plugin-legacy": "^2.0.0",
    "@vitejs/plugin-vue2": "^1.1.2",
    "terser": "^5.14.2",
    "vite": "^3.0.2"
  }
}
```

- @vitejs/plugin-legacy 防止浏览器在低版本白屏

但是我们会看到上面的版本都是非常低的, 所以我们可以给它升级一下版本, `2023.10.13`

```json
{
  "name": "test-pinia",
  "version": "1.0.0",
  "scripts": {
    "dev": "vite",
    "build": "vite build",
    "preview": "vite preview --port 4173"
  },
  "dependencies": {
    "pinia": "^2.1.7",
    "vue": "^2.7.14",
    "vue-router": "^3.6.5"
  },
  "devDependencies": {
    "@vitejs/plugin-legacy": "^4.1.1",
    "@vitejs/plugin-vue2": "^2.2.0",
    "terser": "^5.14.2",
    "vite": "^4.4.11"
  }
}
```

其实不更新也是可以的, 因为差距不大

# 🍎 vue3

使用下面的命令创建

```
npm init vue@3
```

我们看一下依赖, 发现比`vue2`简洁太多了

```json
{
  "name": "test-vue3",
  "version": "1.0.0",
  "private": true,
  "scripts": {
    "dev": "vite",
    "build": "vite build",
    "preview": "vite preview"
  },
  "dependencies": {
    "pinia": "^2.1.7",
    "vue": "^3.3.4",
    "vue-router": "^4.2.5"
  },
  "devDependencies": {
    "@vitejs/plugin-vue": "^4.4.0",
    "vite": "^4.4.11"
  }
}
```

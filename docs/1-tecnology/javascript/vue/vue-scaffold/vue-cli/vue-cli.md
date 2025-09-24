# 🍎 简介

vue官方的项目构建工具, 可以帮助我们快速搭建项目

# 🍎 快速开始

## 🌲 安装vue-cli

两种安装方法, 选其一就可以, 别忘了配置镜像源

```shell
# 使用npm
npm -g i @vue/cli
# 使用yarn
yarn global add @vue/cli
```

## 🌲 查看版本

```shell
vue -V
@vue/cli 5.0.8
```

使用`vue-cli`创建项目是比较方便的, 里面使用了`webpack`作为打包工具, 如果是创建`vue2`的项目还是比较推荐的, 但是`vue3`一般是使用`vite`

```shell
vue create test-app
```

选择第三个可以自定义

```shell
Vue CLI v5.0.3
? Please pick a preset: (Use arrow keys)
  Default ([Vue 3] babel, eslint) 
  Default ([Vue 2] babel, eslint) 
❯ Manually select features 
```

勾选需要的工具包

```shell
Vue CLI v5.0.3
? Please pick a preset: Manually select features
? Check the features needed for your project: (Press <space> to select, <a> to t
oggle all, <i> to invert selection, and <enter> to proceed)
 ◉ Babel
 ◉ TypeScript
 ◯ Progressive Web App (PWA) Support
 ◉ Router
❯◉ Vuex
 ◯ CSS Pre-processors
 ◉ Linter / Formatter
 ◯ Unit Testing
 ◯ E2E Testing
```

选择vue3

```shell
Vue CLI v5.0.3
? Please pick a preset: Manually select features
? Check the features needed for your project: Babel, TS, Router, Vuex, Linter
? Choose a version of Vue.js that you want to start the project with (Use arrow 
keys)
❯ 3.x 
  2.x 
```

后面都点确定就可以了, 创建完我们看一下配置

```json
{
  "name": "test-app2",
  "version": "0.1.0",
  "private": true,
  "scripts": {
    "serve": "vue-cli-service serve",
    "build": "vue-cli-service build",
    "lint": "vue-cli-service lint"
  },
  "dependencies": {
    "core-js": "^3.8.3",
    "vue": "^3.2.13"
  },
  "devDependencies": {
    "@babel/core": "^7.12.16",
    "@babel/eslint-parser": "^7.12.16",
    "@vue/cli-plugin-babel": "~5.0.0",
    "@vue/cli-plugin-eslint": "~5.0.0",
    "@vue/cli-service": "~5.0.0",
    "eslint": "^7.32.0",
    "eslint-plugin-vue": "^8.0.3"
  },
  "eslintConfig": {
    "root": true,
    "env": {
      "node": true
    },
    "extends": [
      "plugin:vue/vue3-essential",
      "eslint:recommended"
    ],
    "parserOptions": {
      "parser": "@babel/eslint-parser"
    },
    "rules": {}
  },
  "browserslist": [
    "> 1%",
    "last 2 versions",
    "not dead",
    "not ie 11"
  ]
}
```
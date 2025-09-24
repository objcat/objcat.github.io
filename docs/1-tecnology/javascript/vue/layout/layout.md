# 🍎 简介

记录常用布局

# 🍎 初始配置

```css
html,
body {
  margin: 0;
  padding: 0;
}
```

# 🍎 屏幕框架

## 🌲 导航栏+内容

```vue
<template>
  <!-- 最外层 -->
  <div class="zy-layout-wrap">
    <!-- 导航栏 -->
    <div class="zy-layout-wrap-navi"></div>
    <!-- 内容 -->
    <div class="zy-layout-wrap-content"></div>
  </div>
</template>

<style scoped>
.zy-layout-wrap {
  display: flex;
  flex-direction: column;
  width: 100vw;
  min-height: 100vh;
  overflow: hidden;
}

.zy-layout-wrap-navi {
  height: 5rem;
  overflow: hidden;
  background-color: rgb(252, 165, 165);
}

.zy-layout-wrap-content {
  flex: 1;
  overflow: hidden;
  background-color: rgb(147, 197, 253);
}
</style>
```

看一下效果

![](images/Pasted%20image%2020240417175821.png)

## 🌲 导航栏+内容+底栏

```vue
<template>
  <!-- 最外层 -->
  <div class="zy-layout-wrap">
    <!-- 导航栏 -->
    <div class="zy-layout-wrap-navi"></div>
    <!-- 内容 -->
    <div class="zy-layout-wrap-content"></div>
    <!-- 页脚 -->
    <div class="zy-layout-wrap-footer"></div>
  </div>
</template>

<style scoped>
.zy-layout-wrap {
  display: flex;
  flex-direction: column;
  width: 100vw;
  min-height: 100vh;
  overflow: hidden;
}

.zy-layout-wrap-navi {
  height: 5rem;
  overflow: hidden;
  background-color: rgb(252, 165, 165);
}

.zy-layout-wrap-content {
  flex: 1;
  overflow: hidden;
  background-color: rgb(147, 197, 253);
}

.zy-layout-wrap-footer {
  height: 8rem;
  overflow: hidden;
  background-color: rgb(253, 224, 71);
}
</style>
```

看一下效果

![](images/Pasted%20image%2020240417185018.png)

## 🌲 导航栏+内容+侧边栏1

```vue
<template>
  <!-- 最外层 -->
  <div class="zy-layout-wrap">
    <!-- 1层横向布局 -->
    <div class="zy-layout-wrap-row">
      <!-- 侧边栏 -->
      <div class="zy-layout-wrap-sidebar"></div>
      <!-- 2层上下布局 -->
      <div class="zy-layout-wrap-column">
        <!-- 导航栏 -->
        <div class="zy-layout-wrap-navi"></div>
        <!-- 内容 -->
        <div class="zy-layout-wrap-content"></div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.zy-layout-wrap {
  display: flex;
  flex-direction: column;
  width: 100vw;
  min-height: 100vh;
  overflow: hidden;
}

.zy-layout-wrap-row {
  display: flex;
  flex-direction: row;
  flex: 1;
  overflow: hidden;
}

.zy-layout-wrap-sidebar {
  width: 20rem;
  overflow: hidden;
  background-color: rgb(134, 239, 172);
}

.zy-layout-wrap-column {
  display: flex;
  flex-direction: column;
  flex: 1;
  overflow: hidden;
}

.zy-layout-wrap-navi {
  height: 5rem;
  overflow: hidden;
  background-color: rgb(252, 165, 165);
}

.zy-layout-wrap-content {
  flex: 1;
  overflow: hidden;
  background-color: rgb(147, 197, 253);
}
</style>
```

看看效果

![](images/Pasted%20image%2020240417190229.png)

## 🌲 导航栏+内容+侧边栏2

```vue
<template>
  <!-- 最外层 -->
  <div class="zy-layout-wrap">
    <!-- 导航栏 -->
    <div class="zy-layout-wrap-navi"></div>
    <!-- 1层横向布局 -->
    <div class="zy-layout-wrap-row">
      <!-- 侧边栏 -->
      <div class="zy-layout-wrap-sidebar"></div>
      <!-- 内容 -->
      <div class="zy-layout-wrap-content"></div>
    </div>
  </div>
</template>

<style scoped>
.zy-layout-wrap {
  display: flex;
  flex-direction: column;
  width: 100vw;
  min-height: 100vh;
  overflow: hidden;
}

.zy-layout-wrap-row {
  display: flex;
  flex-direction: row;
  flex: 1;
  overflow: hidden;
}

.zy-layout-wrap-sidebar {
  width: 20rem;
  overflow: hidden;
  background-color: rgb(134, 239, 172);
}

.zy-layout-wrap-navi {
  height: 5rem;
  overflow: hidden;
  background-color: rgb(252, 165, 165);
}

.zy-layout-wrap-content {
  flex: 1;
  overflow: hidden;
  background-color: rgb(147, 197, 253);
}
</style>
```

![](images/Pasted%20image%2020240426201247.png)

# 🍎 内容区域

我们来看一下内容区域的布局

## 🌲 三等分

分为左中右三个

```vue
<template>
  <!-- 最外层 -->
  <div class="zy-layout-wrap">
    <!-- 1层横向布局 -->
    <div class="zy-layout-wrap-row">
      <!-- 2层左边容器 -->
      <div class="zy-layout-wrap-left"></div>
      <!-- 2层中间容器 -->
      <div class="zy-layout-wrap-middle"></div>
      <!-- 2层右边容器 -->
      <div class="zy-layout-wrap-right"></div>
    </div>
  </div>
</template>

<style scoped>
.zy-layout-wrap {
  display: flex;
  flex-direction: column;
  width: 100%;
  min-height: 100%;
  overflow: hidden;
}

.zy-layout-wrap-row {
  display: flex;
  flex-direction: row;
  flex: 1;
  overflow: hidden;
}

.zy-layout-wrap-left {
  display: flex;
  flex-direction: column;
  flex: 1;
  overflow: hidden;
  background-color: red;
}

.zy-layout-wrap-middle {
  display: flex;
  flex-direction: column;
  flex: 1;
  overflow: hidden;
  background-color: yellow;
}

.zy-layout-wrap-right {
  display: flex;
  flex-direction: column;
  flex: 1;
  overflow: hidden;
  background-color: blue;
}
</style>
```

来看一下效果

![](images/Pasted%20image%2020240418102319.png)

压缩的时候三个会一起压缩, 如果不想一起压缩可以把中间的设置为`rem`的固定宽度即可
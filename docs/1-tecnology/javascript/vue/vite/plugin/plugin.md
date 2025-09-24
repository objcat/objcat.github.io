
# 🍎 快速开始

## 🌲 @vitejs/plugin-legacy

兼容低版本浏览器

```
npm install @vitejs/plugin-legacy -D
```

然后配置插件即可

```js
// vite.config.js
import legacy from '@vitejs/plugin-legacy';

export default {
  plugins: [
    legacy({
      targets: ['ie >= 11'],
    }),
  ],
};
```

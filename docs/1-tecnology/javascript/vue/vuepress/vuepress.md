# 🍎 官方网站

https://vuepress.vuejs.org/guide/getting-started.html#prerequisites

# 🍎 安装

步骤比较简单, 我们一起来看看

## 🌲 创建项目

```shell
yarn create vuepress-site test-vuepress
```

## 🌲 安装依赖

```shell
cd test-vuepress
cd docs
yarn
```

## 🌲 启动

```
yarn dev
```

之后我们可以看到是这个样子的

![](images/Pasted%20image%2020221212170208.png)

到这里vuepress就启动成功了

# 🍎 使用

## 🌲 配置目录

添加栏目我们需要进入到`src`目录, 然后显示隐藏文件, 里面有一个`.vuepress`文件夹, 打开里面的config.js

![](images/Pasted%20image%2020221212170312.png)

里面有一个config文件

我们使用`vscode`打开这个文件就能够编辑了

```js
const { description } = require('../../package')

module.exports = {
  /**
   * Ref：https://v1.vuepress.vuejs.org/config/#title
   */
  title: 'Vuepress Docs Boilerplate',
  /**
   * Ref：https://v1.vuepress.vuejs.org/config/#description
   */
  description: description,

  /**
   * Extra tags to be injected to the page HTML `<head>`
   *
   * ref：https://v1.vuepress.vuejs.org/config/#head
   */
  head: [
    ['meta', { name: 'theme-color', content: '#3eaf7c' }],
    ['meta', { name: 'apple-mobile-web-app-capable', content: 'yes' }],
    ['meta', { name: 'apple-mobile-web-app-status-bar-style', content: 'black' }]
  ],

  /**
   * Theme configuration, here is the default theme configuration for VuePress.
   *
   * ref：https://v1.vuepress.vuejs.org/theme/default-theme-config.html
   */
  themeConfig: {
    repo: '',
    editLinks: false,
    docsDir: '',
    editLinkText: '',
    lastUpdated: false,
    nav: [
      {
        text: 'Guide',
        link: '/guide/',
      },
      {
        text: 'Config',
        link: '/config/'
      },
      {
        text: 'VuePress',
        link: 'https://v1.vuepress.vuejs.org'
      }
    ],
    sidebar: {
      '/guide/': [
        {
          title: 'Guide',
          collapsable: false,
          children: [
            '',
            'using-vue',
          ]
        }
      ],
    }
  },

  /**
   * Apply plugins，ref：https://v1.vuepress.vuejs.org/zh/plugin/
   */
  plugins: [
    '@vuepress/plugin-back-to-top',
    '@vuepress/plugin-medium-zoom',
  ]
}
```

## 🌲 添加栏目

我们在配置中一眼就能看见`nav`下面代表的就是栏目

```js
nav: [
  {
	text: 'Guide',
	link: '/guide/',
  },
  {
	text: 'Config',
	link: '/config/'
  },
  {
	text: 'VuePress',
	link: 'https://v1.vuepress.vuejs.org'
  }
]
```

栏目也文件夹是要对应的, 比如我想添加一个Java栏目, 那么我需要先创建一个Java文件夹

![](images/Pasted%20image%2020221212170803.png)

然后我们在配置文件中添加这个路径

```js
nav: [
  {
	text: 'Java',
	link: '/Java/',
  },
  {
	text: 'Guide',
	link: '/guide/',
  },
  {
	text: 'Config',
	link: '/config/'
  },
  {
	text: 'VuePress',
	link: 'https://v1.vuepress.vuejs.org'
  }
]
```

我们刷新一下页面就可以看见了

![](images/Pasted%20image%2020221212171058.png)

## 🌲 添加目录

添加目录也很简单, 我们在配置文件中可以找到一个`sidebar`

```js
sidebar: {
  '/guide/': [
	{
	  title: 'Guide',
	  collapsable: false,
	  children: [
		'',
		'using-vue',
	  ]
	}
  ],
}
```

我们可以在里面添加一些路径, 我们接下来就添加一下, 我们可以看到, 如果是README.md就不需要配置标题, 但是要留位置使用单引号来填补空位

```js
sidebar: {
  '/guide/': [
	{
	  title: 'Guide',
	  collapsable: false,
	  children: [
		'',
		'using-vue',
	  ]
	}
  ],
  '/Java/': [
	{
	  title: 'Java学习目录',
	  collapsable: false,
	  children: [
		''
	  ]
	}
  ],
}
```

然后我们来预览一下效果

![](images/Pasted%20image%2020221212172446.png)

然后我们再添加一个别的, 比如`hello.md`, 我们首先在目录下面建立文件

![](images/Pasted%20image%2020221212172600.png)

然后再配置一下

```js
sidebar: {
  '/guide/': [
	{
	  title: 'Guide',
	  collapsable: false,
	  children: [
		'',
		'using-vue',
	  ]
	}
  ],
  '/Java/': [
	{
	  title: 'Java学习目录',
	  collapsable: false,
	  children: [
		'',
		'hello'
	  ]
	}
  ],
}
```

再次预览发现可以用了

![](images/Pasted%20image%2020221212172803.png)

# 🍎 打包

```
yarn build
```

然后我们可以看到在`.vuepress`文件夹生成了`dist`
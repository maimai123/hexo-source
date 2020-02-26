---
title: 用 cooking 搭建一个多页面易配置的 Vue 2 项目
date: 2018-08-09
tags: vue
---
学vue第一步，那肯定是要搭建一个简单地脚手架啦，用啥搭建呢 vue-cli是一个很好的脚手架组件，但我们今天用cooking来搭建一个多页面（means-> 不是单页哦 单页项目run build以后生成的文件合并成一个，请求文件过于大，所以不建议。所以打包出多页面就刻不容缓啦~接下来跟着麦麦一起去学习吧）本文出自[知乎](https://zhuanlan.zhihu.com/p/22610408)

## 搭建基础项目--<font size=1>直接通过 cooking 的命令行工具直接生成一个 Vue 项目</font>
**npm i cooking-cli -g**
or 切换至淘宝镜像
**npm i cooking-cli -g --registry=https://registry.npm.taobao.org**
完成后可以到你的目录下执行下面指令创建一个目录名 xxx 的 Vue 项目，第一次执行需要安装脚手架的依赖。
**cooking create xxx vue**
接下来会让你选择一些选项，我们这次选择 Vue2 + bublé + 全局 cooking 的配置。
> [?] Give your app a name: multiple-pages
> [?] Give your app a description: A vue project.
> [?] Private? Yes
> [?] What Vue version do you what? Vue 2
> [?] What ES2015+ compiler do you what to use? bublé (only use wepback 2)
> [?] What way use cooking do you want? Global cooking (webpack 2)
> [?] Need dev server? Yes
> [?] What CSS preprocessor do you want to use? Only CSS
> [?] Setup unit tests with Karma + Mocha? No
> [?] git repository:
> [?] author:
> [?] license: ISC
> [?] Continue? Yes

最后可以试试直接运行 npm run dev 看看能不能正常启动。使用全局 cooking 的好处是可以减少项目的依赖。

## 多页面项目分析
写一个名叫 app.json 的配置文件，每个入口共享公共的 CDN 也可以配置私有的 CDN，还可以配置其他基本信息。
<!--more-->
```javascript
{
  "pages": [
    {
      "entry": "home",
      "title": "首页",
      "cdn": {}
    },
    {
      "entry": "admin",
      "title": "后台",
      "cdn": {}
    }
  ],
  "basePath": "./src/pages/",
  "cdn": {
    "js": [
      "//cdn.jsdelivr.net/vue/2.0.0-rc.7/vue.min.js",
      "//cdn.jsdelivr.net/vuex/2.0.0-rc.5/vuex.min.js"
    ],
    "css": []
  },
  "externals": {
    "vue": "Vue",
    "vuex": "Vuex"
  }
}
```
同时我们在 src/pages 目录下创建 home 和 admin 目录。每个目录下创建一个 index.js 和 app.vue 文件。

**index.js**
```javascript
import Vue from 'vue'
import App from './app'
new Vue({ // eslint-disable-line
  el: '#app',
  render: h => h(App)
})
```
**app.vue**
```javascript
<template>
  <div>
    <h1>首页</h1>
    <p>A vue project.</p>
  </div>
</template>
<script>
  export default {
    name: 'app'
  };
</script>
```
## 配置 cooking --<font size=1>入口文件</font>
接下来我们在生成的 cooking 配置文件上加工，这里我们要传入多入口的配置，从 app.json 里读取 entry 的信息，通过 basePath 拼接成文件路径。

> **var App = require('./app.json')
> var entries = function() {
>   var result = {}
>   App.pages.forEach(p => {
>     result[p.entry] = path.resolve(App.basePath, p.entry)
>   })
>   return result
> }
> cooking.set({
>   entry: entries()
> })**

## 模板文件
所有入口的页面我们都是通过 index.tpl 模板配置，只需要将公用 CDN 和私有 CDN 合并后拼接成 HTML 插入到模板内，同时引入入口文件和 vendor，通过 html-webpack-plugin 的配置选项，可以很方便的实现我们的需求。
```javascript
var App = require('./app.json')
var path = require('path')
var merge = function(a, b) {
  return {
    css: (a.css || []).concat(b.css || []),
    js: (a.js || []).concat(b.js || [])
  }
}
var templates = function() {
  return App.pages.map(p => {
    return {
      title: p.title,
      filename: p.entry + '.html',
      template: path.resolve(__dirname, 'index.tpl'),
      cdn: merge(App.cdn, p.cdn),
      chunks: ['vendor', 'manifest', p.entry]
    }
  })
}
cooking.set({
  template: templates()
})
模板文件也要改造一下。支持生成我们指定的 CDN 的 HTML 以及其他配置项。具体语法参考插件文档。
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title><%= htmlWebpackPlugin.options.title %></title>
    <% for (var i in htmlWebpackPlugin.options.cdn.css) { %>
    <link rel="stylesheet" href="<%= htmlWebpackPlugin.options.cdn.css[i] %>"><% } %>
  </head>
  <body>
    <div id="app"></div>
    <% for (var i in htmlWebpackPlugin.options.cdn.js) { %>
    <script src="<%= htmlWebpackPlugin.options.cdn.js[i] %>"></script><% } %>
  </body>
</html>
```
## 最终配置
最后我们可以优化一下配置，将生成配置的函数提取到另一个文件内，让配置信息更清晰。那么最终的配置内容如下。
```javascript
var path = require('path')
var cooking = require('cooking')
var build = require('./build')
cooking.set({
  entry: build.entries(),
  dist: './dist',
  template: build.templates(),
  devServer: {
    port: 8081,
    publicPath: '/',
  },
  clean: true,
  hash: true,
  sourceMap: true,
  chunk: true,
  publicPath: '/dist/',
  extractCSS: true,
  alias: {
    'src': path.join(__dirname, 'src')
  },
  extends: ['vue2', 'buble', 'lint', 'autoprefixer'],
  externals: build.externals()
})
module.exports = cooking.resolve()
```

## 运行项目
我们直接通过 cooking 命令行启动项目。
**cooking watch -p**
最后我们通过 build 构建项目，最终打包文件在 dist 目录里。
**cooking build -p**

## 开发目录说明

** src --开发目录

 +components 自定义组件

 +pages 页面存放，文件夹名对应页面名字

 +assets 静态资源文件，如公用的样式和字体文件以及图片

举个栗子：

pages
  + index
    index.js   //页面初始化
    app.vue    //页面逻辑，包括tempalte,script,style**

## 引入饿了么组件
饿了么组件一出就引起广大vue爱好者的好评 我们当然不能落下啦 接下来来安装饿了么组件

安装饿了么组件 引入 Element
**npm i element-ui -S**
你可以引入整个 Element，或是根据需要仅引入部分组件。我们先介绍如何引入完整的 Element。
> ¶ 完整引入
在 main.js 中写入以下内容：
```javascript
import Vue from 'vue'
import ElementUI from 'element-ui'
import 'element-ui/lib/theme-default/index.css'
import App from './App.vue'
Vue.use(ElementUI)
new Vue({
  el: '#app',
  render: h => h(App)
})
```
> ¶ 按需引入
借助 babel-plugin-component，我们可以只引入需要的组件，以达到减小项目体积的目的。
首先，安装 **babel-plugin-component：npm install babel-plugin-component -D**
```javascript
import Vue from 'vue';
import App from './app';
import { Button, Select } from 'element-ui'
import 'element-ui/lib/theme-default/index.css'
Vue.use(Button)
Vue.use(Select)

new Vue({
    el: "#app",
    render:h => h(App)
})
```
然后，将 .babelrc 修改为：
```javascript
{
  "presets": ["es2015", "stage-0", "stage-2"],
  "comments": false,
  "env": {
    "production": {
      "plugins": [
        ["transform-runtime", { "polyfill": false, "regenerator": false }],
        ["component", [
          {
            "libraryName": "element-ui",
            "styleLibraryName": "theme-default"
          }
        ]]
      ]
    }
  }
}
```
参考脚手架：[麦麦的github](https://github.com/maimai123/cookingVue2.git)

---
title: Vuex学习小计（麦麦）
---
<i class="icon-adjust"></i> 初学vuex，总得弄懂他的思想。
<font size=2>vuex是什么？ 下面是官网给出的说法。
Vuex 是一个专为 Vue.js 应用程序开发的状态管理模式。它采用集中式存储管理应用的所有组件的状态，
并以相应的规则保证状态以一种可预测的方式发生变化。Vuex 也集成到 Vue 的官方调试工具 devtools extension，
提供了诸如零配置的 time-travel 调试、状态快照导入导出等高级调试功能。
总而言之，言而总之。就是用来管理你的state的！！！</font>
<font size=1>**如有转载，请注明出处。**</font>

**我们从零开始。**
<font size=1>可以参考官网 [vuex](https://github.com/vuejs/vuex) 以及vuex文档 [vuex-文档](https://vuex.vuejs.org/zh-cn/intro.html) 也可以直接clone我的代码 [麦麦的github](https://github.com/maimai123/maimai-vuex-Test.git)</font>
<!--more-->
> <font size=2>打开一个空的文件夹 dua~ 打开了吧？</font>
> <font size=2>安装 vue-cli。(确保你有node和npm)</font>
> <font size=2>npm i -g vue-cli （只需安装一次，日后可省略这步）</font>

> <font size=2>然后创建一个webpack项目并且下载依赖</font>
> <font size=2>vue init webpack Test</font>

> <font size=2>进入 你创建的文件夹下 安装依赖</font>
> <font size=2>npm install</font>

> <font size=2>这些都准备好后，我们需要为我们的路由、vuex和XHR请求下载几个库，我们可以从vue的官网中找到他们。</font>
> <font size=2>npm i vue-resource vue-router vuex --save</font>

> <font size=2>如果你需要一个css编译器 你可以安装less-loader, vuejs-templates模板默认不安装less</font> less-loader
> <font size=2>npm install less less-loader --save-dev</font>

> <font size=2>接着使用 npm run dev 在热加载中运行我们的应用！！！</font>
>
> <font size=2>打开浏览器应该可以看到我们的vue应用啦~</font>
>
>*  <font size=2>我之前启动包了一个错 原因是 vue 的版本和 vue-template-compiler 的版本不一致 ，你可以在 package.json中 修改成相同的版本</font>
>
>*  <font size=2>然后我们就可以开始修改文件啦~ 鸡冻不？</font>
>
>*  <font size=2>**App.vue**是一个类似于layout的文件。路由配置在**router**文件夹下</font>
>
>*  <font size=2>**components**文件夹下用来放你的组件</font>
>
>*  <font size=2>**assets**文件夹下存放你的静态资源</font>
>
>*  <font size=2>我们新建一个**vuex**文件夹，这就开始学习状态管理啦~</font>
>
>* <font size=2>先新建 **store.js** 用来存放我们的状态</font>
>
>* <font size=2>再新建 **getters** 文件夹 用来处理state数据</font>
>
>* <font size=2>再新建 **mutations** 文件夹 用来注册我们各种数据变化的方法</font>
>
>* <font size=2>再新建 **mutations-types.js** 文件 用来记录我们所有的事件名(要写上备注哦 代码可不只是你一个人看的)</font>
>
>* <font size=2>再新建 **actions** 文件夹 可以编写异步的逻辑或者是一些逻辑，再去commit 我们的事件</font>
>
>* <font size=2>文件建好就开始开始写代码了</font>
>
>* <font size=2> 先在store.js文件中引入 vue 和 vuex 。并写一些state。引入各个文件夹
>* 当然 你得在你新建的文件夹下面新建点什么，不然可会报错的哦~</font>
>* <font size=2> 代码如下：(必须要写 Vue.use(Vuex) )</font>
```javascript
import Vue from 'vue'
import Vuex from 'vuex'

import actions from './actions'
import * as getters from './getters'
import mutations from './mutations'

Vue.use(Vuex)

const state = {
    total:2,
    lists: [
      {
        id:0,
        articleName : '如何学好vue',
        avatar : 'http://cn.vuejs.org/images/logo.png',
        date : '2016-12-25',
        comment : 'vue是一套构建用户界面的 渐进式框架。与其他重量级框架不同的是，Vue 采用自底向上增量开发的设计。Vue 的核心库只关注视图层， 并且非常容易学习，非常容易与其它库或已有项目整合。另一方面，Vue 完全有能力驱动采用单文件组件和 Vue 生态系统支持的库开发的复杂单页应用。Vue.js 的目标是通过尽可能简单的 API 实现响应的数据绑定和组合的视图组件。',
        likes: 113,
        liked:true
      },
      {
        id:1,
        articleName : '如何完全读懂javascript高级程序设计',
        avatar : 'https://sfault-avatar.b0.upaiyun.com/147/223/147223148-573297d0913c5_huge256',
        date : '2017-12-25',
        comment : '从最早期Netscape浏览器中的JavaScript开始讲起，直到当前它对XML和Web服务的具体支持，内容主要涉及JavaScript的语言特点、 JavaScript与浏览器的交互、更高级的JavaScript技巧，以及与在Web应用程序中部署JavaScript解决方案有关的问题，如错误处理、调试、安全性、优化/混淆化、XML和Web服务，最后介绍应用所有这些知识来创建动态用户界面。',
        likes: 564,
        liked:false
      }
    ]
};

export default new Vuex.Store({
  state, //状态
  getters, //处理state数据
  mutations, // 注册我们各种数据变化的方法
  actions, //可以编写异步的逻辑或者是一些逻辑，再去commit 我们的事件
})

```

>* <font size=2> state中是你的数据 </font>
>
>* <font size=2> 打开你在各个文件夹下新建的js文件。首先我们先处理数据，即对state进行处理，筛选或过滤或...反正就是当没有这些数据也不至于让你的项目报错！</font>
>
>* <font size=2> 怎么做就不用我告诉你了吧~ 你要导出你写的方法哦 不然 在 store 中挂载不上去~</font>
>
>* <font size=2> 然后在 **mutations-types.js** 文件中写上你创建的应用需要的事件 可以慢慢写 毕竟都是一步一步来的嘛！</font>
>
>* <font size=2> 再在mutations文件夹中写你的方法</font>
>
>* <font size=2> actions commit你的事件</font>
>
>* <font size=2> 流程清楚了嘛？ 不懂点击上面的链接参考代码~</font>
>
































More info: [麦麦](maimai123.github.io)

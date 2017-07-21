---
title: vue或其他小难题整理（麦麦）
---
初学vue或其他各种构建工具时会遇到各种问题，没错！这篇文章又是来整理这些愚蠢的问题的~ :-D
<font size=1>**如有转载，请注明出处。**</font>
## Vue
## 1、emmet在jsx中不能使用的情况
> 大家一定深受其害吧？不怕不怕 麦麦这就告诉你怎么才能在jsx中使用emmet
> 我用的是atom编辑器。
> 首先，当然是打开atom 安装emmet插件 打开设置->key bindings -> 点击 用户键盘映射 ->修改文件
```javascript
'atom-text-editor[data-grammar="source js jsx vue"]':
  'F12':'atom-html-preview:toggle',
  'tab': 'emmet:expand-abbreviation-with-tab'
```
> 这样就ok啦~ 尽情的使用emmet吧！

## 2、一个关于webpack的问题。
> 起服务以后，用localhost访问可以访问，
> 用具体ip 如：192.168.91.75:8080/index 却访问不到地址
> 这是啥问题呐(⊙o⊙)?
> 原来原来 只要在配置文件中加上 --host 0.0.0.0 就可以啦
> 具体在哪里加呢 答案就是在**package.json**中的start中啦~
```javascript
"scripts": {
  "start": "webpack-dev-server --progress --profile --colors --hot --host 0.0.0.0",
  "build": "webpack --progress --profile --colors --config webpack.production.config.js"
}
```

## 3、说个vue的 外链css的问题
> 再写vue的时候，虽说在页面内部可以写style来增加样式，但还是推荐外链css
> 于是我import 'xxx.less' --->报错！！！我简直一个大写的懵逼啊 因为我在react中都是这么引入css的
> 后来我才知道 vue中 外链css 要卸载style标签内部 然后@import 'xxx.less'
> 这样就没问题了~
> 还有个如果你想在页面的style中使用less或sass要怎么办呢
> 给style加个 lang="less"就可以啦

<!--more-->
## 4、对于github一直要输入账户名密码问题
> git config -- global user.name "maimai123"
> git config --global user.email maimaixxx@163.com
> git config -- global credential.helperstore

## 5、vue中路由在浏览器地址栏显示/#/的问题 与react-router中的hashHistory browserHistory 一样的道理，
> 路由中换个模式就好了
> 那么怎么写呢 参考地址：[vue-router](http://router.vuejs.org/en/essentials/history-mode.html)

<img src="https://github.com/maimai123/maimai123.github.io/blob/master/img/router.png?raw=true" />

## 6、vue-cli+webpack搭建脚手架非常容易，但端口是默认为8080，在react时改变端口一般写在webpack.config.json中的 devServer 中，写法如下：
```javascript
  devServer:{ //配置服务器环境
    colors:true,
    historyApiFallback:true,
    inline:true, //自动刷新
    port:'1234', //修改端口号
    host : '0.0.0.0',
    progress:true
},

```

There are some additional options:

--content-base <file/directory/url/port>: base path for the content.
--quiet: don’t output anything to the console.
--no-info: suppress boring information.
--colors: add some colors to the output.
--no-colors: don’t use colors in the output.
--compress: use gzip compression.
--host <hostname/ip>: hostname or IP. 0.0.0.0 binds to all hosts.
--port <number>: port.
--inline: embed the webpack-dev-server runtime into the bundle.
--hot: adds the HotModuleReplacementPlugin and switch the server to hot mode. Note: make sure you don’t add HotModuleReplacementPlugin twice.
--hot --inline also adds the webpack/hot/dev-server entry.
--public: overrides the host and port used in --inline mode for the client (useful for a VM or Docker).
--lazy: no watching, compiles on request (cannot be combined with --hot).
--https: serves webpack-dev-server over HTTPS Protocol. Includes a self-signed certificate that is used when serving the requests.
--cert, --cacert, --key: Paths the certificate files.
--open: opens the url in default browser (for webpack-dev-server versions > 2.0).
--history-api-fallback: enables support for history API fallback.
--client-log-level: controls the console log messages shown in the browser. Use error, warning, info or none.

> 也可以写在package.json的start中。如：webpack-dev-server --devtool eval --progress --colors --inline --hot --content-base build --port1234
> 但vue-cli中如果希望修改端口号，则进入~\config\index.js，修改dev下的port为希望启动的端口号。例如: port: 1234,

## 7、在学习vuex的时候，我曾遇到过一个愚蠢的问题 【捂脸】就是在dispatch action中的方法时显示找不到这个方法
> 我左看右看没找出问题在哪，最后求救师傅，才知道问题所在。
> 我在actions、mutations和getters中都默认输出了，即：export default{ ... }, 而在store中还以
> import * as actions from './actions'
> 这种方法引入。。。简直作死啊~~
> 以后你们可别犯这种傻错啊

## 8、换了新电脑是 --air 用不惯，以前windows下个git就能完成的事现在需要安装一个xcode、sourcetree等搞得我用不惯 不过在努力学
> 真是一步一个坎啊 我先跟大家说一下 我把我的博客的挣个文件夹都从原来的电脑里copy了过来，很难想象我要是脑筋粗大一点没拷过来会是怎样
> 我在github上新建了仓库，里面啥东西也没有 我是打算用来存放我的hexo源文件的。
> 然后我下载了git、xcode、sourcetree等工具。废话有点多 别介意啊
> 我先查了一下我git 安装完成没有 git --vertion看一下版本号有版本号就是安装完成了
> 当然你首先要安装node。这个总不用我教了吧？
> 在安装hexo -> npm install -g hexo-cli
> 安装完也看一下安装了没 然后在命令里新建个ssh文件 命令->ssh-keygen -t rsa -C youremail@example.com(你的Github登录名)
> 然后复制id_rsa.pub的内容并将其填入Github那边的key。
> 安装完 sourcetree 之后你就可以点击新仓库 从url克隆，把你的新仓库复制来放进去，选好文件夹拖进你的hexo源文件
> 最后提交一下就好啦~
> 你试着在hexo源文件下打hexo s 看看有没有起本地服务吧~
> 当然在此之前你需要npm install一下来安装依赖啦 :-D

## 9、vue的小问题
> vue图片地址为空会报错 所以需要先给图片要做一个判断->v-if
> 地址为空的时候不渲染
> Img 添加路径（循环取变量）时要加bind 即:src=“item.imgUrl”
> href、id、disabled也是一样
> 如果是添加行内样式则是:style=“{backgroundImage:’url(‘+list.url+’)’}”
> 双引号单引号你就自己看吧 哈哈 毕竟我也是一脸懵逼

## 10、移动端阻止回弹
> 一些移动设备有缺省的touchmove行为，比如说经典的iOSoverscroll效果，
> 当滚动超出了内容的界限时就引发视图反弹。
> 这种做法在许多多点触控应用中会带来混乱，所以要禁用它。
```javascript
  document.body.addEventListener('touchmove',function(event){
  event.preventDefault();
  },false);
```

## 11、vue props传递数据
> 众所周知，vue中的props和react中的props一样，都是用来父子组件通信用的
> 在父组件中传递给子组件 只需要在子组件上加要传递的数据 ps:注意动态的要用v-bind/:哦~
> 还有个字面量语法问题 <comp some-prop="1"></comp> 这样传递的是一个字符串
> 如果想要传递number，也要使用v-bind
> 在子组件中用props接收即可
> 那么如果在子组件中调用父组件的方法呢？ 也是可以的
> vue 使用v-on绑定自定义事件
> 使用 \$on(eventName) 监听事件
> 使用 \$emit(eventName) 触发事件
> 父组件在使用子组件的地方直接用 v-on 来监听子组件触发的事件，直接在模板里绑定
> 如：
```javascript
<button-counter @increment="incrementTotal"></button-counter>
```
> incrementTotal是父组件的方法 increment是传递给子组件的props名字
> 子组件用 this.$emit('increment') 调用




## 12、请耐心等待，麦麦会不定期更新哒~
More info: [麦麦](https://github.com/maimai123)

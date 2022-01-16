---
title: 服务端渲染原理初探
date: 2022-01-15
tags: ssr
---
服务端渲染到底是怎么回事呢？为什么要做服务端渲染，有什么好处？带着这些问题，我们来看看服务端渲染的真面目吧~

有很多时候，我们做项目的时候，接入服务端渲染都是用一些应用框架，如react的Next.js， vue的Nuxt.js、vue-server-renderer插件，那么本质是什么呢？
没有深入了解之前，我知道是通过renderToString()方法，用来将组件输出成 HTML 字符串，node作为中间层，把html字符串返回给浏览器，那么具体实现是怎么样呢？以下以 React 项目举例。

# let's start!
<font size=1>**如有转载，请注明出处。**</font>

> 为什么要做服务端渲染

1. 利于SEO，搜索引擎爬虫工具可以查看到完全渲染的页面，有利于排到搜索引擎的前面。
2. 提高首屏加载速度，无需等待所有的js加载执行完，用户可以更快地看到完整的页面。
在 SPA 单页模式下，所有的数据请求和 Dom 渲染都在浏览器端完成，所以当我们第一次访问页面的时候很可能会存在“白屏”等待，而服务端渲染所有数据请求和 html内容已在服务端处理完成，浏览器收到的是完整的 html 内容，可以更快的看到渲染内容。

虽然服务端渲染有一说好处，但也有缺点
1. 需要在node server环境上运行，而完全静态的SPA可以在任何静态文件服务器上运行。
2. 更多的服务端负载。

因此在使用ssr之前要权衡下是否真的需要它，取决于对首屏加载时间的要求和SEO优化。服务端渲染适合特别依赖搜索引擎流量或对首屏时间有特殊要求的项目，如官网等。

> 如何做服务端渲染？

在前文中提到利用renderToString()方法可以有能力将虚拟dom编译成 HTML 字符串，但我们在开发过程中UI有一些事件绑定，编译出来的 HTML 事件绑定无效，因为 renderToString 并没有做事件相关的处理，因此返回给浏览器的内容不会有事件绑定。那么怎么解决这个问题呢？

这就需要【同构】了。所谓同构，通俗的讲，就是服务端渲染完成页面结构，浏览器端渲染完成事件绑定。即后端渲染和前端交互。

同构的流程如下：
【服务端运行React代码生成HTML】 => 【发送HTML给浏览器】 => 【浏览器显示内容】 => 【浏览器加载js文件】 => 【执行js并接管页面的操作】

接下来让我们实践一下吧~

```javascript
  npm init
  npm i react express react-dom react-router-dom
```

新建server.js

```javascript
const express = require('express');
const app = express();
const React = require('react');
const {renderToString} = require('react-dom/server');
const App = class extends React.PureComponent{
  render(){
    return React.createElement("h1",null,"Hello World");;
  }
};
app.get('/',function(req,res){
  const content = renderToString(React.createElement(App));
  res.send(content);
});
app.listen(3000);
```
node server.js启动localhost:3000就能看到浏览器输出的 Hello World。
打开源代码也可以看到相对应的Dom。如果是客户端渲染的话，是无法看到了Hello World的。

上面的代码我们不能使用jsx,也不能使用es6的import、export语法，
所以我们加入webpack, 修改webpack配置, 引入babel。

```javascript
npm i webpack babel-loader @babel/core @babel/preset-react @babel/preset-env --save-dev

// 新增.babelrc文件，写入
{
  "presets": ["@babel/preset-env", "@babel/preset-react"]
}

// 新建webpack文件 build/webpack.serve.js
const path = require('path');
const nodeExternals = require('webpack-node-externals');

module.exports = {
  entry:{
    index:path.resolve(__dirname,'../server.js')
  },
  mode:'development',
  target:'node',
  output:{
    filename:'[name].js',
    path:path.resolve(__dirname,'../dist/server')
  },
  externals:[nodeExternals()],
  resolve:{
    alias:{
      '@':path.resolve(__dirname,'../src')
    },
    extensions:['.js', '.jsx']
  },
  module:{
    rules:[{
      test:/\.js|.jsx$/, // 注意要配置jsx结尾的文件，不然解析不了jsx
      use:'babel-loader',
      exclude:/node_modules/
    }]
  },
}

```

在package.json中添加script
```javascript
  "scripts": {
    "build": "npm-run-all --parallel build:* -l -n",
    "build:server": "webpack --config build/webpack.server.js --watch",
    "build:client": "webpack --config build/webpack.client.js --watch",
    "server": "nodemon dist/server/index.js"
  },
```

```javascript
// 修改 server.js
import express from 'express';
import React from 'react';
const app = express();
const {renderToString} = require('react-dom/server');
import App from '@/index';
app.get('/',function(req,res){
  const content = renderToString(<App/>);
  console.log(content);
  res.send(content);
});
app.listen(3000);

// app.jsx
import React from 'react';

export default () => {
    const handleClick = () => {
        console.log(111)
    }

    return (
        <div>
            <h1>Page</h1>
            <button onClick={handleClick}>点击</button>
        </div>
    )
}
```

npm run server 启动以后我们发现，点击按钮还是没反应的，这时候就需要执行客户端打包，引用打包出来的js代码

修改如下
```javascript
// 新增webpack.client.js
const path = require('path');
module.exports = {
  entry:{
    index:path.resolve(__dirname,'../src/index.jsx')
  },
  mode:'development',
  output:{
    filename:'[name].js',
    path:path.resolve(__dirname,'../dist/client')
  },
  resolve:{
    alias:{
      '@':path.resolve(__dirname,'../src')
    },
    extensions:['.js', '.jsx']
  },
  module:{
    rules:[{
      test:/\.js|.jsx$/,
      use:'babel-loader',
      exclude:/node_modules/
    }]
  }
}

// index.js
import React from 'react';
import { hydrate } from 'react-dom';
import App from './app';
hydrate(<App/>,document.getElementById("root"));

// 修改 server.js
import express from 'express';
import React from 'react';
const app = express();
const {renderToString} = require('react-dom/server');
import App from '@/App';
app.use(express.static("dist"))
app.get('/',function(req,res){
  const content = renderToString(<App/>);
  res.send(`
        <!doctype html>
        <html>
            <title>ssr</title>
            <body>
                <div id="root">${content}</div>
                <script src="/client/index.js"></script>
            </body>
        </html>
    `);
});
app.listen(3000);
```
虽然已经可以点击执行函数了，但是chrome会报一个警告：Warning: render(): Calling ReactDOM.render() to hydrate server-rendered markup will stop working in React v18. Replace the ReactDOM.render() call with ReactDOM.hydrate() if you want React to attach to the server HTML.
提醒我们在服务端渲染时用ReactDOM.hydrate()来取代ReactDOM.render()，并警告我们在react17时将不能用ReactDOM.render()去混合服务端渲染出来的标签。

> ReactDOM.render()会将挂载dom节点的所有子节点全部清空掉，再重新生成子节点。
> ReactDOM.hydrate()则会复用挂载dom节点的子节点，并将其与react的virtualDom关联上。

改下index.js代码
```javascript
import React from 'react';
import { hydrate } from 'react-dom';
import App from './app';
hydrate(<App/>,document.getElementById("root"));
```

好啦~ 以上代码已经讲解了如何最简单的实现服务端渲染。下一篇讲如何加入路由、redux等...

不定期更新~~~

More info: [麦麦](https://github.com/maimai123)

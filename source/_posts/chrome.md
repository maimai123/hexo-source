---
title: 简单的写一个chrome扩展程序（使用vue）
date: 2020-03-18
tags: extensions
---
哈哈哈 终于对chrome 扩展程序下手了，之前就看过这方面的资料，刚好这次公司要做一个简单的扩展
之前愚蠢的认为这个扩展只能在chrome里使用，其实除了Chrome浏览器之外，chrome 扩展程序还可以运行在所有webkit内核的国产浏览器，比如360极速浏览器、360安全浏览器、搜狗浏览器、QQ浏览器等等
好啦！！我们下来来学学看！

[这里是我的demo地址](https://github.com/maimai123/chrome-todo)

# let's start!
<font size=1>**[转载至](https://www.cnblogs.com/liuxianan/p/chrome-plugin-develop.html)**</font>
> 开发与调试

*Chrome插件没有严格的项目结构要求，只要保证本目录有一个**manifest.json**即可，也不需要专门的IDE，普通的web开发工具即可。*

*从右上角菜单->更多工具->扩展程序可以进入 插件管理页面，也可以直接在地址栏输入 [chrome://extensions](chrome://extensions) 访问。*

*勾选开发者模式即可以文件夹的形式直接加载插件，否则只能安装.crx格式的文件。Chrome要求插件必须从它的Chrome应用商店安装，其它任何网站下载的都无法直接安装，所以，其实我们可以把crx文件解压，然后通过开发者模式直接加载。*

*开发中，代码有任何改动都必须重新加载插件，只需要在插件管理页按下Ctrl+R即可，以防万一最好还把页面刷新一下。*

> manifest.json

这是一个Chrome插件最重要也是必不可少的文件，用来配置所有和插件相关的配置，必须放在根目录。其中，manifest_version、name、version3个是必不可少的，description和icons是推荐的。

下面是我的 manifest.json
[文档地址](http://chrome.cenchy.com/permission_warnings.html)
```javascript
  // 这里就展示了一下我用到的一些配置，其他的可以查询文档哦 文档地址放在上面了
  {
    "name": "todo",
    "version": "1.0.0",
    "manifest_version": 2, // 必须是2
    "description": "chrome plugin demo",
    "browser_action": {
      "default_icon": "assets/icon.png",
      "default_title": "todo",
      "default_popup": "index.html"
    },
    "icons":
    {
      "16": "assets/icon.png",
      "48": "assets/icon.png",
      "128": "assets/icon.png"
    },
    "content_security_policy": "style-src 'self' 'unsafe-inline';script-src 'self' 'unsafe-eval'; object-src 'self' ;",
    "homepage_url": "https://maimai123.github.io", // 插件主页，不要浪费了这个免费广告位
    "background":
    {
      // 2种指定方式，如果指定JS，那么会自动生成一个背景页 我这边就是用的js
      // "page": "background.html",
      "scripts": ["background.js"]
    },
    "permissions": // 权限申请
      [
        "contextMenus",
        "tabs",
        "notifications",
        "storage",
        "cookies",
        "webRequest",
        "webRequestBlocking",
        "background",
        "https://*/*",
        "http://*/*"
      ]
  }

```
> background:

*background是一个常驻的页面，它的生命周期是插件中所有类型页面中最长的，它随着浏览器的打开而打开，随着浏览器的关闭而关闭，所以通常把需要一直运行的、启动就运行的、全局的代码放在background里面。*

*background的权限非常高，几乎可以调用所有的Chrome扩展API（除了devtools），而且它可以无限制跨域，也就是可以跨域访问任何网站而无需要求对方设置CORS*

*需要特别说明的是，你怎么查看你再background里面打印的东西呢，直接扩展上右击打开的可不是background哦，需要你再扩展程序页面打击背景页查看哦。

```javascript
// 这里是我的background.js 简单的获取数据
let url = 'http://192.168.31.194:8002';

let cookie;

chrome.cookies.get({ // 获取cookie
	url: 'http://192.168.31.221',
	name: 'SESSION'
}, function (cookie) {
	cookie = cookie.value;
});

  function fetchUserInfo () { // 在js上可以通过chrome.extension.getBackgroundPage().fetchUserInfo()调用到方法
    return fetchData(url + '/api/user/getUserInfo');
  }

  function fetchData (url) {
    return new Promise((resolve, reject) => {
      fetch(url, {
        method:'GET',
        credentials: 'include',
        headers: {
          Authorization: 'admin',
          Cookie: cookie
        }
      })
      .then(async function(response) {
        const { data } = await response.json();
        resolve(data);
      })
      .catch(function(err){
        console.log("err:" + err);
        reject(err);
      });
    })
  }
```

> popup

*popup是点击图标时打开的一个小窗口网页，焦点离开网页就立即关闭，一般用来做一些临时性的交互。*
*在权限上和background非常类似，它们之间最大的不同是生命周期的不同，popup中可以直接通过chrome.extension.getBackgroundPage()获取background的window对象。*

> 如何在扩展程序中使用vue呢?

```javascript
  // 在popup.html中直接引用 我也用了swiper 也是直接引入
  <link rel="stylesheet" href="css/swiper.min.css" />
  <script src="./js/vue.js"></script>
  <script src="./js/swiper.min.js"></script>
  <script src="./js/vue-awesome-swiper.js"></script>

  <div id="app">
    <swiper ref="mySwiper" :options="swiperOption">
      <swiper-slide v-for="item in list" :key="item.date">
        <div class="item" >
          <div class="week">{{ item.week }}</div>
          <div class="day">{{ new Date(item.date).getDate() }}</div>
        </div>
      </swiper-slide>
    </swiper>
  </div>

  // js文件中实例化
  var vm = new Vue({
    el:"#app",
    data() {
      let self = this; // 用self指向this是因为直接在里面拿不到正确的this
      return {
        swiperOption: { // swiper参数可以看文档哦
          slidesPerView: 3,
          spaceBetween: 20,
          centeredSlides: true,
          slideToClickedSlide:true,
          on: {
            click: () => {
              let index = this.swiper.clickedIndex;
              self.handleChangeCurrent(index)
            }
          }
        },
        list: []
      }
    },
    computed: {
      swiper() { // 在方法中直接this.swiper可以拿到swiper
        return this.$refs.mySwiper.swiper
      }
    },
    beforeDestroy () { // 离开了记得销毁哦
      this.swiper = null;
    },
    methods: {
      handleChangeCurrent(index) {
        // 可以在滑动的之后做一些事情
      }
    }
  });
```

> 在开发过程中我也需要了一点问题，如果你们也遇到了可以参考哦
1、在文件内直接写js报错

```javascript
  <script type="text/javascript">这里是你的代码</script> // 运行到此处出错 解决办法：将代码保存为独立的js文件，然后动态引用即可。
  // 下面是参考文章
```
[Chrome扩展popup页面使用JavaScript脚本报错: Refused to execute inline script because....](http://www.oceancoder.cn/post/popup-invoke-js-error.html)

> 2、Chrome Extension 中的 CSP（Content Security Policy）

*在进行Chrome拓展程序开发的时候，我们经常会遇到需要加载第三方库的情况，常见的如Jquery/Bootstrap库等,然而Chrome Extension的开发与一般网页不同，当我们在页面中加入如下代码时*

*我们看到浏览器拒绝加载这个执行语句，因为它违反了Content Security Policy(简称CSP)机制*

*setTimeout 等都是不允许的*

*提示拒绝 ‘evaluate a string as JavaScript’ ，我们查阅官方文档就可以发现 eval() 函数同样是被CSP所禁止,类似的还有setTimeout(String), setInterval(String) 和new Function(String)*

*那要怎么解决呢，官方文档提供了一种解决办法，叫做 放宽默认策略 （关于放宽默认策略更多的介绍，可以访问Content Security Policy Reference来获取），通过添加 ‘unsafe-eval’ 来实现，即在mainfest.json中加入下面代码：*

```javascript
  "content_security_policy": "style-src 'self' 'unsafe-inline';script-src 'self' 'unsafe-eval' https://cdn.bootcss.com; object-src 'self' ;",
  // 下面是参考文章
```
[Chrome Extension 中的 CSP（Content Security Policy） 开发小记](https://blog.csdn.net/qq_21859119/article/details/78802687)


More info: [麦麦](maimai123.github.io)

---
title: 在react中实现滑动到某处执行动画
date: 2020-04-01
tags: react
---
hello~ 近日要做个官网。我打算给官网做一丢丢的动效，搜了一下wowjs挺好用的，然后又看到有个react-wow，就是把wow封装了一下，就都拿来试了一下，方法就写在下面拉。[wow demo地址](https://www.delac.io/wow/)

# let's start!
<font size=1>**如有转载，请注明出处。**</font>

> 引入react-wow的用法

```javascript
  import ReactWOW from 'react-wow';

  // 在global.less中引用animate.css

  <ReactWOW delay='0.1s' animation='rollIn'>
    <div>这里是你需要用到动画的dom</div>
  </ReactWOW>

```
> 引入wow的用法

```javascript
  import wow from 'wowjs';

  // 在global.less中引用animate.css

  useEffect(() => {
    const wows = new wow.WOW({
      live: false
    });
    wows.init();
  }, []);

  <div className="wow slideInLeft" data-wow-duration="2s" data-wow-delay="0.3s">这里是你需要用到动画的dom</div>
```

More info: [麦麦](maimai123.github.io)

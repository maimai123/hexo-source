---
title: [wangEditor](http://www.wangeditor.com/) 在光标处插入变量
date: 2020-03-26
tags: react
---
hello~ 项目中需要用到编辑器，寻寻觅觅找了个中国本土的wangEditor， 传说中最轻量级的编辑器。用起来毫不费力，直到某天遇到了个问题

# let's start!
<font size=1>**如有转载，请注明出处。**</font>
> 日常使用

```javascript
  import Editor from 'wangeditor';

  const elem = this.editorElem; // this.editorElem是dom元素
    this.editor = new Editor(elem);
    this.editor.customConfig.menus = [
      'head',
      'bold',
      'fontSize',
      'foreColor',
      'backColor',
      'italic',
      'link',
      'list',
      'underline',
      'justify',
      'image',
      'table',
      'emoticon',
    ];
    this.editor.customConfig.onchange = (html: any) => {
      // change回调
      this.setState({
        editData: {
          ...editData,
          content: html
        }
      })
    }
    this.editor.create();

    // HTML
    <div ref={ref => { this.editorElem = ref }}></div> // 这是react的写法
    <div ref="editorElem" /> // 这是vue的写法
```
> 下面是点击在光标处插入你需要插入的dom

```javascript
  addContent = (clickTagText: any) => {
    this.editor.cmd.do('insertHTML', `<span style="color: red;">我是标签啊</span>`)
  }
```

> 那么问题来了！人的欲望总是不穷无尽的 怎么才能实现删除的时候 整个删除标签呢？

```javascript
  // 用input去包裹 就能一整个删除拉，但是还是有点不好的地方 你需要去计算文字的宽度 一下我只做了中文的计算 一个中文差不多13个像素
  /* clickTagText 是要插入的标签文字 */
  this.editor.cmd.do('insertHTML', `<input readonly style="text-align: center;width: ${clickTagText.length * 13}px;border: none; height: 20px;color: red;" value="${clickTagText}" />`)
```

More info: [麦麦](maimai123.github.io)

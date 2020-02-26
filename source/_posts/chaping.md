---
title: 进入差评后的小笔记
date: 2018-02-05
tags: js
---
啦啦啦啦啦啦~
<font size=1>**如有转载，请注明出处。**</font>
## Intersting
## 1、Vue.filter 过滤
```javascript
filterShoppingList: function () {
    var key = this.key;
    var shoppingList = this.shoppingList;
    return shoppingList.filter(function (item) {
        return item.toLowerCase().indexOf(key.toLowerCase()) != -1
    });;
}
```

## 2、数组对象去重（根据时间一样去重，把title合并）
```javascript
var arr = [
  {
    time:'211521',
    title:[
      '111111111111111'
    ]
  },
  {
    time:'23333333333',
    title:[
      '我有一只小毛驴',
    ]
  },
  {
    time:'23333333333',
    title:[
      '我重来也不骑'
    ]
  },
  {
    time:'23333333333',
    title:[
      '有一天我心血来潮'
    ]
  },
  {
    time:'23werwef3',
    title:[
      '111111111111111'
    ]
  }
];
var result = [], hash = {};
for (var i = 0; i<arr.length; i++) { //去重
  var elem = arr[i].time;
  if (!hash[elem]) {
      result.push(arr[i]);
      hash[elem] = true;
  }else{
    for (var j = 0; j<result.length; j++) { //填数据
      if(result[j].time == elem){
        console.log(arr[i].title)
        result[j].title.push(arr[i].title[0])
      }
    }
  }
}
console.log(result)
```
<!--more-->
## 3、页面判断是手机就打开手机网址
```javascript
  let ua = window.navigator.userAgent.toLocaleLowerCase()
  let murl ="//m.jd.com",
  let reg =/iphone|android|symbianos|windows\sphone/g
  if (reg .test(ua )) {
      window.location.href = murl
  }
```

## 4、 返回对象的函数能够用于链式操作
```javascript
function Person(name) {
  this.name = name;
  this.sayName = function() {
    console.log("Hello my name is: ", this.name);
    return this;
  };
  this.changeName = function(name) {
    this.name = name;
    return this;
  };
}
var person = new Person("John");
person.sayName().changeName("Timmy").sayName();
//Hello my name is: John
//Hello my name is: Timmy
```
## 5、微信小程序实现多选样式更改
```javascript
// wxml
<checkbox-group bindchange="checkboxChange">
  <view class="checkbox_selection">
   <label wx:for="{{allValue}}" wx:key="{{item.value}}" class="checkbox {{item.checked?'is_checked':''}}">
    <checkbox value="{{item.value}}" checked="{{item.checked}}" hidden="false" />{{item.name}}
   </label>
  </view>
</checkbox-group>
// wxss
.checkbox_selection {
  padding: 15px 10px;
  background: #fff;
}
.checkbox_selection label {
  background: #f0f1ec;
  padding: 6px 7px;
  font-size: 12px;
  border-radius: 4px;
  margin:4rpx;
}

.checkbox_selection .is_checked {
  background: lightblue;
}
// js
var app = getApp();
Page({
  data: {
    allValue: [
      { name: '人力资源', value: '0', checked: true },
      { name: '项目资金', value: '1', checked: false },
      { name: '项目资金', value: '2', checked: false },
      { name: '项目资金', value: '3', checked: false },
    ],
  },
  checkboxChange: function (e) {
    var allValue = this.data.allValue;
    var checkArr = e.detail.value;
    console.log(checkArr)
    for (var i = 0; i < allValue.length; i++) {
      if (checkArr.indexOf(i + "") != -1) {
        allValue[i].checked = true;
      } else {
        allValue[i].checked = false;
      }
    }
    this.setData({
      allValue: allValue
    })
  }
})
```

## 6、微信小程序实现九宫格拖拽
![img](https://github.com/maimai123/maimai123.github.io/blob/master/img/jgg.gif?raw=true)
```javascript
记得把图片换成自己本地的
// wxml
<view bindtap="box" class="box" >
   <image
     disable-scroll="true"
     wx:for="{{content}}"
     bindtouchmove="move"
     bindtouchstart="movestart"
     bindtouchend="moveend"
     data-index="{{item.id}}"
     data-main="{{mainx}}"
     class="main {{mainx == item.id? 'mainmove':'mainend'}}"
     style="left:{{start.x}}px; top:{{start.y}}px"
     bindtap="previewImages"
     data-id="{{idx}}"
     mode="widthFix"
     src="{{item.content}}"
       ></image>
</view>
// wxss
.box{width:100%; position: relative}
.main{
  width:115px;
  height:115px;
  background: #eee;
  border: 1px solid #ccc;
  margin:2rpx 4rpx ;
  box-sizing: border-box;
  text-align: center;
  line-height:115px;
  display: inline-block;
}
.mainmove{position: absolute; opacity: 0.7}
.maind{background: #fff; border:1px dashed #efefef;}
.mainend{position: static; opacity: 1;}
// js
var app = getApp();
var x, y, x1, y1, x2, y2, index, currindex, n, yy;
var arr1 = [
{ content: 'wxfile://tmp_795160320o6zAJsxg0kaja8hhRyhkO81BBPuUdc0fc00003a6d08484020a52c2c4aefc.png', id: 1 },
{ content: 'wxfile://tmp_795160320o6zAJsxg0kaja8hhRyhkO81BBPuUcb013dedafe0a58c103d27536f665ff7.png', id: 2 },
{ content: 'wxfile://tmp_795160320o6zAJsxg0kaja8hhRyhkO81BBPuU13f3e7ed7c4e9e08b38753d1ac5ab0b3.png', id: 3 },
{ content: 'wxfile://tmp_795160320o6zAJsxg0kaja8hhRyhkO81BBPuUcb013dedafe0a58c103d27536f665ff7.png', id: 4 },
{ content: 'wxfile://tmp_795160320o6zAJsxg0kaja8hhRyhkO81BBPuU5ebd2117e840f1a99944c866d3ee1d52.jpg', id: 5 },
{ content: 'wxfile://tmp_795160320o6zAJsxg0kaja8hhRyhkO81BBPuUcb013dedafe0a58c103d27536f665ff7.png', id: 6 },
{ content: 'wxfile://tmp_795160320o6zAJsxg0kaja8hhRyhkO81BBPuUe5a6e958fb019ab474b24705d2b6a630.png', id: 7 },
{ content: 'wxfile://tmp_795160320o6zAJsxg0kaja8hhRyhkO81BBPuUcb013dedafe0a58c103d27536f665ff7.png', id: 8 },
{ content: 'wxfile://tmp_795160320o6zAJsxg0kaja8hhRyhkO81BBPuU4bdd160fc442cae480c94ad8324fb0de.png', id: 9 }];
Page({
  data: {
    motto: 'Hello World',
    userInfo: {},
    mailen: 0,
    content: [
{ content:'wxfile://tmp_795160320o6zAJsxg0kaja8hhRyhkO81BBPuUdc0fc00003a6d08484020a52c2c4aefc.png', id: 1 },
{ content: 'wxfile://tmp_795160320o6zAJsxg0kaja8hhRyhkO81BBPuUcb013dedafe0a58c103d27536f665ff7.png', id: 2 },
{ content: 'wxfile://tmp_795160320o6zAJsxg0kaja8hhRyhkO81BBPuU13f3e7ed7c4e9e08b38753d1ac5ab0b3.png', id: 3 },
{ content: 'wxfile://tmp_795160320o6zAJsxg0kaja8hhRyhkO81BBPuUcb013dedafe0a58c103d27536f665ff7.png', id: 4 },
{ content: 'wxfile://tmp_795160320o6zAJsxg0kaja8hhRyhkO81BBPuU5ebd2117e840f1a99944c866d3ee1d52.jpg', id: 5 },
{ content: 'wxfile://tmp_795160320o6zAJsxg0kaja8hhRyhkO81BBPuUcb013dedafe0a58c103d27536f665ff7.png', id: 6 },
{ content: 'wxfile://tmp_795160320o6zAJsxg0kaja8hhRyhkO81BBPuUe5a6e958fb019ab474b24705d2b6a630.png', id: 7 },
{ content: 'wxfile://tmp_795160320o6zAJsxg0kaja8hhRyhkO81BBPuUcb013dedafe0a58c103d27536f665ff7.png', id: 8 },
{ content: 'wxfile://tmp_795160320o6zAJsxg0kaja8hhRyhkO81BBPuU4bdd160fc442cae480c94ad8324fb0de.png', id: 9 },
    ],
    start: { x: 0, y: 0 },
    tempFilePaths: [],
    previewImg:[]
  },
  movestart: function (e) {
    currindex = e.target.dataset.index;
    x = e.touches[0].clientX;
    y = e.touches[0].clientY;
    x1 = e.currentTarget.offsetLeft;
    y1 = e.currentTarget.offsetTop;
  },
  move: function (e) {
    yy = e.currentTarget.offsetTop;
    x2 = e.touches[0].clientX - x + x1;
    y2 = e.touches[0].clientY - y + y1;
    this.setData({
      mainx: currindex,
      opacity: 0.7,
      start: { x: x2, y: y2 }
    })
    // console.log(x2,y2)
  },
  moveend: function () {
    var arr = [];
    for (var i = 0; i < this.data.content.length; i++) {
      arr.push(this.data.content[i]);
    }
    var nx = this.data.content.length;
    if (x2 != 0 || y2 != 0){
      var xz = 0, yz=0 , n=0;
      for (var k = 1; k < nx / 3 +1; k++) {
        if (x2 > (115 * k - 58)) {
          xz = k ;
          console.log(xz)
        }
        if (y2 > (115 * k - 58)) {
          yz = k;
          console.log(xz)
        }
      }
      if (x2 > (115 * 3 + 10)) {
        xz = 3 ;
      }
      if (y2 > (115 * 3 + 10) ) {
        yz = 3;
      }
      n = xz + (3*yz);
      arr.splice((currindex - 1), 1);
      arr.splice(n, 0, arr1[currindex - 1]);
      arr1 = [];
      for (var m = 0; m < this.data.content.length; m++) {
        arr[m].id = m + 1;
        arr1.push(arr[m]);
      }
      this.setData({
        mainx: "",
        content: arr,
        opacity: 1
      })
    }
  },
  upload: function () {
    var self = this;
    var arr = [];
    wx.chooseImage({
      count: 9, // 默认9
      sizeType: ['original', 'compressed'], // 可以指定是原图还是压缩图，默认二者都有
      sourceType: ['album', 'camera'], // 可以指定来源是相册还是相机，默认二者都有
      success: function (res) {
        // 返回选定照片的本地文件路径列表，tempFilePath可以作为img标签的src属性显示图片
        var tempFilePaths = res.tempFilePaths;
        for (var i = 0; i < tempFilePaths.length;i++){
          arr.push({
            content: tempFilePaths[i],
            id:i+1
          })
        }
        self.setData({
          tempFilePaths: self.data.tempFilePaths.concat(arr),
          previewImg: self.data.previewImg.concat(tempFilePaths)
        })
      }
    })
  },
  previewImages: function (e) {
    var self = this;
    var index = e.target.dataset.id;
    console.log(self.data.tempFilePaths)
    wx.previewImage({
      current: self.data.previewImg[index-1], // 当前显示图片的http链接
      urls: self.data.previewImg // 需要预览的图片http链接列表
    })
  },
  delete: function (e) {
    var index = e.currentTarget.dataset.index;
    var images = this.data.tempFilePaths;
    images.splice(index-1, 1);
    this.setData({
      tempFilePaths: images
    });
  }
})
```

## 7、有趣的css小动画
![img](https://user-images.githubusercontent.com/8554143/27017024-cf387976-4f57-11e7-9063-7e9fb2893a24.gif?_=6992177)
```javascript
<div class="triangle2rect">
    <div class="a"></div>
    <div class="b"></div>
    <div class="c"></div>
    <div class="d"></div>
</div>
.triangle2rect {
  position: absolute;
  width: 100px;
  height: 100px;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  animation: aniContainer 2s infinite alternate;
}
.triangle2rect div {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
}
.a {
    background: deeppink;
    clip-path: polygon(0% 0%, 0% 100%, 50% 50%);
    animation: a 2s infinite alternate;
}
.b {
    background: deeppink;
    clip-path: polygon(0% 0%, 100% 0%, 50% 50%);
    animation: b 2s infinite alternate;
}
.c {
    background: deeppink;
    clip-path: polygon(100% 0%, 100% 100%, 50% 50%);
    animation: c 2s infinite alternate;
}
.d {
    background: deeppink;
    clip-path: polygon(100% 100%, 0% 100%, 50% 50%);
    animation: d 2s infinite alternate;
}
@keyframes a {
    0%, 10% {
        background: white;
        clip-path: polygon(0% 0%, 0% 100%, 50% 50%);
    }
    90%, 100% {
        background: #000;
        clip-path: polygon(0% 100%, 25% 100%, 12.5% 0%);
    }
}
@keyframes b {
    0%, 10% {
        background: white;
        clip-path: polygon(0% 0%, 100% 0%, 50% 50%);
    }
    90%, 100% {
        background: #000;
        clip-path: polygon(25% 0%, 50% 0%, 37.5% 100%);
    }
}
@keyframes c {
    0%, 10% {
        background: white;
        clip-path: polygon(100% 0%, 100% 100%, 50% 50%);
    }
    90%, 100% {
        background: #000;
        clip-path: polygon(62.5% 0%, 75% 100%, 50% 100%);
    }
}
@keyframes d {
    0%, 10% {
        background: white;
        clip-path: polygon(100% 100%, 0% 100%, 50% 50%);
    }
    90%, 100% {
        background: #000;
        clip-path: polygon(100% 0%, 87.5% 100%, 75% 0%);
    }
}
```

## 7、请耐心等待，麦麦会不定期更新哒~
More info: [麦麦](https://github.com/maimai123)
